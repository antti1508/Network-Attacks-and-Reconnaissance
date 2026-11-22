# h5 Labratakki

# a) Aja tunnilla esitetty ARP-hyökkäys ja tutki, miten se toimii.

Tässä harjoituksessa käytetään ympäristönä Larin Mininet-virtuaalikonetiedostoa ja VMware-alustaa (Iso-Anttila 2026a). "Pienten :D" alkuongelmien jälkeen sain VMwaren asennettua ja virtuaalikoneen käyntiin. Noin parin tunnin konfiguroinnin jälkeen ympäristö oli toimintavalmis.

<img width="1524" height="412" alt="kuva" src="https://github.com/user-attachments/assets/75969d48-f2ad-4210-8860-3c8394a86a63" />

Kuva 1. Mininet toiminnassa.

Seuraavaksi seurasin tunnilta saatuja ohjeita ja ajoin ARP-hyökkäyksen ohjeiden mukaisesti suorittamalla seuraavat komennot järjestyksessä (Iso-Anttila 2026a; Iso-Anttila 2026b):

- **`ryu-manager ryu.app.simple_switch_13`**: Käynnistää Ryu SDN -kontrollerin perusmuotoisena oppivana kytkimenä.
- **`sudo mn --topo single,3 --mac --switch ovsk --controller remote`**: Luo Mininetillä verkon, jossa on yksi Open vSwitch -kytkin, kolme virtuaalikonetta (`h1`, `h2`, `h3`), selkeät MAC-osoitteet (`--mac`) ja yhteys ulkoiseen kontrolleriin.

Kuvassa 2 Mininet on käynnissä, ja virtuaalikoneeseen on otettu yhteys SSH:lla:

<img width="1718" height="1020" alt="kuva" src="https://github.com/user-attachments/assets/4ee83a44-9613-4636-a3c9-ea020f330421" />

Kuva 2. Mininet käynnissä.

Tämän jälkeen työasemalle piti vielä asentaa X11-palvelin, jotta virtuaalikoneiden erilliset pääteikkunat saatiin avattua. Minun täytyi myös ajaa xtermiä varten skripti komennolla ```./get_xauth.sh``` ja ```sudo -s xauth add mininet-vm/unix:10 MIT-MAGIC-COOKIE-1 <palaute komennosta get_xauth.sh>``` (Iso-Anttila 2026a).

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
- **`Warning: the ED25519 host key for 'h2' ja niin edelleen`**: SSH-asiakas varoittaa, että palvelimen sormenjälki on muuttunut (koska vastassa on nyt `h3` eikä oikea `h2`), mikä on klassinen merkki Man-in-the-Middle-hyökkäyksestä.
- **`Are you sure you want to continue connecting (yes/no)? Yes`**: Käyttäjä ohittaa varoituksen ja hyväksyy hyökkääjän väärennetyn palvelinavaimen.
- **`Node: h1> ssh-keygen –R h2`**: Poistetaan vanha avain

Kaikkien annettujen komentojen jälkeen tilanne näytti tältä:

<img width="1701" height="1375" alt="kuva" src="https://github.com/user-attachments/assets/64b28516-facd-4181-b1b4-e87a4458d8ba" />

Kuva 3. ARP-hyökkäys.

Kuvassa värit kertovat seuraavaa:

- **Sininen:** ARP-hyökkäys Wiresharkin kaappaamana.
- **Keltainen:** H3-node, joka ajaa Python-skriptillä ARP-hyökkäystä.
- **Vihreä:** H1-node, jolla yhdistetään H2-nodeen SSH:lla hyökkäyksen aikana.
- **Punainen:** H3-nodella pyörivä `sshesame`-ohjelma, joka kaappaa ja tunnistaa H2-nodeen tarkoitetun kirjautumisen ja autentikoinnin.

### Miten ARP-hyökkäys toimii kokonaisuutena?

ARP-protokolla (Address Resolution Protocol) on tilaton eikä se todenna viestien lähettäjää, minkä vuoksi lähiverkon laitteet päivittävät ARP-välimuistinsa sokeasti viimeisimmän vastaanotetun ARP-vastauksen (`is-at`) perusteella (MITRE 2024b). Hyökkääjä (`h3`) hyödyntää tätä luottamusta lähettämällä jatkuvasti väärennettyjä ARP-vastauksia sekä `h1`:lle (väittäen olevansa `h2`) että `h2`:lle (väittäen olevansa `h1`) (MITRE 2024b).

Tämän seurauksena molempien uhrien ARP-tauluihin tallentuu `h3`:n MAC-osoite vastapuolen IP-osoitteen kohdalle, ja kaikki niiden välinen liikenne alkaa kulkea `h3`:n kautta (Man-in-the-Middle) (MITRE 2024b). Koska `h3`:ssa on kytketty päälle IP-pakettien edelleenlähetys (`ip_forward=1`) ja `iptables`-sääntö ohjaa portin 22 liikenteen paikalliselle `sshesame` -ohjelmalle, `h1`:n SSH-yhteys päätyy hyökkääjän valepalvelimelle (Iso-Anttila 2026b).

# b) Samassa hakemistossa on myös ICMP Spoofing ja TCP Session Hijacking. Aja molemmat labrat läpi ja kerro, miten molemmat tekniikat toimivat.

## 1. ICMP Spoofing

Aloitin ICMP Spoofing -harjoituksen käynnistämällä Mininetin `hub_topo.py`-topologialla ja avaamalla `xterm`-komennolla omat terminal-ikkunat kaikille kolmelle hostille (`h1`, `h2` ja `h3`) (ssam246 2024).

Repositorion `README.md`-tiedoston *Usage*-ohjeessa oli mainittu vain liikenteen kuuntelu (`sniff_icmp.py`) ja pingaaminen, joten varsinaisen hyökkäyksen todentamiseksi ajoin lisäksi kansiossa olevan `spoof_icmp.py`-skriptin kolmannella hostilla (ssam246 2024):

- **`Node: h3> python spoof_icmp.py`**: Käynnistää hyökkääjän koneella Scapy-skriptin, joka tarkkailee verkon ICMP-liikennettä ja lähettää väärennettyjä ICMP Echo Reply -paketteja.
- **`Node: h1> python sniff_icmp.py`**: Käynnistää kohdekoneella kuunteluskriptin, joka tulostaa kaikki verkossa liikkuvat ICMP-paketit.
- **`Node: h2> ping 10.0.0.1`**: Asiakas lähettää normaaleja ICMP Echo Request -paketteja kohteelle `h1` (`10.0.0.1`).

<img width="2543" height="1380" alt="kuva" src="https://github.com/user-attachments/assets/c6bb19e0-088f-4ab5-8ef9-2f5d4376b01a" />

Kuva 4. ICMP Spoofing toiminnassa (vasemmalla hyökkääjä `h3`, keskellä pingaava `h2` ja oikealla liikennettä seuraava `h1`).

### Havainnot ja miten ICMP Spoofing toimii

ICMP (Internet Control Message Protocol) on Layer 3 -tason protokolla, jota käytetään verkkoyhteyksien testaamiseen (`ping`) ja virheilmoituksiin (Postel 1981, 1). Protokolla ei tarkista paketin lähettäjän henkilöllisyyttä, joten vastaanottaja ei pysty varmistamaan, kuka paketin on oikeasti lähettänyt (Postel 1981, 1–2).

Tässä harjoituksessa hyökkäys etenee seuraavasti:
1. Kun `h2` lähettää ICMP Echo Request -pyynnön osoitteeseen `10.0.0.1`, hub-verkossa oleva hyökkääjä (`h3`) näkee liikenteen ja nappaa paketin (ssam246 2024).
2. Hyökkääjän `spoof_icmp.py`-skripti kopioi pyynnöstä tarvittavat tiedot ja rakentaa oman ICMP Echo Reply -vastauksen, jonka source IP -osoitteeksi on väärennetty `10.0.0.1` (`Sent 1 packets.`) (ssam246 2024).
3. Koska myös oikea `h1` vastaa ping-pyyntöön, oikeanpuoleisessa `h1`-ikkunassa näkyy jokaiselle pyynnölle **kaksi** `10.0.0.1 > 10.0.0.2 echo-reply` -vastausta: kohdekoneen `h1` oma vastaus sekä hyökkääjän `h3` väärentämä vastaus.

Laajemmissa hyökkäyksissä ICMP Spoofingia hyödynnetään esimerkiksi ICMP Redirect (Type 5) -viesteillä, joilla uhrin routing table saadaan ohjaamaan liikenne hyökkääjän kautta (MITRE 2024a), tai palvelunestohyökkäyksissä, joissa uhrin IP-osoitteella lähetetään suuri määrä väärennettyjä ping-pyyntöjä verkon broadcast-osoitteisiin.

## 2. TCP Session Hijacking

Jatkoin TCP Session Hijacking -harjoitukseen suoraan edellisestä tehtävästä valmiiksi käynnissä olleessa Mininetin `hub_topo.py`-ympäristössä hyödyntäen samoja kolmea hostia (`h1`, `h2` ja `h3`) (ssam246 2024).

Testasin aluksi repositorion `README.md`-ohjeessa mainittuja erillisiä `sniff_tcp_session.py`- ja `tcp_hijack.py`-skriptejä (ssam246 2024), mutta havaitsin, että `tcp_hijack.py` käytti kovakoodattua lähdeporttia (`52632`) eikä ehtinyt osua lyhytkestoiseen TCP-istuntoon ennen sen sulkeutumista. Tämän vuoksi käytin kansiossa ollutta `tcp_sniff_and_hijack.py`-skriptiä (ssam246 2024), joka kuuntelee aktiivista yhteyttä reaaliajassa ja injektoi paketin lennosta:

- **`Node: h1> python tcp_server.py`**: Käynnistää TCP-palvelimen, joka jää kuuntelemaan yhteyksiä porttiin `12345` ja tulostaa vastaanotetut viestit.
- **`Node: h3> python tcp_sniff_and_hijack.py`**: Käynnistää hyökkääjän koneella skriptin, joka tarkkailee `h3-eth0`-liitäntää, poimii aktiivisen istunnon portti- sekä SEQ/ACK-numerot ja lähettää väärennetyn TCP-paketin.
- **`Node: h2> python tcp_client.py`**: Asiakas muodostaa TCP-yhteyden osoitteeseen `10.0.0.1:12345`, lähettää viestin `Hello from Host 2` ja vastaanottaa palvelimen vastauksen.

<img width="2483" height="1351" alt="kuva" src="https://github.com/user-attachments/assets/f463a229-9d61-48df-aa3d-fd7eee8bc88d" />

Kuva 5. TCP Session Hijacking toiminnassa (vasemmalla hyökkääjä `h3`, keskellä asiakas `h2` ja oikealla palvelin `h1`).

### Havainnot ja miten TCP Session Hijacking toimii

TCP (Transmission Control Protocol) on yhteydellinen Layer 4 -tason protokolla, joka pitää yllä aktiivista istuntoa osapuolten välillä IP-osoitteiden, porttinumeroiden sekä vaihtuvien **SEQ** (*Sequence Number*) ja **ACK** (*Acknowledgement Number*) -numeroiden avulla (OWASP 2024a). Koska salaamaton TCP-liikenne ei sisällä kryptografista eheyden tai lähettäjän tarkistusta, samassa verkkosegmentissä oleva hyökkääjä pystyy lukemaan nämä arvot verkosta ja syöttämään istuntoon omaa dataansa (OWASP 2024a).

Tässä harjoituksessa kaappaus etenee seuraavasti:
1. Kun asiakas (`h2`) ja palvelin (`h1`) avaavat TCP-yhteyden ja vaihtavat viestejä, hub-verkossa oleva hyökkääjä (`h3`) havaitsee liikenteen (`Sniffed TCP Data Packet`) (ssam246 2024).
2. Hyökkääjän `tcp_sniff_and_hijack.py`-skripti lukee paketista dynaamisen lähdeportin (`57698`) sekä senhetkiset SEQ- ja ACK-numerot (OWASP 2024a; ssam246 2024). Näiden tietojen pohjalta skripti rakentaa väärennetyn TCP-paketin, jonka *source IP* -osoitteeksi on huijattu `10.0.0.2` (`Injecting modified TCP payload: 10.0.0.2:57698 -> 10.0.0.1:12345`).
3. Oikeanpuoleisessa `h1`-ikkunassa näkyy, kuinka palvelin vastaanottaa ensin oikean asiakkaan viestin (`Received: Hello from Host 2`) ja heti sen perään hyökkääjän injektoiman viestin (`Received: Hello from Host 1 You are hacked!`). Koska paketin portti- ja sekvenssinumerot täsmäävät avoimeen yhteyteen, palvelin hyväksyy paketin osaksi samaa luotettua istuntoa (OWASP 2024a).
4. Lopuksi palvelimen päätteessä näkyy `ConnectionResetError: [Errno 104] Connection reset by peer`. Tämä johtuu siitä, että hyökkääjän syöttämä ylimääräinen payload kasvatti palvelimen odottamaa SEQ-numeroa, jolloin asiakkaan (`h2`) ja palvelimen (`h1`) tilat menivät epätahtiin ja yhteys katkesi TCP RST -pakettiin (OWASP 2024a).

Tuotantoverkoissa TCP Session Hijacking estetään käyttämällä salattuja protokollia (kuten TLS/HTTPS tai SSH), jolloin ulkopuolinen tarkkailija ei pysty lukemaan liikennettä eikä muokkaamaan paketteja ilman salausavaimia (OWASP 2024a).

## c) TCP SYN-Flood -hyökkäys SDN-ympäristössä

Toteutin TCP SYN-Flood -harjoituksen eristetyssä Mininet-simulaatioympäristössä (`02-SDN_DDoS_Simulation-tryout`), joka pohjautuu avoimeen SDN DDoS -simulaatioprojektiin (Santhisenan 2020). Aluksi siivosin edellisten harjoitusten verkkoliitännät (`sudo mn -c`), jotta uusi virtuaaliverkko käynnistyi ilman ristiriitoja.

Harjoituksen suorittaminen vaati komponenttien käynnistämisen tietyssä järjestyksessä:
- **`ryu-manager simple_switch_13.py &`**: Käynnisti ensin SDN-ohjaimen (*SDN controller*) taustalle kuuntelemaan verkon kytkimiä.
- **`sudo python3 tree_topology.py`**: Loi virtuaalisen puutopologian (*tree topology*), jossa on seitsemän kytkintä (`s1–s7`) ja useita host-koneita, sekä aloitti automaattisesti vuorottelevat hyökkäysepisodit (*Episode 0, 1, 2...*) (Santhisenan 2020).
- **`sudo wireshark &`**: Käynnistettiin liikenteen tarkkailua varten, ja kaappaus (*live capture*) kohdistettiin suoraan kytkimen `s4-eth2`-verkkoliitäntään.

<img width="2424" height="1345" alt="kuva" src="https://github.com/user-attachments/assets/2380cdae-32df-4365-aaa8-382b2f0fe96d" />

Kuva 6. DDoS Flooding -simulaatio ja verkkoliikenteen kaappaus Wiresharkissa.

Kuvakaappaukseen on korostettu kolme keskeistä kohtaa:
- **Punainen laatikko (vasen pääteikkuna):** Näyttää käynnissä olevan Mininet-simulaation (`tree_topology.py`), joka ajaa peräkkäisiä hyökkäysepisodeja (*Episode 5–11*), joissa yksi host lähettää DDoS-tulvaliikennettä samalla kun toinen host lähettää normaalia liikennettä.
- **Vihreä laatikko (Wiresharkin pakettilista):** Osoittaa kaapattua verkonsisäistä DDoS-hyökkäysliikennettä, jossa hyökkääjä (`10.0.0.4`) lähettää peräkkäisiä TCP-paketteja kasvavista lähdeporteista (`2649–2654`) kohdeosoitteeseen `10.0.0.8` (portti `8`), sekä kohteen lähettämän `RST, ACK` -vastauksen (punaisella korostettu rivi). Lisäksi listassa näkyy verkon osoitteenselvitykseen liittyviä ARP-kyselyitä.
- **Sininen laatikko (Wiresharkin alareuna):** Todentaa, että kaappaus suoritettiin suoraan Mininet-kytkimen `s4-eth2`-verkkoliitännältä (`s4-eth2: <live capture in progress>`).

### Havainnot ja miten TCP SYN-Flood toimii

TCP SYN-Flood on palvelunestohyökkäys (Denial of Service, DoS), joka hyödyntää TCP-yhteyden muodostamiseen käytettävää kolmiosaista kättelyä (OWASP 2024b). Normaalissa yhteydenotossa asiakas lähettää ensin `SYN`-paketin, palvelin vastaa `SYN-ACK`-paketilla ja asiakas vahvistaa yhteyden `ACK`-paketilla (OWASP 2024b).

Tässä harjoituksessa hyökkäys toimii seuraavasti:
1. **Half-open connections:** Hyökkäävä host (kuvassa `10.0.0.4`) lähettää kohteelle (`10.0.0.8`) suuren määrän yhteyspyyntöjä vaihtuvista lähdeporteista (`2649–2654`) suorittamatta kättelyä loppuun (OWASP 2024b).
2. **Resurssien täyttyminen:** Oikeassa palvelinympäristössä kohdekone varaa jokaiselle saapuvalle `SYN`-pyynnölle muistia ja yhteysjonon (*backlog queue*) odottaessaan asiakkaan viimeistä `ACK`-kuittausta (OWASP 2024b). Kun pyyntöjä tulee jatkuvana tulvana, yhteysjonot täyttyvät eivätkä oikeat käyttäjät enää pääse muodostamaan yhteyttä palvelimeen (OWASP 2024b).
3. **Wireshark-havainnot simulaatiossa:** Koska simulaation kohdehostilla (`10.0.0.8`) ei ollut varsinaista palvelinsovellusta kuuntelemassa porttia `8`, kohdekone hylkäsi tulvivat paketit välittömästi lähettämällä `RST, ACK` -paketin (*Reset*), mikä näkyy Wiresharkissa punaisella korostettuna rivinä.

Tuotantoverkoissa TCP SYN-Flood -hyökkäyksiltä suojaudutaan esimerkiksi rajoittamalla saapuvien yhteyspyyntöjen määrää, käyttämällä SYN cookies -tekniikkaa (joka estää muistin varaamisen ennen kättelyn valmistumista) sekä suodattamalla poikkeavaa liikennettä palomuurin tai SDN-ohjaimen sääntöjen avulla (OWASP 2024b).

## Lähteet

Iso-Anttila, L. 2026a. 03-mininet. Oppimateriaali. Haaga-Helia ammattikorkeakoulu. Luettavissa: https://hhmoodle.haaga-helia.fi/pluginfile.php/5055842/mod_resource/content/1/03-mininet.pdf. Luettu: 22.11.2026.

Iso-Anttila, L. 2026b. Verkkoon tunkeutuminen ja tiedustelu. Luento 17.11.2026. Haaga-Helia ammattikorkeakoulu.

MITRE 2024a. Adversary-in-the-Middle. MITRE ATT&CK. Luettavissa: https://attack.mitre.org/techniques/T1557/. Luettu: 22.11.2026.

MITRE 2024b. Adversary-in-the-Middle: ARP Cache Poisoning. MITRE ATT&CK. Luettavissa: https://attack.mitre.org/techniques/T1557/002/. Luettu: 22.11.2026.

OWASP 2024a. Session hijacking attack. OWASP Foundation. Luettavissa: https://owasp.org/www-community/attacks/Session_hijacking_attack. Luettu: 22.11.2026.

OWASP 2024b. SYN Flood. OWASP Foundation. Luettavissa: https://owasp.org/www-community/attacks/SYN_Flood. Luettu: 22.11.2026.

Postel, J. 1981. RFC 792: Internet Control Message Protocol. Internet Engineering Task Force (IETF). Luettavissa: https://datatracker.ietf.org/doc/html/rfc792. Luettu: 22.11.2026.

Santhisenan 2020. SDN_DDoS_Simulation. GitHub-repositorio. Luettavissa: https://github.com/santhisenan/SDN_DDoS_Simulation. Luettu: 22.11.2026.

ssam246 2024. Network-Security-Lab. GitHub-repositorio. Luettavissa: https://github.com/ssam246/Network-Security-Lab. Luettu: 22.11.2026.

Tekoälyn hyödyntäminen: Raportin tekstin oikeinkirjoituksen ja kieliasun tarkastamisessa on hyödynnetty Google Gemini -tekoälyä (22.11.2026).
