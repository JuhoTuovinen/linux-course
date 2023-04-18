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

Ma 17.4. Kello 12:10

Aloitin luomalla GitHubiin varaston nimeltä "summer", jonne loin tiedostot README.md ja GNU General Public License 3.

<img src="/images/kuva37.png" alt="testi" title="testi" width="70%" height="70%">

## Dolly

12:15

Asensin tmaster-virtuaalikoneelleni Gitin. 
   
    sudo apt-get install git
    
    
Jotta saadaan varasto kopioitua laitteelle, pitää lisätä luoda avainpari ja lisätä käyttäjän julkinen avain GitHubiin. 
     
     ssh-keygen
     
     cat /home/vagrant/.ssh/id_rsa.pub
     
Kopioin avaimen ja lisäsin sen GitHubiin Settings -> SSH and GPG keys -> New SSH key.


Sen jälkeen latasin luomani "summer"-varaston tmaster-virtuaalikoneelle kopioimalla SSH-osoitteen GitHubista.


    git clone git@github.com:JuhoTuovinen/summer.git


<img src="/images/kuva39.png" alt="testi" title="testi" width="70%" height="70%">



14:50

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

22:45

Seuraavaksi tarkastelen lokin toimintaa. Lisäsin tekstiä testi.txt- itedostoon ja sen jälkeen tarkastelin lokia komennolla:

     git log --patch

<img src="/images/kuva44.png" alt="testi" title="testi" width="70%" height="70%">


Lokista näkee, että nimeni ja sähköpostiosoitteeni on oikein. Lisäksi lokista huomataan, että olen luonut uuden tiedoston ja sinne tekstiä.

"Commit + kirjain-numerosarja" kertoo muutoksen yksilöllisen ID:n. "Author" kertoo kuka muutoksen on tehnyt. "Date" kertoo kellonajan, jolloin muutos on tehty. Sen jälkeen näkyy kommentti, jossa kerrotaan lyhyesti mitä muutoksia on tehty. Vihreällä tai punaisella tekstillä, jossa + tai - näyttää, mitä on lisätty tai poistettu.

## Se toinen järjestelmä

Ti 18.4.2023 Kello 10:30

Koska äsken suorittamani tehtävät ovat suoritettu virtuaalikoneen Debian-käyttöjärjestelmää käyttäen, aion testata Gitin käyttöä omalla Apple-koneellani, jossa macOs-käyttöliittymä.


Asennetaan Git Macbookille.

     brew install git
     
Sen jälkeen tunnistaudutaan, ja se tapahtuu samoilla komennoilla kun aikaisemmin. Lisätään vain lainausmerkkien sisään omat tiedot.

    git config --global user.email "you@example.com"
    git config --global user.name "Your Name"
    
Luodaan avainpari samalla komennolla kuin Debianissa. Ja "Enter"-näppäimellä eteenpäin ellei halua lisätä salalausetta.

    ssh-keygen
    
Sen jälkeen navigoidaan tiedostoon, jossa julkinen avain sijaitsee. Tiedoston polku lukee avainparin luonnin yhteydessä. Yleensä täällä:

    cat ~/.ssh/id_rsa.pub
    
Kopioidaan avain ja liitetään se GitHubiin Settings -> SSH and GPG keys -> New SSH key.

Kopioidaan GitHubista varaston SSH-osoite ja lisätään se käyttäjälle.

    git clone <varaston osoite>
    
Sen jälkeen voi lisätä esim. tiedoston.

    touch file.txt
    
 Tämä luo tyhjän tekstitiedoston. Sen jälkeen tiedosto lisätään GitHub-varastoon Gitin avulla.
 
     git add . && git commit; git pull && git push
     
 Lisätään kommentti mitä ollan muutettu ja sitten voidaankin katsoa onko muutos tullut voimaan.

<img src="/images/kuva45.png" alt="testi" title="testi" width="70%" height="70%">

Tarkistetaan vielä, että loki näyttää oikealta ja, että nimi ja sähköpostiosoite on oikein.

<img src="/images/kuva46.png" alt="testi" title="testi" width="70%" height="70%">


## Yhteistyötä

Seuraavaksi aion testata varaston jakamista toiselle käyttäjälle. Loin itselleni toisen käyttäjän GitHubiin. Minulla oli aikaisemmin tähän tehtävään luotu varasto, jota aion käyttää tässä tehtävässä.

Navigoin GitHub-varastossa Settings->Collaborators ja lisäsin luomain käyttäjän. Lisäämällä toinen käyttäjä, lähetetään kutsu ensin käyttäjälle, jonka pitää se hyväksyä. Hyväksyin sen toisella käyttäjälläni ja varasto näkyy nyt toiselle käyttäjälle.

<img src="/images/kuva47.png" alt="testi" title="testi" width="70%" height="70%">

Kirjauduin t001 koneelle. asensin gitin, loin avainparin, liitin sen githubiin. kopioidaan varasto. loin uuden tiedoston t001.txt


Tunnistauduin luomalla käyttäjän. ja lähetin luomani tiedoston.

Lisäämäni tiedsoto näkyi GitHubin käyttöliittymässä.

<img src="/images/kuva48.png" alt="testi" title="testi" width="70%" height="70%">

Sekä lokissä näkyy, että minulla on käytössä toinen sähköpostiosoite sekä käyttänänimen perässä numer 2, jotta erotetaan, että tämä käyttäjä oli äsken luomani.


<img src="/images/kuva49.png" alt="testi" title="testi" width="70%" height="70%">
