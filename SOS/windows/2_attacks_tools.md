# Attacks and Tools

## Anatomy of an attack

* Attack pattern:
    * initial foothold
    * recon
    * credential theft
    * privilege escalation
    * persistence

## Initial foothold

* Infect an employee’s workstation
* Compromise an internet-facing server
* Steal an employee password and use a VPN
* Physical break-in in the company building
* Rogue employees
* Bribery of an unscrupulous employee
* Buying an existing access (DarkWeb)
* …

## Attacker tools

* Metasploit (free)
* Empire (free)
* Fuzz Bunch (leaked from NSA)

## Metasploit 101

### Modules

* auxiliary: reconnaissance, bruteforce
* exploit: ready to use exploit
* payloads: payloads to execute
* post: post-exploitation

### Basic commands

* `msfconsole`: launch of the interactive console
* `search [txt]`: search a particular module
* `use [mod]`: use a particular module
* `info [mod]`: display information about a module
* `help`: display available commands
* `sessions`: display established sessions

### Module context

* `options`: display current module options
* `set [att] [val]`: define attribute value
* `run`: execute module
* common attributes:
    * `RHOST`/`RPORT`: target machine
    * `LHOST`/`LPORT`: attacker machine
    * `PAYLOAD`: payload to execute
    * `THREADS`: number of simultaneous threads

## Windows Reconnaissance

* What are the active machines?
* What services are available?
* Which version of the services?

### db_nmap

* Allows to use nmap directly from Metasploit and imports results in the database
    * `db_nmap -p 22 192.168.1.0/24 --open`
* Results are available using `services`:
    * `services -h`: display the command help
    * `services -p`: filter results for a given TCP port
    * `services -S`: filter results for a given string
    * `services -R`: use the query results as RHOSTS
