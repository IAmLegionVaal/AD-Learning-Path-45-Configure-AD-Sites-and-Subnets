# Activity 45: Configure AD Sites and Subnets

## Goal
Create `HQ-Site` and `Branch-Site`, associate the lab IPv4 prefixes with those sites, place each domain controller in the correct location, and verify client site discovery.

## Prerequisites
- Two healthy domain controllers
- Healthy AD replication
- Documented networks: `10.10.10.0/24` and `10.20.20.0/24`

## Setup steps
1. Open **Active Directory Sites and Services**.
2. Rename the default site to `HQ-Site`.
3. Create a second site named `Branch-Site`.
4. Under **Subnets**, add `10.10.10.0/24` and associate it with `HQ-Site`.
5. Add `10.20.20.0/24` and associate it with `Branch-Site`.
6. Place `DC01` under `HQ-Site` and `DC02` under `Branch-Site`.
7. Confirm each server object contains an NTDS Settings object.
8. Test site detection from a client attached to each subnet.

## Validation
```powershell
Get-ADReplicationSite -Filter *
Get-ADReplicationSubnet -Filter * | Select-Object Name,Site,Location
Get-ADDomainController -Filter * | Select-Object HostName,Site,IPv4Address
nltest.exe /dsgetsite
nltest.exe /dsgetdc:corp.lab
```

## Evidence
Store screenshots of both sites, subnet properties, domain-controller placement, client site output, DC locator output, errors, remediation, and final pass/fail state in `evidence/`.

## Troubleshooting
- A client reports the wrong site: compare its actual address to the configured prefixes.
- A client has no site: add the exact missing subnet and wait for replication.
- A domain controller appears in the wrong location: correct its server-object placement in Sites and Services.

## Security note
Sites influence replication and service location but do not provide authorization isolation.

## Cleanup
Remove only temporary site and subnet objects after restoring the intended placement.
