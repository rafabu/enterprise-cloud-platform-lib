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

To disable files in this library completely, it is suggested to follow the name pattern `*.vpnSite.disabled.json`.

Example artefacts shipped in this folder use the pattern `*.vpnSite.example.json`. These files are not picked up by the inclusion pattern `*.vpnSite.json`, because that pattern only matches files that end exactly with `.vpnSite.json` and therefore ignores any files with additional segments such as `.example.json`.
## JSON Format

The JSON format is based on the REST schema of [Azure REST API Vpn Sites](https://learn.microsoft.com/en-us/rest/api/virtualwan/vpn-sites), where the vpnSite resource body is represented as a flattened object containing both top-level fields (for example `name`, `location`, `properties`) and all nested `properties.*` fields. The following additional mandatory properties have to be added on this root object:

- `artefactName` as the unique internal identifier used by the ECP deployment to reference this build definition artefact
- `name` becoming `displayName`

## Supported Properties

Artefact deployment uses the `azurerm_vpn_site` or `azapi_resource` under the hood. Only the following properties are supported:

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
