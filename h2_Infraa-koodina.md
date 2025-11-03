# Esivalmistelut
- Käyttöjärjestelmä: Windows 11
- Virtualisointiohjelma: VirtualBox
- Virtuaalikoneen käyttöjärjestelmä: Debian 13 Trixie

# x) Lue ja tiivistä
## Hello Salt Infra-as-Code
Artikkelissa opetetaan tekemään "Hello World" tyyppinen ohjelma Saltilla.

Lyhyesti:
- Asennetaan Salt
- luodaan "hello" moduulille kansio
- kirjoitetaan skripti tiedostoon
- suoritetaan Salt-komento

(Karvinen 3.4.2024)

## Salt overview

**YAML:**
- oletus renderöijä monissa Saltin käyttämissä tiedostoissa
- renderin tehtävänä on kääntää YAML dataa Python dataksi Saltia varten
- datan luomista varten on noudatettava sen perussääntöjä
- koostuu kolmesta perus elementtityypistä; skalaareista, listoista ja sanakirjoista

(VMware 2025)

## The top file

- **Top file** - käsite kuvastaa tiedostoa, joka sisältää koneiden muodostamien ryhmien väliset mäppäykset
- Oletuksena top file - tiedostot ovat nimeltään **top.sls**, jotta ne sijaitsisivat aina päällimmäisenä hakemiston hierarkiassa, jotka ovat nimeltään **state tree**.
- Top file koostuu kolmesta komponentista: ympäristö, kohde ja tilatiedostoista
- Ympäristö sisältää kohteita, kohde sisältää tilatiedostoja.

(VMware 18.9.2025)


# a) Hei infrakoodi!
Tässä tehtävässä käytin apunani [Teron ohjeita](https://terokarvinen.com/2024/hello-salt-infra-as-code/).

Asensin ensin Saltin sekä micro editorin
```
sudo apt-get update
sudo apt-get -y install salt-minion
sudo apt-get -y install micro
export EDITOR=micro
```

Loin "hello" moduulille kansion sekä loin ja lisäsin skriptin "init.sls"-tiedostoon.
```
sudo mkdir -p /srv/salt/hello/
cd /srv/salt/hello/
sudoedit init.sls
```

<img width="451" height="378" alt="image" src="https://github.com/user-attachments/assets/ae05d87a-56ea-488e-a334-2b000632dd62" />

Tarkistetaan vielä että tiedosto löytyy

<img width="388" height="42" alt="image" src="https://github.com/user-attachments/assets/07611a5f-cfe8-4a6d-834b-47e594fd218a" />


# b) Toppping

```
cd /srv/salt/
sudoedit top.sls
```

<img width="177" height="83" alt="image" src="https://github.com/user-attachments/assets/af065260-ed1a-4274-a87d-2d71c55d1c87" />

Ajetaan kaikki tilat kerralla, suorittamalla top file.

`sudo salt-call --local state.apply`

<img width="536" height="476" alt="image" src="https://github.com/user-attachments/assets/4582b811-51f9-4eb0-894e-8d73a03d3676" />

Näyttäisi toimivan.


# c) Viisikko tiedostossa
Tässä tehtävässä käytin apuna tekoälyä ja cmd:n manuaaleja

Loin aluksi kaikille omat kansiot:
```
sudo mkdir -p /srv/salt/hellopkg
sudo mkdir -p /srv/salt/helloservice
sudo mkdir -p /srv/salt/hellouser
sudo mkdir -p /srv/salt/hellocmd
```

<img width="697" height="36" alt="image" src="https://github.com/user-attachments/assets/ee55b491-604a-4059-a89b-1f9ea3e48c83" />

Tämän jälkeen loin jokaiseen kansion omat init.sls tiedostot skriptineen.

**pkg**

<img width="211" height="121" alt="image" src="https://github.com/user-attachments/assets/29da7cd6-106d-490a-b5d3-4195dad85c01" />

**service**

<img width="222" height="103" alt="image" src="https://github.com/user-attachments/assets/d8ed0c6f-246b-4d3d-b2f5-fc713c928908" />

**cmd**

<img width="316" height="92" alt="image" src="https://github.com/user-attachments/assets/fe46c02d-6398-4dfe-97a1-260865b459b3" />

**user**

<img width="272" height="127" alt="image" src="https://github.com/user-attachments/assets/8b7cd2a6-e94f-4fb0-b549-64ded93ce8a6" />

Kokeillaan muutaman tilafunktion suorittamista:

`sudo salt-call --local state.apply hellocmd`

<img width="721" height="477" alt="image" src="https://github.com/user-attachments/assets/1e26addd-723e-44e4-b086-435dd0890e99" />

`sudo salt-call --local state.apply helloservice`

<img width="495" height="341" alt="image" src="https://github.com/user-attachments/assets/4395fa03-9f20-49a9-954e-3a1a8a5208fe" />

Skriptit toimivat!

# d) Sls-tiedosto
Hyödynnän aikaisemmin luomaani top fileä ja lisään sinne esimerkin vuoksi tilafunktiot **user** ja **pkg**.

```
cd /srv/salt
sudoedit top.sls
```

<img width="182" height="122" alt="image" src="https://github.com/user-attachments/assets/c09768d3-4346-4258-be31-bb3a4bc2874b" />

Nyt suoritetaan top file

`sudo salt-call --local state.apply`

<img width="661" height="277" alt="image" src="https://github.com/user-attachments/assets/c3c25f90-5226-400c-9f95-87562d6f55ac" />

<img width="362" height="532" alt="image" src="https://github.com/user-attachments/assets/c08acedb-f5f2-4154-b6aa-3ad6da36d985" />

<img width="257" height="138" alt="image" src="https://github.com/user-attachments/assets/45a992a9-4e7e-4112-934a-d13d065b177b" />

Kaksi uutta muutosta, näyttäisi toimivan.
Testataan vielä onko sls-tiedosto idempotentti, suorittamalla komento uudelleen.

<img width="292" height="135" alt="image" src="https://github.com/user-attachments/assets/1d69af3d-4b6e-4bf6-876c-d0f271e5e808" />

Monen suorituksen jälkeen ei näyttäisi olevan muutoksia, eli toimii.

# Lähteet
Karvinen, T. 3.4.2024. Hello Salt Infra-as-Code. Tero Karvinen. Luettavissa: https://terokarvinen.com/2024/hello-salt-infra-as-code/. Luettu: 3.11.2025.

VMware. 18.9.2025. The Top File. Saltproject.io. Luettavissa: https://docs.saltproject.io/en/latest/ref/states/top.html. Luettu: 3.11.2025.

VMware. 2025. Salt overview. Saltproject.io. Luettavissa: https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml. Luettu: 3.11.2025.
