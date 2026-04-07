# H3 EternalHomework | Nathaniel Ssendagire 07.04.2026
## Ympäristö
### OS: Kali Linux

### Browser: Firefox 128.3.1esr (64-bit)

### Hardware Model: VMWare Workstation Pro Memory: 4.0 GB

### Processor: AMD Ryzen 3 7320U - 4 cores used

### Disk: 35 GB

### Network: NAT / Host-Only

## Tiivistelmä

## B) Porttiskannaus ja tallennus Metasploitin tietokantaan

Harjoitus aloitettiin varmistamalla, että Metasploitin PostgreSQL-tietokanta on toiminnassa (db_status). Tämän jälkeen suoritettiin versioskannaus komennolla:
db_nmap -sV 192.168.18.128

<img width="1722" height="845" alt="db_nmpa" src="https://github.com/user-attachments/assets/dbe4c1be-62bd-4d38-be73-f038e1d267a6" />

Tämä komento suorittaa Nmap-skannauksen ja tallentaa tulokset suoraan Metasploitin tietokantaan myöhempää käyttöä varten. -sV (Service Version) on kriittinen, sillä se tunnistaa palveluiden tarkat versiot (esim. vsftpd 2.3.4), mikä helpottaa oikean hyökkäysmoduulin valintaa.

## C) Tietokannan tarkastelu: "hosts" ja "services"

### Skannauksen jälkeen tarkasteltiin tietokantaan tallennettuja tietoja:

- hosts: Näytti kohteen IP-osoitteen, MAC-osoitteen ja tunnistetun käyttöjärjestelmän (Linux).

- services: Listasi kaikki 24 avointa porttia

<img width="800" height="585" alt="services" src="https://github.com/user-attachments/assets/b7acf89c-2c6b-44e5-94fb-50ffec569253" />

## D) Internet famous: Tunnettu haavoittuvuus

Kohteesta löytyi useita historiallisesti merkittäviä haavoittuvuuksia. Valitsin vsftpd 2.3.4 -palvelun. Tämä on kuuluisa "supply chain" -hyökkäyksen kohde vuodelta 2011, jolloin ohjelmiston lähdekoodiin lisättiin luvatta takaovi (backdoor), joka aktivoituu tietyllä merkkijonolla (:)) käyttäjänimessä.

<img width="1003" height="248" alt="search vsftpd" src="https://github.com/user-attachments/assets/70c3c910-1c5c-4a3f-aa91-06d1020a7347" />

## E) Nmap-tiedostojen (-oA) ja Metasploit-tietokannan vertailu

### Vertailin kahta eri tallennustapaa:

Nmap -oA foo: Luo kolme tiedostoa (.nmap, .gnmap, .xml).

  - Hyvää: Pysyvyys, siirrettävyys ja helppo tekstitiedostojen haku (grep).

Metasploit DB: Tallentaa tiedot PostgreSQL-tietokantaan.

  - Hyvää: Integraatio. Voin asettaa maalin automaattisesti komennolla hosts -R ilman IP-osoitteen kopiointia.

  - Yhteenveto: DB on paras työskentelyyn, -oA on paras raportointiin ja arkistointiin.

<img width="608" height="435" alt="version file" src="https://github.com/user-attachments/assets/40aa30c4-d04a-408c-86cc-03d33af9f640" />

## F) Murtautuminen vsftpd-palveluun

Käytin hyökkäykseen moduulia exploit/unix/ftp/vsftpd_234_backdoor

- 1 . set RHOSTS 192.168.18.128

- 2 . exploit
 
<img width="1262" height="410" alt="show options" src="https://github.com/user-attachments/assets/e963c28d-17aa-489f-a66f-b35e3cd839c7" />

Hyökkäys onnistui välittömästi ja avasi suoran komentoriviyhteyden (shell) porttiin 6200. Tarkistin oikeudet komennolla whoami, joka palautti root.

<img width="932" height="241" alt="were innnnnnn les go" src="https://github.com/user-attachments/assets/1643dbe5-fbdf-4909-8506-0c2a4785359f" />
<img width="893" height="577" alt="valuable info" src="https://github.com/user-attachments/assets/6dc1a1e8-f6e9-4de8-8c01-4893a42b249c" />
<img width="682" height="382" alt="proper ifconfig" src="https://github.com/user-attachments/assets/dbd8c8d8-172a-4fdf-bb55-eceaff12a4f6" />

<img width="437" height="142" alt="commaaands in meterpreter" src="https://github.com/user-attachments/assets/0b3b8510-8789-4573-8e7b-762bc116d0e1" />

<img width="596" height="695" alt="seeing insideee" src="https://github.com/user-attachments/assets/e4463874-d465-49e3-8a0e-b7b66f6e33f8" />

## G) Lateral movement tiedon kerääminen

Keräsin kohteesta tietoja, joita voisi käyttää verkossa etenemiseen:

- cat /etc/passwd: Listasi käyttäjät (esim. msfadmin, user, service).

- ifconfig: Varmisti verkkorajapinnat.

- arp -a: Näytti muiden laitteiden MAC-osoitteet verkossa.

- cat /etc/shadow: Koska olin root, pääsin käsiksi salasanojen tiivisteisiin (hashes), joita voisi käyttää "Pass-the-Hash" -hyökkäyksissä muille koneille.

- ## H) Toinen murtautumistapa: UnrealIRCd

- Kokeilin toisena tapana IRC-palvelun takaovea:

<img width="1087" height="245" alt="ircd" src="https://github.com/user-attachments/assets/e0ef235a-7119-43ad-b286-9a3c5cdab7ee" />

```bash
- exploit/unix/irc/unreal_ircd_3281_backdoor
```

Aluksi tuli pieni ongelma...
<img width="757" height="131" alt="error" src="https://github.com/user-attachments/assets/004774b2-2244-49dc-acbc-6c4bcc38b0f9" />

Syy oli se, että minulla ei ollut payloadia valittu, joten kävin katsomassa mitä vaihtoehjtoja minulla on:

<img width="760" height="636" alt="payloads" src="https://github.com/user-attachments/assets/eb0e104e-a044-4535-bc85-39262b2a208a" />
<img width="748" height="822" alt="2nd session" src="https://github.com/user-attachments/assets/50499840-842a-4841-bd40-edd550a2335f" />



Asetin payloadiksi cmd/unix/reverse_perl ja määrittelin LHOST (oma IP). Tämä avasi toisen istunnon kohteeseen


## i) Meterpretrin toiminta ja  ominaisuudet

### Istunnon taustauttaminen

Painoin Ctrl+Z, mikä pysäyttää aktiivisen yhteyden ja palauttaa hallinnan Metasploit-konsoliin sulkematta itse yhteyttä. Tämä on välttämätöntä, jotta voimme ajaa "post-exploitation"-moduuleita istuntoa vasten.

<img width="747" height="106" alt="tausta meterpreter" src="https://github.com/user-attachments/assets/c31669be-3333-4394-8934-c5ff691b6f80" />

### Moduulin valinta:

Otin käyttöön moduulin post/multi/manage/shell_to_meterpreter. Tämän moduulin tarkoitus on lähettää uhrin koneelle Meterpreter-payload, joka tarjoaa huomattavasti enemmän ominaisuuksia kuin pelkkä perinteinen Linux-shell.

<img width="763" height="510" alt="meterpreter activated" src="https://github.com/user-attachments/assets/f0852ba2-686a-45d0-adde-0217bf144551" />

Päivitin perusyhteyden Meterpreter-sessioon (shell_to_meterpreter). Meterpretrillä demonstroin:

- sysinfo: Järjestelmän tiedot.

- getuid: Vahvisti root-oikeudet.

- hashdump: Yritin tiivisteiden ajoa, mutta Linux-ympäristössä hyödynsin manuaalista /etc/shadow-lukua tiedon keräämiseen.

<img width="437" height="142" alt="commaaands in meterpreter" src="https://github.com/user-attachments/assets/0b3b8510-8789-4573-8e7b-762bc116d0e1" />

<img width="596" height="695" alt="seeing insideee" src="https://github.com/user-attachments/assets/e4463874-d465-49e3-8a0e-b7b66f6e33f8" />

## j) Istunnon dokumentointi ja tekniset haasteet

Harjoituksen alussa tavoitteena oli käyttää script -fa log001.txt -työkalua istunnon reaaliaikaiseen nauhoittamiseen. Teknisen erehdyksen vuoksi työkalu ei kuitenkaan tallentanut alkuvaiheen msfconsole-tapahtumia odotetulla tavalla.

Vaihtoehtoinen menetelmä (Simulaatio):
Koska reaaliaikainen loki puuttui, palautin tilanteen simuloimalla script-työkalun toimintaa yhdistämällä kaksi tietolähdettä:

- Linux-historia: Tallensin terminaalikomennot (history > log001.txt).

- Metasploit-historia: Hain kaikki hyökkäysvaiheet suoraan Metasploitin sisäisestä lokista (cat ~/.msf4/history >> log001.txt).

<img width="382" height="52" alt="log file 3" src="https://github.com/user-attachments/assets/8b98d8ab-47ed-4198-aef2-24efda28de3a" />
<img width="211" height="66" alt="log file 2" src="https://github.com/user-attachments/assets/a52ea79d-9e66-4e09-a2eb-1768eeb8686a" />

<img width="376" height="47" alt="reaaaad" src="https://github.com/user-attachments/assets/8d32b1b7-3d12-49a4-b801-635120705739" />
<img width="621" height="706" alt="reaaaad 2" src="https://github.com/user-attachments/assets/68e380fb-b811-4bc1-a855-f43fe6e43f44" />

Tämä yhdistetty lokitiedosto vastaa sisällöltään script-työkalun tuottamaa dataa, sisältäen sekä konfigurointivaiheet että suoritetut exploitit.

## K) Pivot point ja tiedonhallinta

Tässä vaiheessa kaikki harjoituksen aikana kertyneet tiedostot kerättiin yhteen raportointikansioon (~/metasploit_lab_raportti). Kansio sisältää seuraavat artefaktit:

- log001.txt: Koottu komentohistoria (Linux ja Metasploit).

- metasploit_scan_results.nmap / .gnmap / .xml: Nmap-skannauksen eri tallennusmuodot.

- metasploitable_scan.xml: Metasploit-tietokannan export-tiedosto.

Tiedostojen keskittäminen mahdollistaa tehokkaan tiedonhaun koko projektin datasta yhdellä komennolla.

### Tekninen haku (grep -r)

Käytin grep -r -komentoa löytääkseni kriittisen hyökkäyspisteen (Pivot Point) kootusta datasta:
```bash
grep -r "vsftpd 2.3.4"
```
Tämä haku palautti tiedon, että IP-osoitteessa 192.168.18.128 ja portissa 21/tcp pyörii haavoittuva palvelu. Samalla skannausdatasta (Nmap-tiedosto) varmistui laitteen MAC-osoite: 00:0C:29:BA:D6:0F.

<img width="606" height="213" alt="file thingy" src="https://github.com/user-attachments/assets/7210ee60-7e59-48c1-883d-99a3d852b888" />

<img width="753" height="443" alt="data outt-2" src="https://github.com/user-attachments/assets/6eb83f3b-a62e-48a9-afd8-79aebfc38198" />

<img width="608" height="435" alt="version file" src="https://github.com/user-attachments/assets/58b1f8b6-f380-4bf7-aae2-47f6152e9a4f" />

## I) Attaack! – MITRE ATT&CK analyysi

Tässä harjoituksessa suoritetut toimenpiteet ja hyökkäysvaiheet voidaan kartoittaa suoraan kansainväliseen **MITRE ATT&CK** viitekehykseen. Analyysi osoittaa, miten yksittäiset komennot muodostavat loogisen hyökkäysketjun.

| Taktiikka (Tactic) | Tekniikka (Technique) | ID | Kuvaus harjoituksesta |
| :--- | :--- | :--- | :--- |
| **Reconnaissance** | Active Scanning | T1595 | Suoritin kohdejärjestelmän tiedustelun käyttämällä `db_nmap` ja `-sV` -lippuja haavoittuvien palveluiden tunnistamiseksi. |
| **Discovery** | Network Service Discovery | T1046 | Analysoin skannauksen tuloksia (esim. `vsftpd 2.3.4` ja `UnrealIRCd`), mikä toimi pivot-pisteenä hyökkäykselle. |
| **Initial Access** | Exploit Public-Facing Application | T1190 | Hyödynsin tunnettuja takaovia (backdoor) päästäkseni järjestelmään sisälle ilman käyttäjätunnuksia. |
| **Execution** | Command and Scripting Interpreter: Unix Shell | T1059.004 | Ajoin komentoja suoraan uhrin shell-ympäristössä ja käytin `reverse_perl` -payloadia yhteyden muodostamiseen. |
| **Discovery** | System Information Discovery | T1082 | Keräsin tietoa järjestelmästä (kernel-versio, verkkorajapinnat) komennoilla `sysinfo` ja `ifconfig`. |
| **Credential Access** | OS Credential Dumping | T1003 | Luin `/etc/shadow` -tiedoston sisällön pääkäyttäjänä, mikä mahdollisti salasanojen tiivisteiden haltuunoton. |

## Lähteet
https://terokarvinen.com/tunkeutumistestaus/#taysin-laillinen-sertifikaatti
http://docs.rapid7.com/metasploit/metasploitable-2-exploitability-guide/
https://attack.mitre.org/
https://attack.mitre.org/tactics/TA0003/
https://attack.mitre.org/techniques/T1189/
https://attack.mitre.org/resources/faq/
