# Git

Tässä raportissa demonstroin Gitin käyttöä GitHub-palvelun kanssa. Aion luoda uuden varaston GitHubiin ja lisätä, muokata ja poistaa sieltä tiedostoja käyttäen Git:iä. Lähden toteuttamaan tehtävää ohjeiden mukaisesti https://terokarvinen.com/2023/palvelinten-hallinta-2023-kevat/#h3-git.

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

(Ma 17.4. Kello 12:10)
Aloitin luomalla GitHubiin varaston nimeltä "summer", jonne loin tiedostot README.md ja GNU General Public License 3.

<img src="/images/kuva37.png" alt="testi" title="testi" width="70%" height="70%">

## Dolly
(12:15) 
Asensin tmaster-virtuaalikoneelleni Gitin. 
   
    sudo apt-get install git
    
    
Jotta saadaan varasto kopioitua laitteelle, pitää lisätä luoda avainpari ja lisätä käyttäjän julkinen avain GitHubiin. 
     
     ssh-keygen
     
     cat /home/vagrant/.ssh/id_rsa.pub
     
Kopioin avaimen ja lisäsin sen GitHubiin Settings -> SSH and GPG keys -> New SSH key.


Sen jälkeen latasin luomani "summer"-varaston tmaster-virtuaalikoneelle kopioimalla SSH-osoitteen GitHubista.


    git clone git@github.com:JuhoTuovinen/summer.git


<img src="/images/kuva39.png" alt="testi" title="testi" width="70%" height="70%">



(14:50) 
Seuraavaksi testaan Gitin toiminnan lisäämällä tekstitiedoston "testi.txt" varastoon ja katson ilmestyykö se Githubin käyttöliittymään aikaisemmin luomani tiedostojen README.md ja GNU General Public License 3 rinnalle.


    touch testi.txt
    
    git add . && git commit; git pull && git push
    
Sain virheilmoituksen, että minun tulee tunnistautua. Tunnistauduin lisäämällä henkilökohtaiset tietoni kohtiin "you@example.com" ja "Your Name".

    git config --global user.email "you@example.com"
    git config --global user.name "Your Name"
    
<img src="/images/kuva40.png" alt="testi" title="testi" width="70%" height="70%">

Ajoin uudestaan "git add . && git commit; git pull && git push", jonka jälkeen kerroin mitä muutoksia tein. Tarkistin GitHub "summer"-varastoni, ja lisäämäni tiedosto on ilmestynyt onnistuneesti.

<img src="/images/kuva42.png" alt="testi" title="testi" width="70%" height="70%">

## Doh

Seuraavaksi testaan toimiiko muutosten peruuttaminen. Kirjoitin aikaisemmin luotuun testi-tiedostoon tekstiä. Sen jälkeen ajoin komennon "git add .", minkä jälkeen "git reset --hard", jonka kuuluisi peruuttaa tehdyt muutokset. Testasin löytyykö tiedostosta äsken kirjoittamani teksti, ja tiedosto olikin tyhjä. Peruutus siis onnistui.

<img src="/images/kuva43.png" alt="testi" title="testi" width="70%" height="70%">


## Tukki

(22:45) 

Seuraavaksi tarkastelen lokin toimintaa. Lisäsin tekstiä testi.txt- itedostoon ja sen jälkeen tarkastelin lokia komennolla:

     git log --patch

<img src="/images/kuva44.png" alt="testi" title="testi" width="70%" height="70%">


Lokista näkee, että nimeni ja sähköpostiosoitteeni on oikein. Lisäksi lokista huomataan, että olen luonut uuden tiedoston ja sinne tekstiä.

## Se toinen järjestelmä

(Ti 18.4.2023 Kello 10:30)
Koska äsken suorittamani tehtävät ovat suoritettu virtuaalikoneen Debian-käyttöjärjestelmää käyttäen, aion testata Gitin käyttöä omalla Apple-koneellani, jossa macOs-käyttöliittymä.


Asennan Gitin.

     brew install git
     
Ajoin samat komennot kuin aikaisemmin suorittaessa. Loin avainparin ja liitin julkisen avaimen GitHubiin kuten aikaisemmin. Latasin aikaisemmin luomani varaston koneelleni. Lisäsin uuden tiedoston "file.txt", jonne lisäsin tekstiä. Suoritin komennon "git add . && git commit; git pull && git push", jonka jälkeen tiedosto pitäisi näkyä GitHubissa. Tiedosto saatiin onnistuneesti lisättyä käyttäen macOs-käyttöjärjestelmää.

<img src="/images/kuva45.png" alt="testi" title="testi" width="70%" height="70%">

<img src="/images/kuva46.png" alt="testi" title="testi" width="70%" height="70%">

