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


# Lähteet
Karvinen, T. 3.4.2024. Hello Salt Infra-as-Code. Tero Karvinen. Luettavissa: https://terokarvinen.com/2024/hello-salt-infra-as-code/. Luettu: 3.11.2025.

VMware. 18.9.2025. The Top File. Saltproject.io. Luettavissa: https://docs.saltproject.io/en/latest/ref/states/top.html. Luettu: 3.11.2025.

VMware. 2025. Salt overview. Saltproject.io. Luettavissa: https://docs.saltproject.io/salt/user-guide/en/latest/topics/overview.html#rules-of-yaml. Luettu: 3.11.2025.
