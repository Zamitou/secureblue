This repo is a fork of [secureblue](https://github.com/secureblue/secureblue) with some modifications for private use:

eg.
- removing chromium
- configuring yafti for a personal selection of packages (flatpaks to install after first boot)

## What

The following Hardening still applies:

- Setting numerous hardened sysctl values (Inspired by but not the same as Kicksecure's)
- Disabling coredumps in limits.conf
- Disabling all ports and services for firewalld
- Adds per-network MAC randomization
- Blacklisting numerous unused kernel modules to reduce attack surface
- Require a password for sudo every time it's called
- Disable passwordless sudo for rpm-ostree
- Brute force protection by locking user accounts for 24 hours after 50 failed login attempts, hardened password encryption and password quality suggestions
- Installing chkrootkit
- (Non-userns variants) Disabling unprivileged user namespaces
- (Non-userns variants) Replacing bubblewrap with bubblewrap-suid so flatpak can be used without unprivileged user namespaces
- Enabling only the [flathub-verified](https://flathub.org/apps/collection/verified/1) remote by default
- Sets numerous hardening kernel parameters (Inspired by [Madaidan's Hardening Guide](https://madaidans-insecurities.github.io/guides/linux-hardening.html))
- Installs and enables [hardened_malloc](https://github.com/GrapheneOS/hardened_malloc) globally, including for flatpaks

## Why

Fedora is one of the few distributions that ships with selinux and associated tooling built-in and enabled by default. This makes it advantageous as a starting point for building a hardened system. However, out of the box it's lacking hardening in numerous other areas. This project's goal is to improve on that significantly.


For more info on uBlue, check out the [uBlue homepage](https://universal-blue.org/) and the [main uBlue repo](https://github.com/ublue-os/main/)

## Installation

> **Warning**
> [This is an experimental feature](https://www.fedoraproject.org/wiki/Changes/OstreeNativeContainerStable) and should not be used in production, try it in a VM for a while!

Have a look at [PREINSTALL-README](PREINSTALL-README.md) before proceeding.

### Available Images

#### Without User Namespaces

##### desktop
- silverblue-main-hardened
- silverblue-nvidia-hardened
  
##### laptop
- silverblue-main-laptop-hardened
- silverblue-nvidia-laptop-hardened
  
#### With User Namespaces

##### desktop
- silverblue-main-userns-hardened
- silverblue-nvidia-userns-hardened

##### laptop
- silverblue-main-laptop-userns-hardened
- silverblue-nvidia-laptop-userns-hardened

### Rebasing

To rebase an existing Silverblue/Kinoite installation to the latest build:

- First rebase to the unsigned image, to get the proper signing keys and policies installed:
  ```
  rpm-ostree rebase ostree-unverified-registry:ghcr.io/Zamitou/$IMAGE_NAME:latest
  ```
- Reboot to complete the rebase:
  ```
  systemctl reboot
  ```
- Then rebase to the signed image, like so:
  ```
  rpm-ostree rebase ostree-image-signed:docker://ghcr.io/Zamitou/$IMAGE_NAME:latest
  ```
- Reboot again to complete the installation
  ```
  systemctl reboot
  ```
  
### Post-install

After installation, [yafti](https://github.com/ublue-os/yafti) will open. Make sure to follow the steps listed carefully and read the directions closely.

#### Kargs
To append kernel boot parameters that apply additional hardening (reboot required):

```
just set-kargs-hardening 
```

#### Nvidia
If you are using an nvidia image, run this after installation:

```
rpm-ostree kargs \
    --append=rd.driver.blacklist=nouveau \
    --append=modprobe.blacklist=nouveau \
    --append=nvidia-drm.modeset=1
```

## Contributing

Follow the [contributing documentation](CONTRIBUTING.md#contributing), and make sure to respect the [CoC](CODE_OF_CONDUCT.md).

### Development

For local Development [building locally](CONTRIBUTING.md#building-locally) is the recommended approach.
