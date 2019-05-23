# Security components

* Rolling updates every 6 months since Windows 10 (`winver`).

## Active Directory

* The Microsoft **Active Directory** service allows the centralised management of Microsoft devices.
* Active Directory provides:
    * centralised authentication & authorisation,
    * device configuration.
* Active Directory is structured according to three **hierarchical levels**:
    * domain: logical group of network objects,
    * tree: collection of domains,
    * forest: collection of trees.
* Active Directory **objects** include:
    * users,
    * computers,
    * groups,
    * organizational units.

### Domain Controllers

* Store all of the domain data in the NTDS.dit file.

### Group Object Policy (GPO)

* GPO centralises the Windows device configuration.
* GPO is stored on the SYSVOL share as XML files.
* Member computers retrieve and apply them on boot and on a regular basis.

## Security components

### Authentication

* Performed by the LSASS process and coordinated by Winlogon.
* Winlogon: creates and manages local sessions.
* LSASS: authenticates users & issues access tokens.

### Authorisation

* Authorisation is verified by the SRM upon the Object Manager request. It compares the access tokens with the Access control list (Security Descriptor) and Integrity level.

### Security Descriptor

* A secure object is protected by a Security Descriptor.
* It indicates who can do what.

### SID & RID

* Relative ID, starts at 1000.
    * 500 = Administrator
    * 501 = Guest
* Constant SIDs:
    * S-1-0-0: Nobody
    * S-1-1-0: Everybody
    * S-1-2-0: LOCAL

### Computer account

* Ends with `$`.
* Represents the computer in a Windows domain.
* Created when the computer joins the domain
* Password managed by the domain controller.

## Logs

* Stored in binary format (.evtx, .evl). Event View needed.
