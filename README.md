# funfair

This is a game that happens in a small town where funfair arrives.

Written in Python. Uses SQL database.

## Pelin rakenteen suunnittelua

Muutama sananen rakenteesta...

###Parser

**Parser** palauttaa listan: *verb*, *object*, *indirect_object*

Jokainen verbi on oma aliohjelmansa. talk(), ask() jne

Parserin palauttama lista prosessoidaan ja tehdään valinta, mitä aliohjelmaa kutsutaan. Aliohjelmalle annetaan *object* ja *indirect_object*

### Vihjeet
Tehdään **vihjemoduuli**, joka pitää kirjaa vihjeistä
Pelin alussa kutsutaan arpominen: *tips.create_connections()*. Tämä aliohjelma arpoo, ketkä henkilöt pitävät toisistaan.
Pelin tai päivän alussa kutsutaan *tips.generate_tips*

Talk-phasessa kutsutaan tips-moduulia esim näin: *tips.give_tip()*
give_tip -aliohjelma sitten ratkaisee, antaako se tässä keskustelussa vihjeen vai ei. Se myös päättää, antaako tosi vihjeen vai paikkansapitämättömän vihjeen.
Aliohjelma palauttaa valmiiksi muotoillun vihjeen, jos vihjeitä on jäljellä. Vaihtoehtoisesti ei mitään.

### Päivät ja kyselyt
Tehdään *days*-muuttuja, joka laskee päivien määrää.

Tehdään *asks*-muuttuja, joka laskee, montako kertaa on kysytty

Kun pelaaja yrittää tehdä connectionin sanomalla
<pre>ASK Elna to Carousel</pre>
*asks*-muuttujan arvoa kasvatetaan yhdellä.

Jos *asks* >= 2, ajetaan
*nighttime()*-aliohjelma, joka tulostaa tietoa yöstä, ja tekee muuttujia:
*asks* nollataan, *days* kasvaa yhdellä.

Jos *days* > 3, mennään loppuvaiheeseen, joka on oma aliohjelmansa
final()

### Campfire
Campfire on oma erillinen looppinsa. Siellä voi puhua kaikille eri henkilöille.
Jos campfirellä puhuu Ferris Wheel -operatorille, pääsee maailmanpyörään.

## Moduulit
* **lib/database.py** - tietokannan yhteys, kytketään käskyllä 
<pre>from lib.database import FunDb
connect=FunDb.connect()</pre>
jonka jälkeen tietokantaan otettaan yhteyttä "connect"-muuttujan kautta, esim "cur=connect.cursor()"
* **globals.py** - tallettaa globaalit muuttujat, joita kutsutaan kaikista moduuleista

    *Muista aina moduulin alkuun:*
    <pre>import globals</pre>
    *Ja muuttujia kutsutaan:*
    <pre>globals.verbs</pre>
* **parser.py** - Sisältää parserin
* **tips.py** - Vihjemekaniikka
* **main.py** Pelin päämoduuli, joka kutsuu kaikkia muita
