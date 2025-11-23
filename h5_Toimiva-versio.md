# Esivalmistelut
- Käyttöjärjestelmä: Windows 11
- Virtualisointiohjelma: VirtualBox
- Virtuaalikoneen käyttöjärjestelmä: Debian 13 Trixie



# x) Lue ja tiivistä
## Pro Git

- Git versionhallintajärjestelmä eroaa muista työkaluista eniten siten, millä tavalla se näkee datansa
- Muille työkaluille data on ikään kuin lista muutoksista, Gitille se on enemmän kokoelma kuvakaappauksista
- Melkein kaikki toimenpiteet tapahtuvat lokaalisti Gitillä
- Gittiin on hyvin vaikeaa tehdä muutoksia, ilman että ne näkyisivät jossain.
- Gitin perus workflow:
  1. Tiedostojen muokkaus työhakemistossa
  2. Muutosten valikoiminen (staging)
  3. Muutosten hyväksyminen (commit)

(Chacon, S. & Straub, B. 2024)


## git komentoja

**git add**
- Lisätään uudet tai muutetut tiedostot valmistelualueeseen (**staging area**).

**git commit**
- Luodaan uusi "commit", joka sisältää tämänhetkisen valmiusalueen sisällön ja ikään kuin hyväksyy nämä muutokset

**git pull**
- Hakee ajankohtaiset muutokset etärepositoriosta ja integroi ne omaan nykyiseen haaraan.

**git push**
- Päivittää haarat, jotka sijaitsevat etärepositoriossa (**remote repository**) puskemalla kaikki tarvittava data


# Lähteet
Chacon, S. & Straub, B. 2024. Pro Git book. 1.3: Getting Started - What is Git?. Git. Luettavissa: https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F. Luettu: 23.11.2025.
