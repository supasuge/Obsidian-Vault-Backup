## Table of Contents

    - [Pacman Cheatsheet](#Pacman\Cheatsheet)
    - [Yay Cheatsheet](#Yay\Cheatsheet)
    - [General Tips](#General\Tips)
  - [Storage](#Storage)
    - [Data-at-rest encryption](#Data-at-rest\encryption)

### Pacman Cheatsheet
- **Update Package Database:**
  ```bash
  sudo pacman -Sy
  ```
- **Upgrade Installed Packages:**
  ```bash
  sudo pacman -Syu
  ```
- **Search for Packages:**
  ```bash
  pacman -Ss keyword
  ```
- **Install a Package:**
  ```bash
  sudo pacman -S package_name
  ```
- **Remove a Package and Its Dependencies (not required by other packages):**
  ```bash
  sudo pacman -Rs package_name
  ```
- **Clean the Package Cache (keep only the latest 3 versions of installed packages):**
  ```bash
  sudo pacman -Sc
  ```
- **Remove Uninstalled Packages' Cache:**
  ```bash
  sudo pacman -Scc
  ```
- **List Installed Packages:**
  ```bash
  pacman -Q
  ```
- **Find Which Package Owns a File:**
  ```bash
  pacman -Qo /path/to/file
  ```
### Yay Cheatsheet
- **Synchronize and Upgrade All Packages (from official repositories and AUR):**
  ```bash
  yay -Syu
  ```
- **Install a Package (from the official repositories or AUR):**
  ```bash
  yay -S package_name
  ```
- **Remove a Package and Its Unneeded Dependencies:**
  ```bash
  yay -Rns package_name
  ```
- **Search for Packages in the Official Repositories and AUR:**
  ```bash
  yay -Ss keyword
  ```
- **List Installed Packages (including from AUR):**
  ```bash
  yay -Q
  ```
- **Check for Outdated Packages from the AUR:**
  ```bash
  yay -Pu
  ```
- **Clean Unneeded Dependencies (not required by other packages):**
  ```bash
  yay -Yc
  ```
- **Remove Unused Packages (orphans) and Their Dependencies:**
  ```bash
  yay -Yc
  ```
### General Tips
- It's recommended to regularly update your system with `sudo pacman -Syu` or `yay -Syu` to keep it secure and up-to-date.
- Before removing a package, check its dependencies to avoid accidentally removing something important with `pacman -Qi package_name` or `yay -Qi package_name`.
- Use `yay` for a seamless AUR experience, as it wraps around `pacman` functionalities and extends them to AUR packages.


## Storage

### Data-at-rest encryption

[Data-at-rest encryption](https://wiki.archlinux.org/title/Data-at-rest_encryption "Data-at-rest encryption"), preferably full-disk encryption with a [strong passphrase](https://wiki.archlinux.org/title/Security#Passwords), is the only way to guard data against physical recovery. This provides data confidentiality when the computer is turned off or the disks in question are unmounted.

Once the computer is powered on and the drive is mounted, however, its data becomes just as vulnerable as an unencrypted drive. It is therefore best practice to unmount data partitions as soon as they are no longer needed.

You may also [encrypt a drive with the key stored in a TPM](https://wiki.archlinux.org/title/Trusted_Platform_Module#Data-at-rest_encryption_with_LUKS "Trusted Platform Module"), although it has had [vulnerabilites in the past](https://tpm.fail) and the key can be extracted by a [bus sniffing attack](https://pulsesecurity.co.nz/articles/TPM-sniffing).

Certain programs, like [dm-crypt](https://wiki.archlinux.org/title/Dm-crypt "Dm-crypt"), allow the user to encrypt a loop file as a virtual volume. This is a reasonable alternative to full-disk encryption when only certain parts of the system need to be secure.

While the block-device or filesystem-based encryption types compared in the [data-at-rest encryption](https://wiki.archlinux.org/title/Data-at-rest_encryption "Data-at-rest encryption") article are useful at protecting data on physical media, most can not be used to protect data on a remote system that you can not control (such as [cloud storage](https://wiki.archlinux.org/title/Data-at-rest_encryption#Cloud-storage_optimized "Data-at-rest encryption")). In some cases, individual file encryption will be useful.

These are some methods to encrypt files:

- Some [archiving and compressing](https://wiki.archlinux.org/title/Archiving_and_compression "Archiving and compression") tools also provide basic encryption. Some examples are [p7zip](https://archlinux.org/packages/?name=p7zip) (`-p` flag), [zip](https://archlinux.org/packages/?name=zip) (`-e` flag). The encryption should only be relied on particular care, because the tools may use custom algorithms for cross-platform compatibility.[[2]](https://math.ucr.edu/~mike/zipattacks.pdf)
- [GnuPG](https://wiki.archlinux.org/title/GnuPG "GnuPG") can be used to [encrypt files](https://wiki.archlinux.org/title/GnuPG#Encrypt_and_decrypt "GnuPG").
- [age](https://archlinux.org/packages/?name=age) is a simple and easy to use file encryption tool. It also supports multiple recipients and encryption using SSH keys, which is useful for secure file sharing.















































































































