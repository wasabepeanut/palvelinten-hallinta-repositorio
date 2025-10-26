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

Nyt kun minulla oli repositorio, päätin seurata Teron [Quickstart-ohjetta](https://terokarvinen.com/2018/03/28/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/).

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

`sudo systemctl status salt-minion`

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

<img width="447" height="127" alt="image" src="https://github.com/user-attachments/assets/9636bdad-59f5-404b-b9ee-e5f83eaad522" />

Tarkistetaan vielä saadaanko alikoneelta vastaus:

``sudo salt '*' cmd.run 'whoami'``

<img width="546" height="52" alt="image" src="https://github.com/user-attachments/assets/34f0bb96-0a07-4489-804e-117c22baa9d4" />

Näyttää toimivan!


# c) Viisi tärkeintä
Tässä tehtävässä analysoin Saltin viisi tärkeintä tilafunktiota testaamalla niiden käyttöä komentokehotteella sekä käyttämällä hyödyksi [Teron ohjeita](https://terokarvinen.com/2021/salt-run-command-locally/) ja tekoälyä.

## cmd
Tilafunktio on melko itseselitteinen. Sillä pystyy suorittamaan komentoja komentokehotteessa.

``sudo salt '*' cmd.run 'echo "Hello master"'``

  <img width="668" height="55" alt="image" src="https://github.com/user-attachments/assets/56372003-c421-4535-ba29-b400f9769ae1" />

Kuvassa näkyy, että alikone "duy" tulostaa tekstin "Hello master". 
Tässä tapauksessa, isäntäkone antaa käskyn alikoneille suorittamaan komennon 'echo "Hello master"'.
Jos alikoneita olisi useampi, tämä komento pistäisi kaikki alikoneet suorittamaan saman komennon.


## pkg
Tilafunktiolla pystyy hallitsemaan asennettuja ohjelmia.

``sudo salt-call --local -l info state.single pkg.installed tree`` 

<img width="790" height="303" alt="image" src="https://github.com/user-attachments/assets/77f6a682-ea8b-43bd-ac6c-f9ef0a9d3ab5" />

Kuvassa tilafunktio **pkg.installed** aloittaa joidenkin tiedostojen/pakettien asennuksen. 
Tässä tapauksessa, paikallinen alikone asentaa paketin "tree", kuten kuvan tulostuksessa näkyy.


## file
Tilafunktiolla pystyy hallitsemaan nimensä mukaisesti tiedostoja.

``sudo salt-call --local -l info state.single file.absent /tmp/hellotero``

<img width="482" height="326" alt="image" src="https://github.com/user-attachments/assets/c221095c-ff33-4288-9326-5f28c6b40b0d" />

Kuvassa tilafunktio **file.absent** tarkistaa minulle onko kyseinen tiedosto "/tmp/hellotero" puutteellinen. Tulos on kyllä koska minulla ei ole kyseistä tiedostoa tuossa polussa.


## service
Tällä tilafunktiolla pystyy tarkistamaan palveluiden tilaa.

``sudo salt-call --local -l info state.single service.running apache2 enable=True``

<img width="690" height="430" alt="image" src="https://github.com/user-attachments/assets/d4f3a744-1687-4c18-97ad-0b755aa53b4f" />

Funktio **service.running** tarkistaa onko "apache2" käynnissä tällä hetkellä, sekä käynnistää sen Saltin kautta jos ei. Tässä tapauksessa niin ei kuitenkaan käy, koska kyseistä palvelua ei löydy minulta. 

## user
Tällä tilafunktiolla pystytään hallita käyttäjiä.

``sudo salt-call --local -l info state.single user.present duyp``

<img width="527" height="342" alt="image" src="https://github.com/user-attachments/assets/5883f5f9-b504-43eb-ae1e-a6b53cca9621" />

Tässä funktiossa **user.present** tarkistaa onko käyttäjää "duyp" olemassa. Tulos on kyllä, koska se on minun oma käyttäjäni. 

# d) Idempotentti
En aluksi tiennyt mitä idempotenssi tarkoittaa, joten otin ihan aluksi selvää siitä.
Idempotenssi lyhyesti kuvattuna on ominaisuus, jossa tiettyä toimenpidettä voidaan toistaa moneen kertaan, eikä lopputulos muutu (LoadFocus 2025). 

Testataan nyt muutamia komentoja: 

kuva1
<img width="508" height="322" alt="image" src="https://github.com/user-attachments/assets/7570fa3e-e0b3-40c6-a37f-59c5a45f6cb9" />

kuva2
<img width="503" height="320" alt="image" src="https://github.com/user-attachments/assets/17017f58-7d5e-491d-9d95-d81868da0038" />


``sudo salt-call --local -l info state.single user.present duyp``



# References
Karvinen, T. 4.6.2006. Raportin kirjoittaminen. Tero Karvinen. URL: https://terokarvinen.com/2006/06/04/raportin-kirjoittaminen-4/. Accessed: 25.10.2025.

Karvinen, T. 28.3.2018. Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux. Tero Karvinen. URL: https://terokarvinen.com/2018/03/28/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/. Accessed: 25.10.2025.

Karvinen, T. 28.10.2021. Run Salt Command Locally. Tero Karvinen. URL: https://terokarvinen.com/2021/salt-run-command-locally/. Accessed: 25.10.2025.

LoadFocus. 2025. Mikä on Idempotenssi?. LoadFocus. URL: https://loadfocus.com/fi-fi/glossary/what-is-idempotency. Accessed. 27.10.2025.

VMware. 2025. Salt Install Guide: Linux (DEB). Saltproject.io. URL: https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/linux-deb.html. Accessed: 25.10.2025.

