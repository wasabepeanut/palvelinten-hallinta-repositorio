# Esivalmistelut
- Käyttöjärjestelmä: Windows 11
- Virtualisointiohjelma: VirtualBox
- Virtuaalikoneen käyttöjärjestelmä: Debian 13 Trixie


# x) Lue ja tiivistä
## Pkg-File-Service
- Konfiguraationhallintajärjestelmillä voi hallita suuria määriä daemoneja, yleisin malli tähän on **pkg-file-service**.
- Toimintavaiheet lyhyesti:
  1. Ohjelmiston asennus
  2. Konfigurointitiedoston korvaus
  3. Daemonin uudelleenkäynnistys
- Vaiheiden jälkeen uusi konfiguraatio on käyttövalmis.

(Karvinen, T 3.4.2018)


# a) SSHouto
Käytin tässä tehtävässä apuna sekä [Teron ohjeita](https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh) että tekoälyä (ChatGPT). Tein tehtävän Debian 13 Trixie virtuaalikoneella.

Heti aluksi loin ensin uuden kansion moduulille:

```
cd /srv/salt
sudo mkdir ssh
cd ssh
```

Tämän jälkeen loin state-tiedoston ja lisäsin sinne sisällön:

```
sudoedit sshd.sls
```
```
openssh-server:
  pkg.installed

/etc/ssh/sshd_config:
  file.managed:
    - source: salt://sshd_config
    - user: root
    - group: root
    - mode: 600

sshd:
  service.running:
    - enable: True
    - reload: True
    - watch:
      - file: /etc/ssh/sshd_config
```

<img width="355" height="302" alt="image" src="https://github.com/user-attachments/assets/fa8dc414-808e-4993-8bc3-59ef6d0ffc08" />

Samat vaiheet konfigurointitiedostolle:

```
sudoedit sshd_config
```

```
# MANAGED BY SALT - DO NOT EDIT

Port 8888

HostKey /etc/ssh/ssh_host_rsa_key
HostKey /etc/ssh/ssh_host_ecdsa_key
HostKey /etc/ssh/ssh_host_ed25519_key

PermitRootLogin prohibit-password
PubkeyAuthentication yes
PasswordAuthentication yes
ChallengeResponseAuthentication no

SyslogFacility AUTH
LogLevel INFO

X11Forwarding yes
X11DisplayOffset 10

PrintMotd no
PrintLastLog yes
TCPKeepAlive yes
AcceptEnv LANG LC_*

Subsystem sftp /usr/lib/openssh/sftp-server

UsePAM yes
```

Sitten asetetaan statet alikoneille:

```
sudo salt '*' state.apply ssh.sshd
```

<img width="732" height="432" alt="image" src="https://github.com/user-attachments/assets/8c109ced-c24f-453c-895e-6fd8897d98b7" />

Jokin taisi mennä pieleen.
Pyysin ChatGPT:tä tarkistamaan asian, ja syynä oli sls-tiedosto. Koska statet ovat ssh hakemiston sisällä, ne täytyy sisällyttää tiedostossa.

<img width="647" height="67" alt="image" src="https://github.com/user-attachments/assets/1062135f-6f52-465e-98ae-3f44acb3f020" />

Kokeillaan uudestaan.

<img width="301" height="382" alt="image" src="https://github.com/user-attachments/assets/9c4043c7-96eb-40b4-9fc9-969bce01be78" />

Jipii!
Nyt testataan saanko yhteyden

```
sudo salt 'kingbob' cmd.run 'hostname -I'
ssh -p 8888 kingbob@10.0.2.15
```

<img width="798" height="175" alt="image" src="https://github.com/user-attachments/assets/977618a1-59ac-4ef3-8f06-190b0a8b703d" />

Toimii!

# b) Apache & Name Based Virtual Host
Tässä tehtävässä käytän apuna netistä löytyviä [Apachen](https://www.geeksforgeeks.org/techtips/install-apache-web-server-in-linux/) ja [Virtual Hostin](https://www.geeksforgeeks.org/linux-unix/how-to-setup-virtual-hosts-with-apache-web-server-on-linux/) asennus ohjeita sekä tekoälyä (ChatGPT).

Ensin asennetaan Apache:
```
sudo apt update
sudo apt upgrade -y
sudo apt install apache2 -y
```

Käynnistetään ja tarkistetaan että se meni päälle:
```
sudo systemctl enable apache2
sudo systemctl status apache2
```
**KUVA**

```
sudo tee /etc/apache2/sites-available/web.testingserver.com.conf > /dev/null << 'EOF'
<VirtualHost *:80>
    ServerName web.testingserver.com
    DocumentRoot /var/www/html/website
    DirectoryIndex index.html
    ErrorLog ${APACHE_LOG_DIR}/web.testingserver.com_error.log
    CustomLog ${APACHE_LOG_DIR}/web.testingserver.com_access.log combined
</VirtualHost>
EOF

```


# Lähteet
Karvinen, T. 3.4.2018. Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port. Tero Karvinen. Luettavissa: https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh. Luettu: 16.11.2025.
