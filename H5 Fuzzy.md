# H5 Fuzzy | Nathaniel Ssendagire 21.04.2026
## Ympäristö
### OS: Kali Linux

### Browser: Firefox 128.3.1esr (64-bit)

### Hardware Model: VMWare Workstation Pro Memory: 4.0 GB

### Processor: AMD Ryzen 3 7320U - 4 cores used

### Disk: 35 GB

### Network: NAT / Host-Only

## Tiivistelmä

## a) Fuzzzz: dirfuz-1 -haasteen ratkaisu

<img width="652" height="472" alt="ffuf" src="https://github.com/user-attachments/assets/5c36e5d8-5f77-44d1-b930-87991aa827e8" />

### Vaihe 1: Sanalistan ja maalin valmistelu

Ensimmäiseksi latasin kattavan sanalistan (`common.txt`), joka toimii hyökkäyksen "polttoaineena".

<img width="647" height="392" alt="terokarvinen fuzz" src="https://github.com/user-attachments/assets/ae118bf0-e139-429d-96d1-b56e2a56188f" />


Latasin maalitiedoston (`dirfuzt-1`), annoin sille suoritusoikeudet (`chmod u+x`) ja käynnistin palvelimen porttiin 8000.


### Vaihe 2: Kohinan suodatus (Filtering)
Ensimmäinen ajo ilman suodattimia osoitti tyypillisen haasteen: palvelin palauttaa useimmille pyynnöille saman vastauksen (kuten "404 Not Found"). Huomasin, että suurin osa vastauksista oli kooltaan **28** tai **154** tavua. Tämä "kohina" peittää alleen todelliset löydökset.

<img width="743" height="831" alt="fufff without any limitations" src="https://github.com/user-attachments/assets/3c7b73e5-9904-4d97-b916-ffab66692e05" />

### Vaihe 3: Discovery ja varmistus
Ajoin komennon uudelleen käyttäen `-fs 28,154` (Filter Size) -lippua. Tämä ohjeistaa ffufia hylkäämään kaikki vastaukset, jotka ovat kyseisen kokoisia. Suodatuksen jälkeen jäljelle jäi vain kiinnostavia tuloksia, kuten **wp-admin**.

<img width="765" height="622" alt="wp-admin" src="https://github.com/user-attachments/assets/1b453a84-a145-406b-b9b9-8302889d6f13" />

Lopuksi varmistin löydöksen `curl`-komennolla, jolloin sivuston sisältä paljastui tehtävän "lippu" (flag).

<img width="611" height="270" alt="curl the website" src="https://github.com/user-attachments/assets/761aa992-c20d-4fb3-b840-fc013bef8ac1" />

---

## FuffMe-ympäristön asennus
Asensin **FfufMe**-harjoitusmaalin Docker-konttiin simuloidakseni monimutkaisempia suojausmekanismeja. Käytin asennuksessa Karvisen (2023) ohjetta: [Ffufme - Install Web Fuzzing Target](https://terokarvinen.com/2023/fuffme-web-fuzzing-target-debian/).

<img width="742" height="607" alt="ffuf me website home page" src="https://github.com/user-attachments/assets/b3de087e-5eeb-4312-acd5-abd5d833e8d9" />
<img width="762" height="862" alt="ffuf on the browser" src="https://github.com/user-attachments/assets/63252789-c58e-48d7-a53e-226019d6b64f" />

---

## c) Basic Content Discovery
Suoritun perusmuotoisen hakemistofuzzauksen `http://localhost/cd/basic/FUZZ`. Tämä vaihe vahvisti resurssit kuten `class` ja `development.log` analysoimalla HTTP 200 OK -vastauskoodeja.

<img width="757" height="522" alt="ffuf c)" src="https://github.com/user-attachments/assets/88b98149-ca82-43df-b3e3-65a261fea837" />

---

## d) Content Discovery With Recursion
Todellisissa skenaarioissa tiedostot on usein piilotettu usean hakemistotason taakse. Käytin `-recursion` -lippua, joka kytkee päälle automaattisen crawlaamisen löydettyihin hakemistoihin.

<img width="748" height="687" alt="ffuf d)" src="https://github.com/user-attachments/assets/4e67b064-961b-4bb1-a632-bdb5ab980a2b" />

**Analyysi:** ffuf löysi ensin `admin/`-hakemiston ja jatkoi automaattisesti syvemmälle `admin/users/`-hakemistoon, kunnes lopullinen tiedosto löytyi.

---

## e) Content Discovery With File Extensions
Käytin `-e` (extensions) -lippua kokeilemaan sanalistan sanojen perään yleisimpiä päätteitä (`.php`, `.txt`, `.log`).

<img width="760" height="560" alt="ffuf e) 2" src="https://github.com/user-attachments/assets/4b240586-d1b8-440d-b4ce-aa652aafcb5c" />
<img width="761" height="847" alt="ffuf e) 1" src="https://github.com/user-attachments/assets/0737fd0c-af3c-4bb0-ac43-81a7191debd1" />

Löysin `logs`-hakemiston, jonka statuskoodi **301** (Moved Permanently) paljasti kyseessä olevan hakemiston, joka ohjaa käyttäjän eteenpäin.

---

## f) No 404 Status
Tämä tehtävä simuloi "Catch-all" -konfiguraatiota, jossa palvelin vastaa aina `200 OK`, vaikka sivua ei olisi.

<img width="754" height="562" alt="ffuf f)" src="https://github.com/user-attachments/assets/0114625e-da60-4bca-a1b1-b57db79b0f89" />
<img width="762" height="690" alt="ffuf f) 2" src="https://github.com/user-attachments/assets/6d60ed51-2ff2-4a09-95c3-3a1177d4551e" />

Suodattamalla pois koon **669** (`-fs 669`), onnistuin erottamaan kohinasta todellisen hakemiston nimeltä **secret**.

<img width="765" height="532" alt="ffuf f) 3" src="https://github.com/user-attachments/assets/67e0c604-97be-4bdc-842b-cc2dae5101e5" />

---

## g) Param Mining
Etsin piilotettuja URL-parametreja, jotka voivat paljastaa kriittisiä toiminnallisuuksia. Latasin uuden sanalistan (`burp-parameter-names.txt`) ja kokeilin parametreja muodossa `?FUZZ=1`.

<img width="762" height="367" alt="ffuf g)" src="https://github.com/user-attachments/assets/7873de9d-0b5d-4f85-9df2-fd4387835077" />
<img width="762" height="461" alt="ffuf g) 1" src="https://github.com/user-attachments/assets/a75abe0f-5392-4679-8946-36e6898ef046" />

**Löydös:** Hakemistosta paljastui piilotettu **debug**-parametri.

---

## h) Rate Limited
Tässä tehtävässä palvelin alkoi rajoittaa yhteyksiä (Rate Limiting) liian nopean skannauksen seurauksena. Säädin ffufin nopeutta vastaamaan "ihmismäisempää" tahtia.

Käytetyt liput:
* `-t 1`: Vain yksi säie (thread) kerrallaan.
* `-p 0.1`: 0,1 sekunnin tauko pyyntöjen välillä.

<img width="761" height="492" alt="ffuf h)" src="https://github.com/user-attachments/assets/7aa11101-b349-47aa-a1c0-edda767d9c3c" />

Hidas mutta varma ajo paljasti **oracle**-sivun ilman, että palvelin esti hyökkäystä.

---

## i) Subdomains - Virtual Host Enumeration
Viimeisessä vaiheessa tutkittiin Virtual Host -konfiguraatioita fuzzaamaalla HTTP **Host**-otsikkoa.

<img width="760" height="547" alt="ffuf i)" src="https://github.com/user-attachments/assets/a2ff88c4-86c8-4976-861b-4f064a0544d7" />

Ensimmäinen yritys 5 000 sanan listalla ei tuottanut tuloksia koon `1495` suodatuksen jälkeen.
<img width="757" height="317" alt="ffuf i) 2" src="https://github.com/user-attachments/assets/f09b097d-5075-4cb0-a8fb-296adf2d508f" />

Päivitin sanalistan 20 000 sanan versioon (`top1million-20000.txt`), jolloin työkalu löysi välittömästi **redhat**-alidomainin. Tämä korosti sanalistojen laadun merkitystä tiedusteluvaiheessa.
<img width="740" height="371" alt="ffuf i) 1" src="https://github.com/user-attachments/assets/b90b53ba-080e-4772-84d3-7787fbaf94a1" />

