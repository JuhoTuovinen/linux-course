# Vaihtoehdot

Juho Tuovinen

Tässä raportissa esitän 

Tietokoneen speksit, jolla suoritan toimenpiteen:
- Apple MacBook 2015
- macOs Monterey versio 12.6.4 
- Ram -muistia 16GB
- levytilaa vapaana 57,97 GB.

## Tiivistelmä

Kuinka ohjata Salt:ia Windowsilla.



(Lähde: Karvinen, Tero: Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port https://terokarvinen.com/2018/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=salt%20ssh)

## Saltin asentaminen macos

Tiistai 2.5.2023 Kello 16.30

Koska Windows-virtuaalikoneen asentaminen virtuaalibobiin ei onnistunut, päätin asentaa saltin Apple Macbookilleni. Syy, miksi virtuaalikone ei lähde käyntiin on tuntematon.

Latasin saltin "salt-latest-py3-x86_64.pkg" macille osoitteesta https://repo.saltproject.io/osx/.

<img src="/images/kuva68.png" alt="testi" title="testi" width="40%" height="40%">

Salt asennettiin onnistuneesti

<img src="/images/kuva69.png" alt="testi" title="testi" width="40%" height="40%">

## Ei voi kalastaa

Kello 17.10

Testaan Saltin toimivuutta paikallisesti ilman verkkoa.

testatin toimivuutta

    sudo salt-call --local test.ping
    
Onnistui

<img src="/images/kuva70.png" alt="testi" title="testi" width="40%" height="40%">

## Hei ikkuna!

Luon tyhjän kansion "hello" käyttämällä indempotenttia komentoa file.managed.

Menin polkuun /ver/tmp ja loin knasion testikansio
   
    sudo mkdir testikansio
    
loin uuden tiedoston "hello"

    sudo salt-call --local state.single file.managed /var/tmp/testikansio/hello

tiedoston luominen onnistui


<img src="/images/kuva71.png" alt="testi" title="testi" width="40%" height="40%">
<img src="/images/kuva72.png" alt="testi" title="testi" width="40%" height="40%">


## Installed

Asennan macille ohjelman Saltilla.

latasin micron kotihakemistoon osoitteesta https://micro-editor.github.io/ komennolla

    curl https://getmic.ro | bash
    
Micro asentui ja toimi. 

<img src="/images/kuva73.png" alt="testi" title="testi" width="40%" height="40%">

kopioin micro polkuun /usr/local/bin

nyt micro aukee komennolla "micro"

Seuraavaksi tarkoitukseni oli luoda tila /srv/salt- kansioon, mutta sellaista ei ole, enkä voi sellaista luoda.

<img src="/images/kuva74.png" alt="testi" title="testi" width="40%" height="40%">
    


