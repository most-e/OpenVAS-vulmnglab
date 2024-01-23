# Vulnerability Management with OpenVAS
![OpenVAS logo](https://i.imgur.com/Roo9TD4.png)

# Project Overview

In this project, I established a secure Azure network and deployed two virtual machines configured to run OpenVAS Vulnerability Management Scanner and Windows 10. To create a deliberately vulnerable environment, the Windows 10 virtual machine was intentionally exposed by disabling security controls and installing outdated software.

In order to emphasize the importance of proper vulnerability scan configurations, I conducted both unauthenticated and credentialed scans. Two scans were conducted on the Windows 10 machine. The first one was unauthenticated, and once it finished, the second, which was credentialed, was initiated. 

Remediations were implemented based on the results of the credentialed scan to address major vulnerabilities, followed by a final credentialed scan to validate the effectiveness of the applied fixes.

# Technologies Used

- Azure Virtual Network
- OpenVAS Vulnerability Management Scanner
- Windows 10 Pro virtual machine
- Outdated software known to have vulnerabilities
  - Mozilla Firefox v97.0b5
  - VideoLAN VLC Media Player v1.1.7
  - Adobe Reader v10.0.0

#  Deploy Resources and Configure Virtual Machines for an Unauthenticated Scan

The initial step involved creating the OpenVAS Vulnerability Management Scanner, specifically using OpenVas by HOSSTED with a default developer configuration. Concurrently, a Windows 10 Pro virtual machine was set up. This VM's firewall was disabled and outdated versions of Firefox, VLC Media Player, and Adobe Reader were installed. 

The goal was for the Windows 10 machine to be vulnerable. Once the firewall was disabled and the vulnerable software was installed, the machine was restarted.

![Vulmnglab firewall&oldsoftware](https://i.imgur.com/voEAqCj.png)

To configure OpenVAS for an unauthenticated scan:
1. A new host was created by using the Windows 10 virtual machine’s private IP Address.

![Vulmnglab newhost](https://i.imgur.com/LLqzpBJ.png)

2. A new target was created using the host from the previous step. All other configurations were left as default, and no credentials were provided to OpenVAS.

![Vulmnglab newtarget](https://i.imgur.com/2VVHt59.png)

3. A new task was created with the target from the previous step. Again, all other configurations were left as default.

![Vulmnglab newtask](https://i.imgur.com/vyu6F1O.png)

# Unauthenticated Scan Results

Given the limitations of unauthenticated scans, the vulnerabilities identified did not accurately represent those in the outdated software. The unauthenticated scan results did not fully capture the vulnerabilities in the deliberately exposed virtual machine.

![Vulmnglab uncredpt1](https://i.imgur.com/NiXAY5d.png)
![Vulmnglab uncredpt2](https://i.imgur.com/MuKidYm.png)

# Credentialed Scan Configurations for Windows 10 Machine

Several adjustments were made to prepare the Windows 10 machine for a credentialed scan. The first being verifying and modifying the Windows Firewall profiles which had already been completed in the initial configuration.

The following steps were then completed:
1. Disabling User Account Control.

![Vulmnglab uac](https://i.imgur.com/5mF6rUX.png)

2. Enabling Remote Registry.

![Vulmnglab remoteregistry](https://i.imgur.com/ViWm8Cd.png)

3. Navigate to the Windows Registry and create a new DWORD named “LocalAccountTokenFilterPolicy” and set the value to “1”.

![Vulmnglab regedit](https://i.imgur.com/WdAuWs4.png)

4. Finally, restart the VM.

# OpenVAS Configurations for a Credentialed Scan

While the Windows 10 machine restarted, OpenVAS was configured for a credentialed scan using the following steps:
1. Create a new credential using the VM's username and password.

![Vulmnglab newcredential](https://i.imgur.com/8Tf9oGT.png)

2. Clone the existing target by clicking the sheep icon found under “Actions.” Edit the cloned target and enable SMB by selecting the credentials created in the previous step.

![Vulmnglab targetclone](https://i.imgur.com/fLdaj92.png)

3. Clone the existing task and edit the clone to use the credentialed target created in the previous step.

![Vulmnglab credscan](https://i.imgur.com/gBNN8Q7.png)

# Credentialed Scan Results

The credentialed scan revealed a substantial difference in vulnerabilities compared to the unauthenticated scan. The severity rating increased, and 96 additional vulnerabilities were identified.

The credentialed scan enabled OpenVAS to comprehensively assess the system, encompassing the identification of vulnerabilities within the outdated software. For detailed insights into these vulnerabilities, OpenVAS offers a section dedicated to Common Vulnerabilities and Exposures (CVE). This inclusion of CVEs facilitates a straightforward breakdown of each vulnerability for better understanding.

![Vulmnglab resultsnoREMpt1](https://i.imgur.com/AES3T0H.png)
![Vulmnglab resultsnoREMpt2](https://i.imgur.com/MaosH0W.png)
![Vulmnglab resultsnoREMpt3](https://i.imgur.com/fnBZzIa.png)

# Remediation, Verification Scan, and Conclusion

To address most of the vulnerabilities identified in the credentialed scan, the outdated software was uninstalled. A subsequent credentialed scan verified the effectiveness of the remediations.

The scan indicated a significant reduction in vulnerabilities. After the outdated software was removed, 71 vulnerabilities had been remediated.

![Vulmnglab resultsafterREMpt1](https://i.imgur.com/vZDkLcQ.png)
![Vulmnglab resultsafterREMpt2](https://i.imgur.com/vi6hy2F.png)
![Vulmnglab resultsafterREMpt3](https://i.imgur.com/QNAWlnC.png)


This project showcased the setup of OpenVAS and the subsequent resolution of vulnerabilities. It also showed the significance of opting for credentialed scans whenever possible, highlighting that an unauthenticated scan may not precisely portray the system's security. While some high-severity vulnerabilities persisted in the scan, addressing them fell outside the scope of this project.

# Conclusion

The primary objective of this project was to provide me with practical exposure to vulnerability management, encompassing configurations and remediations. In the future I plan on doing more projects that will give me more exposure to such tools. Thanks for stopping by!
