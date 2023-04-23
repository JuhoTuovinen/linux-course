# Komennus

Juho Tuovinen

Tässä raportissa demonstroin Gitin käyttöä GitHub-palvelun kanssa. Aion luoda uuden varaston GitHubiin ja lisätä, muokata ja poistaa sieltä tiedostoja käyttäen Git:iä. Lähden toteuttamaan tehtävää ohjeiden mukaisesti (Karvinen, Tero: Oppitunnit 2023-04-13, Palvelinten hallinta, h4-Komennus, https://terokarvinen.com/2023/palvelinten-hallinta-2023-kevat/#h4-komennus).

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

Su 20.20

Loin bash skriptin nano-tekstieditorilla joka tulostaa shine

    nano skripti

```
#!/bin/bash
echo 'shine'
```

Testikti ajoin skriptin. Skripti tulosti sanan shine niinkuin pitää.


<img src="/images/kuva50.png" alt="testi" title="testi" width="70%" height="70%">

Kopioin skriptin polkuun /usr/locla/bin koska se on kaikille. Annoin kaikille käyttäjille oikeudet ajaa skriptin.

    sudo chmod ugo+x skripti

<img src="/images/kuva51.png" alt="testi" title="testi" width="70%" height="70%">

Ajoin vileä testiksi skiprtin.

<img src="/images/kuva51.png" alt="testi" title="testi" width="70%" height="70%">


Loin kanasiot salt ja script polkuun /srv/salt/script, jonne lisään init.sls tiedoston

```
/usr/local/bin/skripti:
  file.managed:
    - mode: "0755"
    - source: "salt://script/skripti"   
``` 
Kopioin myös skriptin /usr/local/bin polusta samaan kansioon. Tarkistan viel, että kaikilla on oikeudet ajaa skripti.

    ls -l
    
<img src="/images/kuva53.png" alt="testi" title="testi" width="70%" height="70%">
 
 Lähetin tilan orjakoneille
 
     sudo salt '*' state.apply script
     
 Kirjauduin ssh yhteydelä orjakoneelle, jolle lähetin tilan ja annoin käskyn "skripti", jolloin skripti ajettiin onnistuneesti ja kone tulosti sanan shine.
 
<img src="/images/kuva54.png" alt="testi" title="testi" width="70%" height="70%">
 
 ## Pyhton
 
 Loin hello.py nimisen tiedoston jonne lisäsin koodia joka tulostaa ajettaess "hello wordl"
 
     print("hello world")
     
 Ajoin testiksi skriptin ja sain tulosteen "hello world" eli skrpiti toimi.
 
<img src="/images/kuva55.png" alt="testi" title="testi" width="70%" height="70%">
 
 Siirsin skriptin /usr/local/bin kansioon, niinkuin aikaisemmassaa vaiheessa ja annoin oikeudet kaikille käyttäjille ajaa skriptin.
 
     sudo chmod ugo+x hello.py
     
 <img src="/images/kuva56.png" alt="testi" title="testi" width="70%" height="70%">
 
 Loin uuden kansion "python-script" polkuun /srv/salt
 
 ```
 /usr/local/bin/hello.py:
  file.managed:
    - mode: "0755"
    - source: "salt://python-script/hello.py"
```

Tarkistin vielä että oikeudet pysyivät.

Ajoin tilan orjakoneille

<img src="/images/kuva57.png" alt="testi" title="testi" width="70%" height="70%">

kello 21.10. menin t001 koneellee testatakseni skriptiä ja sain seuraavanlaisen virheilmoituksen.

<img src="/images/kuva58.png" alt="testi" title="testi" width="70%" height="70%">

Tarkistin vielä, että skripti kuitenkin löytyy eli se oli tullut orjakoneille perille, mutta ei aja sitä oikein.

liässin shebang linen poolkujen /usr/local/bin/hello.py ja /srv/salt/python-script/hello.py pythonskripteihin

https://stackoverflow.com/questions/6908143/should-i-put-shebang-in-python-scripts-and-what-form-should-it-take

    #!/usr/bin/env python 
    
lähetän tilan uudelleen orjakoneille ja testaan uudestaan. ei onnistunut

<img src="/images/kuva59.png" alt="testi" title="testi" width="70%" height="70%">

johtuu todennäköisesti siitä, että polku on merkattu väärin.

Sain ohjelman toimimaan orjakoneella, ja vika oli siinä, että minulla oli pääritetty python ajamaan komento, eikä uusin python3. Liässin shebangiin python3 jolloin skripti toimi

    #!/usr/bin/env python3

<img src="/images/kuva60.png" alt="testi" title="testi" width="70%" height="70%">

# yhden binäärin ohjelma

aion asentaa micron

https://github.com/zyedidia/micro/releases/tag/v2.0.11

versio: micro-2.0.11-linux64-static.tar.gz

asennus komennolla:

    wget https://github.com/zyedidia/micro/releases/download/v2.0.11/micro-2.0.11-linux64-static.tar.gz
    
avasin tar tiedoston

    tar -xf micro-2.0.11-linux64-static.tar.gz 
    
testasin, että micro toimii ja sen jälkeen kopioin sen polkuun /usr/local/bin 

<img src="/images/kuva61.png" alt="testi" title="testi" width="70%" height="70%">

Sen jälkee micro aukeaa komennolla "micro"

Loin /srv/salt polkuun uuden kansion "micro". Kopioin mciron ja tarkistin oikeudet.

<img src="/images/kuva62.png" alt="testi" title="testi" width="70%" height="70%">

Loin init.sls tiedoston

```
/usr/local/bin/micro:
  file.managed:
    - mode: "0755"
    - source: "salt://micro/micro"
```


testaan t001 orjakoneella

    sudo salt "t001" state.apply micro
    
<img src="/images/kuva63.png" alt="testi" title="testi" width="70%" height="70%">


ssh yhteys t001. komennolla "micro" tekstieditori micro aukeaa onnistuneesti.

<img src="/images/kuva67.png" alt="testi" title="testi" width="70%" height="70%">


## Lähteet


Karvinen, Tero: Oppitunnit 2023-04-13, palvelinten hallinta, h4-Komennot (https://terokarvinen.com/2023/palvelinten-hallinta-2023-kevat/#h4-komennus)

https://stackoverflow.com/questions/6908143/should-i-put-shebang-in-python-scripts-and-what-form-should-it-take

https://github.com/zyedidia/micro/releases/tag/v2.0.11



