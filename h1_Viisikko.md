# x) Lue ja tiivistä
## Run Salt Command Locally
- Salt-komentojen suorittaminen lokaalisti on käytännöllistä harjoituksen ja testaamisen kannalta
- Tärkeimmät tilafunktiot: **pkg**, **file**, **service**, **user** ja **cmd**
- Salt-menetelmää käytetään alikoneiden hallitsemiseen verkossa.
  
(Karvinen 28.10.2021)

**(huomio)**


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
    
**(huomio)**


## Raportin kirjoittaminen
- Muistiinpanojen tekeminen
- Pääpointit:
  - **Toistettava** = Raportin ohjeita seuratessa tulisi saada samanlainen tulos
  - **Täsmällinen** = Toimenpiteiden ja tilanteiden tarkka raportointi 
  - **Helppolukuinen** = Tekstien jäsentely ja selkeää kieltä
  - **Viittaa lähteisiin**
  - **Raportointivirheet** = Plagiointi ja väärän tiedon raportointi.
    
**(huomio)**

(Karvinen 4.6.2006)


## Salt Install Guide: Linux (DEB)
Artikkelissa ohjeistetaan hieman tarkemmin Salt-menetelmän asentamisesta Linux-koneelle.
Lyhyesti:
- Salt-projekti repositorion asennus:
- metadatan päivitys
- Salt-komponenttien asennus
- Salt-palvelujen käyttöönotto ja käynnistys

(VMware 2025)


# References
Karvinen, T. 4.6.2006. Raportin kirjoittaminen. Tero Karvinen. URL: https://terokarvinen.com/2006/06/04/raportin-kirjoittaminen-4/. Accessed: 25.10.2025.

Karvinen, T. 28.3.2018. Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux. Tero Karvinen. URL: https://terokarvinen.com/2018/03/28/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/. Accessed: 25.10.2025.

Karvinen, T. 28.10.2021. Run Salt Command Locally. Tero Karvinen. URL: https://terokarvinen.com/2021/salt-run-command-locally/. Accessed: 25.10.2025.

VMware. 2025. Salt Install Guide: Linux (DEB). Saltproject.io. URL: https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/linux-deb.html. Accessed: 25.10.2025.

