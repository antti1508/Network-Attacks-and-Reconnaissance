# h5 Labratakki

# a) Aja tunnilla esitetty ARP hyökkäys ja tutki, miten se toimii.

Tässä harjoituksessa käytetään ympäristönä Larin Mininet-virtuaalikonetiedostoa ja VMware-alustaa. "Pienten :D" alkuongelmien jälkeen sain VMwaren asennettua ja virtuaalikoneen käyntiin. Noin parin tunnin konfiguroinnin jälkeen ympäristö oli toimintavalmis.

<img width="1524" height="412" alt="kuva" src="https://github.com/user-attachments/assets/75969d48-f2ad-4210-8860-3c8394a86a63" />

Kuva 1. Mininet toiminnassa.

Seuraavaksi seurasin tunnilta saatuja ohjeita ja ajoin ARP-hyökkäyksen ohjeiden mukaisesti suorittamalla seuraavat komennot järjestyksessä:

- **`ryu-manager ryu.app.simple_switch_13`**: Käynnistää Ryu SDN -kontrollerin perusmuotoisena oppivana kytkimenä.
- **`sudo mn --topo single,3 --mac --switch ovsk --controller remote`**: Luo Mininetillä verkon, jossa on yksi Open vSwitch -kytkin, kolme virtuaalikonetta (`h1`, `h2`, `h3`), selkeät MAC-osoitteet (`--mac`) ja yhteys ulkoiseen kontrolleriin.

Kuvassa 2 Mininet on käynnissä, ja virtuaalikoneeseen on otettu yhteys SSH:lla:

<img width="1718" height="1020" alt="kuva" src="https://github.com/user-attachments/assets/4ee83a44-9613-4636-a3c9-ea020f330421" />

Kuva 2. Mininet käynnissä.

Tämän jälkeen työasemalle piti vielä asentaa X11-palvelin, jotta virtuaalikoneiden erilliset pääteikkunat saatiin avattua. Minun täyti myös ajaa xtermiä varten skripti komennolla ```./get_xauth.sh``` ja ```sudo -s xauth add mininet-vm/unix:10 MIT-MAGIC-COOKIE-1 <palaute komennosta get_xauth.sh>```

- **`mininet> pingall`**: Testaa, että kaikki koneet (`h1`–`h3`) saavat yhteyden toisiinsa, ja täyttää samalla niiden ARP-taulut.
- **`mininet> xterm h1`**: Avaa oman erillisen pääteikkunan virtuaalikoneelle `h1`.
- **`Node: h1> arp -n`**: Näyttää `h1`-koneen ARP-välimuistin eli muiden koneiden IP- ja MAC-osoitteet ennen hyökkäystä.
- **`mininet> xterm h3`**: Avaa erillisen pääteikkunan virtuaalikoneelle `h3` (hyökkääjä).
- **`Node: h3> sysctl -w net.ipv4.ip_forward=1`**: Kytkee IP-pakettien edelleenlähetyksen päälle `h3`:ssa, jotta verkkoliikenne ei katkea hyökkäyksen aikana.
- **`Node: h3> iptables -t nat -A PREROUTING -p tcp --dport 22 -j REDIRECT --to-port 22`**: Ohjaa `h3`:n läpi kulkevan, muualle osoitetun SSH-liikenteen (portti 22) `h3`:n omaan paikalliseen porttiin 22.
- **`Node: h3> tcpdump -i h3-eth0 -w arp-p.pcap &`**: Tallentaa kaiken `h3-eth0`-verkkoliitännän liikenteen taustalla `arp-p.pcap`-tiedostoon myöhempää tarkastelua varten.
- **`Node: h3> wireshark &`**: Käynnistää Wireshark-pakettianalysaattorin taustalle verkkoliikenteen reaaliaikaista seurantaa varten.
- **`Node: h3> sshesame -port 22 -listen_address 0.0.0.0`**: Käynnistää SSH-hunajapurkin (valepalvelimen), joka ottaa vastaan kaapatut SSH-yhteydet ja tallentaa kirjautumisyritykset sekä komennot.
- **`Node: h3> python3 ./arp_poison.py`**: Käynnistää Python-skriptin, joka lähettää väärennettyjä ARP-vastauksia ja uskottelee `h1`:lle ja `h2`:lle, että `h3` on niiden kohde (MAC-osoitteiden väärennys).
- **`Node: h1> ssh h2`**: `h1` yrittää ottaa SSH-yhteyden `h2`-koneeseen, mutta ARP-myrkytyksen ja `iptables`-ohjauksen vuoksi yhteys päätyykin salaa hyökkääjän (`h3`) `sshesame`-valepalvelimelle.
- **`Node: h1> ssh-keygen -R h2`**: Poistaa `h2`-palvelimen aiemmin tallennetun julkisen avaimen `h1`:n `known_hosts`-tiedostosta, jotta yhteys ei esty suoraan avainristiriidan vuoksi.
- **`Node: h1> ssh h2`**: `h1` ottaa uudelleen SSH-yhteyden `h2`-osoitteeseen, mutta liikenne ohjautuu ARP-myrkytyksen vuoksi hyökkääjän `sshesame`-hunajapurkille.
- **`Warning: the ED25519 host key for 'h2'ja niin edelleen`**: SSH-asiakas varoittaa, että palvelimen sormenjälki on muuttunut (koska vastassa on nyt `h3` eikä oikea `h2`), mikä on klassinen merkki Man-in-the-Middle-hyökkäyksestä.
- **`Are you sure you want to continue connecting (yes/no)? Yes`**: Käyttäjä ohittaa varoituksen ja hyväksyy hyökkääjän väärennetyn palvelinavaimen.

Kaikkien annettujen komentojen jälkeen tilanne näytti tältä:

<img width="1701" height="1375" alt="kuva" src="https://github.com/user-attachments/assets/64b28516-facd-4181-b1b4-e87a4458d8ba" />

Kuva 3. ARP-hyökkäys.

Kuvassa värit kertovat seuraavaa:

- **Sininen:** ARP-hyökkäys Wiresharkin kaappaamana.
- **Keltainen:** H3-node, joka ajaa Python-skriptillä ARP-hyökkäystä.
- **Vihreä:** H1-node, jolla yhdistetään H2-nodeen SSH:lla hyökkäyksen aikana.
- **Punainen:** H3-nodella pyörivä `sshesame`-hunajapurkki, joka kaappaa ja tunnistaa H2-nodeen tarkoitetun kirjautumisen ja autentikoinnin.

### Miten ARP-hyökkäys toimii kokonaisuutena?

ARP-protokolla (Address Resolution Protocol) on tilaton eikä se todenna viestien lähettäjää, minkä vuoksi lähiverkon laitteet päivittävät ARP-välimuistinsa sokeasti viimeisimmän vastaanotetun ARP-vastauksen (`is-at`) perusteella (MITRE ATT&CK, T1557.002). Hyökkääjä (`h3`) hyödyntää tätä luottamusta lähettämällä jatkuvasti väärennettyjä ARP-vastauksia sekä `h1`:lle (väittäen olevansa `h2`) että `h2`:lle (väittäen olevansa `h1`) (MITRE ATT&CK, T1557.002).

Tämän seurauksena molempien uhrien ARP-tauluihin tallentuu `h3`:n MAC-osoite vastapuolen IP-osoitteen kohdalle, ja kaikki niiden välinen liikenne alkaa kulkea `h3`:n kautta (Adversary-in-the-Middle / Man-in-the-Middle) (MITRE ATT&CK, T1557.002). Koska `h3`:ssa on kytketty päälle IP-pakettien edelleenlähetys (`ip_forward=1`) ja `iptables`-sääntö ohjaa portin 22 liikenteen paikalliselle `sshesame`-hunajapurkille, `h1`:n SSH-yhteys päätyy hyökkääjän valepalvelimelle.

# b) Samassa hakemistossa on myös ICMP Spoofing ja TCP Session Hijacking. Aja molemmat labrat läpi ja kerro, miten molemmat tekniikat toimivat.

# c) Hakemistossa 02-SDN-DDos_Simulation tryout-kansiossa on työkalut, jotta voit ajaa TCP SYN-Flood-hyökkäyksen turvallisesti. Kirjoita, miten ajoit hyökkäyksen ja miten kyseinen hyökkäys toimii.

## Lähteet
Iso-Anttila, L. 2026. Verkkoon tunkeutuminen ja tiedustelu. Luento 17.11.2026. Haaga-Helia ammattikorkeakoulu.

https://hhmoodle.haaga-helia.fi/pluginfile.php/5055842/mod_resource/content/1/03-mininet.pdf

https://attack.mitre.org/techniques/T1557/002/
