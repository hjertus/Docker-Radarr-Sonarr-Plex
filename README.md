# Docker-Radarr-Sonarr-Plex
A docker-compose file to download: plex, sonarr, radarr, lidarr, readarr, prowlarr, jacket , overseer, watchtower, and transmission. Samba file management. 

## Mount hard drive (Optional)
Mount a new drive to use as storage
```bash
mkdir /mnt/hdd
```
```bash
mount /dev/sdb /mnt/hdd
```

### Multiple drives (Only needed if you want to use multiple hard drives)
```bash
mkdir /mnt/a
mount /dev/sdb /mnt/a
```
```bash
mkdir /mnt/b
mount /dev/sdb1 /mnt/b
```
```bash
mkdir /mnt/c
mount /dev/sdb2 /mnt/c
```
Mount all the hard drives to one directory using mhddfs

```bash
apt-get install mhddfs
```
```bash
mkdir /mnt/hdd
mhddfs /mnt/a/,/mnt/b/,/mnt/c/ /mnt/hdd
```

## Directory setup

Make a new directory called plex_server either in the mnt/hdd or another place. 
```bash
mkdir plex_server
```
Inside plex_server create all these opt folders and a media folder. 
```bash
mkdir -p /opt/{jackett,sonarr,radarr,lidarr,readarr,prowlarr,plex,overseerr}
```
```bash
mkdir media/content
```
```bash
mkdir -p {Books,downloads,Movies,Music,Other,Photos,TV}
```
Inside the downloads folder create these 3 folders
```bash
mkdir -p {completed,incomplete,watch}
```

## Make a new user

Make a new user called plex. 
```bash
useradd plex
```
Find the PUID and PGID
```bash
id plex
```
Make the plex user owner of the plex_server directory. (1001 should be the id of the user.)
```bash
chown -R 1001:1001 /mnt/hdd/plex_server
```

## Samba user (Optinal)
Download samba
```bash
apt install samba
```

Add user to samba
```bash
smbpasswd -a plex
```

Change samba conf
```bash
nano /etc/samba/smb.conf
```
Scroll to the bottom and add this code. 
```bash
[Plex_server]
  path = /mnt/hdd/plex_server
  readonly = no
  inherit permission = yes
```

Restart samba
```bash
service smbd restart
```

## Docker-compose

Download the docker-compose.yml file, the .env.example, and the .env.example file. 

Change the .env.example to have the right PUID, PGID, and timezone. Change the name of it to .env. 
```bash
nano .env.example
```

Change the .env_openvpn.example to have the right PUID, PGID, timezone, and OpenVPN settings (I use nordvpn as an example.). Change the name of it to .env_openvpn. 
```bash
nano .env_openvpn.example
```
Change the docker-compose.yml file to use the right directory. 
If you have the directory as /mnt/hdd/plexserver the docker-compose file should be right, else change the directories to match what you use. 
```bash
nano docker-compose.yml
```

## Run the docker-compose file

```bash
docker-compose up -d
```
Hopefully, you won't see any failures.




