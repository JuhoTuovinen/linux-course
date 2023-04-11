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
 
 ![Kuva](/images/kuva14.png)
 
 (Kello 20:05) Seuraavaksi muokkasin sshd-konfiguraatiotiedostoa /etc -kansiossa ja annoin toiseksi kuunneltavaksi portiksi portin 20. Muokkasin tiedostoa komennolla:

    sudoedit /etc/ssh/ssh_config
    

  ![Kuva](/images/kuva15.png)
  
  
  Testasin yhteyttä master- koneeseen portin 20 kautta komennolla:

    ssh -p 20 vagrant@192.168.12.3
    
    
 Yhteys ei muodostunut ja sain seuraavanlaisen virheilmoituksen
 
![Kuva](/images/kuva16.png)

(Klelo 21:00) Kokeilin myös ssh-yhteyttä toiseen hallittavista koneista onnistumatta.

![Kuva](/images/kuva17.png)



(Raportti jatkuu)


## Lähteet


Karvinen, Tero: Oppitunnit 2023-04-6, Pkg-File-Service – Control Daemons with Salt – Change SSH Server Port (https://terokarvinen.com/2018/pkg-file-service-control-daemons-with-salt-change-ssh-server-port/?fromSearch=salt%20ssh)



