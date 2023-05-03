# Vaihtoehdot

Juho Tuovinen

Tässä raportissa kerron, kuinka asennan Saltin Apple-tietokoneeseeni ja kuinka asennan ohjelman käyttämällä Salt:ia. Lähden suorittamaan tehtävää ohjeen mukaisesti (Tero Karvinen, Infra as Code course, h5-Vaihtoehdot, https://terokarvinen.com/2023/palvelinten-hallinta-2023-kevat/#h5-vaihtoehdot)

Tietokoneen speksit, jolla suoritan toimenpiteen:
- Apple MacBook 2015
- macOs Monterey versio 12.6.4 
- Ram -muistia 16GB
- levytilaa vapaana 57,97 GB.

## Tiivistelmä

Kuinka ohjata Salt:ia Windowsilla.

- lataa Salt, master-koneella version täytyy olla sama tai uudempi kuin orjakoneilla
- hyväksy orjat
- testaa, että orjat vastaavat
- ota käyttöön Salt Windows ohjelmistojen varastot
- asenna jokin ohjelmlistopaketti
- ohjaa Saltia Windowsin PowerShellissä
- Chocolateylla on yli 5000 asennettavaa pakettia
- lisää Salt minioni Windows serverille
- anna minioneille rooleja


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

<img src="/images/kuva70.png" alt="testi" title="testi" width="60%" height="60%">

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


## Lähteet


Karvinen, Tero: Oppitunnit 2023-04-27, palvelinten hallinta, h5-Vaihtoehdot, https://terokarvinen.com/2023/palvelinten-hallinta-2023-kevat/#h5-vaihtoehdot)

Micro, https://micro-editor.github.io/

Salt Project Package Repo, https://repo.saltproject.io/osx/

Get a Windows 11 development environment, https://developer.microsoft.com/en-us/windows/downloads/virtual-machines/

