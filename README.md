# OpenVPN and Transmission with WebUI

This is a fork from haugene/transmission-openvpn
---

This container contains OpenVPN and Transmission with a configuration
where Transmission is running only when OpenVPN has an active tunnel.
It bundles configuration files for many popular VPN providers to make the setup easier.

You need to specify your provider and credentials with environment variables,
as well as mounting volumes where the data should be stored.
An example run command to get you going is provided below.

It also bundles an installation of Tinyproxy to also be able to proxy web traffic over your VPN,
as well as scripts for opening a port for Transmission if you are using PIA or Perfect Privacy providers.

[![Credits on DigitalOcean](images/digitalocean.png)](https://m.do.co/c/ca994f1552bc)

You can also help out by submitting pull-requests or helping others with
open issues or in the gitter chat. A big thanks to everyone who has contributed so far!
And if you could be interested in joining as collaborator, let me know.


```
$ docker run --cap-add=NET_ADMIN --device=/dev/net/tun -d \
              -v /your/storage/path/:/data \
              -v /etc/localtime:/etc/localtime:ro \
              -e OPENVPN_PROVIDER=PIA \
              -e OPENVPN_CONFIG=CA\ Toronto \
              -e OPENVPN_USERNAME=user \
              -e OPENVPN_PASSWORD=pass \
              -e WEBPROXY_ENABLED=false \
              -e LOCAL_NETWORK=192.168.0.0/16 \
              --log-driver json-file \
              --log-opt max-size=10m \
              -p 9091:9091 \
              haugene/transmission-openvpn
```

You must set the environment variables `OPENVPN_PROVIDER`, `OPENVPN_USERNAME` and `OPENVPN_PASSWORD` to provide basic connection details. 

The `OPENVPN_CONFIG` is an optional variable. If no config 
is given, a default config will be selected for the provider you have chosen. Find available OpenVPN configurations by looking in the openvpn folder of the GitHub repository. The value that you 
should use here is the filename of your chosen openvpn configuration *without* the .ovpn file extension. For example: 

``` 
-e "OPENVPN_CONFIG=ipvanish-AT-Vienna-vie-c02"
```

You can also provide a comma separated list of openvpn configuration filenames. If you provide a list, a file will be randomly chosen in the list, this is useful for redundancy setups. For example: 

```
-e "OPENVPN_CONFIG=ipvanish-AT-Vienna-vie-c02,ipvanish-FR-Paris-par-a01,ipvanish-DE-Frankfurt-fra-a01"
```

### Supported providers
This is a list of providers that are bundled within the image. Feel free to create an issue if your provider is not on the list, but keep in mind that some providers generate config files per user. 
This means that your login credentials are part of the config an can therefore not be bundled. In this case you can use the custom provider setup described later in this readme. The custom provider 
setting can be used with any provider.
| Provider Name | Config Value (`OPENVPN_PROVIDER`) |
|:-----------------------------|:-------------|
| Anonine | `ANONINE` |
| AnonVPN | `ANONVPN` |
| BlackVPN | `BLACKVPN` |
| BTGuard | `BTGUARD` |
| Cryptostorm | `CRYPTOSTORM` |
| Cypherpunk | `CYPHERPUNK` |
| FreeVPN | `FREEVPN` |
| FrootVPN | `FROOT` | 
| FrostVPN | `FROSTVPN` |
| Giganews | `GIGANEWS` |
| HideMe | `HIDEME` |
| HideMyAss | `HIDEMYASS` |
| IntegrityVPN | `INTEGRITYVPN` |
| IPredator | `IPREDATOR` |
| IPVanish | `IPVANISH` |
| IronSocket | `IRONSOCKET` |
| Ivacy | `IVACY` |
| IVPN | `IVPN` |
| Mullvad | `MULLVAD` |
| Newshosting | `NEWSHOSTING` |
| NordVPN | `NORDVPN` |
| OVPN | `OVPN` |
| Perfect Privacy | `PERFECTPRIVACY` |
| Private Internet Access | `PIA` |
| PrivateVPN | `PRIVATEVPN` |
| ProtonVPN | `PROTONVPN` |
| proXPN | `PROXPN` |
| proxy.sh | `PROXYSH ` |
| PureVPN | `PUREVPN` |
| RA4W VPN | `RA4W` |
| SaferVPN | `SAFERVPN` |
| SlickVPN | `SLICKVPN` |
| Smart DNS Proxy | `SMARTDNSPROXY` |
| SmartVPN | `SMARTVPN` |
| TigerVPN | `TIGER` \
| TorGuard | `TORGUARD` |
| Trust.Zone | `TRUSTZONE` |
| TunnelBear | `TUNNELBEAR` |
| UsenetServerVPN | `USENETSERVER` |
| Windscribe | `WINDSCRIBE` | 
| VPNArea.com | `VPNAREA` |
| VPN.AC | `VPNAC` |
| VPN.ht | `VPNHT` |
| VPNBook.com | `VPNBOOK` |
| VPNFacile | `VPNFACILE` |
| VPNTunnel | `VPNTUNNEL` |
| VyprVpn | `VYPRVPN` |
