# H1 Kybertappoketju | Nathaniel Ssendagire 25.03.2026
## Ympäristö
### OS: Kali Linux

### Browser: Firefox 128.3.1esr (64-bit)

### Hardware Model: VMWare Workstation Pro Memory: 4.0 GB

### Processor: AMD Ryzen 3 7320U - 4 cores used

### Disk: 35 GB

### Network: NAT

## Tiivistys

- Darknet Diaries on Jack Rhysiderin juontama podcast, joka syventyy internetin pimeään puoleen, kuten kyberrikollisuuteen, tietomurtoihin ja hakkerointiin.
On todella kiehtovaa, miten ohjelma onnistuu saamaan jopa entisiä kyberrikollisia (kuten jaksossa mainittu luottokorttivaras tai verkossa rahoja vienyt hakkeri) puhumaan teoistaan näin avoimesti. Kuinkahan paljon taustatyötä ja luottamuksen rakentamista tällaisten haastateltavien suostuttelu oikein vaatii?

- Abstract: Perinteiset tietoturvatyökalut eivät riitä edistyneitä ja pitkäkestoisia kyberuhkia (APT) vastaan, joten puolustuksen on perustuttava uhkatietoon ja hyökkäysketjun (kill chain) mallintamiseen.

- 3.2 Intrusion Kill Chain: Kyberhyökkäys etenee aina seitsemän vaiheen kautta: tiedustelu, aseistaminen, toimitus, hyväksikäyttö, asennus, komento ja hallinta (C2) sekä lopullisten tavoitteiden toteuttaminen (esim. tiedon varastaminen).

- Pääpaino on Nmap-työkalun käytännön hyödyntämisessä: sillä skannataan kohdeverkon avoimia portteja ja tunnistetaan siellä pyöriviä palveluita.

- Vanha oikeustapaus herättää kieltämättä aika kylmäävän ajatuksen siitä, miten kalliiksi pelkkä digitaalinen uteliaisuus voi tulla. Vaikka nuori koodari ei päässyt pankin järjestelmään sisään eikä varastanut mitään, lasku pelkästä virtuaalisten "ovien kokeilemisesta" ja siitä seuranneesta selvitystyöstä nousi pilviin, eikä oikeus antanut iän perusteella armoa 😕.


## Asenna Kali virtuaalikoneeseen.
Kali linuxin latauksessa ei ilmennyt mitään ongelmaa, latasin sen<a href="https://www.kali.org/get-kali/#kali-installer-images"> täältä</a>.

## b) Irrota Kali-virtuaalikone verkosta

<img width="213" height="65" alt="disconnecting the connection " src="https://github.com/user-attachments/assets/d4a0ef24-7ac6-426e-a030-8f004d53be53" />

<img width="565" height="178" alt="no response" src="https://github.com/user-attachments/assets/f852f220-2638-4d68-b97c-3bc92ba0a7c3" />

Katkaisin verkkokortin yhteyden virtuaalikoneesta ja kokeilin pingata googlen dns:ää. Vastausta ei tullut, joten voidaan todeta, että koneella ei ole yhteystä nettiin.

Testasin vielä toisinpäin varmistaakseni, että kaikki toimii kuten pitääkin. 

<img width="191" height="88" alt="connecting to the adapter again" src="https://github.com/user-attachments/assets/dfb6eea7-26be-4f21-bd04-a7534673a26f" />

<img width="537" height="236" alt="getting a reply " src="https://github.com/user-attachments/assets/de1d3eee-b869-46ba-8e9b-eb67053442d4" />

## c) Porttiskannaa 1000 tavallisinta tcp-porttia omasta koneestasi 

<img width="913" height="323" alt="nmap without the services up" src="https://github.com/user-attachments/assets/7dafc6d6-cfb5-4c6a-af76-41a42d5dc9a4" />

Tein porttiskannauksen localhostiin ja mitään porttia ei tietenkään näkynyt, koska ei ole mitään demonia aktiivisena ja ennen kuin ihmettelet, kyllä muistin katkaista yhteyden nettiin ennen kuin tein porttiskannauksen :D.  

## d) Asenna kaksi vapaavalintaista demonia ja skannaa uudelleen. Analysoi ja selitä erot.

<img width="431" height="38" alt="service installation" src="https://github.com/user-attachments/assets/90d838f5-7017-43c9-bc67-6abb051919c0" />

Seuraavaksi latasin Apache2 ja openssh-serverin.

<img width="302" height="97" alt="starting the services" src="https://github.com/user-attachments/assets/41943035-1c41-4f53-a9fe-7d2e07292b9b" />
<img width="747" height="398" alt="ssh status" src="https://github.com/user-attachments/assets/b3d0b649-73a1-47b5-87b6-96dfce2ac9c8" />
<img width="882" height="381" alt="apache2 status" src="https://github.com/user-attachments/assets/c5030603-445a-476a-8e4a-a2ac30d1facd" />


Demonien latauksen jälkeen pistin ne molemmat päälle ja varmistin, että ne olivat oikeasti päällä.

<img width="910" height="411" alt="nmap scan results" src="https://github.com/user-attachments/assets/880641d4-aaba-47fa-aecd-188d732bdd1f" />

Seuraavaksi tein porttiskannauksen uudestaan ja kappas sieltä löytyi kaksi porttia, joita nämä demonit siis käyttää.

## Erot

Huomasin, että Nmap vain totesi kaikkien 1000 testatun portin olevan kiinni. Koska mitään tutkittavaa ei ollut, skannaus oli ohi reilussa parissa sekunnissa (2.37 s). Nmap ei myöskään pystynyt kunnolla tunnistamaan käyttöjärjestelmääni, koska mikään portti ei vastannut sille.

Nmap löysikin heti kaksi avointa porttia: 22 (SSH) ja 80 (HTTP). Koska käytin -A -lippua, se ei vain kertonut porttien olevan auki, vaan kaivoi esiin tarkat versiot siellä pyörivistä ohjelmista (OpenSSH ja Apache). Tämä syvempi tutkinta vei vähän enemmän aikaa, melkein 10 sekuntia.

- nmap: Itse skannausohjelman käynnistys.
- -T4: Nopeusasetus, joka käskee ohjelman toimia reippaalla vauhdilla. Nopeus voi mennä T0 (Erittäin hidas mutta möys vaikeampi havaita) -T5, mutta mitä nopeampaa asetusta käytetään, sitä todennäköisemmin erroreita tulee, tätä kannattaa siis käyttää jos on hyvä yhteys internettiin tai et halua nmpain odottavan liian pitkään jotain vastausta.
- -A: Aggressiivinen tila, joka tonkii esiin syvälliset tiedot, kuten palveluiden tarkat versiot ja käyttöjärjestelmän. Tämä on kuitenkin aika "äänekäs" joten palomuurit todennäköisesti huomaavat jotain, mutta tämä on tosi hyvä jos pitää saada paljon tietoa.
- localhost: Tämä on se itse kohde eli tässä tehtävässä se nyt sattui olemaan oma tietokoneeni tai no siis virtuaalikoneeni, mutta se voi olla ihan mikä tahansa muu osoite. 

## e) Ratkaise vapaavalintainen kone HackTheBoxista

<img width="1387" height="226" alt="image" src="https://github.com/user-attachments/assets/f4eae804-0697-4d5f-ba13-91cf3745c583" />

Päätin tehdä "Dancing" koneen HTB sivulta. 

Suurinosa tehtävästä oli itsestään selvää, joten skippaan nyt siihen kohtaan mikä vaati vähän miettimistä, eli root flagin saaminen...

<img width="817" height="252" alt="shares on smb" src="https://github.com/user-attachments/assets/ced3a7d1-ebf8-4ffc-a873-26db34daedc1" />

- Ensiksi listasin kaiken mitä voin löytää koneen ip osoitteesta smb yhteydellä. Siellä näkyy "WorkShares", joka ei vaadi mitään käyttäjätunnusta eikä salasanaa.

<img width="602" height="92" alt="smb connection" src="https://github.com/user-attachments/assets/d489c250-f7f3-4624-879c-1d0d9f7953bc" />

- Sitten otin smb yhteyden koneeni ip osoitteeseen.

<img width="681" height="123" alt="smb list" src="https://github.com/user-attachments/assets/07277e52-73fd-485b-9165-f89d0b1cf13f" />

- Seuraavaksi kirjoitin "L" komennon joka tarkoittaa listaa, niin pystyn näkemään mitä koneelta löytyy...Siellä oli kansio James.P, jossa oli root flag.txt tiedosto.

<img width="905" height="86" alt="smb get" src="https://github.com/user-attachments/assets/e1916947-3218-492f-a73a-c8ace631d96c" />

- Sitten kirjoitin "get" komennon, joka siis lataa tämän tiedoston omalle koneelleni. Sitten kirjoitin "exit" komennon, jotta pääsen takaisin omalle koneelleni.

<img width="695" height="440" alt="file shown on my pc" src="https://github.com/user-attachments/assets/044ad51b-c113-4550-bbb9-50b61d4d5dc8" />

<img width="307" height="66" alt="flag captured" src="https://github.com/user-attachments/assets/91d0f6a4-ac30-44a3-ac64-c09b36d6c470" />

Tiedosto sitten näkyi omalla koneellani, kirjoitin cat komennon terminaliin, joka tulostaa mitä tahansa tekstiä mikä siinä tiedostossa on, ja sieltä tuli se lippu!

## Lähteet

- <a href="https://darknetdiaries.com/episode/"> Darknet Diaries</a>, 
- <a href="https://lockheedmartin.com/content/dam/lockheed-martin/rms/documents/cyber/LM-White-Paper-Intel-Driven-Defense.pdf"> Lockheedmartin</a>,
- <a href="https://www.oreilly.com/videos/the-art-of/9780135767849/9780135767849-SPTT_04_04/"> Oreilly</a>,
- <a href="https://www.finlex.fi/fi/oikeuskaytanto/korkein-oikeus/ennakkopaatokset/2003/36#OT0_OT3"> Finlex</a>,
- <a href="https://terokarvinen.com/tunkeutumistestaus/#h1-kybertappoketju">Tero Karvinen</a>
