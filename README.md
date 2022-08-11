# useful-bash-alias-function
Custom alias or functions that are helpful during CTF / engagement

# Return to previous directory

`alias back='cd $OLDPWD'`

## Usage:

```
kali@kali:~$ cd Downloads

kali@kali:Downloads$ back

kali@kali:~$
```

# Anonymous SMB server in the current directory

"kali" is the name of the share. Change that if you want it to be less obvious.

`alias smbserver-here='sudo impacket-smbserver kali . -smb2support'`

## Usage:

```
# On Kali
kali@kali:~$ smbserver-here
[SUDO] Password for kali:

# Connect to the share on windows machine (replace with your Kali IP):
> net use \\10.0.0.0\kali

# File transfer from Kali to Windows
# Or the other way around if you need to transfer from Windows to Kali
> copy \\10.0.0.0\kali\mimikatz.exe C:\Users\Administrator\Desktop\
```

# Authenticated SMB server in the current directory

Sometimes windows refuses to connect to anonymous SMB shares. Use this one instead then:

`smbserver-here-pass='sudo impacket-smbserver kali . -smb2support -username root -password toor'`

Change the username and password if you need to.

## Usage:

```
# On Kali
kali@kali:~$ smbserver-here-pass
[SUDO] Password for kali:

# Connect to the share on windows machine (replace with your Kali IP):
> net use \\10.0.0.0\kali /user:root toor

# The rest is the same
```

# mkdir and then immediately cd into it

```
mkcd () {
    mkdir -p -- "$1" &&
    cd -P -- "$1"
}
```

## Usage:

Provide the name of the new directory.

```
kali@kali:~$ mkcd newdir

kali@kali:newdir$
```

# Create a new CTF directory and immediately cd into it

During CTF I like to use the following directory structure, to store the various tools/loots:
- logs: outputs of various tools, e.g. gobuster, nmap
- screenshots: screenshots of websites, exploit steps, proof of CTF flags etc.
- payloads: self explanatory. Usually msfvenom outputs.
- exploits: self explanatory. Usually searchsploit/github scripts.
- downloads: stuff I downloaded from the host, e.g. host's FTP/SMB.
- loots: self explanatory. Usually hashes, tickets, keys, etc.

This function quickly creates a new directory, with the above subdirectories, and then cd into it

```
new-ctf-dir () {
    mkdir -p -- "$1" &&
    cd -P -- "$1" &&
    mkdir logs screenshots payloads exploits downloads loots
}
```

## Usage:

Provide the name of the new directory.

```
kali@kali:~$ new-ctf-dir newdir

kali@kali:newdir$
```

# quick-ish nmap scan all TCP ports with default scripts

```
nmap-fast-tcp-all () {
    if [ -f tcp-scan.nmap ]; then
        echo "Scan results already exist in current directory"
    else
        sudo nmap -vv -T4 --min-rate=800 -A -p- -oA tcp-scan $1
    fi
}
```

## Usage:

Supply the target IP

```
$ nmap-fast-tcp-all 127.0.0.1
```

# quick-ish nmap scan all UDP ports

```
nmap-fast-udp-all () {
    if [ -f udp-scan.nmap ]; then
        echo "Scan results already exist in current directory"
    else
        sudo nmap -vv -T4 --min-rate=1200 -p- -sU -oA udp-scan $1
    fi
}
```

## Usage:

Supply the target IP

```
$ nmap-fast-udp-all 127.0.0.1
```

# quick-ish nmap scan seletected UDP ports with default scripts

```
nmap-fast-udp-details () {
    if [ -f udp-details.nmap ]; then
        echo "Scan results already exist in current directory"
    else
        sudo nmap -vv -T4 --min-rate=1200 -sU -A -p $1 -oA udp-details $2
    fi
}
```

## Usage:

Supply the target ports and then the IP

```
$ nmap-fast-udp-details 53,123,137 127.0.0.1
```

# Pretty print XML with colours

```
pprint-xml-colours () {
    cat $1 | xmllint --format - | pygmentize -l xml
}
```

## Usage:

Supply the XML file name

```
$ pprint-xml-colours sitemap.xml
```

# Pretty print XML (without colours)

```
pprint-xml () {
    cat $1 | xmllint --format -
}
```

## Usage:

Supply the XML file name

```
$ pprint-xml sitemap.xml
```
