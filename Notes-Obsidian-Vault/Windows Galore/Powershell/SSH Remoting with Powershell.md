## Table of Contents

- [Cheatsheet: PowerShell Remoting over SSH](#cheatsheet:\powershell\remoting\over\ssh)
  - [Setting Up SSH for PowerShell Remoting](#Setting\Up\SSH\for\PowerShell\Remoting)
  - [Configuring SSH on the Host](#Configuring\SSH\on\the\Host)
  - [Establishing a Remote Session via SSH](#Establishing\a\Remote\Session\via\SSH)
  - [Running Commands Remotely Over SSH](#Running\Commands\Remotely\Over\SSH)
  - [Using Key-Based Authentication](#Using\Key-Based\Authentication)
  - [Troubleshooting SSH Remoting](#Troubleshooting\SSH\Remoting)

Continuing with the topic of PowerShell Remoting over SSH:

---

# Cheatsheet: PowerShell Remoting over SSH

## Setting Up SSH for PowerShell Remoting
- **Install OpenSSH Client and Server** (if not already installed):
  ```powershell
  Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
  Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
  ```

## Configuring SSH on the Host
- **Start SSH services**:
  ```powershell
  Start-Service sshd
  Set-Service -Name sshd -StartupType 'Automatic'
  ```

- **Configure SSH-based remoting** (on the host):
  Edit the SSH daemon configuration file (typically located at `C:\ProgramData\ssh\sshd_config`) to allow PowerShell remoting.

## Establishing a Remote Session via SSH
- **Enter a remote session using SSH**:
  ```powershell
  Enter-PSSession -HostName <RemoteHostName> -UserName <Username> -SSHTransport
  ```
  *Note: You might be prompted for a password or SSH key authentication.*

## Running Commands Remotely Over SSH
- **Invoke a command on a remote machine via SSH**:
  ```powershell
  Invoke-Command -HostName <RemoteHostName> -UserName <Username> -ScriptBlock { <Command> } -SSHTransport
  ```

## Using Key-Based Authentication
- **Set up key-based authentication for SSH** (more secure):
  Generate SSH keys and transfer the public key to the remote host's authorized keys.

## Troubleshooting SSH Remoting
- **Verify SSH connectivity** (independent of PowerShell):
  ```bash
  ssh <Username>@<RemoteHostName>
  ```
  Ensure basic SSH connectivity before troubleshooting PowerShell-specific issues.

---

