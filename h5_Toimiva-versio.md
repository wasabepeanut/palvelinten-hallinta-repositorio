# Esivalmistelut
- Käyttöjärjestelmä: Windows 11
- Virtualisointiohjelma: VirtualBox
- Virtuaalikoneen käyttöjärjestelmä: Debian 13 Trixie
- Versionhallinta: GitHub


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
- Hakee ajankohtaiset muutokset etärepositoriosta  (**remote repository**) ja integroi ne omaan nykyiseen haaraan (**branch**).

**git push**
- Päivittää haarat, jotka sijaitsevat etärepositoriossa puskemalla kaikki tarvittava data

(Git 17.11.2025)



## Varaston "Suolax"-historia
Tarkastellaan hieman Teron luomaa [Git-repositoriota](https://github.com/terokarvinen/suolax/commits/main/) suoraan GitHubista.

- Kaikkiaan repositorioon oli tehty 8 committia, kaikki samana päivänä 10.4.2024
- Aluksi repositorioon lisättiin README.md ja GNU General Public - lisenssi
- Testattiin "Hello World" state, joka luo tiedoston hakemistoon hello/tmp/hello-salt-from-git
- Luotiin uusi state, joka asentaa Teron lempi paketin (tree)
- Lisättiin uusia paketteja asennettavaksi sekä lisättiin myös top.sls-tiedosto
- Lopuksi lisättiin README.md tiedostoon ohjeita.



# a) Online
Tässä tehtävässä luodaan repositorio GitHubiin, jonka nimessä ja sisältötekstissä lukee sana "snow". Repositorion luonnin aikana lisäsin README.md ja lisenssin GNU General Public License 3.0.

<img width="768" height="1107" alt="image" src="https://github.com/user-attachments/assets/150f194e-96e4-41a6-aff8-24da88f3d395" />

Lopuksi painoin luo repositorio.

<img width="1240" height="563" alt="image" src="https://github.com/user-attachments/assets/3690c5e3-8b61-4329-8bf8-2970cf74e781" />

❄️❄️❄️



# b) Dolly
Tässä tehtävässä testataan juuri luomaamme repositoriota. 
(Ennen tätä olin myös lisännyt julkisen SSH-avaimeni GitHub käyttäjälleni ja muuttanut kansion omistajaa userille).

```
ssh-keygen
# kopioi julkinen avain GitHub profiiliin
cat .ssh/id_ed25519.pub

# muutetaan kansion ja tiedostojen omistajaa
sudo chown -R $USER:$USER /home/duy/coding
```

Kloonasin aluksi repositorion virtuaalikoneelleni komennolla `git clone git@github.com:wasabepeanut/snowtest_repository.git` (SSH linkki).

Tämän jälkeen siirryin README tiedostoon, kirjoitin sinne jotain ja suoritin seuraavat komennot:

```
git add .
git commit
git pull
git push
```

<img width="438" height="230" alt="image" src="https://github.com/user-attachments/assets/2481ac50-dca9-445c-a4e2-dc6a89637d32" />

Kuten kuvassa näkyy, puskeminen onnistui!



# c) Doh!

```
ls
rm README.md
ls
```

O-ou. Vahingossa poistin README.md tiedostoni. 😲

```
git reset --hard
ls
```

<img width="601" height="196" alt="image" src="https://github.com/user-attachments/assets/98c06634-06c6-49a8-b4d0-492d482d3f79" />

Huh.😌



# d) Tukki

# Lähteet
Chacon, S. & Straub, B. 2024. Pro Git book. 1.3: Getting Started - What is Git?. Git. Luettavissa: https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F. Luettu: 23.11.2025.

Git. 17.11.2025. git-add. Git. Luettavissa: https://git-scm.com/docs/git-add. Luettu: 23.11.2025.

Git. 17.11.2025. git-commit. Git. Luettavissa: https://git-scm.com/docs/git-commit. Luettu: 23.11.2025.

Git. 17.11.2025. git-pull. Git. Luettavissa: https://git-scm.com/docs/git-pull. Luettu: 23.11.2025.

Git. 17.11.2025. git-push. Git. Luettavissa: https://git-scm.com/docs/git-push. Luettu: 23.11.2025.
