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

Wireshark-ohjelmassa oli päivitys, kuva alla:

<img width="853" height="148" alt="kuva" src="https://github.com/user-attachments/assets/21cbadf1-8dc1-4069-af69-617185611a0e" />

Kuva 5. Komennon tulos.

Tämän jälkeen avasin Wiresharkin komennolla ```sudo wireshark``` ja sieppasin hieman omaa liikennettä verkkokortiltani:

<img width="1913" height="682" alt="kuva" src="https://github.com/user-attachments/assets/17994759-c3ba-4e1a-a63f-5056fd6e8120" />

Kuva 6. Siepattua liikennettä.

# d) Oikeesti TCP/IP

Valitsin siepatusta liikenteestä paketin 3 ja merkitsin siihen väreillä TCP/IP-mallin neljä kerrostä
* Keltainen = Link Layer
* Sininen = Network Layer
* Vihreä = Transport Layer
* Punainen = Application Layer

<img width="1919" height="778" alt="tcpip" src="https://github.com/user-attachments/assets/ecd6222f-a0cd-4faf-aec1-82570e4ee70d" />

Kuva 6. TCP/IP-mallit kuvassa väreillä.

# e) Mitäs tuli surffattua?

Latasin tiedoston surfing-secure.pcap ja avasin tiedoston Wiresharkilla normaalisti vain Kali-Linuxin UI:n kautta. Käytin avukseni alkuun Statistics välilehteä Wireshark ohjelmasta ja sain sieltä seuraavat tiedot:

* I/O Graph kertoo että aikajana pcap-tiedostolla on noin 7 sekuntia.
* Protocol hierarchy kertoo että pakettien jakauma on seuraavanlainen: TCP 83.7%, UDP 15.5% ja ARP 0.7%.
* Pakettien lukumäärä on 283.
* IPv4-osoitteita on 7 (3.75.10.80, 34.117.188.166, 135.181.139.209, 139.162.131.217, 192.168.122.1 (private), 192.168.122.7 (private) & 216.58.210.164)

Seuraavaksi katsoin hieman lisätietoa IPv4 osoitteista Virustotal-verkkosivun avulla, sillä siellä voi hakea montaa osoitetta samaan aikaan.

<img width="1550" height="609" alt="kuva" src="https://github.com/user-attachments/assets/369559a0-9aca-493f-8aef-19dfc90ca2ce" />

Kuva 7. Ipv4-osoitetiedot.

Tiedostossa näyttäisi olevan liikennettä DNS, QUICK, TCP ja TLS paketeilla ja domainit joihin otetaan yhteyttä ovat ainakin: www[.]google[.]com, terokarvinen[.]com, gc[.]zgo[.]at, goatcounter[.]netlify[.]com & terokarvinen[.]goatcounter[.]com 

# f) Vapaaehtoinen, vaikea: Mitä selainta käyttäjä käyttää?

Koska pcap-tiedoston DNS liikenteestä ei löytynyt kyselyitä selaimien domaineihin päin aloin selvittämään selainta TLS-liikenteen kautta. TLS-paketeista löytyy JA4 & JA3 hasheja, joita pystyy käyttämään selaimen tunnistamiseen. 

<img width="1904" height="873" alt="kuva" src="https://github.com/user-attachments/assets/6cc0391e-0dab-4423-8386-288ef13f24d2" />

Kuva 8. JA3 hash TLS-liikenteestä.

Kopioin JA3 hashin ja etsin neitstä "JA3 hash detector", jonka jälkeen menin sivulle https://ja3.zone/ ja syötin hashin sinne, tulokseksi sain Firefox.

<img width="1904" height="856" alt="kuva" src="https://github.com/user-attachments/assets/f76fc788-366f-49f3-bd33-9e6c35ceb067" />

Kuva 9. JA3 hashin tunnistus.

# g) Minkä merkkinen verkkokortti käyttäjällä on?

Käyttäjän verkkokortti selviää todennäköisesti Ethernet II kohdan sisältä, aiemmin löysimme IP-osoitteet 192.168.122.1 joka todnäk on Default Gateway ja osoitteen 192.168.122.7, joka todnäk on client host IP. Avasin paketin 1 ja tarkastin Source MAC-osoitteen sillä paketin 1 source on 192.168.122.7. Paketissa MAC-osoitteen arvo on 52:54:00:cc:d7:12.

# h) Millä weppipalvelimella käyttäjä on surffaillut?

# i) Analyysi

## Lähteet
Karvinen, T. Verkkosivu. 2026 https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/

Karvinen, T. Luento verkkoon tunkeutuminen ja tiedustelu. 27.10.2026
