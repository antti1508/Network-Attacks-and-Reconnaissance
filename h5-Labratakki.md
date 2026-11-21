# h5 Labratakki

# a) Aja tunnilla esitetty ARP hyökkäys ja tutki, miten se toimii.

Tässä harjoituksessa käytetään ympäristönä Larin mininet virtuaalikone tiedostoa ja VMware alustaa (fuck broadcom). "Pienten" ongelmien jälkeen sain VMwaren ladattua ja virtuaalikoneen päälle. Noin parin tunnin säätämisen jälkeen sain infran pystyyn :D.

<img width="1524" height="412" alt="kuva" src="https://github.com/user-attachments/assets/75969d48-f2ad-4210-8860-3c8394a86a63" />

Kuva 1. Mininet toiminnassa.

Seuraavaksi seurasin tunnilta saatuja ohjeita ja ajoin ARP-hyökkäyksen ohjeiden mukaisesti eli ajoin seuraavat komennot järjestyksessä:

* **`ryu-manager ryu.app.simple_switch_13`**: Käynnistää Ryu SDN -kontrollerin perusmuotoisena oppivana OpenFlow 1.3 -kytkimenä.
* **`sudo mn --topo single,3 --mac --switch ovsk --controller remote`**: Luo Mininetillä verkon, jossa on yksi Open vSwitch -kytkin, kolme virtuaalikonetta (`h1`, `h2`, `h3`), selkeät MAC-osoitteet (`--mac`) ja yhteys ulkoiseen kontrolleriin.
* **`mininet> pingall`**: Testaa, että kaikki koneet (`h1`–`h3`) saavat yhteyden toisiinsa ja täyttää samalla niiden ARP-taulut.
* **`mininet> xterm h1`**: Avaa oman erillisen pääteikkunan virtuaalikoneelle `h1` (uhri).
* **`Node: h1> arp –n`**: Näyttää `h1`-koneen ARP-välimuistin, eli muiden koneiden IP- ja MAC-osoitteet ennen hyökkäystä.
* **`mininet> xterm h3`**: Avaa erillisen pääteikkunan virtuaalikoneelle `h3` (hyökkääjä).
* **`Node: h3> sysctl -w net.ipv4.ip_forward=1`**: Laittaa IP-pakettien edelleenlähetyksen päälle `h3`:ssa, jotta liikenne ei katkea hyökkäyksen aikana.
* **`Node: h3> iptables -t nat -A PREROUTING -p tcp --dport 22 -j REDIRECT --to-port 22`**: Ohjaa `h3`:n läpi kulkevan, muualle osoitetun SSH-liikenteen (portti 22) `h3`:n omaan paikalliseen porttiin 22.
* **`Node: h3> tcpdump –i h3-eth0 –w arp-p.pcap &`**: Tallentaa kaiken `h3-eth0`-verkkoliitännän liikenteen taustalla `arp-p.pcap`-tiedostoon myöhempää tarkastelua varten.
* **`Node: h3> wireshark &`**: Käynnistää Wireshark-pakettianalysaattorin taustalle verkkoliikenteen reaaliaikaista seurantaa varten.
* **`Node: h3> sshesame -port 22 -listen_address 0.0.0.0`**: Käynnistää SSH-hunajapurkin (valepalvelimen), joka ottaa vastaan kaapatut SSH-yhteydet ja tallentaa kirjautumisyritykset/komennot.
* **`Node: h3> python3 ./arp_poison.py`**: Käynnistää Python-skriptin, joka lähettää väärennettyjä ARP-vastauksia ja uskottelee `h1`:lle ja `h2`:lle, että `h3` on niiden kohde (MAC-osoitteiden väärennys).
* **`Node: h1> ssh h2`**: `h1` yrittää ottaa SSH-yhteyden `h2`-koneeseen, mutta ARP-myrkytyksen ja `iptables`-ohjauksen vuoksi yhteys päätyykin salaa hyökkääjän (`h3`) `sshesame`-valepalvelimelle.

# b) Samassa hakemistossa on myös ICMP Spoofing ja TCP Session Hijacking. Aja molemmat labrat läpi ja kerro, miten molemmat tekniikat toimivat.

# c) Hakemistossa 02-SDN-DDos_Simulation tryout-kansiossa on työkalut, jotta voit ajaa TCP SYN-Flood-hyökkäyksen turvallisesti. Kirjoita, miten ajoit hyökkäyksen ja miten kyseinen hyökkäys toimii.

## Lähteet
Iso-Anttila, L. 2026. Verkkoon tunkeutuminen ja tiedustelu. Luento 17.11.2026. Haaga-Helia ammattikorkeakoulu.
