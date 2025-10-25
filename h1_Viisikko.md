# x) Lue ja tiivistä
## Run Salt Command Locally
- Tärkeimmät tilafunktiot: pkg, file, service, user ja cmd
- Salt-menetelmää käytetään alikoneiden hallitsemiseen verkossa. (Karvinen 28.10.2021)

**(huomio)**

## Salt Quickstart
- Salt-menetelmä voidaan pikaisesti aloittaa seuraavin vaihein:
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

# References
Karvinen, T. 28.3.2018. Salt Quickstart – Salt Stack Master and Slave on Ubuntu Linux. Tero Karvinen. URL: https://terokarvinen.com/2018/03/28/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/. Accessed: 25.10.2025.

Karvinen, T. 28.10.2021. Run Salt Command Locally. Tero Karvinen. URL: https://terokarvinen.com/2021/salt-run-command-locally/. Accessed: 25.10.2025.

