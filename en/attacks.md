# Execution and detection of attacks using mnsec and Suricata 

The usage of Mininet-sec deatiled in this text was based in the use of the basic topology cantained in *firewall.py* file.

## Brute Force

##### IMAP 

Command executed in the directory that contained the passwords and logins list:

```
sudo mnsecx o1 hydra  -L top-usernames-shortlist.txt -P top-usernames-shortlist.txt imap://192.168.1.103/LOGIN -V -I -F
```

Rules that generated alerts:

1. alert tcp $EXTERNAL_NET any -> $HOME_NET 143 (msg:"ET SCAN Rapid IMAP Connections - Possible Brute Force Attack"; flow:to_server; flags: S,12; threshold: type both, track by_src, count 30, seconds 60; classtype:misc-activity; sid:2002994; rev:7; metadata:created_at 2010_07_30, updated_at 2019_07_26;)
2. alert tcp $EXTERNAL_NET any -> $HOME_NET 143 (msg: "Possible IMAP Brute Force attack"; flags:S; flow: to_server; threshold: type limit, track by_src, count 20, seconds 40; tcp.mss:1460; dsize:0; window:42340; classtype: credential-theft; sid: 100000137; rev:1;) (self-authored)

#### SSH

Command executed in the directory that contained the passwords and logins list:

```
sudo mnsecx o1 hydra  -L top-usernames-shortlist.txt -P top-usernames-shortlist.txt ssh://192.168.1.103/LOGIN -V -I -F
```

Rule that generated alerts:

1. alert tcp $EXTERNAL_NET any -> $HOME_NET 22 (msg:"ET SCAN Potential SSH Scan"; flow:to_server; flags:S,12; threshold: type both, track by_src, count 5, seconds 120; reference:url,en.wikipedia.org/wiki/Brute_force_attack; classtype:attempted-recon; sid:2001219; rev:20; metadata:created_at 2010_07_30, updated_at 2019_07_26;)

#### POP3

Command executed in the directory that contained the passwords and logins list:

```
sudo mnsecx o1 hydra  -L top-usernames-shortlist.txt -P top-usernames-shortlist.txt pop3://192.168.1.103/LOGIN -V -I -F
```

Rule that generated alerts:

1. alert tcp $EXTERNAL_NET any -> $HOME_NET 110 (msg: "Possible POP3 Brute Force attack"; flags:S; flow: to_server; threshold: type threshold, track by_src, count 5, seconds 20; classtype: credential-theft; sid: 100000138; rev:1;) (self-authored)

## TCP, UDP and ICMP Flood

#### --rand-source


