# Puolikas

Juho Tuovinen

Tämän miniprojektin aiheena on rakentaa "heti valmis" työpöytä Linux -käyttöjärjestelmälle. Ohjelmat ovat hyödyllisiä, mutta niitä ei ole valmiiksi asennettu. Saltin avulla voidaan asentaa kasa hyödyllisiä ohjelmia kuten Micro, Libre Office, SSH, Apache2, Gimp, Firefox ja VLC. Versio on alpha ja tulee jatkumaan. 

# Saltin asentaminen
(Ohjeet Saltin asentamiseen)

## Työpöydän asentaminen 

Luo kansio "ohjelmat" kohteeseen /srv/salt.

    sudo mkdir -p /srv/salt/ohjelmat

Lisää sls-tiedosto kohteeseen.

    sudo nano /srv/salt/ohjelmat/init.sls

Lisää alla oleva YAML-teksti.

```
ohjelmat:
  pkg.installed:
    - names:
      - apache2
      - micro
      - gimp
      - libreoffice
      - ssh
      - firefox-esr
      - vlc
```

Tilan voi ajaa koneille komennolla:

    sudo salt '*' state.apply ohjelmat
    
    
 Asennuksessa voi kestää useita minuutteja (5-10 min), kun ohjelmat asennetaan. Asennuksen jälkeen ohjelmat löytyvät koneilta, joille ne on asennettu.
 
 Asennettu SSH
 
 <img src="/images/kuva89.png" alt="testi" title="testi" width="60%" height="60%">
 
 Asennettu Micro
 
 <img src="/images/kuva90.png" alt="testi" title="testi" width="60%" height="60%">

## Lähteet


Karvinen, Tero: Oppitunnit 06/03/2023-04/05/2023, palvelinten hallinta, h6-Puolikas, https://terokarvinen.com/2023/palvelinten-hallinta-2023-kevat/#h6-puolikas)

How to install Mozilla FireFox on Debian 11 Bullseye, https://linux.how2shout.com/how-to-install-mozilla-firefox-on-debian-11-bullseye/


