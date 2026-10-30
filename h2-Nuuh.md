# h2 Nuuh

# x) Lue ja tiivistä

## Tiivistelmä Karvinen 2025: Wireshark - Getting Started (https://terokarvinen.com/wireshark-getting-started/)

Wiresharkin asennus tapahtuu komennoilla:

```bash
$ sudo apt-get update
$ sudo apt-get install wireshark
```
* Muidenkin kun sudoers-käyttäjien tulisi saada napata pakketteja ohjelmalla
* Liikenteen kaappaus aloitetaan valitsemalla aktiivinen verkkoliitäntä (tai kaikki liitännät "any"-valinnalla), ja kerätty liikenne voidaan tallentaa pcap-tiedostomuotoon myöhempää tarkastelua varten.
* Tilastotyökalut tarjoavat yleiskuvan kaapatusta liikenteestä, kuten laitteista (Endpoints), liikenteen määrästä suhteessa aikaan (I/O Graphs) sekä protokollajakaumasta (Protocol hierarchy).
* Liikennettä voidaan suodattaa hakukentässä esimerkiksi protokollan ``dns``, ``tls``, ``http`` perusteella.

## Tiivistelmä Karvinen 2025: Network Interface Names on Linux (https://terokarvinen.com/network-interface-linux/)

* Verkkoliitäntä vastaa fyysistä tai virtuaalista verkkokorttia, ja nykyaikaiset systemd-pohjaiset Linux-jakelut nimeävät ne yhtenäisen käytännön mukaisesti.
* Nimen alkuosa määrittää yhteystyypin: en tarkoittaa langallista Ethernetiä, wl langatonta WLAN-verkkoa ja lo koneen sisäistä loopback-sovitinta.
* Etuliitteen jälkeiset merkit viittaavat laitteen fyysiseen kytkentäpaikkaan tai MAC-osoitteeseen.

Oman koneen käytössä olevat verkkoliitännät ja reititykset voi tarkistaa komentoriviltä komennoilla:

```bash
$ ip a
$ ip route
```

# a) Linux

En asentanut uudelleen lokaalisti Debian-konetta sillä minulla ei ole mitään ongelmia asennuksen kanssa (Tätä alakohtaa ei poikkeuksellisesti tarvitse raportoida, jos sinulla ei ole mitään ongelmia. (Karvinen, T. Verkkosivu. 2026) )

# b) Ei voi kalastaa

Käynnistin Kali-Linux virtuaalikoneeni ja sen auettua katkaisin Wired connection 1:n oikeasta yläkulmasta painamalla disconnect nappia:

<img width="473" height="210" alt="kuva" src="https://github.com/user-attachments/assets/b04ae0b9-e479-437c-b870-f1ab8083f677" />

Kuva 1. Kali-Linuxin verkon katkaisu

Kokeilin, että verkkoyhteys katkesi oikeasti (1.1.1.1 on Cloudflaren  julkinen DNS-palvelin ja 8.8.8.8 on Googlen julkinen DNS-palvelin):

```bash
$ ping 1.1.1.1 # Cloudflare
$ ping 8.8.8.8 # Google
```
Alempana kuvassa on testin tulokset: 

<img width="867" height="561" alt="kuva" src="https://github.com/user-attachments/assets/29b369aa-34e8-43dc-9410-3bbca528b049" />

Kuva 2. Ping-testin tulokset.

Verkon virtuaalikoneeseen sai palautettua oikeasta yläkulmasta laittamalla Wired connection 1:n takaisin päälle:

<img width="422" height="253" alt="kuva" src="https://github.com/user-attachments/assets/91b84ca6-dd99-4903-bb00-e6c2e80df7d1" />

Kuva 3. Kali-Linuxin yhdistäminen takaisin verkkoon.

Tämän jälkeen ajoin saman ping-testin täysin samoihin osoitteisiin, alempana tulos kuvasssa: 

<img width="1917" height="1077" alt="kuva" src="https://github.com/user-attachments/assets/0d428c2f-db0c-43c7-beef-571317e3f799" />

Kuva 4. Ping-testin tulos kun kone on verkossa.

# c) Wireshark

Yleisesti Kali-Linux sisältää Wireshark ohjelman valmiiksi, mutta kokeilun vuoksi ajoin komennot:

```bash
sudo apt update
sudo apt install wireshark
```

Tuloksena komentorivi kertoi, että Wireshark on jo asennettu virtuaalikoneelle.

# d) Oikeesti TCP/IP

# e) Mitäs tuli surffattua?

# f) Vapaaehtoinen, vaikea: Mitä selainta käyttäjä käyttää?

# g) Minkä merkkinen verkkokortti käyttäjällä on?

# h) Millä weppipalvelimella käyttäjä on surffaillut?

# i) Analyysi

## Lähteet
Karvinen, T. Verkkosivu. 2026 https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/

Karvinen, T. Luento verkkoon tunkeutuminen ja tiedustelu. 27.10.2026
