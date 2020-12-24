# auto-ip-blocker
Auto IP Blocker is an automated script to daily banning all IP addresses in a list, using fail2ban and iptables.

The tool use [ipsum](https://github.com/stamparm/ipsum) list as a source of IP addresses to ban. So, in order to add new IPs by yourself, you must respect ipsum format :
```
<IP_ADDRESS> <THREAT_LVL>
```

Where `<IP_ADDRESS>` is the IP to ban and `<THREAT_LVL>` is the level of danger of this IP, which, in ipsum, is the number of list the IP appears in.


## Installation
> :warning: Auto IP Blocker must be run by root, and use bash `source` instruction to load configuration + inject datas extracted from config files into bash commands
> therefore, you must be extra carefull about access rights and content of config files, without what an attacker could use thoses files to gain root access on system.

Auto IP Blocker only require `iptables`, `bash` and `curl`. To install it :
```
    
```

## Usage
To use Auto IP Blocker, simply add an entry in your crontab with
```
0 4 * * * /usr/bin/auto-ip-blocker --conf '/etc/auto-ip-blocker/global.conf'
```

## Configuration
You can set your own configuration (ex, using another source for IP addresses list, another name for fail2ban) by modifying the file `/etc/auto-ip-blocker/global.conf`.

## Adding additionnal IPs
You can add additionnal IPs to ban by creating files containing the IP addresses and their threats levels in the directory `/etc/auto-ip-blocker/ip.d/`.

Be carefull though to not use the file `/etc/auto-ip-blocker/ip.d/generated.conf` at he is automaticly override by Auto IP Blocker everytime.
