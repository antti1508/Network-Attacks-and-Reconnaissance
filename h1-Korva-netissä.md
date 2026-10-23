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

Sitten aloin etsimään kanavaa joka käyttäisi AM, FM... modulaatiota. Selailin hieman kanavia ja vastaan tuli seuraavksi kanava Kopernikus SDR Nordhessen. Etsin kanvalta AM modulaatiolla vesiputouksesta liikennettä ja siltä se löytyikin. 

![Kuva websdr2](/kuvat/websdr_saksa.png)

c) ADS-B. Etsi jokin sinua kiinnnostava lentokone ADS-B Exchange -palvelusta. Kuvaile konetta ja sen lentoa. Selitä, miten tiedot on saatu.

d), e) & f) Vapaaehtoiset tehtävät jäi suorittamatta sillä F-droidin asennus ilman android puhelinta vaatii aikamoisen säädön IOS käyttöjärjestelmällä tai emulaattorin asentelun.

## Lähteet
Karvinen. T 2026. Verkkoon tunkeutuminen ja tiedustelu. https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/

Karvinen. T 2026. Verkkoon tunkeutuminen ja tiedustelu. Luento. Haaga-Helia ammattikorkeakoulu. Luento 20.10.2026.

Wikipedia. Aallonpituus. https://fi.wikipedia.org/wiki/Aallonpituus

Wikipedia. Sähkömagneettinen spektri. https://fi.wikipedia.org/wiki/S%C3%A4hk%C3%B6magneettinen_spektri
