# Active-Directory

<h1 = align=center>𝙷𝙾𝙼𝙴 𝙻𝙰𝙱 - 𝙰𝙲𝚃𝙸𝚅𝙴 𝙳𝙸𝚁𝙴𝙲𝚃𝙾𝚁𝚈</h1>
<h2 = align=center>Bulk User Provisioning with PowerShell</h2>

<p align="center">
<img width="1342" height="883" alt="Untitled Diagram drawio (3)" src="https://github.com/user-attachments/assets/978f6667-84a3-403e-b02b-9328f6341827" />
</p>

---

## 🛠️ TECHNOLOGY & TOOLS UTILIZED

- **`VirtualBox:`**  
  Used as the primary virtualization platform to host multiple virtual machines in an isolated local environment for cybersecurity testing and infrastructure simulation.

- **`Windows Server 2019:`**  
  Deployed as the domain controller within the lab. Configured to run Active Directory Domain Services (AD DS), DNS, and Group Policy to emulate enterprise-level identity and access management scenarios.

- **`Windows 10:`**  
  Installed as a domain-joined workstation to simulate a typical enterprise endpoint. Used for user behavior simulation, GPO testing, and endpoint visibility through logging tools.

- **`Active Directory:`**  
  Implemented to manage user accounts, groups, and organizational units (OUs). Used for hands-on experience with authentication flows, access control, privilege escalation testing, and administrative scripting.

- **`PowerShell:`**  
  Used for automating administrative tasks such as creating users, managing AD objects, and configuring system settings within both the server and client machines.
  
- **`Group Policy Management:`**  
  Applied GPOs to enforce password policies, audit policies, restrict user privileges, and configure security baselines across the domain environment.

---

## OBJECTIVE

Designed and deployed a virtualized Windows domain lab using `VirtualBox`, `Windows Server 2019`, and `Windows 10` to gain hands-on experience with enterprise network infrastructure and identity management. The project focused on configuring `Active Directory`, `Group Policy`, and domain-joined `endpoints` to simulate real-world authentication, access control, and administrative workflows. This lab environment served as a foundation for practicing common security tasks, including privilege escalation prevention, user access auditing, and scripting automation with `PowerShell`.

## 👣 STEP-BY-STEP: SETTING UP THE DOMAIN CONTROLLER

### Step 1: `Server 19` Virtual Machine Creation and Provisioning

The virtual machine, named `Domain-Controller`, was configured as a domain controller using the `Windows Server 2019 ISO`. It was provisioned with 2048 MB of RAM and 4 virtual CPUs to support `Active Directory` and core infrastructure services.

<img width="1283" height="685" alt="Image" src="https://github.com/user-attachments/assets/bc0b5d6e-37d6-4e42-bcb7-7ad756fef060" />

---

### Step 2: Configure `Network Adapters`

For networking, `Network Adapter 1` was enabled and attached to `NAT` to allow internet access. `Network Adapter 2` was connected to an `internal network` named `intnet` to facilitate isolated communication between lab machines without external exposure.

<img width="1386" height="597" alt="Image" src="https://github.com/user-attachments/assets/44180757-445a-42e5-a6d1-699e6107daf0" />

<img width="1385" height="595" alt="Image" src="https://github.com/user-attachments/assets/72c5847e-75b4-4197-a930-c974a3273bbf" />

---

### Step 3: `Windows Setup`

Powered on the virtual machine and proceeded through the Windows Server 2019 installation process using the ISO image.

<img width="637" height="477" alt="Image" src="https://github.com/user-attachments/assets/c0d80f2e-1a97-4f8d-a9a8-730f796edee2" />

*Windows Server 2019 Standard Evaluation (Desktop Experience) → Accept the license terms → Username: Administrator → Password: Password1*

---

### Step 4: Check `Network Configuration`

Checked the `network connection details` of the virtual machine and observed it was automatically assigned an IPv4 address of `169.254.120.207`, indicating an `APIPA` (Automatic Private IP Addressing) address.

<img width="357" height="434" alt="Lab 9" src="https://github.com/user-attachments/assets/45c3d59a-f363-406e-b31b-a33ed6a64961" /></br>

Under `Network Connections`,  renamed the NAT adapter to `_Internet` and the internal network adapter to `_X_Internal_Network` for better clarity. The `_X_Internet_Network` adapter was then manually configured with the following static IP settings:  
- **IP address:** `172.16.0.1`  
- **Subnet mask:** `255.255.255.0`  
- **Preferred DNS server:** `127.0.0.1`

<img width="761" height="525" alt="Image" src="https://github.com/user-attachments/assets/75dbe910-e228-475c-8aa3-0c31cc9d7e04" />

---

### Step 5: Set Up `Active Directory`

In `Server Manager`, we began by launching the `Add Roles and Features` Wizard to install `Active Directory Domain Services` (AD DS). During the setup, also included `Group Policy Management` and `Remote Server Administration Tools` (RSAT) to enable centralized domain control and administrative functionality.

<img width="997" height="645" alt="Image" src="https://github.com/user-attachments/assets/73646b34-37f2-431f-8b95-527c29c9d6a5" />

<img width="783" height="556" alt="Image" src="https://github.com/user-attachments/assets/d2b698f4-b1fb-45d3-adbf-7476d755e1fe" />


After completing the role installation, we proceeded with the `post-deployment configuration` by promoting the server to a domain controller. During the configuration, we created a new forest with the root domain name `Mydomain.com`, and enabled both the `Domain Name System` (DNS) server and `Global Catalog` (GC) options. We also set the NetBIOS domain name to `MYDOMAIN`.


<img width="1007" height="767" alt="Image" src="https://github.com/user-attachments/assets/3b2a067b-ec45-4b7c-8cc5-88602f6559f7" />


---

### Step 6: Set up `Domain Admins` and `Domain Users`

After logging back into the server, we navigated to `Active Directory Users and Computers` to begin configuring domain objects. We created a new `Organizational Unit` (OU) named `_ADMINS`, and within that OU, added a new user account with the full name `Maffy Nepolian` and the user logon name `Maffyn`.

<img width="437" height="402" alt="Image" src="https://github.com/user-attachments/assets/ee2fbafd-2645-44c3-a512-e04bd75ae4fc" />

Within `Maffy Nepolian’` account properties in `Active Directory Users and Computers`, we navigated to the `Member Of` tab and added the user to the `Domain Admins` group. This granted her administrative privileges across the domain. 

<img width="1003" height="750" alt="Image" src="https://github.com/user-attachments/assets/5a2b1ca9-0b73-4aca-af10-23529aadddbe" />

---

### Step 7: Set up `Remote Access` Services

In `Server Manager`, we returned to `Add Roles and Features` to install the `Remote Access role`. During the setup, we selected the role services `DirectAccess and VPN` (RAS) as well as `Routing` to enable secure remote connectivity and traffic management for the network.

<img width="781" height="555" alt="Lab 49" src="https://github.com/user-attachments/assets/9f3e9a0b-ac0a-4e73-baa5-1f18c2166a33" /></br>

Next, we navigated to `Tools` > `Routing and Remote Access` and launched the Routing and Remote Access Server Setup Wizard. We proceeded to configure `NAT` by selecting our internet-facing network interface, named `Internet`, to provide network address translation for internal clients. Upon successful configuration, the domain controller (local server) displayed a green status arrow, indicating the service was running properly.

<img width="783" height="555" alt="Image" src="https://github.com/user-attachments/assets/f5bb9ea1-0122-40ec-b661-c2ed9579e535" />

---

### Step 8: Set up `DHCP Server`

Back in `Server Manager`, we launched the `Add Roles and Features` Wizard again to install the `DHCP Server` role, enabling the server to assign IP addresses dynamically within the network.

<img width="625" height="446" alt="Image" src="https://github.com/user-attachments/assets/026b905e-a508-4de8-b47a-320a349b6c71" />


<img width="761" height="528" alt="Image" src="https://github.com/user-attachments/assets/a49b7a9a-c504-43de-a06a-0b6510518195" />
We navigated to `Tools` > `DHCP` and launched the `New Scope Wizard` to configure a DHCP scope. We named the scope `172.16.0.100-200` and set the IP address range from `172.16.0.100` to `172.16.0.200` with a subnet mask of `255.255.255.0` (prefix length 24). The router (default gateway) was set to `172.16.0.1`, and the parent domain was specified as `mydomain.com`. After activating, authorizing, and refreshing the DHCP server, its status displayed a green icon indicating it was functioning properly.

<img width="167" height="172" alt="Lab 77" src="https://github.com/user-attachments/assets/cbfe1307-c73c-4d5b-9284-b3705fd0899e" /></br>

<img width="271" height="378" alt="Lab 78" src="https://github.com/user-attachments/assets/f484b260-c072-473e-9c35-a2888a5f4902" /></br>


---

### Step 9: Bulk User Creation with `PowerShell` in `Active Directory`

To efficiently add 1,000+ user accounts to `Active Directory`, we used a `PowerShell` script in combination with a text file containing first and last names. The script created a new organizational unit called `_USERS` and then processed each name to generate a standardized username consisting of the user’s first initial and last name in lowercase. Each user was created with a default password `Password18` and placed within the _USERS OU. This automated method significantly streamlined bulk user provisioning, reducing manual effort and ensuring consistent account configuration across the domain.

**Script:**
```powershell
$PASSWORD_FOR_USERS   = "Password18" 
$USER_FIRST_LAST_LIST = Get-Content .\names.txt
# ------------------------------------------------------ #

$password = ConvertTo-SecureString $PASSWORD_FOR_USERS -AsPlainText -Force
New-ADOrganizationalUnit -Name _USERS -ProtectedFromAccidentalDeletion $false

foreach ($n in $USER_FIRST_LAST_LIST) {
    $first = $n.Split(" ")[0].ToLower()
    $last = $n.Split(" ")[1].ToLower()
    $username = "$($first.Substring(0,1))$($last)".ToLower()
    Write-Host "Creating user: $($username)" -BackgroundColor Black -ForegroundColor Cyan
    
    New-AdUser -AccountPassword $password `
               -GivenName $first `
               -Surname $last `
               -DisplayName $username `
               -Name $username `
               -EmployeeID $username `
               -PasswordNeverExpires $true `
               -Path "ou=_USERS,$(([ADSI]`"").distinguishedName)" `
               -Enabled $true
}
```
**Results:**
Before running the script to create all 1,000 users, I tested its functionality by manually inserting my name, `Briana Willis`, as the first entry in the `names.txt` file. After executing the PowerShell script, a total of 1,001 users were created in Active Directory, confirming that the script worked as intended. The test user account `bwillis` was successfully generated using the naming convention implemented in the script, verifying that both the account creation logic and OU assignment were functioning properly.

---
## 👣 STEP-BY-STEP: SETTING UP THE CLIENT

### Step 1: `Windows 10` Virtual Machine Creation and Provisioning

The virtual machine, named `Client1`, was created using the Windows 10 ISO and provisioned with 4096 MB of RAM and 4 virtual CPUs. Network Adapter 1 was enabled on the same internal network as our domain controller, `test 3`.

---

### Step 2: `Windows Setup`

Powered on the virtual machine and proceeded through the Windows 10 installation process using the ISO image.
*Windows Server 10 Pro → Accept the license terms → Username: bwillis → Password: Password18*

---

### Step 3: Confirming `DHCP` Functionality

After setting up `Client1`, I logged in using the `bwillis` account with the default credentials for the first time. I then opened a command prompt and ran `ipconfig` to verify network settings. The Ethernet adapter showed the following configuration:

These values confirm that the `DHCP` server is functioning correctly and issuing addresses within the defined scope, along with proper DNS domain suffix assignment. To validate `DNS` resolution, I successfully pinged `www.google.com` to confirm external connectivity and pinged `mydomain.com` to verify internal name resolution to the domain controller.

---

### Step 4: Join `Client1` to the Domain

In `System Properties`, I changed the computer name to `Client1` and selected `Domain` under the `Member of` section. I entered `mydomain.com` as the domain name and was prompted to provide credentials. After authentication, the system was successfully added to the domain and prompted for a restart to apply the changes.
---

## 🔑 HOW TO MANUALLY RESET A PASSWORD

To reset a user's password in `Active Directory`, press `Win + R`, type `dsa.msc`, and press Enter to open `Active Directory Users and Computers`. Locate the user account—in this case, `bwillis`—by browsing to the appropriate Organizational Unit. Right-click the user and select `Reset Password`.

<img width="512" height="509" alt="Lab 116" src="https://github.com/user-attachments/assets/6c6f078a-b4d0-4283-a918-3bc202cd1397" /></br>

Assign an easy-to-remember password `Password1`, check `User must change password at next logon`, and also check `Unlock the user’s account` if it was previously locked. This process restores access while enforcing a secure password update upon next login.
---

## HOW TO AUTOMATE PASSWORD RESETS

Password resets can also be automated using `PowerShell`, which is especially useful for bulk or remote management tasks. The following script resets the password for the user `bwillis`, enforces a password change at next logon, unlocks the account if it was locked, and ensures the account is enabled:

```powershell

# Reset password
Set-ADAccountPassword -Identity bwillis -Reset -NewPassword (ConvertTo-SecureString "Password1234567890!@" -AsPlainText -Force)

# Require password change at next logon
Set-ADUser -Identity bwillis -ChangePasswordAtLogon $true

# Unlock and enable the account
Unlock-ADAccount -Identity bwillis
Enable-ADAccount -Identity bwillis
```

---

## GROUP POLICY MANAGEMENT EDITOR

For the final test, I launched the `Group Policy Management` Console by pressing `Win + R`, typing `gpmc.msc`, and pressing Enter. This opened the `Group Policy Management Editor`, allowing me to explore how `Group Policy Objects` (GPOs) are created, linked to `Organizational Units` (OUs), and used to manage user and computer configurations across the domain.

Within the `Group Policy Management Editor`, I navigated to `Computer Configuration` > `Policies` > `Windows Settings` > `Security Settings` > `Account Policies` > `Password Policy` to enforce stricter password requirements and establish an account lockout policy across the domain.
To enable auditing for user activity, I configured `audit policies` to track logon events, logoffs, and account lockouts. These settings allow security teams to monitor authentication activity and detect potential unauthorized access attempts, account misuse, or lockout patterns across the domain.

These audits can be analyzed in `Event Viewer` (`Win + R` > `eventvwr.msc`) under `Windows Logs` > `Security`.

| Task Category   | Event ID | Description                                                |
|:----------------|:---------|:-----------------------------------------------------------|
| Logon           | 4624     | Successful user logon                                      |
| Logoff          | 4634     | User logoff                                                |
| Logon Failure   | 4625     | Failed user logon attempt                                  |
| Account Lockout | 4740     | A user account was locked out due to failed login attempts |
| Special Logon   | 4672     | Logon with special privileges (e.g., admin rights)         |

---

*This project simulates a realistic enterprise network environment to practice and demonstrate core cybersecurity and IT infrastructure skills. It involves setting up a virtualized lab with a Windows Server 2019 domain controller, Active Directory, DHCP, and client machines, allowing hands-on experience with domain management, user provisioning, network configuration, and security policy enforcement.*


