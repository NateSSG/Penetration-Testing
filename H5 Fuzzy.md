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

Ensiksi latasin sanalistan, jota fuzz tulee käyttämään.

<img width="647" height="392" alt="terokarvinen fuzz" src="https://github.com/user-attachments/assets/ae118bf0-e139-429d-96d1-b56e2a56188f" />

Seuraavaksi latasin dirfuzt-1 tehtävän ja vaihdoin priviledges. 

<img width="743" height="831" alt="fufff without any limitations" src="https://github.com/user-attachments/assets/3c7b73e5-9904-4d97-b916-ffab66692e05" />

Sitten käynnistin ffufin ja huomaan, että tiedostojen koot ovat suurimmaksi osaksi sama, joten tämä meinaa sitä, että se pitää suodattaa pois, jotta näemme ne "tärkeämmät / kiinnostavammat" tiedot. 

<img width="765" height="622" alt="wp-admin" src="https://github.com/user-attachments/assets/1b453a84-a145-406b-b9b9-8302889d6f13" />

Ajoin saman komennon, mutta lisäsin -fs komennon siihen perään, joka siis karsii koot 28 ja 154 pois. Mistä sitten tuo löytyi wp-admin sivu.

<img width="611" height="270" alt="curl the website" src="https://github.com/user-attachments/assets/761aa992-c20d-4fb3-b840-fc013bef8ac1" />

Seuraavaksi curlasin sivun ja kappas vain sieltä löytyi lippu!

## Fuff me. Asenna FuffMe-harjoitusmaali. Karvinen 2023

<img width="742" height="607" alt="ffuf me website home page" src="https://github.com/user-attachments/assets/b3de087e-5eeb-4312-acd5-abd5d833e8d9" />

<img width="762" height="862" alt="ffuf on the browser" src="https://github.com/user-attachments/assets/63252789-c58e-48d7-a53e-226019d6b64f" />

Käytin tehtäväannossa Tero Karvisen laatimaa ohjetta FuffMe-harjoitusmaalin asentamiseen joka löytyy tästä osoitteest: https://terokarvinen.com/2023/fuffme-web-fuzzing-target-debian/

## c) Basic Content Discovery

<img width="757" height="522" alt="ffuf c)" src="https://github.com/user-attachments/assets/88b98149-ca82-43df-b3e3-65a261fea837" />

Tässä ei ollut oikein mitään erikoista. Käytin samaa sanalistaa ja ffufasin tuon hakemiston josta löytyi pari tiedostoa

## d) Content Discovery With Recursion

<img width="748" height="687" alt="ffuf d)" src="https://github.com/user-attachments/assets/4e67b064-961b-4bb1-a632-bdb5ab980a2b" />

Seuraavassa tehtävässä lisäsin tuohon samaan komentoon "-recursion" siihen perään mikä siis lisää alihakemiston löytämäänsä hakemistoon eli se menee syvemmälle siitä hakemistosta minkä se on löytänyt.

## e) Content Discovery With File Extensions

<img width="760" height="560" alt="ffuf e) 2" src="https://github.com/user-attachments/assets/4b240586-d1b8-440d-b4ce-aa652aafcb5c" />

Seuraavassa tehtävässä piti ffufata tiedosto päätteitä

<img width="761" height="847" alt="ffuf e) 1" src="https://github.com/user-attachments/assets/0737fd0c-af3c-4bb0-ac43-81a7191debd1" />

Seassa oli aika paljon "turhaa" mutta sieltä löyty tuo logs hakemisto. Veikkaan että se on jokin hakemisto, koska status on 301 eikä 200 ok niin kuin edellisissä tehtävissä.

## f) No 404 Status

<img width="754" height="562" alt="ffuf f)" src="https://github.com/user-attachments/assets/0114625e-da60-4bca-a1b1-b57db79b0f89" />

<img width="762" height="690" alt="ffuf f) 2" src="https://github.com/user-attachments/assets/6d60ed51-2ff2-4a09-95c3-3a1177d4551e" />

Tämä tehtävä on suurin piirtein sama kuin se ensimmäinen (eli siis a) ). Ffufasin tuon hakemiston, josta tuli hirvee lista kaikkea "turhaa"

<img width="765" height="532" alt="ffuf f) 3" src="https://github.com/user-attachments/assets/67e0c604-97be-4bdc-842b-cc2dae5101e5" />

Seuraavaksi suodatin / karsin kaikki nuo "669" kokoiset tiedostot pois ja sieltä löytyi sitten tuo "secret" niminen sivu!




