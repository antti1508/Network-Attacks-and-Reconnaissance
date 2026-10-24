# h1 Korva netissä

x) Spektrini. Piirrä oma sähkömagneettinen spektri. Eli jana, jossa taajuus kasvaa. Ja 3-10 itseäsi kiinnostavaa sovellusta. Saavat olla aivan tavanomaisia. Merkitse nuolilla, mihin suuntaa kasvavat: aallonpituus, antennin koko, läpäisevyys, kyky siirtää tietoa. Tämä alakohta ei edellytä testejä tietokoneella. 

![Kuva spektri](/kuvat/spektri.png)

a) WebSDR. Etäkäytä WebSDR-ohjelmaradiota, joka on kaukana sinusta ja kuuntele radioliikennettä. Radioliikenne tulee siepata niin, että radiovastaanotin on joko eri maassa tai vähintään 400 km paikasta, jossa teet tätä tehtävää. Käytä esimerkkinä julkista, suurelle yleisölle tarkoitettua viestiä, esimerkiksi yleisradiolähetystä. Kerro löytämäsi taajuus, aallonpituus ja modulaatio. Kuvaile askeleet ja ota ruutukaappaus. (Tehtävässä ei saa ilmaista sellaisen viestin sisältöä tai olemassaoloa, joka ei ole tarkoitettu julkiseksi. Voit sen sijaan kuvailla, miten sait julkisen radiolähetyksen kuulumaan kaiuttimistasi. Julkisten, esimerkiksi yleisradiolähetysten sisältöä saa tietysti kuvailla.)

Käytetty vastaanotin: Pardinho WebSDR, Brasilia, noin 10500km, linkki: http://appr.org.br:8911/

Lösin vastaanottimetin WebSDR verkkosivuston etusivulta ja katsoin että se on tarpeeksi kaukana (yli 400km), sen jälkeen avasin vastaanottimen ja yllätykseksi se toimi heti. Puheesta en saanut hirveästi selvää kielimuurin takia, mutta verkon perusteella radio toistaa mahdollisesti muusikko Pardinhon ääntä ja musiikkia.

Taajuus: 7135.00kHz, 7135000Hz

Aallonpituus: λ = c (valonnopeus) / f (taajuus) eli aallonpituus on 299792458 / 7135000 = 42,01716 metriä ≈ 42 metriä

![Kuva websdr1](/kuvat/websdr_brasilia.png)

b) Moduwhat? Kuuntele kanavaa, joka käyttää eri modulaatiota (AM, FM...) kuin edellisessä kohdassa käyttämäsi kanava.

Sitten aloin etsimään kanavaa joka käyttäisi jotain muuta kuin LSB modulaatiota. Selailin hieman kanavia ja vastaan tuli seuraavksi kanava Kopernikus SDR Nordhessen. Etsin kanavalta AM modulaatiolla vesiputouksesta liikennettä ja siltä se löytyikin. 

![Kuva websdr2](/kuvat/websdr_saksa.png)

c) ADS-B. Etsi jokin sinua kiinnnostava lentokone ADS-B Exchange -palvelusta. Kuvaile konetta ja sen lentoa. Selitä, miten tiedot on saatu.

ADS-B (Automatic Dependent Surveillance–Broadcast) on ilmailuun liittyvä seurantateknologia, jonka avulla ilma-alukset ja maa-asemat voivat seurata reaaliajassa toistensa sijaintia, nopeutta ja muita oleellisia tietoja. Yksinkertaistaen järjestelmä perustuu siihen, että siihen osallistuvat ilma-aluksen jakavat GPS- ja paikkatietojaan siten, että osallistuvat alukset näkevät ne. (Wikipedia. s.a. b.)


<img width="250" height="437" alt="kuva" src="https://github.com/user-attachments/assets/54081ba5-e1a8-4bfd-9209-89054bb696be" />

ADS-B-järjestelmän toimintaperiaate (Wikipedia s.a.)

Koneen tyyppi: EMBRAER EMB-550 Praetor 600

Tietoa lennosta: Kuvassa lennon nopeus on 436 solmua ja sen korkeus on tasan 40 000 jalkaa eli noin 12 kilometriä. Lento oli lähtenyt Joensuusta ja se suuntaa Suomenlahdelle päin.

![Kuva ads-b-exchange](/kuvat/ads-b-exchange.png)

d), e) & f) Vapaaehtoiset tehtävät jäi suorittamatta sillä F-droidin asennus ilman android puhelinta vaatii aikamoisen säädön IOS käyttöjärjestelmällä tai emulaattorin asentelun.

## Lähteet
ADS-B Exchange s.a. About. Luettavissa: https://www.adsbexchange.com/about/. Luettu: 23.10.2026.

Karvinen, T. 2026a. Verkkoon tunkeutuminen ja tiedustelu. Luento 20.10.2026. Haaga-Helia ammattikorkeakoulu.

Karvinen, T. 2026b. Verkkoon tunkeutuminen ja tiedustelu. Luettavissa: https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/. Luettu: 22.10.2026.

Wikipedia s.a. a. Aallonpituus. Luettavissa: https://fi.wikipedia.org/wiki/Aallonpituus. Luettu: 23.10.2026.

Wikipedia s.a. b. ADS-B. Luettavissa: https://fi.wikipedia.org/wiki/ADS-B. Luettu: 24.10.2026.

Wikipedia s.a. c. Sähkömagneettinen spektri. Luettavissa: https://fi.wikipedia.org/wiki/S%C3%A4hk%C3%B6magneettinen_spektri. Luettu: 23.10.2026.
