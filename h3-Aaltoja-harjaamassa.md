# h3 Aaltoja harjaamassa

# x) Lue ja tiivistä

## Hubacek 2019: Universal Radio Hacker SDR Tutorial on 433 MHz radio plugs: (https://youtu.be/sbqMqb6FVMY?t=199)

* Kaukosäätimen lähetystaajuus etsitään URH:n spektrianalysaattorilla painamalla kaukosäätimen nappia.
* Tallennustaajuus asetetaan hieman sivuun todellisesta taajuudesta SDR-vastaanottimen DC-piikin välttämiseksi.
* Modulaatioksi valitaan ASK, jolloin ohjelman automaattitunnistus havaitsee bittipituudeksi noin 700 mikrosekuntia.
* Demoduloinnin jälkeen varmistetaan pakettien yhteneväisyys, ja purettu heksakoodi kopioidaan talteen signaalin toistamista varten.

## Cornelius 2022: (https://www.onetransistor.eu/2022/01/decode-433mhz-ask-signal.html)

* Signaali tunnistettiin aluksi rtl_433-työkalulla, joka paljasti sen Nexus-TH-sääanturiksi ja antoi valmiit tiedot 36-bittisen datapaketin rakenteesta.
* Lähete tallennettiin URH:lla taajuussiirrosta käyttäen, ja anturin lähetysväliksi mitattiin noin 56,75 sekuntia.
* Koska URH ei suoraan tue PDM-koodausta, signaali invertoitiin ja purettiin hyödyntämällä ohjelman morsekoodidekooderia.

## Vapaaehtoinen, vaikeahko: Lohner 2019: (https://github.karllohner.com/SDR/Decoding/Example_2019-01-24/)

* Signaalinäyte ladataan URH-ohjelmaan, ja kohinaa vaimennetaan kaistanpäästösuodattimella (Bandpass Filter).
* Signaali invertoidaan ja dekoodataan Morsekoodi-funktiolla, jolloin datasisällöksi paljastuu heksamuodossa 0x148.
* Ohjelmalle rtl_433 määritellään joustava dekooderisääntö (-X), johon syötetään ajoitukset s=1008, l=3024, nollausaika r=3200 sekä toleranssi t=100.
* Dekooderisääntöä testataan useilla näytetiedostoilla, joista osa tuottaa dataksi 0x148 ja osa 0x74d osoittaen lähetteen vaihtelevan.

# b) rtl_433 (a kohta puuttui ohjeista)

Aloitin asennusprosessin käynnistämällä Debian 13 virtuaalikoneeni, jonka jälkeen ajoin komentoriviltä komennon: ```sudo apt update && sudo apt install rtl_433 -y```. Komennon jälkeen oli aika kokeilla, että rtl-433 asennettiin onnistuneesti ja että versio vastaa ohjeissa anettua rtl_433 version 25.02:a. Varmistin tämän ajamalla komennon ```rtl_433 -V``` komentorivillä, parametri "V" tulostaa rtl-433:n versionumeron. Alempana kuva tuloksesta:

<img width="1126" height="229" alt="kuva" src="https://github.com/user-attachments/assets/ad0664cf-8396-48df-a0c6-6c9a311cc1db" />

Kuva 1. rtl-433:n versionumero.

# c) Automaattinen analyysi

Aloitin tämän tehtävän avaamalla rtl-433 ohjelmalla tiedoston Converted_433.92M_2000k.cs8. Käytin tähän komentoa ```rtl_433 -r Converted_433.92M_2000k.cs8```. Komennon parametri "-r" lukee datan määritetystä tiedostosta sen sijaan, että rtl_433 siirtyisi kuuntelutilaan. Komennolla saadaan seuraavanlainen tuloste:

<img width="1362" height="859" alt="kuva" src="https://github.com/user-attachments/assets/30a028d0-c048-4f8f-b042-a0384571f45c" />

Kuva 2. Tiedoston sisältö.

Seuraavaksi tehtänannossa ohjeistettiin analysoimaan tiedoston Converted_433.92M_2000k.cs8 sisältöä joka näkyy ylempänä kuvassa.

# d) Too compex 16?

# e) Ultimate

# f) Yleiskuva

# g) Bittistä

# h) Vapaaehtoinen: Sdr++

# i) Vapaaehtoinen, vaikeahko: GNU Radio

## Lähteet

Karvinen, T. Verkkoon tunkeutuminen ja tiedustelu. 2026. Luettavissa: https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/ Luettu: 6.11.2026.

Karvinen, T 2026. Verkkoon tunkeutuminen ja tiedustelu. Luento. 3.11.2026.
