# Ryans bluefin
Customized base container for Fedora Silverblue

**THIS FORKS TARGET AUDIENCE IS ME**

This is only doable because the contributors to bluefin.
Seriously, this is amazing and amazingly done.

[![release-please](https://github.com/prairir/bluefin/actions/workflows/release-please.yml/badge.svg)](https://github.com/prairir/bluefin/actions/workflows/release-please.yml)

# Usage

1. download and install fedora silverblue

1. install a container

1. [AMD/Intel GPU users only] Open a terminal and rebase the OS to this image:

    Bluefin:
    ```
    sudo rpm-ostree rebase ostree-unverified-registry:ghcr.io/prairir/bluefin:38
    ```

    Bluefin DX:
    ```
    sudo rpm-ostree rebase ostree-unverified-registry:ghcr.io/prairir/bluefin-dx:38
    ```


1. [Nvidia GPU users only] Open a terminal and rebase the OS to this image:

    Bluefin:
    ```
    sudo rpm-ostree rebase ostree-unverified-registry:ghcr.io/prairir/bluefin-nvidia:38
    ```
        
    Bluefin DX:
    ```
    sudo rpm-ostree rebase ostree-unverified-registry:ghcr.io/prairir/bluefin-dx-nvidia:38      
    ```

1. Reboot the system and you're done!

1. Build the distrobox container
    ```
    distrobox assemble create --replace --file /etc/distrobox/distrobox.ini
    ```

1. To install doom emacs in the container
    ```
    $HOME/.config/emacs/bin/doom install
    ```

1. If you want doom emacs, you have to install manually

1. To revert back:
    ```
    sudo rpm-ostree rebase fedora:fedora/38/x86_64/silverblue
    ```

Check the [Silverblue documentation](https://docs.fedoraproject.org/en-US/fedora-silverblue/) for instructions on how to use rpm-ostree. 
We build date tags as well, so if you want to rebase to a particular day's release you can use the version number and date to boot off of that specific image:
  
    sudo rpm-ostree rebase ostree-unverified-registry:ghcr.io/ublue-os/bluefin:37-20230310 

The `latest` tag will automatically point to the latest build. 

# Features

**This image heavily utilizes _cloud-native concepts_.** 

System updates are image-based and automatic. Applications are logically seperated from the system by using Flatpaks, and the CLI experience is contained within OCI containers: 

## For Ryan

- Ubuntu-like GNOME layout
  - Includes the following GNOME Extensions
    - Dash to Dock - for a more Unity-like dock
    - Appindicator - for tray-like icons in the top right corner
    - GSConnect - Integrate your mobile device with your desktop
    - POP! Shell - for tiling
- GNOME Software with [Flathub](https://flathub.org)
    - Use a familiar software center UI to install graphical software
- Built on top of the the [uBlue main image](https://github.com/ublue-os/main) 
  - Extra udev rules for game controllers and [other devices](https://github.com/ublue-os/config) included out of the box
  - All multimedia codecs included
  - System designed for automatic staging of updates
    - If you've never used an image-based Linux before just use your computer normally
    - Don't overthink it, just shut your computer off when you're not using it


## For Also Ryan
    
- Built-in Ubuntu user space 
    - `Ctrl`-`Alt`-`u` - will launch an Ubuntu image inside a terminal via [Distrobox](https://github.com/89luca89/distrobox), your home directory will be transparently mounted
    - A [BlackBox terminal](https://www.omgubuntu.co.uk/2022/07/blackbox-gtk4-terminal-emulator-for-gnome) is used just for this configuration
    - Use this container for your typical CLI needs or to install software that is not available via Flatpak or Fedora
    - Optional [ubuntu-toolbox image](https://github.com/ublue-os/bluefin/pkgs/container/ubuntu-toolbox) with Python, NPM, and other convenience development tools.  `just distrobox-bluefin` to get started. To configure `just` follow the [guide](https://ublue.it/guide/just/).
    - Optional [universal image](https://mcr.microsoft.com/en-us/product/devcontainers/universal/about) with Python, Node.js, JavaScript, TypeScript, C++, Java, C#, F#, .NET Core, PHP, Go, Ruby, and and Conda. `just distrobox-universal` to get started
    - Refer to the [Distrobox documentation](https://distrobox.privatedns.org/#distrobox) for more information on using and configuring custom images
    - GNOME Terminal
      - `Ctrl`-`Alt`-`t` - will launch a host-level GNOME Terminal if you need to do host-level things in Fedora (you shouldn't need to do much).   
- Cloud Native Tools
    - [kind](https://kind.sigs.k8s.io/) - Run a Kubernetes cluster on your machine. Do a `kind create cluster` on the host to get started!
    - [kubectl](https://kubernetes.io/docs/reference/kubectl/) - Administer Kubernetes Clusters
    - [Podman-Docker](https://github.com/containers/podman) - Automatically aliases the `docker` command to `podman`
- Nix-powered Development Experience (Alpha) 
    - [Introducing Fleek](https://github.com/ublue-os/fleek) - a user-friendly wrapper around Nix and Nix Home Manager
    - Run `/usr/bin/ublue-nix-install` to get started, then `fleek help` to learn more
    - This feature is experimental and not considered ready for production. It is for experienced users only, but is improving quickly
- Quality of Life Improvements
    - systemd shutdown timers adjusted to 15 seconds
    - [Tailscale](https://tailscale.com/) for VPN
    - [Just](https://github.com/casey/just) task runner for post-install automation tasks
    - `zsh` available as an optional shell, use `just zsh` and follow the prompts to configure it

## bluefin-dx - The Bluefin Developer Experience

Dedicated developer image with bundled tools. It endevaours to be the world's most powerful cloud native developer environment. :) It includes everything in the base image plus: 

- VSCode and related tools
- virt-manager and associated tooling
- Cockpit and goodies for local and remote management 
- podman extras (docker compat tools and convenience shortcuts too)
- LXC/LXD
- A collection of well curated monospace fonts 
- hashicorp repo included and enabled
  - Too many to list
  - None of them installed by default, but you can just add them to the Containerfile as you need them
- Kubernetes Tools
  - helm, ko, flux, minio-client -- if it's an incubated project we intend to add it where appropriate

### Roadmap and Future Features

- Fedora 38 will be the initial release and will be considered Beta
- Fedora 39 is the target for an initial GA release

These are currently unimplemented ideas that we plan on adding:

- Provide a `:gts` tag aliased to the Fedora -1 release for an approximation of Ubuntu's release cadence
- Provide a `:lts` tag derived from CentOS Stream for a more enterprise-like cadence
- [Firecracker](https://github.com/firecracker-microvm/firecracker) - help wanted with this!

### Applications

- Mozilla Firefox, Mozilla Thunderbird, Extension Manager, Libreoffice, DejaDup, FontDownloader, Flatseal, and the Celluloid Media Player
- Core GNOME Applications installed from Flathub
  - GNOME Calculator, Calendar, Characters, Connections, Contacts, Evince, Firmware, Logs, Maps, NautilusPreviewer, TextEditor, Weather, baobab, clocks, eog, and font-viewer
- All applications installed per user instead of system wide, similar to openSUSE MicroOS. Thanks for the inspiration Team Green!

### Recommended Extensions

The authors recommend the following extensions if you'd like to round out your experience. Use the included "Extensions Manager" application to search for these extensions, everything you need to get them to run is already included:

<img src="https://user-images.githubusercontent.com/1264109/224862317-569d018f-a7be-4895-82ff-e2c67652a0ab.png" width="400">

(Note: Installing extensions via extensions.gnome.org won't work, the extensions must be installed via this application)

- [Tailscale Status](https://extensions.gnome.org/extension/5112/tailscale-status/) for VPN
- [Pano](https://extensions.gnome.org/extension/5278/pano/) for clipboard management
- [Desktop Cube](https://extensions.gnome.org/extension/4648/desktop-cube/) if you really want to go retro

## Verification

These images are signed with sigstore's [cosign](https://docs.sigstore.dev/cosign/overview/). You can verify the signature by downloading the `cosign.pub` key from this repo and running the following command:

    cosign verify --key cosign.pub ghcr.io/ublue-os/bluefin
    
## Building Locally

1. Clone this repository and cd into the working directory

       git clone https://github.com/prairir/bluefin.git
       cd bluefin

1. Make modifications if desired
    
1. Build the image (Note that this will download and the entire image)

       podman build . -t bluefin
    
1. [Podman push](https://docs.podman.io/en/latest/markdown/podman-push.1.html) to a registry of your choice.
1. Rebase to your image to wherever you pushed it:

       sudo rpm-ostree rebase ostree-unverified-registry:whatever/bluefin:latest
   
