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
Tässä tehtävässä käytän apunani Teron Vagrantin asennus [ohjeita](https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/).

Asensin Vagrantin Windows asentajalla.

Tarkistetaan, että asennus onnistui:
`vagrant --version`

<img width="302" height="52" alt="image" src="https://github.com/user-attachments/assets/8b23ca25-a001-44e0-9f98-bd1d85fae124" />


# b) Linux Vagrant


# Lähteet
Karvinen, T. 11.4.2021. Two Machine Virtual Network With Debian 11 Bullseye and Vagrant. Tero Karvinen. Luettavissa: https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/. Luettu: 7.11.2025.

Karvinen, T. 28.3.2023. Salt Vagrant - automatically provision one master and two slaves. Tero Karvinen. Luettavissa: https://terokarvinen.com/2023/salt-vagrant/#infra-as-code---your-wishes-as-a-text-file. Luettu: 7.11.2025.
