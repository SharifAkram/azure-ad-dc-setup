# Installing Active Directory Domain Services (AD DS) and creating a new forest on Windows Server 2022 on Azure Virtual Machine

## Set variables:

$domainName: The fully qualified domain name (FQDN) for the new forest.

```powershell
$domainName = "example.local"
```

$netbiosName: The NetBIOS name for backward compatibility with older systems.

```powershell
$netbiosName = "EXAMPLE"
```

$safeModePassword: This sets the Directory Services Restore Mode (DSRM) password. 

It uses ConvertTo-SecureString to securely store it.

```powershell
$safeModePassword = ConvertTo-SecureString "P@ssw0rd!"
 -AsPlainText -Force
```

Storing passwords in plain text (even in scripts) can be a security risk in real environments. 

## Install AD DS role:

This installs the Active Directory Domain Services role along with the management tools, like Active Directory Users and Computers.

- Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
- Install new forest.
- Install-ADDSForest: This promotes the server to a domain controller and creates a new AD forest.
- InstallDNS: Installs the DNS server role as part of the domain setup.
- Force: Suppresses confirmation prompts.
- NoRebootOnCompletion: $false: Ensures the server reboots automatically after installation.

```powershell
Install-ADDSForest `
    -DomainName $domainName `
    -DomainNetbiosName $netbiosName `
    -SafeModeAdministratorPassword $safeModePassword `
    -InstallDNS `
    -Force `
    -NoRebootOnCompletion:$false
```

