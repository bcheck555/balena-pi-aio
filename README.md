# balena-pi-aio

This is a [RaspberryPi](https://www.raspberrypi.org/) [balenaCloud](https://www.balena.io/cloud) stack project borrowing heavily from the following:

* [Pi-hole](https://hub.docker.com/r/pihole/pihole/)
* [dnscrypt-proxy](https://github.com/DNSCrypt/dnscrypt-proxy) _(optional)_
* [nfs-server](https://github.com/sjiveson/nfs-server-alpine)
* [deluge](https://github.com/linuxserver/docker-deluge)

## Getting Started

To get started you'll first need to sign up for a free balenaCloud account and flash your device.

<https://www.balena.io/docs/learn/getting-started>

## Deployment

Once your account is set up, deployment is carried out by downloading the project and pushing it to your device either via Git or the balena CLI.

<https://www.balena.io/blog/deploy-network-wide-ad-blocking-with-pi-hole-and-a-raspberry-pi/>


# Reference:
https://github.com/klutchell/balena-pihole

https://github.com/sjiveson/nfs-server-alpine

https://github.com/linuxserver/docker-deluge

# Balena Environment Variables:
## PiHole:

Device Variables apply to all services within the application, and can be applied fleet-wide to apply to multiple devices.

|Name|Example|Purpose|
|---|---|---|
|`TZ`|`America/New_York`|To inform services of the timezone in your location, in order to set times and dates within the applications correctly. Find a [list of all timezone values here](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones).|
|`DNSMASQ_LISTENING`|`eth0`|We set this to `eth0` to indicate we want DNSMASQ to listen on the ethernet interface of the Raspberry Pi. If you're connecting to your network with WiFi replace this with `wlan0`|
|`INTERFACE`|`eth0`|As above.|
|`WEBPASSWORD`|`mysecretpassword`|_(optional)_ password for accessing the web-based interface of Pi-hole - you won’t be able to access the admin panel without defining a password here.
|`DNS1`|`127.0.0.1#5053`|_(optional)_ Tell Pi-hole where to forward DNS requests that aren’t blocked. We’re using the [dnscrypt-proxy](https://github.com/DNSCrypt/dnscrypt-proxy) project here but you can specify your own.|
|`DNS2`|`127.0.0.1#5053`|_(optional)_ Secondary DNS server - see above.|
|`ServerIP`|`x.x.x.x`|_(recommended)_ Set to your server's LAN IP, used by web block modes and lighttpd bind address.|
|`ServerIPv6`|`x.x.x.x`|_(recommended)_ Set to your server's LAN IP, used by web block modes and lighttpd bind address.|
|`DNSCRYPT_SERVER_NAMES`|`['scaleway-fr', 'google', 'yandex', 'cloudflare']`|_(optional)_ Provide a toml array of specific [public resolvers](https://download.dnscrypt.info/dnscrypt-resolvers/v2/public-resolvers.md) if using dnscrypt-proxy.|

## NFS-Server:

|Name|Example|Purpose|
|---|---|---|
|`SHARED_DIRECTORY`|`/nfsshare`|Make whatever directory is specified available to NFS v4 clients.|
|`READ_ONLY`|`true`|Will cause the exports file to contain ro instead of rw, allowing only read access by clients.|
|`SYNC`|`true`|Will cause the exports file to contain sync instead of async, enabling synchronous mode. Check the exports man page for more information: https://linux.die.net/man/5/exports.|
|`PERMITTED`|`"10.11.99.*"`|Permit only hosts with an IP address starting 10.11.99 to mount the file share.|

## Deluge:

|Name|Example|Purpose|
|---|---|---|
|`PUID`|`1000` | for UserID|
|`PGID`|`1000` | for GroupID|
|`TZ`|`America/New_York`|To inform services of the timezone in your location, in order to set times and dates within the applications correctly. Find a [list of all timezone values here](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones).|
|`UMASK_SET`|`022` | for umask setting of deluge, default if left unset is 022 |
|`DELUGE_LOGLEVEL`|`error` | set the loglevel output when running Deluge, default is info for deluged and warning for delgued-web |
