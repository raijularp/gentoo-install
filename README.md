# gentoo-install

> **THIS IS FULLY VIBECODED.** So don't fucking hate me, hate Claude.

> **Written entirely by [Claude Code](https://claude.ai/code) (Anthropic's AI coding agent).** The user directed what to build; Claude Code wrote every line, made every design decision, and researched package names autonomously.

> **Use at your own risk.** If it works for you — great. If it doesn't — that's also useful information about where Claude Code is at.

> **NOTE:** This completely throws the Gentoo philosophy in the trash. If you are a new user trying to find an easy way into Gentoo — **manually install it.** Gentoo has extremely difficult challenges for new users. If you're not a seasoned Gentoo user, do not use this script. This is a test for Claude Code, not a production installer (yet lul).

---

## What is this?

A single-script interactive Gentoo installer with a TUI (built with `dialog`). Boot the **Gentoo minimal ISO**, clone it, run it. Answer the prompts and walk away.

> **This script runs inside the Gentoo ISO live environment — not on an existing OS.**

```bash
git clone https://github.com/raijularp/gentoo-install
cd gentoo-install
chmod +x gentoo-install
./gentoo-install
```

---

## What it does

1. Auto-detects UEFI or BIOS
2. Partitions and formats your chosen disk
3. Downloads and SHA256-verifies the latest stage3 tarball for your chosen init system
4. Configures `make.conf` — native march, cpu-count MAKEOPTS, USE flags based on your choices + optional custom flags
5. Chroots in and runs: Portage sync → profile → locale/timezone → kernel → fstab → networking → bootloader
6. Creates user `raiju` with `wheel`, `audio`, `video`, `input` groups and `doas` configured
7. Optionally installs a desktop environment, window manager, or Wayland compositor
8. Installs `vim` and `firefox-bin` on any desktop install
9. Offers to reboot when done

---

## Options

| Option | Choices |
|---|---|
| **Disk** | any block device (`/dev/sda`, `/dev/nvme0n1`, etc.) |
| **libc** | `glibc` (default), `musl` |
| **Filesystem** | `ext4`, `btrfs` (+ snapper), `xfs`, `zfs` |
| **Init system** | `openrc`, `systemd`, `runit`, `s6`, `dinit`* |
| **Bootloader** | `grub`, `systemd-boot`†, `refind`†, `limine`, `uki`† |
| **Kernel** | `genkernel` (automated), `manual` |
| **Audio** | `pipewire` (recommended), `pulseaudio`, `none` |
| **Custom USE flags** | free-text input appended to auto-generated flags |
| **Timezone** | any tz string (`America/New_York`, `Europe/Berlin`, etc.) |
| **Hostname** | any string |

\* `dinit` is experimental — the script will warn you and label it "becareful"  
† UEFI only

### Bootloaders

| Bootloader | BIOS | UEFI | Notes |
|---|---|---|---|
| GRUB | ✓ | ✓ | Default, handles everything including ZFS |
| systemd-boot | — | ✓ | Minimal, fast |
| rEFInd | — | ✓ | Graphical, auto-detects kernels |
| Limine | ✓ | ✓ | Modern, minimal config |
| UKI | — | ✓ | Single `.efi` bundle, registered directly with EFI firmware — no bootloader layer |

### Filesystems

| Filesystem | Notes |
|---|---|
| ext4 | Solid default |
| btrfs | Subvolume layout (`@`, `@home`, `@var_log`) + snapper auto-configured |
| xfs | High performance |
| zfs | zpool + datasets, separate `/boot`, genkernel --zfs |

### Desktop / WM / Compositor

| Type | Options |
|---|---|
| Desktop environments | `none`, `kde`, `gnome`, `xfce` |
| Xorg window managers | `i3`, `openbox`, `bspwm`, `dwm`* |
| Wayland compositors | `sway`, `hyprland`, `niri`, `river`, `wayfire`, `labwc`, `mangowm`†, `weston` |

\* dwm requires manual patching — the script will warn you  
† requires the GURU overlay — handled automatically

### Greeters / Display managers

| Desktop type | Options |
|---|---|
| Xorg WMs (i3, openbox, bspwm, dwm, xfce) | `lightdm` + gtk or slick greeter |
| KDE | `sddm` |
| GNOME | `gdm` |
| Wayland compositors | `ly` (recommended), `tuigreet` (greetd), `gtkgreet` (greetd) |

---

## Raiju Mode

A personal preset for the repo owner. First option in the mode menu. Sets:

- Filesystem: btrfs + snapper
- Compositor: niri **or** mangowm (your choice) + Noctalia Shell (via GURU overlay)
- Audio: PipeWire + WirePlumber (autostarted by compositor)
- Greeter: ly
- Init: your choice
- libc: glibc
- Kernel: genkernel
- Dotfiles: pre-configured niri/mango configs with Noctalia Shell IPC keybinds (Alt modifier, Alt+P for launcher)

> **Don't use Raiju mode.** It's for one person. You are probably not that person.

---

## Requirements

The live environment needs: `bash`, `curl`, `parted`, `mkfs.ext4`, `tar`, `chroot`, `dialog`.

All present on the Gentoo minimal ISO by default.

---

## Notes

- `mangowm` and Noctalia Shell require the GURU overlay — handled automatically
- Desktop installs add significant compile time (KDE can take 2–4 hours on slower machines)
- `genkernel` takes 25–90+ minutes depending on CPU
- UKI forces genkernel on — it needs a real initramfs to bundle
- btrfs installs get snapper with sane retention defaults (5 hourly, 7 daily, 4 weekly, 6 monthly, 2 yearly)
- PipeWire is user-session managed — autostarted by the compositor config, not a system service
- `doas` is used instead of `sudo` — configured with `permit persist :wheel`
- Manual kernel mode leaves sources at `/usr/src/linux` — compile and run your bootloader config step yourself post-chroot
- musl + systemd is blocked (incompatible) — the script will refuse and tell you

---

## License

[WTFPL](LICENSE) — Do What The Fuck You Want To Public License

---

*Written by Claude Code. Directed by a human who typed questions at it.*
