# x) Lue ja tiivistä
## Run Salt Command Locally
- Salt-komentojen suorittaminen lokaalisti on käytännöllistä harjoituksen ja testaamisen kannalta
- Tärkeimmät tilafunktiot: **pkg**, **file**, **service**, **user** ja **cmd**
- Salt-menetelmää käytetään alikoneiden hallitsemiseen verkossa.
  
(Karvinen 28.10.2021)



## Salt Quickstart
- Salt-menetelmä voidaan aloittaa pikaisesti seuraavin vaihein:
  - Asennetaan "Master" ja "Slave":

    **Master**
    ```
    master$ sudo apt-get update
    master$ sudo apt-get -y install salt-master
    master$ hostname -I
    ```
    **Slave**
    ```
    slave$ sudo apt-get update
    slave$ sudo apt-get -y install salt-minion
    ```
  - Hyväksytään "Slave Key"
    ```
    master$ sudo salt-key -A
    ```
  - Testataan että luotu alikone näkyy.
    ```
    master$ sudo salt '*' cmd.run 'whoami'
    ```
(Karvinen 28.3.2018)



## Raportin kirjoittaminen
- Muistiinpanojen tekeminen
- Pääpointit:
  - **Toistettava** = Raportin ohjeita seuratessa tulisi saada samanlainen tulos
  - **Täsmällinen** = Toimenpiteiden ja tilanteiden tarkka raportointi 
  - **Helppolukuinen** = Tekstien jäsentely ja selkeää kieltä
  - **Viittaa lähteisiin**
  - **Raportointivirheet** = Plagiointi ja väärän tiedon raportointi.


(Karvinen 4.6.2006)


## Salt Install Guide: Linux (DEB)
Artikkelissa ohjeistetaan hieman tarkemmin Salt-menetelmän asentamisesta Linux-koneelle.
Lyhyesti:
- Salt-projekti repositorion asennus:
- metadatan päivitys
- Salt-komponenttien asennus
- Salt-palvelujen käyttöönotto ja käynnistys

(VMware 2025)


# a) Debian 12-Bookworm asennus
## Esivalmistelut
- Laite: Windows-11 käyttöjärjestelmällä varustettu kannettava tietokone
- Virtualisointiohjelma: Oracle VirtualBox
- Iso-tiedosto: https://cdimage.debian.org/debian-cd/current-live/amd64/iso-hybrid/debian-live-13.1.0-amd64-xfce.iso
- 
## Asennus
Käytin tässä tehtävässä Teron antamia [ohjeita](https://terokarvinen.com/2021/install-debian-on-virtualbox/), mutta vaihdoin versiotyypiksi Debian 12 Bookworm (64-bit). 
Kaikki muut vaiheet ovat tehty ohjeiden mukaisella tavalla.

<img width="943" height="715" alt="image" src="https://github.com/user-attachments/assets/ce4549c9-83c1-499d-a34f-03e5ca091c60" />


# b) asenna Salt 🧂
Tässä tehtävässä käytän apuna [Salt Projectin ohjeita](https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/linux-deb.html) Saltin asentamiseen.

Suoritin ensin seuraavat komennot, jolla asensin Salt Project repositorion:
```
mkdir -p /etc/apt/keyrings
curl -fsSL https://packages.broadcom.com/artifactory/api/security/keypair/SaltProjectKey/public | sudo tee /etc/apt/keyrings/salt-archive-keyring.pgp
curl -fsSL https://github.com/saltstack/salt-install-guide/releases/latest/download/salt.sources | sudo tee /etc/apt/sources.list.d/salt.sources
```
Päivitin metadatan:
``sudo apt update``

Nyt kun minulla oli repositorio, päätin seurata Teron [Quickstart-ohjeeseen](https://terokarvinen.com/2018/03/28/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/).

Asensin ensin isäntäkoneen (**Master**):
```
sudo apt-get update
sudo apt-get -y install salt-master
hostname -I
```
Ohjeistuksessa mainitaan, että palomuuren ollessa käytössä, pitää sallia portit 4505/tcp ja 4506/tcp. Käytin tässä apuna tekoälyä (ChatGPT):
```
sudo ufw allow 4505/tcp
sudo ufw allow 4506/tcp
sudo ufw reload 
```

Tämän jälkeen asensin alikoneen (**Minion**):
```
sudo apt-get update
sudo apt-get -y install salt-minion
```

Annoin alikoneelle isäntäkoneen ip-osoitteen ja id:n:
``sudo nano /etc/salt/minion``

Käynnistin alikoneen uudelleen:
``sudo systemctl restart salt-minion.service``

Hyväksyin alikoneen avaimen (**Slave Key**):
``sudo salt-key -A``

Jostain syystä avainta ei löytynyt, joten kysyin tekoälyltä apua.

<img width="521" height="36" alt="image" src="https://github.com/user-attachments/assets/71f04162-0477-4e36-9182-048984cc53db" />

Tekoäly käski tarkistaa alikoneen statuksen:
``sudo systemctl status salt-minion``

<img width="801" height="218" alt="image" src="https://github.com/user-attachments/assets/61fb682b-d86a-4040-a22f-e2f1708af0e2" />

Kuten kuvasta huomaa, alikone ei ole vielä yhdistynyt isäntäkoneeseen.
Käynnistin alikoneen uudestaan ja tarkistin vielä kerran statuksen:
```
sudo systemctl start salt-minion
sudo systemctl status salt-minion
```

<img width="770" height="81" alt="image" src="https://github.com/user-attachments/assets/99a8d7df-ff55-4eb7-9a00-e83f6cbb86ea" />

Nyt koneet on yhdistetty, kokeillaan uudestaan avaimen hyväksymistä:
``sudo salt-key -A``


# References
Karvinen, T. 4.6.2006. Raportin kirjoittaminen. Tero Karvinen. URL: https://terokarvinen.com/2006/06/04/raportin-kirjoittaminen-4/. Accessed: 25.10.2025.

Karvinen, T. 28.3.2018. Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux. Tero Karvinen. URL: https://terokarvinen.com/2018/03/28/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/. Accessed: 25.10.2025.

Karvinen, T. 28.10.2021. Run Salt Command Locally. Tero Karvinen. URL: https://terokarvinen.com/2021/salt-run-command-locally/. Accessed: 25.10.2025.

VMware. 2025. Salt Install Guide: Linux (DEB). Saltproject.io. URL: https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/linux-deb.html. Accessed: 25.10.2025.

