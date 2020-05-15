# Fixes and Workarounds for ArchLinux

I can use in emergencies or having bugs.

## Using dnsmasq like in Ubuntu for DNS resolutions

It's useful when using Docker containers + Reverse Proxies. You can just add your local address for wildcard resolution later.

    sudo pacman -S dhclient dnsmasq

Then configure NetworkManager:

    micro /etc/NetworkManager/NetworkManager.conf

To this:

    [main]
    plugins=keyfile
    dhcp=dhclient
    dns=dnsmasq

Rereading `/etc/hosts` and restarting:

    echo 'addn-hosts=/etc/hosts' | sudo tee /etc/NetworkManager/dnsmasq.d/hosts.conf > /dev/null
    sudo systemctl restart NetworkManager

### Adding wildcard subdomains to dnsmasq

Create/edit a NetworkManager configuration for dnsmasq hosts:

    sudo micro /etc/NetworkManager/dnsmasq.d/hosts.conf

Add, changing the domain to your preference:

    address=/domain.tld/127.0.0.3
    address=/sub.domain.tld/127.0.0.4

Restart NetworkManager

    sudo systemctl restart NetworkManager

## Comparing new configuration files after updates

    SUDO_EDITOR=meld sudo -e /etc/file{,.pacnew}

## Bluetooth AD2P not working with Gnome

    yay -S pulseaudio-bluetooth-a2dp-gdm-fix

## Firefox Text fields: Using dark themes with Gnome

Create a new property using `about:config`

Name: `widget.content.gtk-theme-override`
Value: `Adwaita:light`

Adwaita must be present, KDE users can rely on breeze.

## Firefox versus HiDPI and Wayland

### Firefox Developer Edition

    cp /usr/share/applications/firefox-developer-edition.desktop ~/.local/share/applications/
    sed -i 's/Exec=/Exec=env MOZ_ENABLE_WAYLAND=1 /g' ~/.local/share/applications/firefox-developer-edition.desktop

### Regular Firefox

    cp /usr/share/applications/firefox.desktop ~/.local/share/applications/
    sed -i 's/Exec=/Exec=env MOZ_ENABLE_WAYLAND=1 /g' ~/.local/share/applications/firefox.desktop

### Undoing

    rm ~/.local/share/applications/firefox*

## Slow Playback / Hardware Acceleration

If your video playback is slow, change in `about:config` this setting: `layers.acceleration.force-enabled` to `true`
  
It works just fine on my three main computers (AMD A8 with open source radeon/mesa; Intel i5 with iGPU and mesa; Intel i7 with nVidia GTX 1050Ti and proprietary nvidia)
