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











## Lähteet
https://stackoverflow.com/questions/20129029/a-light-solution-to-convert-text-to-pdf-in-linux
