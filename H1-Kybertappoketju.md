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




