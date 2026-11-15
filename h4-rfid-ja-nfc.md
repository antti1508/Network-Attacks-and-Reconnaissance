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

# c) Tutki ja kerro minkä mielenkiintoisen RFID hakkerointi uutiset löysit. 

## Lähteet: 
Apple Inc. 2026. Apple Payn suojaus ja tietosuoja. Verkkosivusto. Luettavissa: https://support.apple.com/fi-fi/101554. Luettu: 15.11.2026.

Iso-Anttila, L. 2026. Verkkoon tunkeutuminen ja tiedustelu. Luento 10.11.2026. Haaga-Helia ammattikorkeakoulu.

Karvinen, T. 2026. Verkkoon tunkeutuminen ja tiedustelu. Verkkosivusto. Luettavissa: https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/. Luettu: 15.11.2026.

Karygiannis, T., Eydt, B., Barber, G., Bunn, L. & Phillips, T. 2007. Guidelines for Securing Radio Frequency Identification (RFID) Systems. NIST Special Publication 800-98. National Institute of Standards and Technology. Luettavissa: https://nvlpubs.nist.gov/nistpubs/Legacy/SP/nistspecialpublication800-98.pdf. Luettu: 15.11.2026.
