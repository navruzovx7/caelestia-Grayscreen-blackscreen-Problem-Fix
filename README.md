 # Caelestia Gray Screen / Black Screen Fix
<p align="center">
  <img alt="Arch Linux" src="https://img.shields.io/badge/Arch_Linux-1793D1?style=for-the-badge&logo=arch-linux&logoColor=white" />
  <img alt="Hyprland" src="https://img.shields.io/badge/Hyprland-1A1B2F?style=for-the-badge&logo=arch-linux&logoColor=white" />
  <img alt="Caelestia" src="https://img.shields.io/badge/Caelestia-shell-8B5CF6?style=for-the-badge" />
  <img alt="Fix" src="https://img.shields.io/badge/Status-Working-success?style=for-the-badge" />
</p>

A practical fix for the issue where Hyprland starts, but the Caelestia shell never appears and the screen remains stuck on a gray background.

## Overview

This usually happens when the system has a conflicting package installed instead of the dependency that Caelestia expects.

Typical symptoms:

- Hyprland starts normally
- mouse cursor is visible
- only a gray or black screen remains
- Caelestia bar, wallpaper, widgets, and launcher do not load
- a message appears at the bottom telling you to activate Hyprland in Settings

## Root cause

The issue is commonly caused by a package mismatch:

- `caelestia-shell` expects `quickshell-git`
- a conflicting package such as `noctalia-qs` may be installed instead
- the package manager may try to replace or conflict with the required dependency, leaving the environment unusable

This can happen when an AUR repository or third-party Arch configuration replaces `quickshell-git` with something incompatible.

## What fixed it

The working fix is to remove the conflicting package and reinstall the correct dependency:

- remove `noctalia-qs`
- install `quickshell-git`
- then restart the system or log back into Hyprland

## Requirements

This fix is intended for Arch Linux or Arch-based distributions using:

- `pacman`
- `git`
- `makepkg`
- an AUR helper such as `yay` or `paru`

## Install an AUR helper if you do not already have one

If you do not have `yay` installed, use the following commands:

```bash
sudo pacman -S yay
```


If you prefer `paru`, install it instead:

```bash
sudo pacman -S paru
```



You only need one of the two. If `yay` is already installed, use it. If not, `paru` works as a good alternative.

## Recommended fix

### Option 1: Remove the conflicting package and reinstall the shell

If you are using `yay`:

```bash
yay -Rdd noctalia-qs
yay -S caelestia-shell
```

If you are using `paru`:

```bash
paru -Rdd noctalia-qs
paru -S caelestia-shell
```

If you want to update the system normally after that:

```bash
yay -Syu
# or
paru -Syu
```

Important: `-Rdd` is a forced removal and should only be used for this specific conflict while fixing the dependency issue.

### Option 2: Manually install the correct dependency

This is the most direct method if you want to fix the underlying dependency problem:

```bash
git clone https://aur.archlinux.org/quickshell-git.git
cd quickshell-git
makepkg -si
```

If the installer asks whether to replace `noctalia-qs`, confirm with `yes`.

Then reboot your machine.

## Alternative quick fix

```bash
sudo pacman -Rdd noctalia-qs
paru -Sy --rebuild aur/quickshell-git

```

If you use `yay` instead, replace `paru` with `yay` in the same workflow.

## Verification

After the fix, your system should behave normally:

- Caelestia wallpaper appears
- launcher and widgets load
- bar renders correctly
- no gray or black loading screen remains

## Troubleshooting

If the problem persists after reinstalling the dependency:

1. Make sure `quickshell-git` is installed and not replaced by a conflicting package.
2. Check whether `noctalia-qs` is still installed and remove it if needed.
3. Reboot and log back into Hyprland.
4. Re-run the system update with your AUR helper.

## Final note

This fix resolved the issue for me by restoring the expected `quickshell-git` dependency and removing the broken package conflict. If you are experiencing the same symptoms, this is the first thing I recommend checking.

## Quick reference copy paste this

```bash
# Install yay
sudo pacman -S yay paru

# Fix the dependency conflict
yay -Rdd noctalia-qs
yay -S caelestia-shell
```
# Or, if needed install the dependency manually
```bash
sudo pacman -S --needed git base-devel
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si

git clone https://aur.archlinux.org/quickshell-git.git
cd quickshell-git
makepkg -si
```

# IF caelestia didn't give error try this after traying all other options 
```bash
sudo pacman -Rns cachyos-hypr-noctalia noctalia
killall noctalia
caelestia shell -d
```

This repository is intended as a clean, practical troubleshooting guide for the Caelestia gray-screen issue on Arch-based systems.

