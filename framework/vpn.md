---

copyright:
  years: 2020, 2026
lastupdated: "2026-05-10"

keywords: isolation for {{site.data.keyword.secrets-manager_short}}, service endpoints for {{site.data.keyword.secrets-manager_short}}, private network for {{site.data.keyword.secrets-manager_short}}, network isolation in {{site.data.keyword.secrets-manager_short}}, non-public routes for {{site.data.keyword.secrets-manager_short}}, private connection for {{site.data.keyword.secrets-manager_short}}, vpn

subcollection: secrets-manager

---

{:codeblock: .codeblock}
{:screen: .screen}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:gif: data-image-type='gif'}
{:important: .important}
{:note: .note}
{:pre: .pre}
{:tip: .tip}
{:preview: .preview}
{:deprecated: .deprecated}
{:beta: .beta}
{:term: .term}
{:shortdesc: .shortdesc}
{:script: data-hd-video='script'}
{:support: data-reuse='support'}
{:table: .aria-labeledby="caption"}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:help: data-hd-content-type='help'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}
{:video: .video}
{:step: data-tutorial-type='step'}
{:tutorial: data-hd-content-type='tutorial'}
{:api: .ph data-hd-interface='api'}
{:cli: .ph data-hd-interface='cli'}
{:ui: .ph data-hd-interface='ui'}
{:terraform: .ph data-hd-interface="terraform"}
{:curl: .ph data-hd-programlang='curl'}
{:java: .ph data-hd-programlang='java'}
{:ruby: .ph data-hd-programlang='ruby'}
{:c#: .ph data-hd-programlang='c#'}
{:objectc: .ph data-hd-programlang='Objective C'}
{:python: .ph data-hd-programlang='python'}
{:javascript: .ph data-hd-programlang='javascript'}
{:php: .ph data-hd-programlang='PHP'}
{:swift: .ph data-hd-programlang='swift'}
{:curl: .ph data-hd-programlang='curl'}
{:dotnet-standard: .ph data-hd-programlang='dotnet-standard'}
{:go: .ph data-hd-programlang='go'}
{:unity: .ph data-hd-programlang='unity'}
{:release-note: data-hd-content-type='release-note'}

# Using Client-to-Site VPN to privately connect to {{site.data.keyword.secrets-manager_short}}
{: #vpn-connection}

You can securely reach a private-only {{site.data.keyword.secrets-manager_full}} over a {{site.data.keyword.cloud_notm}}–native path from a client workstation (for example, a Mac using OpenVPN Connect or another OpenVPN-compatible client), by using Client-to-Site (C2S) VPN Gateway in VPC and a Virtual Private Endpoint (VPE).

When the VPN establishes a connection, it allows the source device to send traffic for the VPE IP through its tunnel. On the VPN server side, a **translate (SNAT) route** makes traffic to the VPE IP appear as if it originates from the VPN server's own private IP inside the VPC. This configuration is required because the VPE accepts traffic only from inside the VPC address space.

The VPE Security Group allows HTTPS (TCP 443) **only from the VPN server's private IP to the VPE IP**. This means that only clients coming through this VPN server can reach {{site.data.keyword.secrets-manager_short}}. No other resources in the VPC or on the internet can communicate with the VPE.

In this example, domain name resolution uses `/etc/hosts`. For production-grade configuration, replace this with {{site.data.keyword.cloud_notm}} DNS Services.

![The diagram shows the connection from outside the {{site.data.keyword.cloud_notm}} network to {{site.data.keyword.secrets-manager_short}} using Client-to-Site VPN](../images/vpn-vpe-connectivity-diagram.svg){: caption="Client-to-Site VPN" caption-side="bottom"}
{: #shortdesc}

## Before you begin
{: #vpn-before-begin}

Before you set up the VPN connection, ensure that you have the following prerequisites in place.

### Required instances and services
{: #vpn-instances-software}

- A {{site.data.keyword.secrets-manager_short}} instance for managing the Client-to-Site VPN certificates
- A private-only {{site.data.keyword.secrets-manager_short}} instance that you want to access through the VPN
- An existing {{site.data.keyword.cloud_notm}} VPC and subnet in your gateway region (GW_REGION)

### Required software and tools
{: #vpn-required-software}

- macOS with OpenVPN Connect (or another OpenVPN-compatible client) installed
- {{site.data.keyword.cloud_notm}} CLI and VPC plugin: `ibmcloud plugin install vpc-infrastructure`
- `jq` command-line JSON processor

Disconnect all other VPN clients before testing this configuration to avoid routing conflicts.
{: important}

### Environment variables
{: #vpn-env-vars}

```sh
export SM_REGION="us-south"     # Secrets Manager region
export GW_REGION="us-south"     # Region where your VPC, VPE, and VPN live
export RG_NAME="Default"        # Resource Group

export SM_CRN="<secrets-manager-instance-crn>"              # Secrets Manager instance CRN
export SM_HOST="<secrets-manager-instance-host>"            # Secrets Manager instance host <instance_id.private.region.secrets-manager.appdomain.cloud>
export SM_PKI_ENDPOINT="<secrets-manager-pki-instance-url>" # Secrets Manager PKI instance endpoint URL <https://instance_id.region.secrets-manager.appdomain.cloud>

export VPC_ID="<existing-vpc-id>"       # Existing VPC ID in GW_REGION
export SUBNET_ID="<existing-subnet-id>" # Existing subnet ID in GW_REGION

export VPE_NAME="test-vpe-gw"     # Name for the VPE gateway
export SG_VPE_NAME="test-vpe-sg"  # Security Group to attach to the VPE

export VPN_NAME="test-vpn"          # Name for the Client-to-Site VPN
export SG_VPN_NAME="test-vpn-sg"    # Security Group to attach to the VPN
export CLIENT_POOL="10.241.0.0/22"  # VPN client IP pool
```
{: pre}

## Log in to your {{site.data.keyword.cloud_notm}} account
{: #vpn-connect-account}
{: step}

```sh
ibmcloud login -r "$GW_REGION" --sso
ibmcloud target -g "$RG_NAME"
```
{: pre}

## Create the VPE Security Group
{: #vpn-create-security-group}
{: step}

Create a dedicated Security Group for the VPE and capture its ID.

```sh
ibmcloud is security-group-create "$SG_VPE_NAME" "$VPC_ID"
export VPE_SG_ID=$(ibmcloud is security-groups --output json | jq -r ".[] | select(.name==\"$SG_VPE_NAME\") | .id")
```
{: pre}

## Create the Virtual Private Endpoint (VPE)
{: #vpn-create-vpe}
{: step}

Create a VPE in your VPC (GW_REGION) targeting the {{site.data.keyword.secrets-manager_short}} instance (SM_REGION) and allocate a reserved IP from your subnet.

```sh
ibmcloud is endpoint-gateway-create \
  --name "$VPE_NAME" \
  --vpc "$VPC_ID" \
  --target "$SM_CRN" \
  --target-type provider_cloud_service \
  --new-reserved-ip "{\"subnet\":{\"id\":\"$SUBNET_ID\"}}" \
  --sg "$VPE_SG_ID" \
  --resource-group-name "$RG_NAME"

# Wait for the VPE to reach stable state
ibmcloud is endpoint-gateway "$VPE_NAME" --output json | jq -r '.lifecycle_state'

# Capture the VPE IP (used later for routing and hosts entry)
export VPE_IP=$(ibmcloud is endpoint-gateway "$VPE_NAME" --output json | jq -r '.ips[0].address')

### Verify the VPE
# Show security groups attached to the VPE (names and IDs)
ibmcloud is endpoint-gateway "$VPE_NAME" --output json | jq -r '.security_groups[]? | "\(.name) \(.id)"'

# Show the reserved IP address allocated to the VPE
ibmcloud is endpoint-gateway "$VPE_NAME" --output json | jq -r '.ips[0].address'
```
{: pre}

## {{site.data.keyword.secrets-manager_short}} Private Certificate Engine
{: #vpn-private-cert-engine}
{: step}

Use the Private Certificate engine to create CAs and issue certificates.
Configure the {{site.data.keyword.secrets-manager_short}} CLI:

```sh
export SECRETS_MANAGER_URL="$SM_PKI_ENDPOINT"
```
{: pre}

### Create a root certificate authority (CLI)
{: #vpn-create-root-certificate-ca}
{: step}

Example (minimal) command to create a root CA configuration:

```sh
ibmcloud secrets-manager configuration-create \
    --config-type "private_cert_configuration_root_ca" \
    --name "vpn-root-CA" \
    --certificate-common-name "vpn.root.ca" \
    --private-cert-max-ttl "3652d" \
    --private-cert-format "pem" \
    --private-cert-private-key-type "rsa" \
    --private-cert-private-key-bits 2048 \
    --private-cert-distribution-points-encoded true \
    --private-cert-issuing-certificate-urls-encoded true
```
{: pre}

- The `name` (here `vpn-root-CA`) is how you refer to this root CA in later steps.
- Adjust fields like `common_name`, `max_ttl`, and key settings to your requirements.

### Create an intermediate CA signed by the root
{: #vpn-create-intermediate-ca}
{: step}

Example command to create an intermediate CA configuration that is signed by the root CA above:

```sh
ibmcloud secrets-manager configuration-create \
    --config-type "private_cert_configuration_intermediate_ca" \
    --name "vpn-intermediate-CA" \
    --certificate-common-name "vpn.int.com" \
    --private-cert-signing-method "internal" \
    --private-cert-issuer "vpn-root-CA" \
    --private-cert-max-ttl "2556d" \
    --private-cert-format "pem" \
    --private-cert-private-key-type "rsa" \
    --private-cert-private-key-bits 4096 \
    --private-cert-distribution-points-encoded true \
    --private-cert-issuing-certificate-urls-encoded true
```
{: pre}

Sign the intermediate CA:

```sh
ibmcloud secrets-manager configuration-action-create \
  --name "vpn-root-CA" \
  --config-action-action-type "private_cert_configuration_action_sign_intermediate" \
  --config-action-intermediate-certificate-authority "vpn-intermediate-CA"
```
{: pre}

### Create a certificate template
{: #vpn-create-certificate-template}
{: step}

A template controls what kinds of private certificates can be issued.

Example command to create a template bound to the intermediate CA:

```sh
ibmcloud secrets-manager configuration-create \
    --config-type "private_cert_configuration_template" \
    --name "vpn-certificate-template" \
    --private-cert-ca-name "vpn-intermediate-CA" \
    --private-cert-allowed-domains "vpn.ibm.com" \
    --private-cert-allowed-domains-template false \
    --private-cert-allow_subdomains true \
    --private-cert-server-flag true \
    --private-cert-client-flag true \
    --private-cert-key-type "rsa" \
    --private-cert-key-bits 2048 \
    --private-cert-max-ttl "365d"
```
{: pre}

### Issue the VPN server certificate
{: #vpn-issue-vpn-server-cert}
{: step}

Use the private certificates engine to issue a server certificate from the template:

```sh
ibmcloud secrets-manager secret-create \
  --secret-name "vpn-server-cert" \
  --secret-type "private_cert" \
  --secret-description "VPN server certificate" \
  --secret-ttl "365d" \
  --private-cert-certificate-template "vpn-certificate-template" \
  --certificate-common-name "server.vpn.ibm.com" \
```
{: pre}

- Capture the resulting secret CRN and set `VPN_CERT_CRN` to that value.

### Issue client certificates
{: #vpn-issue-client-cert}
{: step}

Use the private certificates engine to issue a client certificate from the template:

```sh
ibmcloud secrets-manager secret-create \
  --secret-name "vpn-client-cert" \
  --secret-type "private_cert" \
  --secret-description "VPN client certificate" \
  --secret-ttl "365d" \
  --private-cert-certificate-template "vpn-certificate-template" \
  --certificate-common-name "client.vpn.ibm.com"
```
{: pre}

Capture the server and client certificate CRNs:

```sh
export VPN_SERVER_CERT_CRN="<CRN of VPN server certificate secret>"
export VPN_CLIENT_CERT_CRN="<CRN of VPN client certificate secret>"
```
{: pre}

## Security Group for the VPN server
{: #vpn-security-group-vpn}
{: step}

Allow OpenVPN from your public IP and all outbound.

```sh
ibmcloud is security-group-create "$SG_VPN_NAME" "$VPC_ID"
export SG_ID=$(ibmcloud is security-groups --output json | jq -r ".[] | select(.name==\"$SG_VPN_NAME\") | .id")

# Find your public IPv4 and allow UDP 1194
curl -4 ifconfig.me
ibmcloud is security-group-rule-add "$SG_ID" inbound udp --port-min 1194 --port-max 1194 --remote x.x.x.x/32

# Allow all outbound
ibmcloud is security-group-rule-add "$SG_ID" outbound all --remote 0.0.0.0/0
```
{: pre}

## Create the Client-to-Site VPN server
{: #vpn-create-vpn}
{: step} 

Certificate-based client authentication against your CA; server presents the issued server certificate.

```sh
ibmcloud is vpn-server-create \
  --name "$VPN_NAME" \
  --vpc "$VPC_ID" \
  --subnet "$SUBNET_ID" \
  --client-ip-pool "$CLIENT_POOL" \
  --protocol udp \
  --port 1194 \
  --enable-split-tunnel true \
  --client-auth-methods certificate \
  --client-ca "$VPN_CLIENT_CERT_CRN" \
  --cert "$VPN_SERVER_CERT_CRN" \
  --sg "$SG_ID" \
  --resource-group-name "$RG_NAME"

# Wait for the VPN server to reach stable status
ibmcloud is vpn-server "$VPN_NAME"

# Capture IDs and server private IP
export VPN_ID=$(ibmcloud is vpn-servers --output json | jq -r ".[] | select(.name==\"$VPN_NAME\") | .id")
export VPN_PRIV_IP=$(ibmcloud is vpn-server "$VPN_ID" --output json | jq -r '.private_ips[0].address')

### Allow TCP 443 from the translated source (VPN server private IP) to the VPE.
ibmcloud is security-group-rule-add "$VPE_SG_ID" inbound tcp --port-min 443 --port-max 443 --remote "$VPN_PRIV_IP/32"
```
{: pre}

## Push a host route to the VPE
{: #vpn-push-host-route}
{: step}

```sh
# Create a translated (SNAT) host route to the VPE IP
ibmcloud is vpn-server-route-create "$VPN_ID" --name vpe-host --action translate --destination "$VPE_IP/32"

# Verify for stable status
ibmcloud is vpn-server-routes "$VPN_ID"
```
{: pre}

## Name resolution (example only)
{: #vpn-name-resolution}
{: step}

Map the {{site.data.keyword.secrets-manager_short}} hostname to the VPE IP on your local machine.

```sh
echo "$VPE_IP $SM_HOST" | sudo tee -a /etc/hosts
sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder
```
{: pre}

## Prepare the OpenVPN Connect client
{: #vpn-prepare-openvpn}
{: step}

Download the server profile:

- In the {{site.data.keyword.cloud_notm}} console left navigation menu, select Infrastructure > Network > VPNs.
- On the VPNs for VPC page, select the Client-to-Site servers pane and select `test-vpn`.
- On the `test-vpn` page, select the Clients tab and click the `All client profiles` button to download the client profile .ovpn files in ZIP format.
- Extract the ZIP file and import `client.ovpn` into OpenVPN Connect and connect.

When you modify VPN routes, disconnect and reconnect the VPN client to receive the updated route configuration.
{: tip}

## Validate from macOS
{: #vpn-validate-from-client}
{: step}

- `curl -v https://$SM_HOST`
- If it times out:
   - Confirm the client has a route to `$VPE_IP/32` and the VPN is connected.
   - Confirm the route action is translate and the VPE Security Group allows `$VPN_PRIV_IP/32 -> $VPE_IP/32` on TCP 443 (or your chosen source).
   - Check VPN server and VPE lifecycle states are `stable/ok`.
- Using your browser, open the {{site.data.keyword.secrets-manager_short}} private-only UI.

## Troubleshooting on macOS (routing and VPN)
{: #vpn-troubleshoot}

Common symptoms in Mac:

- `curl -v https://$SM_HOST` hangs and times out.
- `curl` fails immediately with errors like `Network is down` or `Cannot allocate memory` when trying to reach `$VPE_IP`.

Steps to diagnose and fix:

1. Ensure only the IBM C2S VPN is active
   - Disconnect any other VPN clients on the Mac (corporate, personal, etc.).
   - Connect the OpenVPN profile for `test-vpn` and wait until it shows as connected.
2. Check the route to the VPE IP

```sh
route -n get "$VPE_IP"
```
{: pre}

Expected for a healthy setup:
- `interface` is a `utun` device (for example `utun5`).
- `gateway` is the tunnel peer address (for example `10.241.x.y` or `172.30.x.y`).
- If `interface` is something like `en0`/`en4` instead of `utunX`, the traffic is bypassing the VPN.

If the route is wrong, override it to use the VPN tunnel with the IBM VPN connected and other VPNs disabled:

```sh
# Remove any existing host route for the VPE IP
sudo route delete "$VPE_IP" 2>/dev/null || true

# Add a host route via the VPN tunnel interface (replace utun5 with your actual utun)
sudo route add -host "$VPE_IP" -interface utun5

# Verify
route -n get "$VPE_IP"
```
{: pre}

If you see `interface: utun5` (or similar), retry:

```sh
curl -v --max-time 10 "https://$SM_HOST"
```
{: pre}

If `curl` still times out:

- Double-check that the VPN server and VPE are in `stable` state:

   ```sh
   ibmcloud is vpn-server "$VPN_ID" --output json | jq -r '.lifecycle_state'
   ibmcloud is endpoint-gateway "$VPE_NAME" --output json | jq -r '.lifecycle_state'
   ```
   {: pre}

- Confirm the VPE Security Group still has the inbound TCP 443 rule from `$VPN_PRIV_IP/32`:

   ```sh
   ibmcloud is security-group-rules "$VPE_SG_ID"
   ```
   {: pre}

## Next steps
{: #vpn-next-steps}

To enhance this configuration for production use, consider the following improvements:

- Replace `/etc/hosts` with {{site.data.keyword.cloud_notm}} DNS Services private zones for proper DNS resolution
- Implement high availability by adding a second subnet and zone
- Configure Certificate Revocation List (CRL) handling for client certificates
- Tighten Security Group rules to follow the principle of least privilege
- Enable multi-factor authentication (MFA) for additional security
- Establish client certificate revocation policies

## Security considerations
{: #vpn-security}

When implementing this VPN solution, keep the following security best practices in mind:

- **Certificate management**: Regularly rotate VPN certificates and implement automated renewal processes
- **Access control**: Limit VPN access to only the IP addresses and users that require it
- **Monitoring**: Enable logging and monitoring for VPN connections and access attempts
- **Network segmentation**: Use Security Groups to enforce strict network segmentation
- **Encryption**: Ensure all traffic uses strong encryption protocols (TLS 1.2 or higher)

## Related links
{: #vpn-related-links}

- [About Client-to-Site VPN servers](/docs/vpc?topic=vpc-vpn-client-to-site-overview)
- [Creating a VPN server](/docs/vpc?topic=vpc-vpn-create-server)
- [Virtual Private Endpoints for VPC](/docs/vpc?topic=vpc-about-vpe)
- [Secrets Manager Private Certificates](/docs/secrets-manager?topic=secrets-manager-private-certificates)
- [IBM Cloud DNS Services](/docs/dns-svcs)
