# gentoo-install

> This is a Gentoo installation script and a proof of concept for complex tasks for Claude Code. **THIS IS FULLY VIBECODED.** So don't fucking hate me, hate Claude.

> **Written entirely by [Claude Code](https://claude.ai/code) (Anthropic's AI coding agent).** The user directed what to build; Claude Code wrote every line, made every design decision, and researched package names autonomously.
>
> **Use at your own risk.** This has not been tested on real hardware. If it works well enough for you to use — great. If it doesn't — that's also useful information about where Claude Code is at.

---

## What is this?

A single-script interactive Gentoo installer. Boot the **Gentoo minimal ISO**, then clone and run it.

> **This script runs inside the Gentoo ISO live environment — not on an existing OS.**
> Boot from the ISO, get a shell, then:

```bash
git clone https://github.com/raijularp/gentoo-install
cd gentoo-install
./gentoo-install
```

Answer the prompts and walk away. The script handles everything from partitioning to bootloader.

## What it does

1. Detects UEFI or BIOS automatically
2. Partitions and formats your chosen disk
3. Downloads and SHA256-verifies the latest stage3 tarball for your chosen init system
4. Configures `make.conf` (native march, cpu-count MAKEOPTS, USE flags based on your choices)
5. Chroots in and runs: Portage sync → profile → locale/timezone → kernel → fstab → networking → GRUB
6. Optionally installs a desktop environment, window manager, or Wayland compositor
7. Offers to reboot when done

## Options

| Option | Choices |
|---|---|
| Disk | any block device (`/dev/sda`, `/dev/nvme0n1`, etc.) |
| Filesystem | `ext4` (default), `btrfs`, `xfs` |
| Init system | `openrc` (default), `systemd`, `runit`, `s6`, `dinit`* |
| Kernel | `genkernel` (default), `manual` |
| Timezone | any tz string (`America/New_York`, `Europe/Berlin`, etc.) |
| Hostname | any string, default `gentoo` |

\* dinit is experimental — the script will warn you

### Desktop / WM / Compositor

| Type | Options |
|---|---|
| Desktop environments | `none` (default), `kde`, `gnome`, `xfce` |
| Xorg window managers | `i3`, `openbox`, `bspwm`, `dwm`* |
| Wayland compositors | `sway`, `hyprland`, `niri`, `river`, `wayfire`, `labwc`, `mangowm`, `weston` |

\* dwm is not recommended unless you plan to patch and recompile it yourself — the script will warn you

## Requirements

The live environment must have: `bash`, `curl`, `parted`, `mkfs.ext4`, `tar`, `chroot`.

All present on the Gentoo minimal ISO and SystemRescue by default.

## Notes

- `mangowm` requires the GURU overlay — the script handles this automatically
- Desktop installs add significant compile time (KDE can take 2–4 hours on slower machines)
- Manual kernel mode leaves sources at `/usr/src/linux` — compile and run `grub-mkconfig` yourself post-chroot
- UEFI vs BIOS is auto-detected; no config needed

---

*Written by Claude Code. Directed by a human who typed questions at it.*
