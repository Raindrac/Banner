This is a custom GRUB theme using Argon as the base, and icons taken from Hyperfluent.

The backgrounds of Argon were replaced by a single background for both Bluefin and Bazzite, created by myself. The raw .xcf files are included. Additionally, I added a new icon for both Bluefin and Bazzite.

This theme was made for my own personal use, and for that reason the resolution values in them `theme.txt` of each respective theme are intended for my 1440p monitor. You will likely want to tune it yourself. The background might also look strange on other resolutions, as I haven't tested it. If it does, you can resize it in an image editor.

# How I Did It
## Installing It
Installing a GRUB theme on a Universal Blue system is easy. Just run `sudo nano /boot/grub2/user.cfg` and then add the following:
```
set theme="/boot/grub2/themes/<theme>/theme.txt"
```
Replace <theme> with the folder name of the theme you're installing. For my themes here, it'd be replaced with `banner-bazzite` or `banner-bluefin`.

You can add other variables to this script as well. For my own configuration, I added:
```
set timeout=5
set gfxmode=2560x1440,1920x1080,1024x768,800x600
```
This sets the countdown duration and resolutions used, respectively. Put whatever values you'd like there if you want to change these settings at all.

## Getting the Bluefin & Bazzite icons to show
To get the icons for Bluefin and Bazzite to show up, I had to append `grub_class bluefin` and `grub_class bazzite` to `/boot/loader.1/entries/ostree-1.conf` and `/boot/loader.1/entries/ostree-2.conf` for Bluefin and Bazzite, respectively. You will need to do this too if you want their icons to show up.

### TomaszDrozdz's Script
https://codeberg.org/TomaszDrozdz/Fedora-Silverblue-rpm-ostree-generate-grub-theme-icon-class/src/branch/release/README.md

TomaszDrozdz uploaded a script to Codeberg that's intended to automate the process of appending classes to the end of ostree's GRUB entries. However, it was intended for Fedora Silverblue, and so it appends `grub_class fedora`; this means that if you run the script as-is, you'll see plain Fedora icons instead of the Bluefin or Bazzite icons.

If you'd like to use that script, replace all instances of the word "fedora" in the script with "bluefin" or "bazzite". I haven't ran their script myself however, so I can't guarantee it would work.

I chose to append `grub_class bluefin` and `grub_class bazzite` on my own instead of using a script, as it's not that hard to do. You just have to run `sudo nano` and then paste it at the end of each file.

## The Dual Boot
f you're dual booting like me, "Switch to Bazzite" and "Switch to Bluefin" buttons have to be created manually. This is because the specific implementation depends on the UUID of your partitions, whether you have them installed to different partitions on the same drive, or whetheryou have them installed to different drives entirely. Look up documentation for GRUB2's `menuentry` command to understand how this is done. I won't provide help with this, sorry.

### Example menuentry Command
To provide some guidance with adding those entries for people like me who installed Bluefin and Bazzite onto the same drive and set up a dual boot, I'll make an example out of the command that I created to switch to Bazzite. **Replace the UUID after `set=root` with the UUID of your own Bazzite boot partition or this won't work.**

```
menuentry "Switch to Bazzite" --class bazzite --class linux --class os --id go-bazzite {
	insmod efi_gop
	insmod efi_uga
	insmod chain
        search --no-floppy --fs-uuid --set=root 1385-570B
        chainloader /EFI/fedora/grubx64.efi
}
```
