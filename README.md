# Auto IP Blocker
Auto IP Blocker is an bash script to easily banning all IP addresses in a list, using iptables. The tool aim to help prevent attacks and junk network traffic by prevently banning IP addresses known to be suspicious/malicious.

In order to do so, Auto IP Blocker allow downloading of remote IP list as a simple txt file, as well as mixing with static txt files in conf, and whitelisting.

Also, the tool is quite stateless, meaning you can simply make it run everyday using a crontab. No IP will be deleted before the new list have been successfully download, and all IPs will be fully deleted and re-inserted at every launch.

## Format
The tool was originally designed to use the [ipsum list](https://github.com/stamparm/ipsum) as a source of IP addresses to block. Therefore, in order to add new IPs by yourself you must respect ipsum format, wich is :
```
<IP_ADDRESS> <THREAT_LVL>
```

Where `<IP_ADDRESS>` is the IP to ban and `<THREAT_LVL>` is the level of danger of this IP, which, in ipsum, is the number of list the IP appears in.

From a theorical point of view, `<IP_ADDRESS>` could be an IP subnet instead of an IP address, but this is highly not recommended, as IP whitelisting use string comparaison only. If you still want to use IP ranges, be careful this range does not include any of the whitelisted IPs.

If you are looking for a daily currated list of malicious IPs, we highly recommend you the [ipsum list.](https://github.com/stamparm/ipsum)


## Installation
> :warning: Auto IP Blocker must be run by root, and use bash `source` instruction to load configuration + inject datas extracted from config files into bash commands
therefore, you must be extra carefull about access rights and content of config files, without what an attacker could use thoses files to gain root access on system.

Auto IP Blocker only require `iptables`, `bash` and `curl`. To install it (run as root) :
```bash
#Download and unzip
wget https://github.com/OsaAjani/auto-ip-blocker/archive/main.zip -O /tmp/auto-ip-blocker.zip
unzip /tmp/auto-ip-blocker.zip /tmp/

#Copy conf files
mv /tmp/auto-ip-blocker-main/etc /etc

#Copy binary
mv /tmp/auto-ip-blocker-main/usr/bin/auto-ip-blocker /usr/bin/auto-ip-blocker

#Set access rights to ensure only root can modify config and binary
chown root:root /usr/bin/auto-ip-blocker
chmod 744 /usr/bin/auto-ip-blocker
chown -R root:root /etc/auto-ip-blocker
chmod -R 744 /etc/auto-ip-blocker
```

## Usage
To use Auto IP Blocker, simply call it as follow (you must be root)
```
/usr/bin/auto-ip-blocker --conf '/etc/auto-ip-blocker/global.conf'
```

## Configuration
You can set your own configuration (ex, setting your own source for remote IP addresses list, set your own iptables chain) by modifying the file `/etc/auto-ip-blocker/global.conf`.

### Adding additionnal IPs
You can add additionnal IPs to block by creating files ending with `.conf` and containing the IP addresses and their threats levels in the directory `/etc/auto-ip-blocker/ip.d/`.

Be carefull though to not use the file `/etc/auto-ip-blocker/ip.d/generated.conf` at he is automaticly override by Auto IP Blocker everytime.

### Whitelisting IPs
You can set IPs as whitelisted. If an IP is whitelisted, Auto IP Blocker will not block it even if the IP is listed in `ip.d/*`.

To define a list of whitelisted IPs, create a file ending with `.conf` and containing the IP addresses (without threats levels) in the directory `/etc/auto-ip-blocker/whitelist.d/`.
