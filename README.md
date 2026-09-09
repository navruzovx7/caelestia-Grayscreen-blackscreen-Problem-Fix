 # Caelestia Gray Screen / Black Screen Fix


<p align="center">
  <img alt="Arch Linux" src="https://img.shields.io/badge/Arch_Linux-1793D1?style=for-the-badge&logo=arch-linux&logoColor=white" />
  <img alt="Hyprland" src="https://img.shields.io/badge/Hyprland-1A1B2F?style=for-the-badge&logo=arch-linux&logoColor=white" />
  <img alt="Caelestia" src="https://img.shields.io/badge/Caelestia-shell-8B5CF6?style=for-the-badge" />
  <img alt="Fix" src="https://img.shields.io/badge/Status-Working-success?style=for-the-badge" />
</p>

A practical, tested recovery guide for the Caelestia issue where Hyprland starts normally, the cursor appears, but the desktop shell never renders and the screen remains stuck on a gray or black background.

## Summary

This problem is usually caused by a package conflict rather than a broken Hyprland config. In the common case, the system is running a replacement package instead of the expected dependency:

- `caelestia-shell` expects `quickshell-git`
- a conflicting package such as `noctalia-qs` is installed instead
- the dependency mismatch prevents Caelestia from starting properly

The fix is to remove the conflicting package and reinstall the correct dependency, then reboot and log back in.

## Symptoms

You are likely affected if you see all or most of the following:

- Hyprland starts without the Caelestia shell
- the mouse cursor is visible
- the screen remains gray or black
- the wallpaper, bar, widgets, and launcher never load
- a message appears at the bottom telling you to activate Hyprland in Settings
- the system looks visually loaded but functionally stuck in a partial desktop state

## Why this happens

The Caelestia shell depends on the correct QuickShell runtime. If another package replaces or shadows that dependency, the shell can fail to initialize even though Hyprland itself is working.

This is especially common on Arch-based systems when an AUR package or custom repo installs an incompatible package instead of `quickshell-git`.

## Before you begin

> Warning: This fix is safe when the issue matches the dependency conflict described here. Do not remove unrelated packages blindly.

Make sure you have:

- Arch Linux or an Arch-based distro
- `pacman`
- `git`
- `makepkg`
- an AUR helper such as `yay` or `paru`

## Quick confirmation

Check whether the conflicting package is installed:

```bash
pacman -Q | grep -E 'noctalia|quickshell'
```

If you see `noctalia-qs` installed or if `quickshell-git` is missing, this guide is the right fix path.

## Fastest fix

### Option 1: Remove the conflicting package and reinstall the shell

If you use `yay`:

```bash
yay -Rdd noctalia-qs
yay -S caelestia-shell
```

If you use `paru`:

```bash
paru -Rdd noctalia-qs
paru -S caelestia-shell
```

Then reboot or log out and back into Hyprland.

### Option 2: Install the correct dependency directly

This is the cleanest fix if the root cause is purely the dependency mismatch:

```bash
git clone https://aur.archlinux.org/quickshell-git.git
cd quickshell-git
makepkg -si
```

If the installer asks whether to replace `noctalia-qs`, confirm with `yes`.

Then reboot.

## Recommended recovery workflow

If you want the most reliable sequence, use this order:

```bash
# 1. Remove the conflicting package
yay -Rdd noctalia-qs
# or: paru -Rdd noctalia-qs

# 2. Install the correct dependency
yay -S quickshell-git
# or: paru -S quickshell-git

# 3. Reinstall the shell
yay -S caelestia-shell
# or: paru -S caelestia-shell

# 4. Reboot and then log back into Hyprland
```

> `-Rdd` is a forced removal. It is appropriate here because this is a targeted dependency conflict fix, not a general package cleanup.

## Alternative quick fix

```bash
sudo pacman -Rdd noctalia-qs
paru -Sy --rebuild aur/quickshell-git
yay -Syu
```

If you use `yay` instead of `paru`, replace the relevant command with `yay` in the same order.

## Install an AUR helper if needed

If you do not already have an AUR helper:

```bash
sudo pacman -S --needed git base-devel yay paru
```

Manual install example for `yay`:

```bash
sudo pacman -S --needed git base-devel
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si
```

Manual install example for `paru`:

```bash
sudo pacman -S --needed git base-devel
git clone https://aur.archlinux.org/paru.git
cd paru
makepkg -si
```

## Visual overview


## Verification

After the fix, the desktop should behave normally.

Expected result:

- Caelestia wallpaper appears
- the launcher loads
- widgets and panels initialize
- the bar renders correctly
- no gray/black loading screen remains

If the issue is fixed, your Hyprland session should visually complete and the shell should appear as expected.

## Troubleshooting if it still fails

If the gray screen remains after reinstalling the dependency:

1. Confirm that `quickshell-git` is installed:
   ```bash
   pacman -Q quickshell-git
   ```
2. Verify that `noctalia-qs` is no longer installed:
   ```bash
   pacman -Q noctalia-qs
   ```
3. Reboot and log back into Hyprland.
4. Run a full AUR update:
   ```bash
   yay -Syu
   # or
   paru -Syu
   ```
5. If the shell still does not load, remove any stale custom Hyprland config that may be overriding the Caelestia setup and retry.

## Common mistakes to avoid

- Installing a random `quickshell` variant without checking whether `quickshell-git` is the required dependency
- Keeping both `noctalia-qs` and `quickshell-git` installed at the same time
- Ignoring package manager prompts that ask to replace the conflicting package
- Skipping the reboot after the dependency fix

## FAQ

### Why does the shell fail even though Hyprland starts?
Because Hyprland can initialize while the Caelestia shell runtime is still broken. The desktop process is running, but the shell layer never loads.

### Is this a Hyprland config problem?
Often not. In the common case, it is a dependency conflict, not a bad config file.

### Can I fix it without an AUR helper?
Yes, by installing `quickshell-git` from the AUR manually with `makepkg -si`.

### Do I need to reinstall the entire system?
No. This is a targeted dependency fix, not a reinstall.

## Quick reference

```bash
# Install AUR helper if missing
sudo pacman -S --needed git base-devel yay paru

# Remove conflict
yay -Rdd noctalia-qs

# Install the correct runtime
yay -S quickshell-git

# Reinstall the shell
yay -S caelestia-shell

# Reboot
reboot
```

## Final note

This fix resolved the issue by restoring the required `quickshell-git` dependency and removing the conflicting package that prevented Caelestia from starting. If your system matches the symptoms above, this is the first and most likely recovery path to try.

This repository exists as a clean, practical troubleshooting guide for the Caelestia gray-screen issue on Arch-based systems.

