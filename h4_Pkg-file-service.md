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

<img width="475" height="97" alt="image" src="https://github.com/user-attachments/assets/0d53af67-2a2f-4a6d-a5f2-aad9b4e2ae20" />

Näyttäisi olevan päällä.
Tehdään nyt perus HTML-sivu testausta varten.

```
sudo mkdir /var/www/html/test_website
echo '<html><head><title>Example</title></head><body><h1>GFG</h1><p>This is a test.</p></body></html>' | sudo tee /var/www/html/test_website/index.html
```

Lisätään vielä konfigurointitiedosto GeeksforGeeks sivustolta, jonka annoin ChatGPT:n muokattavaksi Debianille. Muutetaan myös testisivun oikeuksia, jotta Apache voi suorittaa sen.

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
sudo chown -R www-data:www-data /var/www/html/test_website
sudo chmod -R 755 /var/www/html/test_websit
```
Koska minulla on palomuuri (**ufw**) käytössä, sallitaan vielä Apache:

```
sudo ufw allow 'Apache'
sudo ufw reload
sudo ufw status 
```

<img width="526" height="142" alt="image" src="https://github.com/user-attachments/assets/3b279ca3-f5a9-42e6-b0e4-ff3b61ebf6a5" />

Siirryin selaimen puolelle ja menin osoitteeseen 'http://localhost/test_website/'.

<img width="788" height="297" alt="image" src="https://github.com/user-attachments/assets/6c89701a-abc8-4e09-8c43-abfb619ef4c1" />

Selain toimii!

Asennetaan vielä Virtual host:

```
sudo mkdir -p /var/www/example.com/public_html
sudo mkdir -p /var/www/example.org/public_html
```

Muokataan oikeudet:

```
sudo chown -R $USER:$USER /var/www/example.com/public_html
sudo chown -R $USER:$USER /var/www/example.org/public_html
sudo chmod -R 755 /var/www
```

Luodaan molemmille hakemistoille index.html tiedostot testaamista varten:

**example.com**
```
sudo tee /var/www/example.com/public_html/index.html > /dev/null << EOF
<html>
  <body>
     <h1>Welcome to example.com!</h1>
  </body>
</html>
EOF
```

**example.org**
```
sudo tee /var/www/example.org/public_html/index.html > /dev/null << EOF
<html>
  <body>
     <h1>Welcome to example.org!</h1>
  </body>
</html>
EOF
```

Luodaan konfigurointitiedostot Virtual Hosteille:

**example.com**
```
sudo nano /etc/apache2/sites-available/example.com.conf

<VirtualHost *:80>
    ServerAdmin webmaster@example.com
    ServerName example.com
    ServerAlias www.example.com
    DocumentRoot /var/www/example.com/public_html
    ErrorLog ${APACHE_LOG_DIR}/example.com_error.log
    CustomLog ${APACHE_LOG_DIR}/example.com_access.log combined
</VirtualHost>
```


**example.org**
```
sudo nano /etc/apache2/sites-available/example.org.conf

<VirtualHost *:80>
    ServerAdmin webmaster@example.org
    ServerName example.org
    ServerAlias www.example.org
    DocumentRoot /var/www/example.org/public_html
    ErrorLog ${APACHE_LOG_DIR}/example.org_error.log
    CustomLog ${APACHE_LOG_DIR}/example.org_access.log combined
</VirtualHost>
```

Otetaan Virtual Hostit käyttöön:
```
sudo a2ensite example.com.conf
sudo a2ensite example.org.conf
sudo systemctl restart apache2
```

Lisätään vielä verkkotunnukset osoittamaan local hostiin:

```
sudo nano /etc/hosts

127.0.0.1  example.com example.org
```

<img width="400" height="101" alt="image" src="https://github.com/user-attachments/assets/f3d97068-9ed9-475c-b2b7-22600f63f217" />

Testataan vielä näkyykö webissä.

**example.com**

<img width="490" height="106" alt="image" src="https://github.com/user-attachments/assets/54e213d7-e356-43fa-8bb7-4d5996f8f7b8" />


**example.org**

<img width="563" height="108" alt="image" src="https://github.com/user-attachments/assets/5fa97a08-b7db-4f6b-9b17-9cdd20e52f7a" />

Toimii!

Muokataan tiedoston oikeuksia:

```
sudo chmod -R 777 /var/www/example.com/public_html/index.html 
sudo chmod -R 777 /var/www/example.org/public_html/index.html 
```

<img width="792" height="132" alt="image" src="https://github.com/user-attachments/assets/1e85314f-0762-42af-aa51-0a36d1def4dd" />

Nyt käyttäjäoikeuksillakin pystyy muokkaamaan nettisivuja.


# Lähteet

Geeksforgeeks. 23.7.2025. How to Install Apache Web Server in Linux: Ubuntu, Fedora, RHEL?. GeeksforGeeks. Luettavissa: https://www.geeksforgeeks.org/techtips/install-apache-web-server-in-linux/. Luettu: 17.11.2025.

Geeksforgeeks. 23.7.2025. How to Setup Virtual Hosts with Apache Web Server on Linux?. GeeksforGeeks. Luettavissa: https://www.geeksforgeeks.org/linux-unix/how-to-setup-virtual-hosts-with-apache-web-server-on-linux/. Luettu: 17.11.2025.

Karvinen, T. 3.4.2018. Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port. Tero Karvinen. Luettavissa: https://terokarvinen.com/2018/04/03/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=karvinen%20salt%20ssh. Luettu: 16.11.2025.
