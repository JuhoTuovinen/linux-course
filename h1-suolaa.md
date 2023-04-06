# Salt

Tässä raportissa kerron kuinka asennan Vagrant- ja salt-ohjelmistot, ja testaan niiden toimivuutta tietokoneellani. Kyseisen toimenpiteen suoritan vuoden 2015 Apple MacBook:lla, jossa käyttöjärjestelmänä macOs Monterey versio 12.6.4. Käytössä on Ram muistia 16GB ja levytilaa vapaana 39,71 GB.

## Valmistelu

Latasin ja asensin VirtualBoxin heidän sivuiltaan: https://www.virtualbox.org/wiki/Downloads. Sen jälkeen asensin Vagrantin komennolla ”brew install vagrant”, minkä jälkeen loin kotihakemistoon kansion ”saltdemo”, komennolla ”mkdir saltdemo”. Kansioon tallensin konfiguraatiotiedoston nimeltä Vagrantfile, joka määrittää kolme virtuaalikonetta t001, t002 ja tmaster, joissa Debian 11-käyttöjärjestelmä. Etenin harjoituksessani ohjeiden mukaan sivustolta https://terokarvinen.com/2023/salt-vagrant/. Seuraavaksi asensin salt:n komennolla ”brew install salt”.

## Virtuaalikoneiden luonti

Aloitin virtuaalikoneiden luomisen komennolla ”vagrant up”, minkä jälkeen t001, t002 ja tmaster -koneet ovat luotu.

Kirjauduin tmaster -koneelle komennolla ”vagrant ssh tmaster”. Sen jälkeen ajoin komennon ” sudo salt-key -A”, jolla hyväksyin ”orjat” onnistuneesti.

![Kuva](/kuva1.png)


## Testaus ja käyttö

Testasin yhteyttä ohjattaviin koneisiin komennolla ” sudo salt '*' test.ping”. Koneet vastasivat eli yhteys toimii.

![Kuva](/kuva2.png)

Ajoin komennon ”sudo salt '*' cmd.run 'hostname -I'”, joka palauttaa meille ohjattavien laitteiden IP-osoitteet.

![Kuva](/kuva3.png)

Ajoin komennon ” sudo salt '*' grains.item osfinger ipv4”, jonka avulla nähdään ohjattavien laitteiden käyttöjärjestelmän tyyppi-ja versiotiedot sekä IPv4-osoitteet.

![Kuva](/kuva4.png)

## Tiedoston luonti laitteille

Ajoin komennon “sudo salt '*' state.single file.managed '/tmp/see-you-at-terokarvinen-com'”, joka luo  /tmp/see-you-at-terokarvinen-com -nimisen tiedoston jokaiselle hallittavalle laitteelle.

![Kuva](/kuva5.png)

## Apache 2

Ajoin komennon ”sudo salt '*' state.single pkg.installed apache2” kokeillakseni latautuuko Apache-palvelin hallittaviin laitteisiin. Apache2 ei asentunut laitteisiin. En saanut ongelmaa ratkaistua.

![Kuva](/kuva6.png)
![Kuva](/kuva7.png)

## Käyttäjän luominen

Loin onnistuneesti käyttäjän terote01 kaikille hallittaville laitteille komennolla ”sudo salt '*' state.single user.present terote01”.

![Kuva](/kuva8.png)

## Infra koodina

Loin kansion komennolla ”sudo mkdir -p /srv/salt/hello”. Kansioon loin ”init.sls”- nimisen YAML-tiedoston komennolla ”sudoedit /srv/salt/hello/init.sls” jonne tallensin seuraavan komennon: 

![Kuva](/kuva10.png)

Tämä ylläpitää tiedostoa ja varmistaa, että sen sisältö on halutunlainen.

Ajoin komennon ”sudo salt '*' state.apply hello”, joka lataa äsken tallentamani hello-tilan laitteille.

![Kuva](/kuva9.png)

Ajoin komennon ”sudo salt '*' state.apply hello^C”, josta sain virheilmoituksen. Sen jälkeen ajoin komennon ”sudoedit /srv/salt/top.sls”, jolla loin tiedoston. Lisäsin sinne kuvan mukaiset komennot.

![Kuva](/kuva13.png)

Sen jälkeen ajoin komennon ”sudo salt '*' state.apply”, joka suorittaa aikaisemmin määritetyn tilan laitteille. Sain virheilmoituksen, enkä saanut vikaa selvitettyä.

![Kuva](/kuva12.png)


## Lähteet

Karvinen, Tero: Salt Vagrant - automatically provision one master and two slaves 
(https://terokarvinen.com/2023/salt-vagrant/ ).

Virtualbox.org
(https://www.virtualbox.org/wiki/Downloads)

