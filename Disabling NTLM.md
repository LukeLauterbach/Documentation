# Disabling NTLM - DRAFT

## Description
In modern Windows environments, Kerberos is the primary way hosts authenticate. However, the default Active Directory configuration has NTLM authentication enabled, allowing legacy hosts and applications to continue authentication. NTLM authentication is problematic for Systems Administrators who are looking to secure their Active Directory environment ([Further Reading](https://www.crowdstrike.com/cybersecurity-101/ntlm-windows-new-technology-lan-manager/#:~:text=NTLM%20was%20subject%20to%20several,protect%20it%20from%20cracking%20techniques.)).

#### NTLM Enabled
![image](https://user-images.githubusercontent.com/104774644/227614137-6f5ada6a-e44a-43b2-b5ab-3be3701f403c.png)
#### NTLM Disabled
![image](https://user-images.githubusercontent.com/104774644/227618022-c5da7615-c8df-427f-a4fe-214cd5d0d3f9.png)


Notibly, having NTLM authentication enabled allows attackers to easily perform pass-the-hash attacks. Even in an environment with NTLM disabled, NTLM hashes can be used to obtain Kerberos tickets. But not all tools support Kerberos authentication, limiting a potential attacker's ability to use publicly available tools.

## Disabling NTLM Authentication

1. Open the Group Policy Management Editor
2. Find the Default Domain Controllers Policy (Forest -> Domains -> {DOMAIN NAME} -> Domain Controllers -> Default Domain Controllers Policy
  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![image](https://user-images.githubusercontent.com/104774644/227615353-f5ef2207-f4e2-4825-a38d-e0685ad023a3.png)

4. Right-click the policy and select **Edit...**
5. Navigate to Policies -> Windows Settings -> Security Settings -> Local Policies -> Security Options
6. Change the following settings:
    * **Network security: Restrict NTLM: Incoming NTLM Traffic** - Deny all accounts
    * **Network security: Restrict NTLM: authentication in this domain** - Deny all
    * **Network security: Restrict NTLM: Outgoing NTLM traffic to remote servers** - Deny all

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;![image](https://user-images.githubusercontent.com/104774644/227615292-cbbf80dc-8e21-4280-82f3-8bffe7aba2cc.png)

## Disclaimer
NTLM is a legacy feature that Microsoft has left enabled by default for a reason. Very old operating systems and applications may not function in an environment where NTLM is disabled. Both testing and a staged rollout are recommended. In the same **Security Options** policy group, options are available to audit existing NTLM authentication. This could give System Administrators a picture of the use of NTLM in their environments. 
