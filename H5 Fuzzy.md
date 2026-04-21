# H5 Fuzzy | Nathaniel Ssendagire 21.04.2026
## Ympäristö
### OS: Kali Linux

### Browser: Firefox 128.3.1esr (64-bit)

### Hardware Model: VMWare Workstation Pro Memory: 4.0 GB

### Processor: AMD Ryzen 3 7320U - 4 cores used

### Disk: 35 GB

### Network: NAT / Host-Only

## Tiivistelmä


## a) Fuzzzz. Ratkaise dirfuz-1 artikkelista Karvinen 2023

<img width="652" height="472" alt="ffuf" src="https://github.com/user-attachments/assets/5c36e5d8-5f77-44d1-b930-87991aa827e8" />


- Ensimmäiseksi latasin kattavan sanalistan (common.txt), joka toimii hyökkäyksen "polttoaineena"


<img width="647" height="392" alt="terokarvinen fuzz" src="https://github.com/user-attachments/assets/ae118bf0-e139-429d-96d1-b56e2a56188f" />

- Latasin maalitiedoston (dirfuzt-1), annoin sille suoritusoikeudet (chmod u+x) ja käynnistin paikallisen palvelimen porttiin 8000.

<img width="743" height="831" alt="fufff without any limitations" src="https://github.com/user-attachments/assets/3c7b73e5-9904-4d97-b916-ffab66692e05" />

- Ensimmäinen ajo ilman suodattimia osoitti tyypillisen haasteen: palvelin palauttaa useimmille pyynnöille saman vastauksen (kuten "404 Not Found" tai kustomoidun virhesivun). Huomasin, että suurin osa vastauksista oli kooltaan 28 tai 154 tavua. Tämä "kohina" peittää alleen todelliset löydökset.

<img width="765" height="622" alt="wp-admin" src="https://github.com/user-attachments/assets/1b453a84-a145-406b-b9b9-8302889d6f13" />

- Ajoin komennon uudelleen käyttäen -fs 28,154 (Filter Size) -lippua. Tämä komento ohjeistaa ffufia hylkäämään kaikki vastaukset, jotka ovat kyseisen kokoisia. Suodatuksen jälkeen jäljelle jäi vain kiinnostavia tuloksia, kuten wp-admin. 

<img width="611" height="270" alt="curl the website" src="https://github.com/user-attachments/assets/761aa992-c20d-4fb3-b840-fc013bef8ac1" />

- Lopuksi varmistin löydöksen curl-komennolla, jolloin sivuston sisältä paljastui tehtävän "lippu" (flag).

## Fuff me. Asenna FuffMe-harjoitusmaali. Karvinen 2023

<img width="742" height="607" alt="ffuf me website home page" src="https://github.com/user-attachments/assets/b3de087e-5eeb-4312-acd5-abd5d833e8d9" />

<img width="762" height="862" alt="ffuf on the browser" src="https://github.com/user-attachments/assets/63252789-c58e-48d7-a53e-226019d6b64f" />

- Seuraavaksi siirryin vaativampaan ympäristöön asentamalla FfufMe-harjoitusmaalin Docker-konttiin.  Käytin asennuksessa Tero Karvisen virallista ohjetta, joka löytyy tästä osoitteest: https://terokarvinen.com/2023/fuffme-web-fuzzing-target-debian/

## c) Basic Content Discovery

<img width="757" height="522" alt="ffuf c)" src="https://github.com/user-attachments/assets/88b98149-ca82-43df-b3e3-65a261fea837" />

- Tässä harjoituksessa suoritettiin perusmuotoinen hakemistofuzzaus. Käytin standardia sanalistaa etsimään piilotettuja tiedostoja http://localhost/cd/basic/FUZZ -osoitteesta. Tämä vaihe vahvisti, että ffuf löytää olemassa olevat resurssit (kuten class ja development.log) analysoimalla HTTP-vastauskoodeja (200 OK).

## d) Content Discovery With Recursion

<img width="748" height="687" alt="ffuf d)" src="https://github.com/user-attachments/assets/4e67b064-961b-4bb1-a632-bdb5ab980a2b" />

- Todellisissa hyökkäysskenaarioissa tiedostot on usein piilotettu usean hakemistotason taakse. Käytin -recursion -lippua, joka tekee ffufista älykkäämmän: aina kun työkalu löytää uuden hakemiston, se aloittaa automaattisesti uuden fuzzauksen kyseisen hakemiston sisällä.

- Tämä on tehokas tapa kartoittaa koko sivuston arkkitehtuuri. Tuloksista nähtiin, miten ffuf löysi ensin admin/-hakemiston ja jatkoi sieltä admin/users/-hakemistoon, kunnes lopullinen tiedosto löytyi.

## e) Content Discovery With File Extensions

<img width="760" height="560" alt="ffuf e) 2" src="https://github.com/user-attachments/assets/4b240586-d1b8-440d-b4ce-aa652aafcb5c" />

- Kaikkia tiedostoja ei löydetä pelkillä hakemistonimillä. Käytin -e (extensions) -lippua kokeilemaan sanalistan sanojen perään yleisimpiä päätteitä, kuten .php, .txt ja .log. Tämä moninkertaistaa pyyntöjen määrän, mutta on välttämätöntä esimerkiksi varmuuskopioiden tai lokitiedostojen löytämiseksi.

<img width="761" height="847" alt="ffuf e) 1" src="https://github.com/user-attachments/assets/0737fd0c-af3c-4bb0-ac43-81a7191debd1" />

- Löysin logs-hakemiston, joka palautti tilakoodin 301 (Moved Permanently). Tämä koodi indikoi usein, että kyseessä on hakemisto, johon pääsy vaatii kenoviivan (trailing slash) osoitteen perään.

## f) No 404 Status

<img width="754" height="562" alt="ffuf f)" src="https://github.com/user-attachments/assets/0114625e-da60-4bca-a1b1-b57db79b0f89" />

<img width="762" height="690" alt="ffuf f) 2" src="https://github.com/user-attachments/assets/6d60ed51-2ff2-4a09-95c3-3a1177d4551e" />

- Tämä tehtävä simuloi tilannetta, jossa palvelin on konfiguroitu huonosti tai se käyttää "Catch-all" -sivua, joka palauttaa aina tilakoodin 200 OK, vaikka sivua ei olisi olemassa. Perinteiset skannerit luulevat tällöin jokaisen sanan löytyvän.

<img width="765" height="532" alt="ffuf f) 3" src="https://github.com/user-attachments/assets/67e0c604-97be-4bdc-842b-cc2dae5101e5" />

- Ratkaisin ongelman analysoimalla vastauksia ja huomaamalla, että virheelliset sivut olivat aina kooltaan 669 tavua. Käyttämällä -fs 669 -suodatinta onnistuin erottamaan massasta todellisen hakemiston nimeltä secret.

## g) Param Mining

<img width="762" height="367" alt="ffuf g)" src="https://github.com/user-attachments/assets/7873de9d-0b5d-4f85-9df2-fd4387835077" />

- Parametrien fuzzaus (Param Mining) on kriittistä, kun etsitään haavoittuvuuksia kuten SQL-injektiota tai IDOR-virheitä. Tässä tehtävässä tavoitteena oli löytää URL-parametri, joka muuttaa sivun käyttäytymistä.

<img width="762" height="461" alt="ffuf g) 1" src="https://github.com/user-attachments/assets/a75abe0f-5392-4679-8946-36e6898ef046" />

- Latasin uuden, nimenomaan parametreille tarkoitetun sanalistan (burp-parameter-names.txt). Ajoin fuzzauksen muodossa ?FUZZ=1, mikä paljasti piilotetun debug-parametrin. Tämä on tyypillinen löydös, joka saattaa paljastaa kehittäjille tarkoitettuja työkaluja hyökkääjälle.

## h) Rate Limited

<img width="761" height="492" alt="ffuf h)" src="https://github.com/user-attachments/assets/7aa11101-b349-47aa-a1c0-edda767d9c3c" />

- Nykyaikaiset Web Application Firewall (WAF) -ratkaisut ja palvelimet osaavat estää liian nopeasti tulevat pyynnöt. Tässä tehtävässä palvelin alkoi rajoittaa yhteyksiä, jos nopeutta ei säädetty.

- Käytin -t 1 (threads) ja -p 0.1 (delay) -lippuja rajoittamaan ffufin nopeutta noin kymmeneen pyyntöön sekunnissa. Vaikka skannaus kesti huomattavasti kauemmin (yli 8 minuuttia), se mahdollisti "tutkan alla" pysymisen ja oracle-sivun löytämisen ilman, että palvelin esti hyökkäystä.

## i) Subdomains - Virtual Host Enumeration

<img width="760" height="547" alt="ffuf i)" src="https://github.com/user-attachments/assets/a2ff88c4-86c8-4976-861b-4f064a0544d7" />

- Viimeisessä tehtävässä siirryttiin Virtual Host -tiedusteluun. Palvelimet voivat isännöidä useita eri sivustoja samassa IP-osoitteessa, ja oikea sivu tarjoillaan HTTP Host-otsikon perusteella.

<img width="757" height="317" alt="ffuf i) 2" src="https://github.com/user-attachments/assets/f09b097d-5075-4cb0-a8fb-296adf2d508f" />

- Fuzzasin Host-otsikkoa (-H "Host: FUZZ.ffuf.me") käyttäen ensin 5 000 sanan listaa. Suodatin oletusvastauksen koon (1495) pois, mutta en saanut tuloksia. Päädyin analyysiin, että sanalista oli liian suppea. 

<img width="740" height="371" alt="ffuf i) 1" src="https://github.com/user-attachments/assets/b90b53ba-080e-4772-84d3-7787fbaf94a1" />

- Päivitin sanalistan 20 000 sanan versioon (top1million-20000.txt), jolloin työkalu löysi välittömästi redhat-alidomainin. Tämä korosti sanalistojen laadun ja laajuuden merkitystä tietoturvatestauksessa.


