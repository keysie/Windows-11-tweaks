# Native SSH in Windows

For now, this will just be a loose collection of facts I have found over time regarding Windows 11's native SSH client. Take with a grain of salt, especially because at the moment this seems to change pretty fast.

1. [2025-04-25] : The native client seems to work pretty well. Check the official documentation on [ssh-server](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse?tabs=powershell&pivots=windows-server-2025) and [ssh-client/ssh-agent](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_keymanagement).  
   Bare minimum steps to get stuff up and running:
    1. Clear out any existing versions of OpenSSH on your system in case you ever dabbled with Microsoft's beta version or similar using an admin powershell:  
       ```Remove-WindowsCapability -Online -Name OpenSSH.Client```  
       ```Remove-WindowsCapability -Online -Name OpenSSH.Server```
    2. Reboot
    3. Use the Windows Optional Features GUI to install OpenSSH-Client and OpenSSH-Server (it feels like the server-component is required at the moment, but haven't thoroughly tested this)
    4. Reboot
    5. Run ```ssh -V``` in a powershell to check successful installation. Result should be smth like  
       ```OpenSSH_for_Windows_9.8p1 Win32-OpenSSH-GitHub, LibreSSL 3.9.2```
    6. Start and enable services in an admin powershell:
       ```
       # Set the sshd service to be started automatically.
       Get-Service -Name sshd | Set-Service -StartupType Automatic
      
       # Start the sshd service.
       Start-Service sshd
       
       # By default, the ssh-agent service is disabled. Configure it to start automatically.
       Get-Service ssh-agent | Set-Service -StartupType Automatic
       
       # Start the service.
       Start-Service ssh-agent
       
       # The following command should return a status of Running.
       Get-Service ssh-agent
       ```
2. [2025-04-25] : Importing FIDO2 resident SSH-keys in ed25519_sk_rk format using ```ssh-keygen -K``` works, but *must be run in an administrator powershell*, otherwise you will get a very misleading error ```Unable to load resident keys: invalid format```
