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

# Toteutus

## Ohjelmien käsin asennus

Aloitin projektin ensimmäisen vaiheen asentamalla aiemmin mainitut ohjelmat käsin paketinhallinnasta. Tällä tavoin testataan, että ohjelmat löytyvät paketinhallinnasta, ja että ne toimivat.

Ensimmäiseksi päivitin paketinhalinnan.

    sudo apt-get update 

Sen jälkeen asennutin jokaisen ohjelman paketinhallinnasta käsin.

    sudo apt install <ohjelma>
    
Testasin, että ohjelmat toimivat.


## Ohjelmien asennuksen automatisointi

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


## Ohjelma paketinhallinnan ulkopuolelta

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

<img src="/images/kuva101.png" alt="testi" title="testi" width="70%" height="70%">


- <code>file.directory</code> varmistaa, että kansio löytyy.
- <code>makedirs: True</code> luo kansion, jos sitä ei ole.
- <code>git.latest</code> kloonaa Github varaston.
- <code>target</code> kertoo, minne github varasto kloonataan.

Lähde: Saltsatck, SALT.STATES.GIT, https://docs.saltproject.io/en/latest/ref/states/all/salt.states.git.html.

## Rockyou

Seuraavaksi tarkoitukseni on ladata salasanojen murtamisessa käytetty rockyou- sanalista. Luodaan kansio polkuun /usr/bin/local, jonne tallennetaa sanalista. Tämä on tarkoitus automatisoida.

Ensiksi latasin sanalistan käsin osoitteesta https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt.

Testivaiheessa luodaan uusi tila, joka luo kansion "wordlists" ja lataa listan kansioon. Loin kansion "rockyou" polkuun /srv/salt, minne lisäsin YAML-koodia.

````
/usr/local/bin/wordlists/rockyou.txt:
  file.managed:
    - makedirs: True
    - source: https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt
    - mode: "0755"
 ````
- <code>file.managed</code> kerrotaan, että halutaan hallita tiedostoa.
- <code>makedirs: True</code> luo kansion, jos sitä ei ole.
- <code>source:</code> mistä tiedosto ladataan.
- <code>mode: "0755"</code> antaa omistaja ajaa, lukea ja kirjoittaa tiedoston päälle sekä ryhmän ja muiden käyttäjien lukea ja ajaa ohjelma.

Ajoin tilan ja sain virheilmoituksen.

<img src="/images/kuva102.png" alt="testi" title="testi" width="70%" height="70%">

Virheessä kerrotaan, että pitää lisätä <code>skip_verify</code> -parametri, joten lisäsin <code>- skip_verify: True
</code>.

Ajoin tilan uudestaan ja kansio luotiin sekä tiedosto ladattiin kansioon.

<img src="/images/kuva103.png" alt="testi" title="testi" width="70%" height="70%">

## Micron konfigurointi

Tarkoitus on vaihtaa Micron oletus teemaa värikkäämmäksi. Haluan teeman "cmc-16", joten ensiksi vaihdan sen käsin.

Avasin Micro-editorin ja painoin <code>ctrl + e</code>, jolloin avautuu komentorivi. Komentoriville kirjoitin <code>set colorscheme cmc-16</code>, jolloin teema vaihtuu. Micron mnuaalissa lukee, että configuraatio tiedosto on polussa <code>~/.config/micro</code> ja sieltä löysin tiedoston <code>settings.json</code>, jossa teema on määritelty.

 ```` 
 {
    "colorscheme": "cmc-16"
}
 ````

Loin uuden tilan "micro" testatakseni ominaisuutta. Lisäsin YAMLL-koodia, joka luo käyttäjän polkuun <code>/etc/skel/</code> kansiot <code>/.config/micro</code>. Sen jälkeen konfiguraatio tiedosto ladataan Github-varastostani ja tallennetaan äsken luotuun polkuun.

 
  ````
 /etc/skel/.config/micro:
  file.directory:
    - mode: "0755"

/etc/skel/.config/micro/settings.json:
  file.managed:
    - source: https://raw.githubusercontent.com/JuhoTuovinen/linux-course/main/>
    - mode: "0755"
    - skip_verify: True
 
 ````
- <code>file.directory</code> varmistaa, että kansio löytyy.
- <code>mode: "0755"</code> antaa omistaja ajaa, lukea ja kirjoittaa tiedoston päälle sekä ryhmän ja muiden käyttäjien lukea ja ajaa ohjelma.
- <code>file.managed</code> kerrotaan, että halutaan hallita tiedostoa.
- <code>source</code> lähde, josta tiedosto ladataan.
- <code>- skip_verify: True</code>ohittaa lähde-URL:n SSL/TLS-sertifikaatin varmennuksen. 
 
Ongelmana on, että konfiguraatiot microo haetaan käyttäjän polusta <code>~/.config/micro</code>, joten muutokset täytyy tallentaa sinne. Tämän automatisoidessa Salt kuitenkin asentaa muutokset "root"-käyttäjälle, eikä henkilökohtaisille käyttäjille. Päädyin ratkaisuun, että käyttäjä itse ajaa terminaalissa komennon <code>sudo mkdir -p ~/.config/micro && sudo cp /etc/skel/.config/micro/settings.json ~/.config/micro/settings.json</code>, joka luo polun konfiguraatiokansioon ja kopioi konfiguraatio tiedosto polusta <code>/etc/skel/.config/micro/settings.json</code>, jonne se konfiguratio oli Saltilla asennettu.


# Käyttöön otto

Luodaan kansio
     
    sudo mkdir -p /srv/salt/pen-tools
    
Tilatiedosto

    sudo nano /srv/salt/pen-tools/init.sls

Lisätään YAML

````
#software installation
tools_installation:
  pkg.installed:
    - pkgs:
      - wireshark
      - micro
      - nmap
      - firefox
      - git
      - hashcat

#John The Ripper installation
/usr/local/bin/john:
  file.directory:
    - makedirs: True

john_repo:
  git.latest:
    - name: https://github.com/openwall/john.git
    - target: /usr/local/bin/john


#Wordlist download
/usr/local/bin/wordlists/rockyou.txt:
  file.managed:
    - makedirs: True
    - source: https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt
    - mode: "0755"
    - skip_verify: True
    
#Micro theme configuration
/etc/skel/.config:
  file.directory
    
/etc/skel/.config/micro:
  file.directory

/etc/skel/.config/micro/settings.json:
  file.managed:
    - source: https://raw.githubusercontent.com/JuhoTuovinen/linux-course/main/micro-config/settings.json
    - mode: "0755"
    - skip_verify: True

    
````

<code>ctrl + X</code> ja <code>Y</code> päästään pois

sen jälkeen navikoidaan aiemmin luotuun /srv/salt polkuun ja kutsutaan tilaa paikallisesti.

    cd /srv/salt
    sudo salt-call --local state.apply pen-tools

Asennuksen jälkeen micron konffa

    sudo mkdir -p ~/.config/micro && sudo cp /etc/skel/.config/micro/settings.json ~/.config/micro/settings.json
