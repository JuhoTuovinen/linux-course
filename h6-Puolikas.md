# Puolikas

Tämän miniprojektin aiheena on rakentaa "heti valmis" työpöytä. Saltin avulla asennetaan kasa hyödyllisiä ohjelmia (kuten Micro, Libre Office, SSH, Apache2, Gimp, Firefox ja VLC) ja konfiguroidaan ne.


Tietokoneen speksit, jolla suoritan toimenpiteen:
- Apple MacBook 2015
- macOs Monterey versio 12.6.4 
- Ram -muistia 16GB
- levytilaa vapaana 57,97 GB.

```
all:
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
