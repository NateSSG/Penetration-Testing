# H7 Toukokuu2026! | Nathaniel Ssendagire 05.05.2026

## Ympäristö

### OS: Kali Linux

### Browser: Firefox 128.3.1esr (64-bit)

### Hardware Model: VMWare Workstation Pro Memory: 4.0 GB

### Processor: AMD Ryzen 3 7320U - 4 cores used

### Disk: 35 GB

### Network: NAT / Host-Only

## a) Asenna Hashcat ja testaa sen toiminta murtamalla esimerkkisalasana.

<img width="266" height="41" alt="hashcat installation" src="https://github.com/user-attachments/assets/f0180ea2-a3bd-408b-b73c-12b886773e8e" />




Aloitin tehtävän asentamalla Hashcatin Kalin paketinhallinnasta.



```bash
sudo apt-get install hashcat
```

<img width="297" height="66" alt="md5sum" src="https://github.com/user-attachments/assets/6db5b867-3df9-4d72-98da-440828b93367" />


Generoin MD5-tiivisteen sanasta "Hello" testitarkoitusta varten. MD5 on 128-bittinen tiivistefunktio, jota käytetään usein tarkistussummiin, mutta se on nykyään murtamiseen turvaton

```bash
echo -n "Hello" | md5sum
```

<img width="645" height="380" alt="hashcat initiation" src="https://github.com/user-attachments/assets/ac1cab4b-fbd6-4b5f-97e4-267f355b784c" />

Käynnistin Hashcatin käyttämällä maskihyökkäystä (brute-force). Valitsin tilaksi MD5:n (-m 0) ja hyökkäystavaksi suoran kokeilun (-a 3)

```bash
hashcat -m 0 8b1a9953c4611296a827abf8c47804d7 -a 3
```
Hashcat löysi salasanan välittömästi. Tämä demonstroi, kuinka heikot ja lyhyet salasanat ilman suolausta (salt) murtuvat sekunneissa modernilla laitteistolla.

## Asenna John the Ripper 

_John the Ripperin asennuksessa käytin Teron laatimia ohjeita._ <a href="https://terokarvinen.com/2023/crack-file-password-with-john/">Tero Karvinen</a>

<img width="646" height="380" alt="wget teros crack file" src="https://github.com/user-attachments/assets/5e9ee2fa-fe79-4244-9b60-f3e6410016d8" />

Latasin Tero Karvisen sivuilta harjoitustiedoston murtamista varten. ZIP-tiedostot käyttävät usein salakirjoitusta, joka voidaan murtaa, jos salasana on heikko.

```bash
wget [https://terokarvinen.com/2023/crack-file-password-with-john/tero.zip](https://terokarvinen.com/2023/crack-file-password-with-john/tero.zip)
```

<img width="642" height="140" alt="file rename" src="https://github.com/user-attachments/assets/0b6e3340-0331-4ce0-9028-f150b2f46ac6" />

Käytin zip2john-työkalua louhimaan tiivisteen ZIP-tiedoston otsikkotiedoista. ZIP-tiedostoa itseään ei voi antaa Johnille, vaan se on ensin muunnettava tekstimuotoiseksi hash-tiedostoksi.

```bash
~/john/run/zip2john tero.zip > tero.zip.hash
```
<img width="642" height="330" alt="cracked" src="https://github.com/user-attachments/assets/b2a97e51-eec0-4951-9e97-2729ad051c38" />

Suoritin murtamisen John the Ripperillä käyttämällä sanastohyökkäystä (dictionary attack). Ohjelma vertaa louhittua tiivistettä sanakirjan sanoihin.

```bash
~/john/run/john tero.zip.hash
```

<img width="640" height="166" alt="john passsss" src="https://github.com/user-attachments/assets/832df164-433c-4907-8058-961ebc487b06" />

Vahvistin tuloksen --show -komennolla: salasana oli butterfly. John on erityisen vahva juuri tällaisten tiedostosäilöjen murtamisessa.

## Tiedosto

<img width="240" height="42" alt="qpdf" src="https://github.com/user-attachments/assets/3ec20531-0622-4c8f-9fe4-fbce98d7f6bd" />

Asensin `qpdf`-työkalun, jota käytetään PDF-tiedostojen muokkaamiseen ja salaamiseen.

```bash
sudo apt-get install qpdf
```

<img width="647" height="343" alt="errorrrr" src="https://github.com/user-attachments/assets/1571c79d-220e-4a74-9e00-c04ceb11e932" />

<img width="750" height="605" alt="pandoc" src="https://github.com/user-attachments/assets/f9258967-9f75-453c-9b47-d5646477eda6" />


Havaitsin, että `qpdf`-työkalu vaatii sisääntulotiedostolta PDF-formaattia, eikä se tue suoraa salausta tekstiedostosta `(.txt)`. Tämän vuoksi tiedosto oli ensin muunnettava PDF-muotoon. StackOverflow-keskustelujen pohjalta päädyin hyödyntämään `pandoc`-ohjelmaa, joka on monipuolinen työkalu dokumenttien väliseen konversioon. Ohjelma mahdollisti tekstisisällön siirtämisen PDF-säiliöön, minkä jälkeen salauksen viimeistely `qpdf`:llä onnistui ongelmitta.


<img width="318" height="93" alt="pandoc file conversion" src="https://github.com/user-attachments/assets/69d20f8d-f5e4-47e9-ac2d-e75b531ba536" />

Sitten muunsin aiemmin luomani secret.txt -tiedoston PDF-muotoon juuri ladatulla pandoc-ohjelmalla.

```bash
pandoc secret.txt -o my_test.pdf
```

<img width="951" height="787" alt="qpdf doc" src="https://github.com/user-attachments/assets/f1fe5e59-5887-4fa8-b267-45092140ca94" />

Kävin katsomassa qpdf dokumentaatiota ja sieltä löytyi esimerkki miten salataan pdf tiedosto.

<img width="606" height="92" alt="encrypted my pdf" src="https://github.com/user-attachments/assets/4be9de0d-6861-4e9a-9ad8-9468d10e18b7" />


Salasin PDF-tiedoston AES-256 -salauksella käyttäen salasanaa "secret123".

```bash
qpdf --encrypt secret123 secret123 256 -- my_test.pdf my_encrypted.pdf
```

<img width="355" height="91" alt="converting pdf to hash" src="https://github.com/user-attachments/assets/761141d3-62f6-45bd-8900-762b7115e212" />

```bash
pdf2john my_encrypted.pdf > my_pdf.hash
```

<img width="648" height="418" alt="password cracked" src="https://github.com/user-attachments/assets/b7363432-ea20-491f-b790-e42d87231f5a" />

Mursin PDF:n Johnilla

## Tiiviste

_Alkuperäinen suunnitelma Linux-käyttäjän murtamisesta hylättiin teknisten haasteiden vuoksi (Yescrypt-algoritmin yhteensopivuus). Sen sijaan demonstroin John the Ripperin kykyä murtaa suoria matemaattisia tiivisteitä, joita käytetään usein verkkopalveluiden tietokannoissa._

<img width="535" height="42" alt="md5 print" src="https://github.com/user-attachments/assets/624a81d0-6544-480d-9a2d-3c23a163dd62" />

Generoin MD5-tiivisteen sanasta "secret123" ja tallensin sen tiedostoon. Käytin awk-komentoa poimimaan vain itse tiivisteen tiedostoon.

```bash
echo -n "secret123" | md5sum | awk '{print $1}' > md5_test.hash
```

<img width="643" height="367" alt="hash been cracked" src="https://github.com/user-attachments/assets/90562815-82b7-4ed7-9dfe-27c3765054c2" />

Mursin tiivisteen Johnilla. Tässä vaiheessa on tärkeää kertoa Johnille oikea formaatti (--format=Raw-MD5), koska kyseessä ei ole tiedostomuoto vaan raaka tiiviste.

```bash
~/john/run/john --format=Raw-MD5 md5_test.hash
```
Vahvistin tuloksen: salasana **secret123** murtui välittömästi.

## Sanakirja

<img width="930" height="676" alt="deepseek reject" src="https://github.com/user-attachments/assets/837bcf6f-e02c-4a4b-a573-b4bce3a65536" />

<img width="903" height="481" alt="deepseek accept" src="https://github.com/user-attachments/assets/14122431-b7e8-4db1-8468-7e5761b5b6e2" />

<img width="897" height="625" alt="deepseek print" src="https://github.com/user-attachments/assets/8c6ff454-ad76-4493-8a37-e4ee7d8ae5e2" />

<img width="246" height="97" alt="micro " src="https://github.com/user-attachments/assets/d3f14600-8170-4abc-ad91-38bb826d601d" />

Loin kustomoidun sanakirjan `nateswords.txt` käyttämällä  `micro`-editoria. Hyödynsin sanaston rakentamisessa tekoälyä (DeepSeek) ja sosiaalisen hakkeroinnin periaatteita: ohjeistin tekoälyn listaamaan tyypillisiä heikkoja salasanoja sekä kohteelle ominaisia termejä, kuten urheilujoukkueita ja vuosilukuja. Tämä osoittaa, kuinka kohdennettu sanasto on usein massiivista yleislistaa (kuten rockyou.txt) tehokkaampi tapa murtaa salasanoja, kun kohteesta on käytettävissä taustatietoa.


<img width="647" height="123" alt="nates words crack" src="https://github.com/user-attachments/assets/a8fdf040-a7a1-4de9-8aa2-6b57eac10f92" />


Mursin testi-MD5-tiivisteen käyttämällä juuri luomaani omaa sanakirjaa.
 
```bash
~/john/run/john --wordlist=nateswords.txt --format=Raw-MD5 md5_test.hash
```
<img width="585" height="96" alt="baseball" src="https://github.com/user-attachments/assets/f751deec-e6a7-44c6-b8c6-80856c80bffd" />

Lopuksi demonstroin sääntöjen (Rules) voiman. Käytin Johnin --rules -vipua, joka muokkaa sanakirjan sanoja lennossa (lisää isoja kirjaimia, numeroita ja erikoismerkkejä). Ohjelma mursi salasanan Baseball1, vaikka sanakirjassa oli vain pienellä kirjoitettu "baseball"


<img width="655" height="327" alt="baseball cracked" src="https://github.com/user-attachments/assets/2ebf8cdd-6149-4cdc-92e1-a03bcff6e760" />

```bash
~/john/run/john --wordlist=nateswords.txt --rules --format=Raw-MD5 rule_test.hash
```
Sääntöjen käyttö on kriittistä murtamisessa, koska se säästää levytilaa ja mallintaa sitä, kuinka ihmiset tyypillisesti muokkaavat perussanoja monimutkaisemmiksi.


## Lähteet
https://stackoverflow.com/questions/20129029/a-light-solution-to-convert-text-to-pdf-in-linux
