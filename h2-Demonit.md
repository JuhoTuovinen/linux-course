# Demonit

Tässä raportissa kerron kuinka asennan Vagrant- ja salt-ohjelmistot, ja testaan niiden toimivuutta tietokoneellani. Kyseisen toimenpiteen suoritan vuoden 2015 Apple MacBook:lla, jossa käyttöjärjestelmänä macOs Monterey versio 12.6.4. Käytössä on Ram muistia 16GB ja levytilaa vapaana 39,71 GB.

## Tiivistelmä

Kuinka vaihtaa SSH palvelinportti käyttäen Salt-tilaa:

-	Luo package-file-service -tila Saltissa, jossa on kolme moduulia. Ensimmäinen moduuli asentaa openssh-server-paketin järjestelmään. Toinen moduuli hallinnoi /etc/ssh/sshd_config-tiedostoa, joka sisältää SSH-palvelimen asetukset. Kolmas moduuli käynnistää SSH-palvelimen ja varmistaa, että palvelu suoritetaan.
-	Asenna tila hallinnoitaville laitteille
-	Testaa, että tila toimii


(Lähde: https://terokarvinen.com/2018/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=salt%20ssh)
)


## OpenSSH:n asennus käsin

Aloitinasentamalla molempiin ssh komennolla 

    sudo salt ’*’ state.single pkg.installed ssh
 
 
 ![Kuva](/images/kuva14.png)
