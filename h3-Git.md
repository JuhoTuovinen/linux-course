# Git

Tässä raportissa esitän kuinka asennan OpenSSH-palvelimen hallittaville koneille ensin käsin, minkä jälkeen automatisoin asennuksen. Lopuksi testaan onko asentamani määritykset voimassa. 

Tietokoneen speksit, jolla suoritan toimenpiteen:
- Apple MacBook 2015
- macOs Monterey versio 12.6.4 
- Ram -muistia 16GB
- levytilaa/muistia vapaana 40,68 GB.

Ohjelmistot
- VirtualBox Version 7.0.6 r155176 (Qt5.15.2)
- Vagrant 2.3.4
- VM OS: Debian Bullseye

## Online

12.10
A) Loin githubiin varaston nimeltä summer, jonne loin tiedostot README.md ja GNU General Public License 3.

<img src="/images/kuva37.png" alt="testi" title="testi" width="70%" height="70%">

## Dolly
Kello 12.15 
asensin gitin 
   
    sudo apt-get install git
    
    ssh-keygen
    
    cat /home/vagrant/.ssh/id_rsa.pub
    
Jotta saadaan varasto kopioitua laitteelle, pitää lisätä käyttäjän julkinen avain githubiin. Settings -> SSH and GPG keys -> New SSH key.



Sen jälkeen latasin luomani varaston tmaster virtuaalikoneelle.

    git clone git@github.com:JuhoTuovinen/summer.git


<img src="/images/kuva39.png" alt="testi" title="testi" width="70%" height="70%">



Kello 14.50. Seuraavaksi testaan Gitin toiminnan lisäämällä tekstitiedoston "testi.txt" varastoon ja katson ilmestyykö se Githubin käyttöliittymään aikaisemmin luomani tiedostojen README.md ja GNU General Public License 3 rinnalle.


    touch testi.txt
    
    git add . && git commit; git pull && git push
    
Sain virheilmoituksen, että minun tulee tunnistautua. Tuniistauduin lisäämällä henkilökohtaiset tietoni kohtiin "you@example.com" ja "Your Name".

    git config --global user.email "you@example.com"
    git config --global user.name "Your Name"
    
<img src="/images/kuva40.png" alt="testi" title="testi" width="70%" height="70%">

Ajoin uudestaan "git add . && git commit; git pull && git push", jonka jälkeen kerroin mitä muutoksia tein. Tarkistin github "summer"-varastoni, ja lisäämäni tiedosto on ilmestynyt onnistuneesti.

<img src="/images/kuva42.png" alt="testi" title="testi" width="70%" height="70%">

## Doh

Kirjoitin aikaisemmin luotuun testi-tiedostoon tekstiä. Senjälkeen ajoin komennon "git add .", minkä jälkeen "git reset --hard", jonka kuuluisi peruuttaa tehdyt muutokset. Testasin löytyykö tiedostosta äsken kirjoittamani teksti, ja tiedosto olikin tyhjä. Peruutus onnistui.

<img src="/images/kuva43.png" alt="testi" title="testi" width="70%" height="70%">

D)
## Loki
(22:45)
Lisäsin tekstiä testi.txt- itedostoon ja sen jälkeen tarkastelin lokia komennolla:

     git log --patch

<img src="/images/kuva44.png" alt="testi" title="testi" width="70%" height="70%">




