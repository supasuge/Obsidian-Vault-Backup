
1. Switch to root
2. `pacman -Sy sbctl`
3. Verify Secure Boot is in setup mode.
- `sbctl status`
4. `sbctl create-key`
5. `sbctl enroll-keys -m`
6. `sbctl sign -s -o /usr/lib/systemd/boot/efi/systemd-bootx64.efi.signed /usr/lib/systemd/boot/efi/systemd-bootx64.efi`
7. `cat /etc/mkinitcpio.d/linux.preset`
8. `sbctl sign -s /efi/EFI/Linux/arch-linux.efi`
9. `bootctl install`
10. `sbctl verify`
11. `reboot`
