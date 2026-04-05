# H2 Dora The Explora | Nathaniel Ssendagire 05.04.2026
## Ympäristö
### OS: Kali Linux

### Browser: Firefox 128.3.1esr (64-bit)

### Hardware Model: VMWare Workstation Pro Memory: 4.0 GB

### Processor: AMD Ryzen 3 7320U - 4 cores used

### Disk: 35 GB

### Network: NAT

## Tiivistelmä

### DORA (Digitaalisen operatiivisen kestävyyden asetus)
- Tavoitteena vahvistaa rahoitusalan digitaalista operatiivista kestävyyttä EU:ssa.
- Velvoittaa rahoituslaitoksia, kuten pankkeja, sijoitusyhtiöitä, maksulaitoksia ja vakuutusyhtiöitä, hallinnoimaan ICT-riskejä tehokkaasti.
- Sisältää säännöt ICT-häiriöiden raportoinnista ja niiden vaikutusten hallinnasta.
- Edellyttää järjestelmien säännöllistä testausta ja haavoittuvuuksien arviointia, erityisesti suurilta toimijoilta.
- Määrittelee myös riskien hallinnan kolmansien osapuolten palveluntarjoajien (esim. pilvipalvelut) suhteen.
- Soveltuu eri laajuudessa riippuen organisaation koosta ja riskiarviosta.
- Tavoitteena on yhtenäinen ja vahva digitaalisen kestävyyden kehys, joka suojaa rahoitusjärjestelmää kyberuhkilta ja teknisiltä häiriöiltä.

### TIBER-FI
- TIBER‑FI: Suomen Pankin kyberturvallisuuden testi‑kehikko finanssialalle
- Uhkapohjaiset Red Team ‑testaukset paljastavat haavoittuvuuksia
- Määrittelee roolit: testausryhmät, kontrolli, puolustajat
- Riskien hallinta: testi ei vaaranna tuotantojärjestelmiä
- Perustuu EU:n TIBER‑EU‑kehikkoon, tukee kansainvälistä yhteentoimivuutta

### Buuri
- Esitys käsittelee EU:n Digital Operational Resilience Act (DORA) ‑asetusta ja sen vaatimuksia kyberturvallisuustestaukselle.
- DORA edellyttää, että rahoitusalan toimijat tekevät uhkabasoitettua penetraatiotestausta (Threat‑Led Penetration Testing, TLPT) merkittävien ICT‑järjestelmien resilienssin arvioimiseksi.
- Materiaalissa kuvataan TIBER‑EU / TIBER‑FI ‑kehikko, joka toimii DORA‑testauksen viitekehyksenä ja ohjeistaa testauksen vaiheet ja roolit (kuten Red Team, Control Team ja Blue Team).
- Testausprosessi sisältää valmistelu‑, testaus‑ ja raportointivaiheet, joissa käytetään uhkatietoa ja realistisia hyökkäysskenaarioita järjestelmän heikkouksien paljastamiseksi.
- TLPT‑testit suunnitellaan siten, että ne jäljittelevät oikeiden uhkatoimijoiden taktiikoita, mutta turvallisesti ja kontrolloidust

## Asenna Metasploitable 2 virtuaalikoneeseen.

Asensin Metasploitablen <a href="https://www.rapid7.com/products/metasploit/metasploitable"> tältä</a>
sivulta. Asennus sujui sujuvasti eikä ongelmia ilmennyt, kunhan koneelta löytyi VMware-ohjelmisto.

<img width="1918" height="1023" alt="metasploitable website where i downloaded it from" src="https://github.com/user-attachments/assets/f15160ad-6d33-4db0-938f-73f3fd690bbd" />

## Tee Kalin ja Metasploitablen välille virtuaaliverkko. Jos säätelet VirtualBoxista / Harjoittelemme omassa virtuaaliverkossa, jossa on Kali ja Metaspoitable. Osoita testein, että 1) koneet eivät saa yhteyttä Internetiin 2) Koneet saavat yhteyden toisiinsa.

Tein tämän tehtävän VMware-ympäristössä, ja prosessi sujui hieman eri tavalla. Sain molemmat koneet yhdistettyä onnistuneesti, kun verkkokortit oli asetettu Host-Only -tilaan. Jos Kali-koneen täytyi käyttää internet-yhteyttä, verkkokortin asetukseksi tuli vaihtaa NAT, mutta muuten kaikki toimi odotetusti.

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

## Porttiskannaa Metasploitable huolellisesti ja kaikki portit (nmap -A -T4 -p-). Poimi 2-3 hyökkääjälle kiinnostavinta porttia. Analysoi ja selitä tulokset näiden porttien osalta. Voit hakea analyysin tueksi tietoa verkosta, muista merkitä lähteet.

<img width="631" height="796" alt="port scaaaaaan" src="https://github.com/user-attachments/assets/2f6dc9b5-3558-491c-9036-24e0dcc5c06f" />


<img width="762" height="772" alt="detailed port scans" src="https://github.com/user-attachments/assets/90962da3-6546-49ec-8f5a-26a8437f00b3" />

<img width="916" height="820" alt="detailed port scans pt 2" src="https://github.com/user-attachments/assets/dd31ca4d-03d9-4e1a-ba55-d140dc636859" />

<img width="750" height="721" alt="detailed port scans pt 3" src="https://github.com/user-attachments/assets/d21118c1-db19-4b75-a9fc-6773bb310441" />

Komentoon sisältyvät valinnat tarkoittavat seuraavaa:

```bash
nmap -A -T4 -p- -v 192.168.18.128
```
- -A (Aggressive scan) – ottaa käyttöön laajennetun skannauksen, joka sisältää käyttöjärjestelmän tunnistuksen, palveluiden versiotunnistuksen, skriptiskannauksen sekä tracerouten.
- -T4 (Timing template) – nopeuttaa skannausta käyttämällä aggressiivisempaa ajoitusta. Sopii luotettaviin verkkoihin, kuten laboratorioympäristöihin.
- -p- – skannaa kaikki TCP-portit (1–65535), ei vain yleisimpiä portteja.
- -v (verbose) – näyttää tarkempaa tietoa skannauksen etenemisestä ja löydöksistä.

Skannauksessa käytiin läpi kaikki TCP-portit ja tunnistettiin kohdekoneen palvelut, niiden versiot sekä mahdolliset haavoittuvuudet. Skannauksen avulla saatiin yksityiskohtainen kuva Metasploitable-koneen aktiivisista palveluista ja potentiaalisista hyökkäyspisteistä. Tulosten perusteella kohteessa havaittiin useita hyökkääjälle kiinnostavia portteja:

- 21/tcp (FTP, vsftpd 2.3.4) – palvelu sallii anonyymin kirjautumisen, mikä mahdollistaa tiedostojen lukemisen, lataamisen ja mahdollisesti myös tiedostojen kirjoittamisen. Vanha vsftpd-versio 2.3.4 sisältää tunnetun backdoor-haavoittuvuuden, joka voi antaa hyökkääjälle etäyhteyden ja pääsyn järjestelmän komentoriville. Tämä tekee palvelusta erityisen riskialttiin hyökkäyksille.

- 139/tcp ja 445/tcp (Samba/SMB) – vanhentunut Samba 3.0.20 sisältää useita tunnettuja haavoittuvuuksia, kuten tiedostojen luvattoman käytön ja etäkomentojen suorittamisen mahdollisuuden. Portti 445 on käytössä myös Microsoftin SMB-palveluissa tiedostojen ja tulostimien jakamiseen LAN-verkossa, ja avoin portti ulkoverkkoon voi altistaa hyökkäyksille, kuten WannaCry-lunnasohjelmalle. Tämä tekee palvelusta houkuttelevan kohteen hyökkääjälle.
  
- 1524/tcp (Metasploitable root shell / Ingreslock backdoor) – Portti on tarkoituksella avoinna Metasploitable-koneessa ja liittyy Ingreslock-palveluun. Se voi toimia backdoorina, jonka kautta hyökkääjä saa täydet juurioikeudet järjestelmään, tehden portista kriittisen hyökkäyspisteen.

## Lähteet

- <a href="https://www.stationx.net/nmap-cheat-sheet/">Nmap cheat-sheet</a>
- <a href="https://medium.com/@josegpach/exploiting-ftp-vulnerabilities-on-metasploitable-2-bbd935d42e23">Exploiting FTP Vulnerabilities on Metasploitable 2</a>
- <a href="https://www.manageengine.com/products/eventlog/logging-guide/firewall/how-to-detect-and-prevent-tcp-445-exploit-and-attack.html">How to detect and prevent a TCP 445 exploit</a>
- <a href="https://sensorstechforum.com/remove-ingreslock-backdoor-and-lock-tcp-1524/">Remove Ingreslock Backdoor and Lock TCP 1524</a>
- <a href="https://terokarvinen.com/tunkeutumistestaus/#h2-dora-the-explora">Tero Karvinen </a>
- <a href="https://eur-lex.europa.eu/eli/reg/2022/2554/oj/eng"> DORA </a>
- <a href="https://www.suomenpankki.fi/globalassets/bof/en/money-and-payments/the-bank-of-finland-as-catalyst-payments-council/tiber-fi/tiber-fi-2.0-procedures-and-guidelines.pdf"> TIBER-FI </a>
- <a href="https://terokarvinen.com/buuri-2026-dora-and-threat-lead-penetration-testing/buuri-2026-dora-and-threat-lead-penetration-testing--teros-pentest-course.pdf"> Buuri 2026 </a>




