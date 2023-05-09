# Vaihtoehdot

Juho Tuovinen

Tässä raportissa kerron, kuinka asennan Saltin Apple-tietokoneeseeni ja kuinka asennan ohjelman käyttämällä Salt:ia. Lähden suorittamaan tehtävää ohjeen mukaisesti (Tero Karvinen, Infra as Code course, h5-Vaihtoehdot, https://terokarvinen.com/2023/palvelinten-hallinta-2023-kevat/#h5-vaihtoehdot).

HUOM! Tehtävää muokattu viimeiseksi PÄIVÄMÄÄRÄ. Aloitin tehtävän teon MacOS-käyttöjärjestelmällä, koska Windows-virtuaalikoneen asennuksessa oli ongelmia. Virtuaali koneen asennus kuitenkin myöhemmin onnistui, joten aloitin tehtävän alusta Windowsilla. Windowsilla tehdyt harjoitukset löytyvät alempaa.

Tietokoneen speksit, jolla suoritan toimenpiteen:
- Apple MacBook 2015
- macOs Monterey versio 12.6.4 
- Ram -muistia 16GB
- levytilaa vapaana 57,97 GB.

## Tiivistelmä

Kuinka ohjata Salt:ia Windowsilla.

- ladasessa Salt, master-koneella version täytyy olla sama tai uudempi kuin orjakoneilla
- uusimmassa versiossa toimivat parheiten
- hyväksy orjat <code> sudo salt-key -A </code>
- testaa, että orjat vastaavat <code> sudo salt '*' test.ping </code>
- ota käyttöön Salt Windows ohjelmistojen varastot
- asenna jokin ohjelmlistopaketti (ensin paikallisesti) <code> salt-call --local </code>
- ohjaa Saltia Windowsin PowerShellissä
- koska Windowsilla ei ole monipuolista paketinhallintajärjestelmää, on hyvä ladata esim. Chocolatey <code> sudo salt ug pkg.install chocolatey </code>
- Chocolateylla on yli 5000 asennettavaa pakettia
- lisää Salt minioni Windows serverille
- voit antaa minioneille rooleja


(Lähde: Karvinen, Tero: Control Windows with Salt https://terokarvinen.com/2018/control-windows-with-salt/)

## Saltin asentaminen

Tiistai 2.5.2023 Kello 16.30

Koska Windows-virtuaalikoneen asentaminen VirtualBoxissa ei onnistunut, päätin asentaa saltin Apple Macbookilleni. Syy, miksi virtuaalikone ei lähde käyntiin on toistaiseksi tuntematon.

Latasin tiedoston "salt-latest-py3-x86_64.pkg" osoitteesta https://repo.saltproject.io/osx/.

<img src="/images/kuva68.png" alt="testi" title="testi" width="60%" height="60%">

Salt asennettiin onnistuneesti.

<img src="/images/kuva69.png" alt="testi" title="testi" width="60%" height="60%">

## Ei voi kalastaa

Kello 17.10

Testaan Saltin toimivuutta paikallisesti ilman verkkoa.

    sudo salt-call --local test.ping
    
Yhteydenotto itseen onnistui.

<img src="/images/kuva70.png" alt="testi" title="testi" width="70%" height="70%">

## Hei ikkuna!

Luon tyhjän kansion "hello", käyttämällä idempotentteja komentoja.

Menin polkuun /var/tmp ja loin kansion "testikansio".
   
    sudo mkdir testikansio
    
Loin uuden tiedoston "hello" käyttämällä "file.managed"-komentoa.

    sudo salt-call --local state.single file.managed /var/tmp/testikansio/hello

Tiedoston luominen onnistui.


<img src="/images/kuva71.png" alt="testi" title="testi" width="60%" height="60%">
<img src="/images/kuva72.png" alt="testi" title="testi" width="60%" height="60%">


## Installed

Asennan Macille ohjelman Saltilla.

Latasin Micro-tekstieditorin kotihakemistoon osoitteesta https://micro-editor.github.io/.

    curl https://getmic.ro | bash
    
Micro asentui ja toimi. 

<img src="/images/kuva73.png" alt="testi" title="testi" width="60%" height="60%">

Kopioin Micro polkuun /usr/local/bin.

Nyt Micro avautuu komennolla "micro".

Seuraavaksi tarkoitukseni on luoda tila /srv/salt- kansioon, mutta sellaista ei ole, enkä voi sellaista luoda.

<img src="/images/kuva74.png" alt="testi" title="testi" width="60%" height="60%">


Raportti tulee jatkumaan.

# Windowsin asennus

Koska aikaisempi windows virtuaalikone ei toiminut, etsin toisen uudesta lähteestä. Asensin Windows ISO- tiedoston lähteestä https://www.microsoft.com/fi-fi/software-download/windows10ISO. Se toimi.



## Asenna Salt Windowsille

Maanantai 08.05.2023 kello 21
latasin salt minionin windowsille https://repo.saltproject.io/salt/py3/windows/latest/Salt-Minion-3006.1-Py3-AMD64-Setup.exe

Aloitin asentamisen ja tarkistin masterin (eli Macbook:in) IP-osoitteen ja annoin sen asennusvaiheessa.

<img src="/images/kuva75.png" alt="testi" title="testi" width="60%" height="60%">

Katsoin Salt:in sivulta ohjeita (https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/macos.html) jakäytin alla olevaa komentoa, jonka tilalle laitoin minionin ip osoitteen ja master koneen ip osoitteen.

    sudo salt-config -i yourminionname -m yoursaltmaster

Sen jälkeen hyväksytin avaimet.

    sudo salt-key -A 
    
<img src="/images/kuva76.png" alt="testi" title="testi" width="60%" height="60%">

Ja testasin yhteyttä pingaamalla konetta

    sudo salt Minion1 test.ping

<img src="/images/kuva77.png" alt="testi" title="testi" width="60%" height="60%">
 
 Kone vastasi jayhteys saatiin.

## Ei voi kalastaa

käytin apuna danielin raporttia https://github.com/danielz95/palvelintenhallinta-2023/blob/main/h5-Salt%20Windows.md ja https://github.com/RAV64/palvelinten-hallinta/blob/main/h5.md

Käytin saltia paikallisesti komennoilla:

    salt-call --local test.ping
    
    salt-call --local state.single cmd.run whoami
    
<img src="/images/kuva78.png" alt="testi" title="testi" width="60%" height="60%">
 
Molemmat komennot onnistuivat ja salt toimi paikallisesti .
    
    
## Hei ikkuna

Loink kansion "win10" polkuun /etc/salt master-konellani.

    salt % sudo mkdir -p /etc/salt/win10
    
Loin tekstitiedoston kansioon.

    sudo touch hello.txt
   
Luon init.sls-tilatiedoston.
    
    sudo pico init.sls
    
  Ja lisäsin komennon, joka asentaa tiedoston kansioon C:/Users/Public/Documents orjakoneella.

```
C:/Users/Public/Documents/hello.txt:
  file.managed:
    - source: "salt://win10/hello.txt"
```

Ajoin tilan, mutta se ei oitminut.

    sudo salt Minion1 state.apply win10
    
<img src="/images/kuva79.png" alt="testi" title="testi" width="60%" height="60%">
    

N myöskään saa luotua /srv/salt kansiota master-koneelleni. Saan alla olevan virheilmoituksen.

    mkdir: /srv: Read-only file system
    
Kokeilin muokata konfiguraatiotiedostoaa /etc/salt/minion  
 ``` 
file_roots:
  base:
    - /etc/salt
```
     
Lisäsin varmuuden vuoksi master-konfiguraatiotedoston, jonne lisäsin saman YAML-tekstin.


Kokeilin hello.txt- tiedoston asennusta uudelleen. Tiedosto ei silti asentunut.

```
Minion1:
Data failed to compile:
No matching sls found for 'win10' in env "base'
```

Tiistai 09.05.2023 Kello 12.25

Päätin kokeilla toista lähestymistapaa ja tilan sijaan päädyin komentoon:

    sudo salt '*' state.single file.managed C:/Users/Public/Documents/hello.txt
    
 Komento suorittaa saman asian kuin kaikaisemmin kokeillun tilan toiminto. Komento luo hello.txt tiedoston hallittavan koneen polkuun C:/Users/Public/Documents/. 

<img src="/images/kuva80.png" alt="testi" title="testi" width="60%" height="60%">

## Installed

Ensiksi tarkistin ettei vlc ole asennettu


    



## Lähteet


Karvinen, Tero: Oppitunnit 2023-04-27, palvelinten hallinta, h5-Vaihtoehdot, https://terokarvinen.com/2023/palvelinten-hallinta-2023-kevat/#h5-vaihtoehdot)

Micro, https://micro-editor.github.io/

Salt Project Package Repo, https://repo.saltproject.io/osx/

Get a Windows 11 development environment, https://developer.microsoft.com/en-us/windows/downloads/virtual-machines/

https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/windows.html

https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/macos.html

https://github.com/danielz95/palvelintenhallinta-2023/blob/main/h5-Salt%20Windows.md

https://github.com/RAV64/palvelinten-hallinta/blob/main/h5.md
