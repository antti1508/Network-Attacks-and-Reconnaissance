# h2 Nuuh

# x) Lue ja tiivistä

## Tiivistelmä Karvinen 2025: Wireshark - Getting Started (https://terokarvinen.com/wireshark-getting-started/)

```console
# Wiresharkin asennus tapahtuu komennoilla:
~$ sudo apt-get update
~$ sudo apt-get install wireshark
```
* Muidenkin kun sudoers-käyttäjien tulisi saada napata pakketteja ohjelmalla
* Liikenteen kaappaus aloitetaan valitsemalla aktiivinen verkkoliitäntä (tai kaikki liitännät "any"-valinnalla), ja kerätty liikenne voidaan tallentaa pcap-tiedostomuotoon myöhempää tarkastelua varten.
* Tilastotyökalut tarjoavat yleiskuvan kaapatusta liikenteestä, kuten laitteista (Endpoints), liikenteen määrästä suhteessa aikaan (I/O Graphs) sekä protokollajakaumasta (Protocol hierarchy).
* Liikennettä voidaan suodattaa hakukentässä esimerkiksi protokollan ``dns``, ``tls``, ``http`` perusteella.

## Tiivistelmä Karvinen 2025: Network Interface Names on Linux (https://terokarvinen.com/network-interface-linux/)

* Verkkoliitäntä vastaa fyysistä tai virtuaalista verkkokorttia, ja nykyaikaiset systemd-pohjaiset Linux-jakelut nimeävät ne yhtenäisen käytännön mukaisesti.
* Nimen alkuosa määrittää yhteystyypin: en tarkoittaa langallista Ethernetiä, wl langatonta WLAN-verkkoa ja lo koneen sisäistä loopback-sovitinta.
* Etuliitteen jälkeiset merkit viittaavat laitteen fyysiseen kytkentäpaikkaan tai MAC-osoitteeseen.

```console
# Oman koneen käytössä olevat verkkoliitännät ja reititykset voi tarkistaa komentoriviltä komennoilla:
~$ ip a
~$ ip route
```

# a) Linux

En asentanut uudelleen lokaalisti Debian-konetta sillä minulla ei ole mitään ongelmia asennuksen kanssa (Tätä alakohtaa ei poikkeuksellisesti tarvitse raportoida, jos sinulla ei ole mitään ongelmia. (Karvinen, T. Verkkosivu. 2026) )

# b) Ei voi kalastaa

# c) Wireshark

# d) Oikeesti TCP/IP

# e) Mitäs tuli surffattua?

# f) Vapaaehtoinen, vaikea: Mitä selainta käyttäjä käyttää?

# g) Minkä merkkinen verkkokortti käyttäjällä on?

# h) Millä weppipalvelimella käyttäjä on surffaillut?

# i) Analyysi

## Lähteet
Karvinen, T. Verkkosivu. 2026 https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/

Karvinen, T. Luento verkkoon tunkeutuminen ja tiedustelu. 27.10.2026
