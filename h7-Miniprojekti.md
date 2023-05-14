# Miniprojekti
Miniprojketin tarkoituksena on Saltin avulla asentaa aloittelevan penetraatiotestaajan työpöytä Ubuntulle. Työpöydälle asentuu penetraatio testauksessa hyödynnettäviä ohjelmistoja kuten NMap, Wireshark, John The ripper ja HAshcat. Lisäksi asennetaan yleishyödyllisiä ohjelmia kuten Micro, git ja firefox.

Ladataan ja asennetaan:
- Nmap
- Wireshark
- HashCat
- John The ripper
- micro
- firefox
- git
- rockyou.txt

KOngiguraatioita:
- micro mcm teema

## Rauta

Apple MacBook 2015
macOs Monterey versio 12.6.4
Ram -muistia 16GB
levytilaa vapaana 57,97 GB.

Ohjelmistot

UTM
Ubuntu virtuaalikone

## Salt

vaatii saltin asentamisen, ohjeet: https://docs.saltproject.io/salt/install-guide/en/latest/topics/install-by-operating-system/ubuntu.html

päivitetään pakettine hallinta

testaan, että saadaan saltilla yhteys itseemme
   
    sudo salt-call --local test.ping
    
 <img src="/images/kuva96.png" alt="testi" title="testi" width="70%" height="70%">

## Toteutus

#Ohjelmien käsin asennus

Aloitin projektin ensimmäisen vaiheen asentamalla aiemmin mainitut ohjelmat käsin paketinhallinnasta. Tällä tavoin testataan, että ohjelmat löytyvät paketinhallinnasta, ja että ne toimivat.

Ensimmäiseksi päivitin paketinhalinnan.

    sudo apt-get update 

Sen jälkeen asennutin jokaisen ohjelman paketinhallinnasta käsin.

    sudo apt install <ohjelma>
    
Testasin, että ohjelmat toimivat.


# Ohjelmien asennuksen automatisointi

Tarkoituksena on automatisoida äsken ladattujen ohjelmistojen asennus Saltilla. Luon tilan, joka asennuttaa ohjelmat.


Loin polun /srv/salt, jonka jälkeen loin polkuun kansion "pen-tools".

    sudo mkdir -p /srv/salt/pen-tools

Loin tilatiedoston pen-tools- kansioon.

    sudo nano /srv/salt/pen-tools/init.sls
    
    
Lisäsin tilatiedostoon YAML-koodia, joilla ohjelmien asennus tapahtuu.

```
tools_installation:
  pkg.installed:
    - pkgs:
      - wireshark
      - micro
      - nmap
      - firefox
      - git
      - hashcat  

```
- <code>tools_installation</code> on tilan ID.
- <code>pkg.installed</code> lataa ohjelmat paketinhallinnasta.
- <code>pkgs</code> parametri kertoo mitkä ohjelmat ladataan.

Testasin onnistuuko automatisoitu ohjelmien asentaminen paikallisesti.

    sudo salt-call --local state.apply pen-tools

Salt ilmoitti, että ohjelmat ovat asennettu. Eli ohjelmat asentuu toivotunlaisesti.

 <img src="/images/kuva97.png" alt="testi" title="testi" width="70%" height="70%">


# Ohjelma paketinhallinnan ulkopuolelta

Tarkoitus on ladata John The Ripper-työkalu paketinhallinnan ulkopuolelta. Hyötyjä ohjelman asentamisen paketinhallinan ulkopuolelta on mm., että saadaan varmasti uusin versio ladattua tai voidaan valita ladattava (esim. vanhempi) versio.

Aloitin lataamalla työkalun ensin käsin Githubista (https://github.com/openwall/john).

     git clone https://github.com/openwall/john.git
     
<img src="/images/kuva98.png" alt="testi" title="testi" width="70%" height="70%">

Seuraavaksi automatisoin latauksen. Testausvaiheessa luon oman tilan pelkästään kyseisen työkalun asennukseen.

Jokaisella käyttäjällä on ainakin yhteinen hakemisto /usr/local/bin. Ladattaessa ohjelma polkuun saadaan se jaettua kaikille Linux-käyttäjille. Ensiksi luodaan kansio "john" kyseiseen polkuun.


````
/usr/local/bin/john:
  file.directory:
    - makedirs: True
````

Kansion luonti onnistui.

<img src="/images/kuva99.png" alt="testi" title="testi" width="70%" height="70%">

Seuraavaksi Github-varasto täytyy ladata polkuun /usr/local/bin/john. Lisäsin tilaan YAML-koodia, joka lataa varaston haluamastani osoitteesta (https://github.com/openwall/john.git) ja tallentaa sen sisällön polkuun /usr/local/bin/john.


````
john_repo:
  git.latest:
    - name: https://github.com/openwall/john.git
    - target: /usr/local/bin/john
    

````
Polkuun /usr/local/bin luodaan onnistuneesti kansio ja Github-varasto latautuu onnistuneesti kohteeseen, vaikkakin saadaan virheilmoitus. Vika on vielä selvittämättä.

````
[ERROR   ] Command 'git' failed with return code: 128
[ERROR   ] stderr: fatal: not a git repository (or any of the parent directories): .git
[ERROR   ] retcode: 128
````

<img src="/images/kuva100.png" alt="testi" title="testi" width="70%" height="70%">


- <code>file.directory</code> varmistaa, että kansio löytyy.
- <code>makedirs: True</code> luo kansion, jos sitä ei ole.
- <code>git.latest</code> kloonaa Github varaston.
- <code>target</code> kertoo, minne github varasto kloonataan.



# Rock you

https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt


# Micro kongiguraatio




