# Demonit

Tässä raportissa esitän kuinka asennan OpenSSH-palvelimen hallittaville koneille ensin käsin, minkä jälkeen automatisoin asennuksen. Lopuksi testaan onko asentamani määritykset voimassa. 

Tietokoneen speksit, jolla suoritan toimenpiteen:
- Apple MacBook 2015
- macOs Monterey versio 12.6.4 
- Ram -muistia 16GB
- levytilaa vapaana 42,57 GB.

## Tiivistelmä

Kuinka vaihtaa SSH-palvelinportti käyttäen Salt-tilaa:

1. Luo package-file-service -tila Saltissa, jossa on kolme moduulia. Ensimmäinen moduuli asentaa openssh-server-paketin järjestelmään. Toinen moduuli hallinnoi /etc/ssh/sshd_config-tiedostoa, joka sisältää SSH-palvelimen asetukset. Kolmas moduuli käynnistää SSH-palvelimen ja varmistaa, että palvelu suoritetaan
2. Asenna tila hallinnoitaville laitteille
3. Testaa, että tila toimii


(Lähde: Karvinen, Tero: Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port https://terokarvinen.com/2018/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=salt%20ssh)

## OpenSSH:n asennus käsin

(Kello 20:00) Aloitin asentamalla OpenSSH:n kaikille hallittaville koneille komennolla:

    sudo salt ’*’ state.single pkg.installed ssh
 
 Asennus onnistui.
 
 
 <img src="/images/kuva14.png" alt="testi" title="testi" width="70%" height="70%">
 
 (Kello 20:05) Seuraavaksi muokkasin sshd-konfiguraatiotiedostoa /etc -kansiossa ja annoin toiseksi kuunneltavaksi portiksi portin 20. Muokkasin tiedostoa komennolla:

    sudoedit /etc/ssh/ssh_config
    

<img src="/images/kuva15.png" alt="testi" title="testi" width="40%" height="40%">

  
  
  
  Testasin yhteyttä master- koneeseen portin 20 kautta komennolla:

    ssh -p 20 vagrant@192.168.12.3
    
    
 Yhteys ei muodostunut ja sain seuraavanlaisen virheilmoituksen
 
<img src="/images/kuva16.png" alt="testi" title="testi" width="70%" height="70%">


(Klelo 21:00) Kokeilin myös ssh-yhteyttä toiseen hallittavista koneista onnistumatta.

<img src="/images/kuva17.png" alt="testi" title="testi" width="70%" height="70%">




## Jatkoa

Jatkoin harjoitusta Pe 14.4.2023.

(Kello 16:30-17:00)

Tuhosin aikaisemmat virtuaalikoneet ja loin uudet. Aloitin kokeilemalla SSH-yhteyttä "tmaster" koneeseen. Yhteys ei onnistu, eli ongelma toistuu.

    ssh vagrant@192.168.12.3
    
 <img src="/images/kuva19.png" alt="testi" title="testi" width="50%" height="50%">
    

 Vaihdoin käyttäjän salasanan onnistuneesti.
 
    sudo passwd vagrant

Seuraavaksi navigoin SSH-asetustiedostoon ja laitoin aseutkseksi "PasswordAuthentication yes"

    sudoedit /etc/ssh/sshd_config

<img src="/images/kuva20.png" alt="testi" title="testi" width="70%" height="70%">


Sen jälkeen yritin SSH-yhteyttä master-koneeseen uudestaan. Aikasemmin määrittämäni salasanan laitettua, sain yhteyden muodostettua.


<img src="/images/kuva21.png" alt="testi" title="testi" width="70%" height="70%">


Seuraavaksi loin SSH-avainparin.

    ssh-keygen

Kopioin onnistuneesti julkisen avaimen etäpalvelimelle.

    ssh-copy-id vagrant@192.168.12.3

<img src="/images/kuva22.png" alt="testi" title="testi" width="70%" height="70%">


Seuraavaksi navigoin takaisin sshd_config- tiedostoon ja vaihdoin aikaisemmin määrittämäni asetuksen "PasswordAuthentication yes" takaisin alkuperäiseen "PasswordAuthentication no".

Uudellenkäynnistin demonin.

    sudo systemctl restart ssh

Testasin uudelleen SSH- yhteyttä masterkoneeseen. Yhteys onnistui ja kirjautumista ei tarvittu.


(Kello 17:00) 
Seuraavaksi testasin kuunteleeko palvelin oletusportin lisäksi toista porttia. 

Avasin aikaisemmin mainitun sshd_config- tiedoston. Otin kommentoinnin pois kohdasta "Port 22" ja lisäsin perään portin 20.

<img src="/images/kuva23.png" alt="testi" title="testi" width="50%" height="50%">

Tallensin muutokset ja uudelleenkäynnistin demonin. Nyt palvelin kuuntelee myös toista porttia 20.

    ssh -p 20 vagrant@192.168.12.3

<img src="/images/kuva24.png" alt="testi" title="testi" width="70%" height="70%">



## Automatisointi


(Kello 18:45)

Navigoin kansioon /srv ja loin kansiot "salt/ssh", jonka sisään loin tila-tiedoston “init.sls”. Lisäsin tiedostoon skriptin (Tero Karvinen: Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port, https://terokarvinen.com/2018/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=salt%20ssh).


```
openssh-server:
 pkg.installed
/etc/ssh/sshd_config:
 file.managed:
   - source: salt://sshd_config
sshd:
 service.running:
   - watch:
     - file: /etc/ssh/sshd_config
```
Kopioin sshd-tiedoston /srv/salt-kansioon.

    sudo cp /etc/ssh/sshd_config /srv/salt/


Sen jälkeen testasin asennusta koneille.

    sudo salt '*' state.apply ssh
    
Asennus onnistui.

<img src="/images/kuva25.png" alt="testi" title="testi" width="50%" height="50%">
<img src="/images/kuva26.png" alt="testi" title="testi" width="50%" height="50%">
    


Testasin onko aikaisemmin määritetty portti 20 auki koneella "t001" ottamalla SSH-yhteyden porttiin ja toisen testin ottamalla yhteyden porttiin, joka kuuluisi olla kiinni. Portista 20 sain vastaukseksi "Permission denied", mikä osoittaa, että kyseinen portti on auki, mutta ei pääsyä. Testatessa porttiin 27 sain vastaukseksi "Connection refused", josta voidaan päätellä, että kyseinen portti on kiinni.

    ssh -p 20 vagrant@192.168.12.100


<img src="/images/kuva27.png" alt="testi" title="testi" width="70%" height="70%">


## Muutos SSH-palveluun


(Kello 19:25-20:30)

Seuraavaksi testaan käynnistääkö Salt demonit uudelleen, kun asetustiedosto on muuttunut. Uusien asetusten on astuttava voimaan, käynnistämättä demonia uudelleen. Lisäämällä kolmannen portin, näen avautuuko portti, ja astuuko asetukset voimaan. Aion lisätä sshd-konfiguraatio tiedostoon aikaisemmin testatun portin 27, joka oli kiinni.

Avasin aikaisemmin käsittelemämme sshd_config- tiedoston ja lisäson portin 27, samoilla metodeilla kuin aikaisemmin.

<img src="/images/kuva28.png" alt="testi" title="testi" width="50%" height="50%">

Käynnistin demonin uudelleen ja ajoin tilan koneille.

    sudo salt '*' state.apply ssh
    
<img src="/images/kuva29.png" alt="testi" title="testi" width="50%" height="50%">


(Kello 19.50) Testasin onko portti 27 auki koneella "t001".

     ssh -p 27 vagrant@192.168.12.100
     
Sain vastaukseksi "Connection refused" eli asennus ei onnistunut. Testasin auttaako portin muutos, ja vaihdoin portin 27 tilalle portin 25. Yritin käynnistää demonia uudelleen, mutta sain virheilmoituksen.

<img src="/images/kuva30.png" alt="testi" title="testi" width="50%" height="50%">

Ajoin virheilmoituksessa mainitut komennot.

<img src="/images/kuva31.png" alt="testi" title="testi" width="50%" height="50%">
<img src="/images/kuva32.png" alt="testi" title="testi" width="50%" height="50%">


Ongelma tulee kolmatta porttia lisätessä. Muokkasin tiedostoa niin, että vain portti 20 on auki. Käynnistin ssh:n uudelleen ja testasin yhteyttä master-koneeseen portin 20 kautta. Yhteys onnistui. Kun portteja on konfiguraatio tiedostossa 3, silloin kyseinen virheilmoitus esiintyy.


Muutin sshd-konfiguraatiotiedostoa niin, että vainportit 22 ja 27 ovat käytössä. Uudelleen käynnistin demonin ja asensin tilan hallittaville koneille. Kuitenkaan asetus ei päivittynyt ja en saanut ssh-yhteyttä portin 27 kautta koneeseen "t001". Yhteys portin 27 kautta onnistui kuitenkin "tmaster" koneelle. Ongelma voi johtua siitä, että asetuksia ei muutettu /salt-kansiossa olevassa sshd_config- tiedostossa.


<img src="/images/kuva33.png" alt="testi" title="testi" width="50%" height="50%">


(Kello 21:05)
Lisäsin porti 27 sshd_config kansioon polussa /srv/salt. Käynnistin demonin uudelleen ja asensin tilan hallittaville koneille. 


<img src="/images/kuva34.png" alt="testi" title="testi" width="50%" height="50%">

Asennus onnistui ja kuvasta näkee, että portti 27 on lisätty. Testasin vielä ssh-yhteyttä portin kautta t001-koneelle.

    ssh -p 27 vagrant@192.168.12.100
    
<img src="/images/kuva35.png" alt="testi" title="testi" width="50%" height="50%">  

Vastaukseksi saatiin "Permission denied", mistä voidaan päätellä, että portti on auki.


Lauantaina 15.4.2023 klo 14.20 jatkoin harjoitusta.

t001 -koneella sallin kirjautumisen salasanalla muokkaamalla sshd_config- tiedostoa kuten aikaisemmin. Testasin SSH-yhteyttä portin 27 kautta uudelleen. Testi onnistui ja yhteys sallittiin.

<img src="/images/kuva36.png" alt="testi" title="testi" width="50%" height="50%">

## Lähteet


Karvinen, Tero: Oppitunnit 2023-04-6, Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port (https://terokarvinen.com/2018/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=salt%20ssh)



