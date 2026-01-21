# VPN Remote Site Artefacts

Artefacts of type vpnSites (Microsoft.Network/vpnSites) are currently consumed by the following ECP units:

- az-alz-connectivity-virtual-wan

This folder contains `*.vpnSite.json` artefacts for Azure VPN Remote Sites and VPN links.

## File Naming Patterns

### Inclusion

In order to be considered by the ECP deployment, the file pattern must be `*.vpnSite.json`.

### Exclusion

To override adding a library file from being added, the module's individual configuration may contain a folder `vpnSites` with a file following the pattern `*.vpnSite.exclude.json`.

### Disabling

To disable files in this library completely, it is suggested to follow the name pattern `*.vpnSite.disabled.json`. This should happen with examples.

## JSON Format

The JSON format is based on the REST schema of [Azure REST API Vpn Sites](https://learn.microsoft.com/en-us/rest/api/virtualwan/vpn-sites), where only the contents of `properties`is used. The following, mandatory properties have to be added:

- `artefactName` as the unique internal identifier used by the ECP deployment to reference this build definition artefact
- `nameElement` becoming `displayName`

## Supported Properties

Artefact deployment does use the `azurerm_vpn_site` or `azapi_resource` under the hood. Only the following properties are supported:

- **name**
- **addressSpace**
  - **addressPrefixes** (list)
- **bgpProperties**
  - **asn**
  - **bgpPeeringAddress**
  - **bgpPeeringAddresses**
  - **peerWeight**
- **deviceProperties**
  - **deviceModel**
  - **deviceVendor**
- **o365Policy** (defaults to `null`)
- **virtualWan**
  - **id**
- **vpnSiteLinks**
  - **name**
  - **properties**
    - **ipAddress**
    - **linkProperties**
      - **linkProviderName**
      - **linkSpeedInMbps**

Properties or objects not mentioned above are ignored.
