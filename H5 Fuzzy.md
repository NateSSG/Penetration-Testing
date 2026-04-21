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

Käytin tehtäväannossa Tero Karvisen laatimaa ohjetta FuffMe-harjoitusmaalin asentamiseen joka löytyy tästä osoitteest: https://terokarvinen.com/2023/fuffme-web-fuzzing-target-debian/
