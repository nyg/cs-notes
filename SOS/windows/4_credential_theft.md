# Credential theft

## Password theft

### SAM

* Registry hive that stores the local account details including passwords.
* LM and NTLM hash.
* Hive encrypted using the SYSKEY.
* Protected against read while the operating system is running.
* The SYSTEM account has access.

### MSCACHE

* Keeps track of domain accounts that have logged in the computer.
* Save the last 10 connected users.
* Allows login when the DC is not reachable.
* Hashed with:
    * DCCH version 1 (Windows XP and below)
    * DCCH version 2 (Windows Vista and above)

### LSA Secret

* Registry key located in the SECURITY hive.
* Stores a wide variety of credentials:
    * services executed with a domain account,
    * Windows auto-login,
    * VPN passwords.
* Passwords are encrypted.

### Credential Manager

* Password manager embedded in Windows.
* Credentials are saved in special folders called vaults that are encrypted using the DPAPI.
* Separated in two sections:
    * Web Credential
    * Windows Credential

### GPP

* Group Policy Preferences is a GPO that allows deploying passwords on remote computers.
* Deprecated by Microsoft. Passwords were encrypted with a key common to all Windows installations (cPassword).
* GPP are stored on the SYSVOL share. Readable by any domain user.

### NTDS.dit

* Contains all the Active Directory data including domain passwords.
* Encrypted with the SYSKEY.
* Protected against reading while the operating system is running but:
    * the SYSTEM account has access,
    * creating a Volume Shadow Copy using vssadmin allows to bypass this protection.
* Having access to the file is like being a domain admin.

### Syskey

* Key used to encrypt various local secret on a Windows computer.
* Password dumpers automatically retrieves it.

### DPAPI

* API to encrypt various data.
* Uses a key derived from the user's password.

### Metasploit modules

* post/windows/gather/hashdump
* post/windows/gather/cachedump
* post/windows/gather/lsa_secrets
* post/windows/gather/credentials/gpp
* post/windows/gather/credentials/domain_hashdum

### Single-Sign-On

* Key element of the Microsoft domain to facilitate user life: once logged-in users do not have to re-enter passwords or credentials.
* LSASS keeps authentication material in memory.

### Security Support Package (SSP)

* LSASS relies on Authentication Packages (DLLs) to properly store and verify secret material.
* Common Authentication Packages: MSV, Kerberos, TsPkg, WDigest, etc.

### Mimikatz

* Accesses the LSASS memory and dumps live secrets.
* SeDebugPrivilege is required.
* Other secrets include Kerberos tickets and PIN.

```
load kiwi
creds_all
kiwi_cmd [cmd]
```

## Password cracking

* Bruteforce, dictionary attacks.

## Pass-the-hash

* LM/NTLM hashes do not need to be cracked: network authentication relies on the hash, not on the clear text…
* Metasploit offers Pass-the-hash in multiple modules like `exploit/windows/smb/psexec`:
    * `set SMBPass [lmhash]:[ntlmhash]`

## psexec

* Drop a binary in ADMIN$ through SMB and create a remote service that executes it.
* Can be used to deploy meterpreter.

## Kerberos 101

* Network authentication protocol.
* Integrated into Windows to replace NTLM.
* Relies on the fact that clients and services share a secret with the authentication server.

## Pass-the-ticket

* Just like hashes, Kerberos tickets are cached in memory. They are valid for 10 hours.
* They can be extracted and imported in other sessions or systems to authenticate elsewhere.

## Kerberoast

```
use auxiliary/gather/get_user_spns
set RHOSTS [target_ip]
set user [domain_user]
set pass [domain_password]
set domain [full_domain_name] > Run
john --format=krb5tgs [hash_file]
```

## Silver Ticket

* Allows impersonating any user on the remote service.
