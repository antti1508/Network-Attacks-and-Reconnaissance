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

# c) Automaattinen analyysi

# d) Too compex 16?

# e) Ultimate

# f) Yleiskuva

# g) Bittistä

# h) Vapaaehtoinen: Sdr++

# i) Vapaaehtoinen, vaikeahko: GNU Radio
