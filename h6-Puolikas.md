# Puolikas

Juho Tuovinen

Tämän miniprojektin aiheena on rakentaa "heti valmis" työpöytä. Saltin avulla asennetaan kasa hyödyllisiä ohjelmia (kuten Micro, Libre Office, SSH, Apache2, Gimp, Firefox ja VLC). Versio on alpha ja tulee jatkumaan. 

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

## Lähteet


Karvinen, Tero: Oppitunnit 06/03/2023-04/05/2023, palvelinten hallinta, h6-Puolikas, https://terokarvinen.com/2023/palvelinten-hallinta-2023-kevat/#h6-puolikas)

How to install Mozilla FireFox on Debian 11 Bullseye, https://linux.how2shout.com/how-to-install-mozilla-firefox-on-debian-11-bullseye/


