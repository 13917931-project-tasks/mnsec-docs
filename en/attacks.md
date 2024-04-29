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

Is a Hydra attacks parameter which promotes the usage of diverse IP addresses to execute floods. In this sense, all ICMP, UDP and TCP flood attacks triggered ET DROP rules. These rules are based in the detection of activities related to IP addresses known by their relation with flood attakcs.

#### Single IP attacks

Usage example (Executed in mnsec shell):

```
<mnsec-host> hping3 <protocol-or-flag> --flood -p <port-number> <ip-address> 
```

```
hping3 --icmp/--udp --flood --rand-source -p 567 --data 10 192.168.0.1
```

```
hping3 -S/-P/-U/-A/-F --flood --rand-source -p 567 --data 10 192.168.0.1
```

#### TCP Flood

Using ports not related to specific functions in internet traffic:

1. alert tcp any 4444 -> any any (msg:"POSSBL SCAN M-SPLOIT R.SHELL TCP"; classtype:trojan-activity; sid:1000013; priority:1; rev:1;)

**SSH port:**

Rule that generated alerts:

1. alert tcp $EXTERNAL_NET any -> $HOME_NET 22 (msg:"ET SCAN Potential SSH Scan"; flow:to_server; flags:S,12; threshold: type both, track by_src, count 5, seconds 120; reference:url,en.wikipedia.org/wiki/Brute_force_attack; classtype:attempted-recon; sid:2001219; rev:20; metadata:created_at 2010_07_30, updated_at 2019_07_26;)

**POP3 ports:**

Rule that generated alerts:

1. alert tcp $EXTERNAL_NET any -> $HOME_NET 110 (msg: "Possible POP3 Brute Force attack"; flags:S; flow: to_server; threshold: type limit, track by_src, count 5, seconds 20; classtype: credential-theft; sid: 100000138; rev:1;) (self-authored)
2. alert tcp $EXTERNAL_NET any -> $HOME_NET 110 (msg:"ET SCAN Rapid POP3 Connections - Possible Brute Force Attack"; flow:to_server; flags: S,12; threshold: type both, track by_src, count 30, seconds 120; classtype:misc-activity; sid:2002992; rev:7; metadata:created_at 2010_07_30, updated_at 2019_07_26;)
3. alert tcp any 4444 -> any any (msg:"POSSBL SCAN M-SPLOIT R.SHELL TCP"; classtype:trojan-activity; sid:1000013; priority:1; rev:1;)

**IMAP Flood**

Rules that generated alerts:

1. alert tcp $EXTERNAL_NET any -> $HOME_NET 143 (msg:"ET SCAN Rapid IMAP Connections - Possible Brute Force Attack"; flow:to_server; flags: S,12; threshold: type both, track by_src, count 30, seconds 60; classtype:misc-activity; sid:2002994; rev:7; metadata:created_at 2010_07_30, updated_at 2019_07_26;)
2. alert tcp any 4444 -> any any (msg:"POSSBL SCAN M-SPLOIT R.SHELL TCP"; classtype:trojan-activity; sid:1000013; priority:1; rev:1;)

#### UDP Flood 

Rule that generated alerts:

1. alert udp any 4444 -> any any (msg:"POSSBL SCAN M-SPLOIT R.SHELL UDP"; classtype:trojan-activity; sid:1000014; priority:1; rev:1;)

#### ICMP Flood 

Was not detected by any rule, and the difficulties related to the detection of ICMP Floods can be justified by the challenge of differentiating legitm traffic involving ICMP from malicious one. 

#### Fragmentation

Flood attacks which were related to seding packets with high data size, promoting packets fragmentation, were not detected.
