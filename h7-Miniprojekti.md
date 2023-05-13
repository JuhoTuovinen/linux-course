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
    
 test ping true kuva


# Ohjelmien asentamisen automatisointi
aion tehdä näin blablabla

Päivitin paketin halinnan

    sudo apt-get update 

Aloitin ensin asentamalla jokaisen ohjelman paketin hallinnasta käsin

    sudo apt install <ohjelma>
    
Testasin, että ohjelmat toimivat.





Loin knasion

    sudo mkdir -p /srv/salt/pen-tools

loin tilatiedoston

    sudo nano /srv/salt/pen-tools/init.sls
    
    
lisäsin tilatiedostoon yaml, joka automatisoidustin asentaa ohjelmat

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
<code>tools_installation</code> on nimi
<code>pkg.installed</code> lataa alla olevat ohjelmat paketinhallinnasta

testasin komennolla

    sudo salt-call --local state.apply pen-tools

Salt ilmoitti, että ohjelmat ovat asennettu. Eli ohjelmat asentuu toivotunlaisesti

kuva 

# Paketinhallinnan ulkopuolelta ladattan ohjelman automatisointi

tarkoitukseni

John the ripper: https://github.com/openwall/john

latasin ensin käsin

     git clone https://github.com/openwall/john.git
     
kuva git clone

Seuraavaksi automatisointi

testiksi loin tilan, joka luo kansion polkuun /usr/local/bin ja lataa varaston githubista sinne.

kokeilin ensin tällä
````
clone_john_repo:
  git.latest:
    - name: https://github.com/openwall/john.git
````

kuva virheestä

virhe kertoo, että "target" parametri puuttuu. lisäsin parametrin sekä komennon jolla luodaan tiedosto "john" polkuun /usr/local/bin/.

````
/usr/local/bin/john:
  file.directory:
    - makedirs: True



john_repo:
  git.latest:
    - name: https://github.com/openwall/john.git
    - target: /usr/local/bin/john
    - require:
      - file: john_clone

````

<code>file</code>

Tiedosto ladattiin githubista ja kansio luotiin.

kuva

kuva

# Rock you





