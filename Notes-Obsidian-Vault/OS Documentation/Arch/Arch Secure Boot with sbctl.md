1. Disable Secure boot, Wipe keys, then put Secure Boot into "Setup Mode"
2. Install `sbctl`
```bash
sudo pacman -Syu sbctl
```
3. Switch to the root user
```bash
sbctl status #Check status of Secure Boot/Setup mode
```
- Next create and enroll the keys
```bash
sbctl create-keys # Create keys

sbctl enroll-keys -m #Enroll keys

sbctl -s -o /usr/lib/systemd/boot/efi/systemd-bootx64.efi.signed /usr/lib/systemd-bootx64.efi #Sign the keys

```
4. Check location of boot path EFI 
```bash
cat /etc/mkinit/cpio.d/linux.preset
> default_uki="/efi/EFI/Linux/arch-linux.efi"
```
5. Sign keys 
```bash
sbctl sign -s /efi/EFI/Linux/arch-linux.efi
```
6. Re-install the boot loader
```bash
bootctl install
```
7. Verify all installations
```bash
sbctl verify
```
8. Reboot
```bash
reboot
```


