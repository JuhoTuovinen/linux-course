# Komennus

Juho Tuovinen

Tässä raportissa demonstroin kuinka luon omat bash ja python -skriptit ja automatisoin skriptin asentamisen orjakoneille. Lisäksi asennan yhden binäärin ohjelman (Micro) Saltilla orjakoneille. Lähden toteuttamaan tehtävää ohjeiden mukaisesti (Karvinen, Tero: Oppitunnit 2023-04-20, Palvelinten hallinta, h4-Komennus, https://terokarvinen.com/2023/palvelinten-hallinta-2023-kevat/#h4-komennus).

Tietokoneen speksit, jolla suoritan toimenpiteen:
- Apple MacBook 2015
- macOs Monterey versio 12.6.4 
- Ram -muistia 16GB
- levytilaa/muistia vapaana 26,91 GB.

Ohjelmistot
- VirtualBox Version 7.0.6 r155176 (Qt5.15.2)
- Vagrant 2.3.4
- VM OS: Debian Bullseye

## hello.sh

Sunnuntai 23.4.2023 kello 20:20

Loin bash-skriptin nano-tekstieditorilla, joka tulostaa ajettaessa tekstin "shine".

```
#!/bin/bash
echo 'shine'
```

Testiksi ajoin skriptin. Skripti tulosti sanan "shine", eli skripti toimi oletetusti.

    bash skripti


<img src="/images/kuva50.png" alt="testi" title="testi" width="70%" height="70%">

Kopioin skriptin vakiohakemistoon /usr/locla/bin. Annoin kaikille käyttäjille oikeudet ajaa skriptin. Sen jälkeen skriptin voi ajaa pelkällä skriptin nimellä ilman "bash" komentoa.

    sudo chmod ugo+x skripti

<img src="/images/kuva51.png" alt="testi" title="testi" width="70%" height="70%">

Ajoin vileä testiksi skiprtin.

<img src="/images/kuva52.png" alt="testi" title="testi" width="70%" height="70%">


Loin kanasiot "salt" ja "script" polkuun /srv/salt/script, jonne lisään init.sls tilatiedoston. Tiedostoon lisään käskyt, joilla saadaan skripti orjakoneille.

```
/usr/local/bin/skripti:
  file.managed:
    - mode: "0755"
    - source: "salt://script/skripti"   
``` 
Kopioin myös skriptin /usr/local/bin polusta /srv/salt/script -kansioon. Tarkistin vielä, että kaikilla on oikeudet ajaa skripti.

    ls -l
    
<img src="/images/kuva53.png" alt="testi" title="testi" width="70%" height="70%">
 
 Lähetin tilan orjakoneille.
 
     sudo salt '*' state.apply script
     
 Kirjauduin SSH-yhteydellä orjakoneelle "t001", jolle lähetin tilan ja annoin käskyn "skripti", jolloin skripti ajettiin onnistuneesti ja kone tulosti sanan "shine". Skripti saatiin siis toimimaan halutunlaisesti ja lisäksi skriptin asennus suoritettin automaattisesti orjakoneille onnistuneesti.
 
<img src="/images/kuva54.png" alt="testi" title="testi" width="70%" height="70%">
<img src="/images/kuva55.png" alt="testi" title="testi" width="70%" height="70%">
 
 ## Python
 
 Loin "hello.py" nimisen tiedoston Nano-tekstieditorilla, jonne lisäsin python-koodia, joka tulostaa ajettaessa: "hello world". 
 
     print("hello world")
     
 Ajoin testiksi skriptin, ja sain tulosteen "hello world", eli skrpiti toimi.
 
<img src="/images/kuva56.png" alt="testi" title="testi" width="70%" height="70%">
 
 Siirsin skriptin /usr/local/bin kansioon, niinkuin aikaisemmassaa vaiheessa ja annoin oikeudet kaikille käyttäjille ajaa skriptin.
 
     sudo chmod ugo+x hello.py
     
 <img src="/images/kuva57.png" alt="testi" title="testi" width="70%" height="70%">
 
 Loin uuden kansion "python-script" polkuun /srv/salt ja lisäsin sinne samat käskyt joilla asennetaan skripti orjakoneille.
 
 ```
 /usr/local/bin/hello.py:
  file.managed:
    - mode: "0755"
    - source: "salt://python-script/hello.py"
```

Tarkistin vielä että oikeudet pysyivät. 

<img src="/images/kuva58.png" alt="testi" title="testi" width="70%" height="70%">

Ajoin tilan orjakoneille.

<img src="/images/kuva59.png" alt="testi" title="testi" width="70%" height="70%">

Kello 21:10 

Kirjauduin "t001" orjakoneellee testatakseni skriptiä ja sain seuraavanlaisen virheilmoituksen.

<img src="/images/kuva60.png" alt="testi" title="testi" width="70%" height="70%">

Tarkistin vielä, että skripti kuitenkin löytyy, eli se oli tullut orjakoneille perille, mutta ei aja sitä oikein.

<img src="/images/kuva61.png" alt="testi" title="testi" width="70%" height="70%">

Lisäsin shebang-rivin poolkujen /usr/local/bin/hello.py ja /srv/salt/python-script/hello.py python-skripteihin.

https://stackoverflow.com/questions/6908143/should-i-put-shebang-in-python-scripts-and-what-form-should-it-take

    #!/usr/bin/env python 
    
Lähetän tilan uudelleen orjakoneille ja testaan uudestaan. Ei toiminut.

<img src="/images/kuva62.png" alt="testi" title="testi" width="70%" height="70%">


Sain ohjelman toimimaan orjakoneella. Vika oli siinä, että minulla oli pääritetty vamhempi versio python-ohjelmasta ajamaan komento, eikä uusin python3. Liäsin shebang-riviin "python3", jolloin skripti toimi

    #!/usr/bin/env python3

<img src="/images/kuva63.png" alt="testi" title="testi" width="70%" height="70%">

# Yhden binäärin ohjelma

Aion asentaa Micro-tekstieditorin ensin käsin "tmaster"-konelle ja sen jälkeen automatisoida asennuksen orjakoneille.

Latasin Micron osoitteesta: https://github.com/zyedidia/micro/releases/tag/v2.0.11. Valitsin iedsoton nimeltä: "micro-2.0.11-linux64-static.tar.gz".

Asensin Micron komennolla:

    wget https://github.com/zyedidia/micro/releases/download/v2.0.11/micro-2.0.11-linux64-static.tar.gz
    
Purin tar-tiedoston.

    tar -xf micro-2.0.11-linux64-static.tar.gz 
    
Testasin, että Micro toimii, ja sen jälkeen kopioin sen polkuun /usr/local/bin.

<img src="/images/kuva64.png" alt="testi" title="testi" width="70%" height="70%">

Sen jälkee micro aukeaa komennolla "micro".

Loin /srv/salt polkuun uuden kansion "micro". Kopioin mciron ja tarkistin oikeudet.

<img src="/images/kuva65.png" alt="testi" title="testi" width="70%" height="70%">

Loin kansioon init.sls tilatiedoston ja lisäsin komennot, joilla asennus tapahtuu orjakoneille.

```
/usr/local/bin/micro:
  file.managed:
    - mode: "0755"
    - source: "salt://micro/micro"
```


Testaan tilaa t001-orjakoneella.

    sudo salt "t001" state.apply micro
    
<img src="/images/kuva66.png" alt="testi" title="testi" width="70%" height="70%">


Otin SSH-yhteyden t001-koneeseen. Komennolla "micro" tekstieditori Micro aukeaa onnistuneesti. Asennuksen automatiosointi onnistui ja yhden binäärin ohjelma on saatu onnistuneesti asennettua orjakoneelle.

<img src="/images/kuva67.png" alt="testi" title="testi" width="70%" height="70%">


## Lähteet


Karvinen, Tero: Oppitunnit 2023-04-20, palvelinten hallinta, h4-Komennot (https://terokarvinen.com/2023/palvelinten-hallinta-2023-kevat/#h4-komennus)

Stack Overflow, Should I put #! (shebang) in Python scripts, and what form should it take? (https://stackoverflow.com/questions/6908143/should-i-put-shebang-in-python-scripts-and-what-form-should-it-take)

GitHub, zyedidia, micro (https://github.com/zyedidia/micro/releases/tag/v2.0.11)



