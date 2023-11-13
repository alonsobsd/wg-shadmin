# wg-shadmin
wg-shadmin is a set of shell script files + dialog used for basic management of wireguard server configuration. Nowadays it works on FreeBSD and OpenBSD. Also it could work on GNU/Linux with few modifications.

#### Main window

![image](https://user-images.githubusercontent.com/11150989/178398603-295de29a-3538-4a4d-9dca-3a91bb9f1eba.png)

#### Server management

![image](https://user-images.githubusercontent.com/11150989/178399203-82a7a7aa-e026-4bd4-b639-ecac5d75b6d8.png)

#### Groups management

![image](https://user-images.githubusercontent.com/11150989/178399297-9a5d0f45-9d01-4af2-9120-85901797f5e9.png)

#### Clients management

![image](https://user-images.githubusercontent.com/11150989/178399330-3d3b7e0f-7e41-45d8-a705-0caf9b547d36.png)


## Quick start
If you want run wg-shadmin, you need install wireguard-tools[-lite], wireguard-kmod and dialog. Some additional firewall configuration should be necessary but that is not detailed in this **README**

### On FreeBSD
#### Installing dependencies
```sh
# pkg install wireguard-kmod # Only for FreeBSD <= 12.x
# pkg install wireguard-tools-lite
```
dialog tool is part of base system on FreeBSD. Also devel/cdialog can be used with wg-shadmin.

#### Wireguard initial configuration 
We need add somes lines to /etc/rc.conf

```sh
# sysrc gateway_enable="YES"
# sysrc wireguard_enable="YES"
# sysrc wireguard_interfaces="wg0"
# sysrc wireguard_wg0_ips="11.0.0.1/24"
```
Start wireguard device

```sh
# sysctl net.inet.ip.forwarding=1
# service wireguard start
```

### On OpenBSD
#### Installing dependencies
```sh
# pkg_add -r dialog
# pkg_add -r wireguard-tools
```
wireguard support is included into kernel on OpenBSD

#### Wireguard initial configuration 

```sh
# cat /etc/hostname.wg0
inet 11.0.0.1 255.255.255.0 11.0.0.255 description "WireGuard"
!/usr/local/bin/wg setconf wg0 /etc/wireguard/wg0.conf

# echo "net.inet.ip.forwarding=1" >> /etc/sysctl.conf
```
Start wireguard device

```sh
# sysctl net.inet.ip.forwarding=1
# sh /etc/netstart wg0
```

## Running wg-shadmin
wg-shadmin can running from current directory but the best is copy its files and directories to right paths

```sh
# cp wg-shadmin /usr/local/sbin/
# chown root:wheel /usr/local/sbin/wg-shadmin
# chmod 750 /usr/local/sbin/wg-shadmin
# cp -R share/wg-shadmin /usr/local/share/
# chown -R root:wheel /usr/local/share/wg-shadmin
# find /usr/local/share/wg-shadmin -type d -exec chmod 750 "{}" \;
# find /usr/local/share/wg-shadmin -type f -exec chmod 640 "{}" \;
# cp -R etc/wg-shadmin /usr/local/etc/
# find /usr/local/etc/wg-shadmin -type d -exec chmod 750 "{}" \;
# find /usr/local/etc/wg-shadmin -type f -exec chmod 640 "{}" \;
```
On first time, wg-shadmin will try copy wg-shadmin.conf.sample to /usr/local/etc/wg-shadmin/ or /etc/wg-shadmin

```sh
root@freebsd:~ # wg-shadmin
Hello!
ERROR: unable to locate /usr/local/etc/wg-shadmin/wg-shadmin.conf file
Do you want create it from sample file? y/n:
```
You can edit **/usr/local/etc/wg-shadmin/wg-shadmin.conf** configuration file from **Server management** section. Edit manually it if **wg-shadmin** doesn't work how you are expecting.

If everything is working fine, take on mind all changes are applied to wireguard server when **Apply configuration** is selected at **Server management** section.

## License
wg-shadmin is released under the BSD-3 license. See the LICENSE file for a full copy of the license.
