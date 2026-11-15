# h4 - NFC ja RFID

# a) Tarkastele käytössäsi olevia RFID tuotteita, mieti miten hyvin olet suojautunut RFID urkinnalta?

RFID (Radio Frequency Identification) ja sen lähilukuun tarkoitettu alalaji NFC (Near Field Communication) ovat mukana lähes jokaisen arjessa. Kävin läpi lompakkoni, avaimeni ja laitteeni, sieltä löytyy luultavasti ainakin osa seuraavista:

* Maksukortit (lähimaksu): Pankkikortti
* Kulunvalvonta-avaimet: Työpaikan lätkät
* Älypuhelin: Lähimaksuominaisuudet (Apple Pay)
* Muut: Kirjastokortti

Olen suojautunut RFID-urkinnalta käyttämällä RFID-suojattua lompakkoa. Lisäksi minimoin riskejä olemalla kantamatta mukanani ylimääräisiä tai "turhia" kortteja, jolloin mahdollisen tietovuodon tai lompakon katoamisen seuraukset jäävät pienemmiksi. Maksutapana suosin Apple Payta, joka on fyysistä korttia turvallisempi, sillä se vaatii biometrisen tunnistautumisen ennen kuin NFC-siru aktivoituu ja maksu menee läpi.

NISTin (Karygiannis ym., 2007, s. 56) mukaan RFID-järjestelmiin kohdistuvien tietoturva- ja yksityisyysriskien tasoon vaikuttavat erityisesti neljä tekijää: hyökkääjän motivaatio ja kyvykkyys, tunnisteelta saatavan tiedon hyödyllisyys, komponenttien sijainti sekä käytössä olevat suojamekanismit.

Omassa arjessani riski korostuu julkisilla paikoilla liikuttaessa, sillä fyysisesti suojaamattomia RFID-tunnisteita voidaan periaatteessa lukea etäältä ilman käyttäjän huomaamista. Maksukorttien osalta urkinnasta saatavan tiedon hyödyllisyys on kuitenkin rajallinen, sillä nykyaikaiset sirukortit hyödyntävät salausta eivätkä luovuta etälukuun kaikkia väärinkäyttöön tarvittavia tietoja (kuten CVV-turvakoodia). Sen sijaan kulunvalvontaan tarkoitetut tunnisteet muodostavat suuremman riskin: kuten NISTin ohjeistuksessa huomautetaan, pelkkä tunnisteen sisältämä yksilöivä tunnistenumerokin (identifier) voi olla hyökkääjälle arvokas kulunvalvontasovelluksissa, jos tunnisteessa ei ole kryptografista suojausta tai salasanasuojausta (Karygiannis ym., 2007, s. 56). Lisäksi suojaamattomien tunnisteiden olemassaolo voi altistaa yksityisyysriskeille, kuten henkilön liikkeiden jäljittämiselle julkisissa tiloissa.

# b) Tutustu APDU komentojen rakenteeseen (voit käyttää tekoälyä tutustumiseen)

Aloitin tehtävän kysymällä Googlen Gemini-tekoälymallilta APDU-komentojen rakentamisesta ja perusrakenteesta. Tekoälyn tuottaman yhteenvedon pohjalta perehdyin tarkemmin siihen, miten kortinlukijan ja älykortin välinen tietoliikenne toimii ISO/IEC 7816-4 -standardin mukaisesti.

Älykorttien yhteydessä APDU (Application Protocol Data Unit) on kortinlukijan ja älykortin välinen tietoliikenneyksikkö. APDU:n rakenne on määritelty ISO/IEC 7816-4 -standardissa (Organization, security and commands for interchange). Kortinlukijan lähettämä komentokehys sisältää pakollisen 4 tavun otsikon (Header) sekä valinnaisesti 0–65 535 tavua dataa. Älykortin lukijalle palauttama vastauskehys sisältää 0–65 536 tavua dataa sekä 2 pakollista tilatavua. (Wikipedia, 2025).

# c) Tutki ja kerro minkä mielenkiintoisen RFID hakkerointi uutiset löysit.

Tutustuin älykorttien tietoturvaan perehtymällä SecurityWeek-sivuston artikkeliin (Major Backdoor in Millions of RFID Cards Allows Instant Cloning), joka käsittelee ranskalaisen tietoturvayritys Quarkslabin tutkijan Philippe Teuwenin löytämää vakavaa laitteistotason takaovea yleisissä RFID-kulkukorteissa (Naraine, 2024).

Tutkimuksen kohteena oli alun perin Philipsin (nykyisin NXP Semiconductors) vuonna 1994 julkaisema MIFARE Classic -korttiperhe, jota käytetään laajasti muun muassa julkisessa liikenteessä, toimistojen kulunvalvonnassa sekä hotellien avainkorteissa maailmanlaajuisesti. Koska alkuperäisiin MIFARE Classic -kortteihin on vuosien saatossa löydetty useita haavoittuvuuksia, kiinalainen siruvalmistaja Shanghai Fudan Microelectronics Group julkaisi vuonna 2020 parannellun FM11RF08S-sirun. Sen tarkoituksena oli estää tunnetut ns. card-only-hyökkäykset, joissa hyökkääjä pystyy murtamaan ja kloonaamaan kortin pelkästään pääsemällä lähelle korttia ilman yhteyttä varsinaiseen kortinlukijaan (Naraine, 2024).

Tutkiessaan FM11RF08S-sirun suojausmekanismeja (static encrypted nonce) Teuwen kehitti aluksi menetelmän, jolla kortin salausavaimet pystyttiin murtamaan muutamassa minuutissa, mikäli samaa avainta oli käytetty uudelleen vähintään kolmessa sektorissa tai kolmessa kortissa. Jatkotutkimuksissa paljastui kuitenkin varsinainen laitteistotason takaovi (hardware backdoor), joka mahdollistaa todentamisen tuntemattomalla salausavaimella (Naraine, 2024).

Takaoven vuoksi kuka tahansa yleisavaimen tunteva taho pystyy ohittamaan kortin suojaukset, lukemaan ja muokkaamaan kortin sisältöä sekä kloonaamaan kortin täydellisesti, vaikka kortin käyttäjäkohtaiset salausavaimet olisi määritetty yksilöllisesti ja oikeaoppisesti (Naraine, 2024)

## Lähteet: 
Apple Inc. 2026. Apple Payn suojaus ja tietosuoja. Verkkosivusto. Luettavissa: https://support.apple.com/fi-fi/101554. Luettu: 15.11.2026.

Google (2026). Gemini (15.11.2026 Hydrogen 4). Saatavilla: https://gemini.google.com. Käytetty 15.11.2026.

Iso-Anttila, L. 2026. Verkkoon tunkeutuminen ja tiedustelu. Luento 10.11.2026. Haaga-Helia ammattikorkeakoulu.

Karvinen, T. 2026. Verkkoon tunkeutuminen ja tiedustelu. Verkkosivusto. Luettavissa: https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/. Luettu: 15.11.2026.

Karygiannis, T., Eydt, B., Barber, G., Bunn, L. & Phillips, T. 2007. Guidelines for Securing Radio Frequency Identification (RFID) Systems. NIST Special Publication 800-98. National Institute of Standards and Technology. Luettavissa: https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-98.pdf. Luettu: 15.11.2026.

Wikipedia (2025). Smart card application protocol data unit. Päivitetty 28.12.2025. Saatavilla: https://en.wikipedia.org/wiki/Smart_card_application_protocol_data_unit. Luettu 15.11.2026.

Naraine, R. (2024). Major Backdoor in Millions of RFID Cards Allows Instant Cloning. SecurityWeek, 20.8.2024. Saatavilla: https://www.securityweek.com/major-backdoor-in-millions-of-rfid-cards-allows-instant-cloning/. Luettu 15.11.2026.
