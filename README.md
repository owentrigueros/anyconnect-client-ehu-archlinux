# anyconnect-client-ehu-archlinux
Cisco AnyConnect client (provided by UPV/EHU) build files for Arch Linux.

## Disclaimer
Better use the `openconnect` package, it just works.

## Installing instructions
Prepare and install the package with `makepkg`:

```
makepkg -sic
```
`-s` installs dependencies, `-i` installs the package after building it, `-c` cleans up the files and directories after building the package.

## Usage
Start the VPN service daemon:

```
systemctl start vpnagentd.service
```

Open Cisco Anyconnect Secure Mobility Client and connect to vpn.ehu.eus with yout LDAP credentials.
