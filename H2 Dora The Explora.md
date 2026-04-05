# H1 Kybertappoketju | Nathaniel Ssendagire 05.04.2026
## Ympäristö
### OS: Kali Linux

### Browser: Firefox 128.3.1esr (64-bit)

### Hardware Model: VMWare Workstation Pro Memory: 4.0 GB

### Processor: AMD Ryzen 3 7320U - 4 cores used

### Disk: 35 GB

### Network: NAT

## Tiivistys

## Asenna Metasploitable 2 virtuaalikoneeseen.

Asensin Metasploitablen <a href="https://www.rapid7.com/products/metasploit/metasploitable"> tältä</a>
sivulta. Asennuksessa ei ilmennyt mitään ongelmaa, se oli tosi suoraviivaista kunhan koneelta löytyi VMware.

<img width="1918" height="1023" alt="metasploitable website where i downloaded it from" src="https://github.com/user-attachments/assets/f15160ad-6d33-4db0-938f-73f3fd690bbd" />

## Tee Kalin ja Metasploitablen välille virtuaaliverkko. Jos säätelet VirtualBoxista / Harjoittelemme omassa virtuaaliverkossa, jossa on Kali ja Metaspoitable. Osoita testein, että 1) koneet eivät saa yhteyttä Internetiin 2) Koneet saavat yhteyden toisiinsa.

Tein tämän tehtävän VMwarella niin se meni vähän eri tavalla. Sain molemmat koneet yhdistettyä jos verkkokortit olivat asetettu host-onlyyn. Jos kalin täytyi yhdistyä nettiin se piti vaihtaa NAT:iin, mutta muuten kaikki toimi niin kuin pitikin

<img width="703" height="436" alt="metasploitable address" src="https://github.com/user-attachments/assets/8c4c4694-4230-4dab-8279-fff4343938ae" />

<img width="712" height="426" alt="network adapter on host only to not have access to the browser" src="https://github.com/user-attachments/assets/19f2ced9-a199-40d9-b279-e67f835a75c0" />

<img width="710" height="617" alt="network adapter on NAT to be able to ping" src="https://github.com/user-attachments/assets/ef3503cf-4877-40b0-ac3f-c71bfb0e3cfe" />

<img width="571" height="328" alt="ping the metasploit" src="https://github.com/user-attachments/assets/0ce3e38d-13e0-4ae5-ac89-11d5f6d50b44" />

## Etsi Metasploitable porttiskannaamalla (nmap -sn). Tarkista selaimella, että löysit oikean IP:n - Metasploitablen weppipalvelimen etusivulla lukee Metasploitable.

<img width="661" height="335" alt="sn scan" src="https://github.com/user-attachments/assets/bec91330-51fb-468f-ac10-58caf3ad2470" />

<img width="661" height="457" alt="host is up" src="https://github.com/user-attachments/assets/3713a0c8-b795-466c-b574-15000e5da9b1" />

<img width="763" height="568" alt="metasploitable web page on the vm" src="https://github.com/user-attachments/assets/52f70f7d-a5d3-4508-b0c0-0ead734d688d" />

Komennolla etsittiin aktiivisia laitteita lähiverkosta ilman porttiskannausta. Skannauksen tuloksena löytyi useita aktiivisia IP-osoitteita, joista yksi oli 192.168.18.128.

Tämän jälkeen varmistin oikean kohteen avaamalla selaimella osoitteen http://192.168.18.128. Sivulla näkyi teksti "Metasploitable", mikä vahvisti, että kyseessä oli Metasploitable-kone.


