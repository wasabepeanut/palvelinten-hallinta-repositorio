# Esivalmistelut
- Käyttöjärjestelmä: Windows 11
- Virtualisointiohjelma: VirtualBox
- Virtuaalikoneen käyttöjärjestelmä: Debian 13 Trixie


# x) Lue ja tiivistä
## Two Machine Virtual Network
Artikkelissa käydään nopeasti läpi miten asennetaan ja saadaan **Vagrant**-työkalu käyttöön
Vagrantin hyötyjä:
- Virtualbox koneiden automaattinen järjestely
- Automatisoi SSH:n sisäänkirjautumisen
- Ei vaadi graafista käyttöliittymää.

(Karvinen 11.4.2021)


##  Salt Vagrant
Tässä artikkelissa oli tarkoituksena lukea vain kohdat "Infra as Code..." ja "top.sls..." 
Olimme tehneet tämän kaltaisia harjoituksia viime viikon tehtävissä sekä luennolla.

- Saltin state.apply komentojen suorittaminen sekä yksittäisellä tiedostolla, että top.file-tiedostolla
- Tiedostoissa YAML syntaksi
  
(Karvinen 28.3.2023)


# a) Hello Vagrant!
Tässä tehtävässä käytän apunani seuraavaa Vagrantin asennus [ohjetta](https://ipv6.rs/tutorial/Windows_11/Vagrant/).

Asensin Vagrantin Windows asentajalla.

Tarkistetaan, että asennus onnistui: `vagrant --version`

<img width="302" height="52" alt="image" src="https://github.com/user-attachments/assets/8b23ca25-a001-44e0-9f98-bd1d85fae124" />


# b) Linux Vagrant
Tässä tehtävässä luodaan yksi Linux-virtuaalikone Vagrantilla.
Jatkoin tehtävää sekä Teron [ohjeen](https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/) että edellämainitun ohjeen mukaisesti.

Luodaan uusi hakemisto ja alustetaan se:
```
mkdir twohost
cd twohost
vagrant init
```

<img width="597" height="125" alt="image" src="https://github.com/user-attachments/assets/f1b05887-f2dc-45a4-b7a7-fa31c7e8abf6" />

Tämä loi minulle tiedoston "**Vagrantfile**", jonne lisään skriptin Teron ohjeista (**muutin myös skriptissä ympäristön nykyiseen: bullseye64 -> bookworm64**) 

<img width="395" height="109" alt="image" src="https://github.com/user-attachments/assets/23201c33-e91e-4331-9729-120c81e15e65" />

Vagrant päälle:
```
vagrant up
```

<img width="795" height="212" alt="image" src="https://github.com/user-attachments/assets/0e1b119d-4694-4fae-83a5-9b1ccd4d289d" />

Asennusten jälkeen kirjauduin sisään **t001**:

```
vagrant ssh t001
```

<img width="597" height="72" alt="image" src="https://github.com/user-attachments/assets/a3c9c3d8-ebd6-45e8-9006-e674edb8a67a" />

Toimii!


# c) Kaksin kaunihimpi
Tässä tehtävässä luodaan yhden sijaan kaksi virtuaalikonetta. (Periaatteessa loimme jo edellisessä tehtävässä kaksi)

Kokeillaan nyt näiden kahden koneen välistä kommunikointia.

t001 -> t002:
`ping 192.168.88.102`

<img width="561" height="86" alt="image" src="https://github.com/user-attachments/assets/88e6fc33-74a3-4f10-a0fb-72b3000ec985" />

t002 -> t001:
`ping 192.168.88.101`

<img width="567" height="77" alt="image" src="https://github.com/user-attachments/assets/6b48730e-e44c-4f7c-b568-c76ecbe0a05d" />

Näyttäisi toimivan.


d) Herra-orja verkossa
Tässä tehtävässä demonstroin herra-orja arkkitehtuurin toimintaa näiden kahden koneen välillä.

Asennetaan ensin Salt Teron [ohjeiden](https://terokarvinen.com/install-salt-on-debian-13-trixie/) mukaisesti.

Asennetaan **wget**, jolla voidaan hakea Salt repositoriot, sekä luodaan Saltille hakemistot: 
```
sudo apt-get update
sudo apt-get install wget
mkdir saltrepo/
cd saltrepo/
wget https://packages.broadcom.com/artifactory/api/security/keypair/SaltProjectKey/public
wget https://github.com/saltstack/salt-install-guide/releases/latest/download/salt.sources
```

Asennetaan myös GnuPG paketti, jolla voidaan tarkistaa itse avain:
```
sudo apt-get install gnupg -y
```

Tarkistetaan että tiedostoissa ei ole vikoja sekä avain:
```
less public
less salt.sources
gpg --show-key --with-fingerprint public 
```

<img width="746" height="133" alt="image" src="https://github.com/user-attachments/assets/d02619a1-e167-425e-b044-0f4c8fa33a6e" />

Kaikki näyttäisi olevan kunnossa. Luotetaan ja asennetaan repot:
```
sudo cp public /etc/apt/keyrings/salt-archive-keyring.pgp
sudo cp salt.sources /etc/apt/sources.list.d/
```

Nyt asennetaan Salt, tehdään koneesta "**t001**" isäntäkone:
```
sudo apt-get update
sudo apt-get install salt-master
salt --version
```

<img width="357" height="43" alt="image" src="https://github.com/user-attachments/assets/a329eace-47da-496e-8b6f-0be5fde83f59" />

Toistetaan toimenpiteet toiselle koneelle (**t002**), mutta tehdään siitä alikone.


# Lähteet
IPv6rs. 2024. How to Install Vagrant on Windows 11. IPv6rs. Luettavissa: https://ipv6.rs/tutorial/Windows_11/Vagrant/. Luettu: 7.11.2025.

Karvinen, T. 11.4.2021. Two Machine Virtual Network With Debian 11 Bullseye and Vagrant. Tero Karvinen. Luettavissa: https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/. Luettu: 7.11.2025.

Karvinen, T. 28.3.2023. Salt Vagrant - automatically provision one master and two slaves. Tero Karvinen. Luettavissa: https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file. Luettu: 7.11.2025.

Karvinen, T. 20.10.2025. Install Salt on Debian 13 Trixie. Tero Karvinen. Luettavissa: https://terokarvinen.com/install-salt-on-debian-13-trixie/. Luettu: 7.11.2025.
