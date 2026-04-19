# H4 Täysin Laillinen Sertifikaatti | Nathaniel Ssendagire 07.04.2026
## Ympäristö
### OS: Kali Linux

### Browser: Firefox 128.3.1esr (64-bit)

### Hardware Model: VMWare Workstation Pro Memory: 4.0 GB

### Processor: AMD Ryzen 3 7320U - 4 cores used

### Disk: 35 GB

### Network: NAT / Host-Only

## Tiivistelmä

### OWASP 2021: OWASP Top 10:2021

- **Laajuus**: Yleinen ongelma – 94 % testatuista sovelluksista sisälsi pääsynhallinnan (Access Control) puutteita.

- **Seuraukset**: Luvaton tiedonsaanti, toisten käyttäjien tilien peukalointi (esim. IDOR) ja ylläpito-oikeuksien anastaminen.

- **Yleisimmät virheet**: "Pienimmän oikeuden" periaatteen laiminlyönti, rikkoutunut CORS tai API-kutsujen suojauksen puute (POST, PUT, DELETE).

- **Ennaltaehkäisy**: Tarkistukset luotettavalla palvelimella, oletuksena aina estetty (deny by default) ja oikeuksien todentaminen jokaisessa pyynnössä.

- **Esimerkkihyökkäys**: Hyökkääjä voi katsoa toisen tietoja muuttamalla URL-osoitetta (esim. ?acct=toisen_tili) tai avata pakottamalla (force browsing) ylläpitäjien sivuja.

### PortSwigger Academy: Insecure direct object references (IDOR)

- **Mikä on IDOR**: Pääsynhallinnan haavoittuvuus, jossa järjestelmä antaa käyttäjälle suoran pääsyn resursseihin (esim. tiedostoihin tai tietokantatietoihin) pelkän syötteen perusteella ilman oikeuksien tarkistusta.

- **Seuraukset**: Mahdollistaa omien oikeuksien luvattoman laajentamisen (privilege escalation) joko toisten peruskäyttäjien tietoihin tai jopa ylläpitäjän tasolle.

- Esimerkki 1 (Tietokannat): Hyökkääjä muuttaa selaimen osoiterivillä olevaa asiakasnumeroa (esim. ?customer_number=132355 -> 132356) ja pääsee lukemaan toisen henkilön tilin tiedot.

- Esimerkki 2 (Tiedostot): Palvelin tallentaa arkaluonteisia tiedostoja, kuten chat-lokeja, ennakoitavilla juoksevilla numeroilla (esim. 12144.txt). Hyökkääjä vaihtaa numeron toiseen ja kaappaa muiden käyttäjien keskusteluja ja salasanoja.

### PortSwigger Academy: Path traversal

- **Mikä se on**: Haavoittuvuus, joka mahdollistaa hyökkääjän lukevan palvelimen mielivaltaisia tiedostoja, kuten lähdekoodia, salasanoja (esim. /etc/passwd) tai käyttöjärjestelmän tietoja.

- **Miten se toimii**: Hyökkääjä syöttää tiedostonimeä kysyvään parametriin hakemistopolun peruutusmerkkejä (esim. ../../../) päästäkseen ulos sovelluksen oletuskansiosta.

- **Suojauksien kierto**: Kehittäjien asettamia filttereitä voidaan huijata esimerkiksi absoluuttisilla poluilla, sisäkkäisillä merkeillä (....//), URL-koodauksella (%2e%2e%2f) tai null-tavuilla (%00).

- Ennaltaehkäisy (Pääsääntö): Tehokkain tapa on olla koskaan viemättä käyttäjän antamaa syötettä suoraan tiedostojärjestelmän rajapintoihin (APIs).

- Ennaltaehkäisy (Vaihtoehto): Jos syötettä on pakko käyttää, vertaa sitä sallittujen arvojen listaan (whitelist) ja varmista kooditasolla, että lopullinen polku alkaa aina oikeasta juurihakemistosta.

### PortSwigger Academy: Cross-site scripting

- Mikä se on: Haavoittuvuus, joka mahdollistaa haitallisen JavaScript-koodin suorittamisen pahaa-aavistamattoman uhrin selaimessa.

- **Seuraukset**: Hyökkääjä voi esiintyä uhrina, tehdä toimintoja hänen puolestaan ja varastaa arkaluonteisia tietoja (kuten kirjautumistunnuksia tai istuntoevästeitä).

- **Kaksi** **päätyyppiä**: Heijastettu (Reflected, hyökkäys piilotetaan esim. uhrille lähetettävään linkkiin) ja Tallennettu (Stored, hyökkäys tallentuu pysyvästi tietokantaan kaikkien nähtäville).

- **DOM-pohjainen XSS**: Kolmas tyyppi, jossa virhe ei tapahdu palvelimella, vaan sivuston oman selainpään (client-side) JavaScriptin käsitellessä tietoa turvattomasti.

- **Ennaltaehkäisy**: Suodata ja rajoita kaikki syötteet tarkasti, koodaa tulosteet (output encoding, esim. < muotoon &lt;) ja käytä CSP-tietoturvaotsikoita (Content Security Policy).


## a) Totally Legit Sertificate. Asenna OWASP ZAP, generoi CA-sertifikaatti ja asenna se selaimeesi.

<img width="751" height="573" alt="proxy config" src="https://github.com/user-attachments/assets/62f9abc3-3ab1-46ad-b5e1-76ec809014f2" />

<img width="751" height="577" alt="save the cert" src="https://github.com/user-attachments/assets/ca20b064-34dd-4088-82b1-d7812e23c30e" />

<img width="628" height="356" alt="save the cert into a folder" src="https://github.com/user-attachments/assets/d091dc6d-909e-4fcc-aea0-e246203f3e4e" />

<img width="761" height="477" alt="view cert" src="https://github.com/user-attachments/assets/fef3c9d5-56d3-4575-8d35-3351d5514585" />

<img width="672" height="468" alt="import cert" src="https://github.com/user-attachments/assets/785ff4f5-1d90-43d6-bbd2-65ed76ef43f6" />

<img width="752" height="352" alt="selecting the cert" src="https://github.com/user-attachments/assets/17bf5ad5-9327-4fdd-b621-a7dc32eb84f5" />

<img width="751" height="287" alt="trust is selected" src="https://github.com/user-attachments/assets/c52ac556-6fcd-4e6c-8249-ab77d3f395fa" />

- Avattuani ZAP-sovelluksen, varmistin, että proxy kuunteli localhostia portissa 8080.
Sen jälkeen tallensin ZAPin juurisertifikaatin (CA Certificate) koneelle ja toin (import) sen Firefoxin varmenteisiin.

<img width="1305" height="473" alt="select manual explore" src="https://github.com/user-attachments/assets/5b437eed-99d5-4d0f-a485-dc7bc02402f5" />

<img width="1308" height="481" alt="select launch browser" src="https://github.com/user-attachments/assets/f603b479-fc74-4293-8357-eacf3a60c68d" />

<img width="1275" height="398" alt="example domain" src="https://github.com/user-attachments/assets/588d9bdc-efca-4228-baf6-01bbbbb743dd" />

<img width="1722" height="188" alt="proxy result" src="https://github.com/user-attachments/assets/938b4f1c-23b8-4f08-a191-3d23531efd84" />

- Seuraavaksi testattiin, ilmestyykö liikennettä ZAPin History-välilehdelle.

- Kuten huomaatte, terminalissa näkyy juuri haettua sivua eli http://example.com/

## b) Kettumaista. Asenna "FoxyProxy Standard" Firefox Addon, ja lisää ZAP proxyksi siihen

- FoxyProxyn asennus oli tuskallista 😂, meni hetki tajuta mitä tekee mikäkin, mutta päästiin kuitenkin maaliin.

<img width="1267" height="448" alt="foxyproxy" src="https://github.com/user-attachments/assets/4240f015-f3b3-41aa-816e-14978783c86b" />


- Ensiksi lisäsin FoxyProxy-lisäosan Firefoxiin.


<img width="295" height="328" alt="foxyproxy select options" src="https://github.com/user-attachments/assets/a0e8edcc-0717-481c-aa38-581b97ae4892" />


- Seuraavaksi avasin FoxyProxyn asetukset.

<img width="748" height="536" alt="foxyproxy config" src="https://github.com/user-attachments/assets/54bb0472-e66e-4641-b4f6-eb72fc038fe6" />


- Määritin uuden välityspalvelimen antamalla sille samat asetukset, jotka olivat ZAPissa (localhost ja portti 8080). Näin kaikki liikenne, jonka FoxyProxy nappaa, menee suoraan ZAPiin.

<img width="1160" height="227" alt="foxyproxyyyyy" src="https://github.com/user-attachments/assets/537eb1cf-e2da-44a7-8334-f83c2ca86eec" />


- Seuraavaksi piti määritellä "Proxy by Patterns". Tämä ominaisuus suodattaa kaiken muun liikenteen pois paitsi ne osoitteet, jotka on erikseen määritelty sallituksi. Testien vuoksi olin laittanut sääntöihin muutamia eri osoitteita.

<img width="311" height="327" alt="select that one" src="https://github.com/user-attachments/assets/21ec8367-71f2-4fb5-9973-5ef9c3be5f90" />


- Valitsin laajennuksesta päälle tilan "Proxy by Patterns".

<img width="760" height="907" alt="portswig search" src="https://github.com/user-attachments/assets/6cf6dd4d-dac2-48bb-ab03-d67af882754e" />


- Sitten testasin, toimiiko määritelty filtteri. Etsin selaimella PortSwiggerin sivuston sekä google.com:in.

<img width="756" height="302" alt="no google" src="https://github.com/user-attachments/assets/a951e505-baa5-482b-9971-e97a70a04ae5" />


- ZAPin historiassa ei näkynyt Googlea, joten voidaan todeta, että filtteri toimii kuten pitääkin 👍.

<img width="416" height="378" alt="SITES" src="https://github.com/user-attachments/assets/37554fde-ef7d-4908-9dac-29ed535a4721" />

- Nyt tulee päivän vaikein osuus (tai no, minulla ainakin kesti noin tunti tämän ratkaisemisessa 😅). Tehtävänannossa luki, että piti näyttää ZAPin löytävän kuvatiedostoja. Kun katsoin History-välilehteä, siellä ei ollut yhtään mitään. Luulin, että FoxyProxyssa oli jokin asetusvirhe ja testasin koko prosessin uudestaan, mutta mikään ei auttanut...

- Kunnes katsoin ZAPin vasenta sivupalkkia, jossa lukee "Sites". Klikkasin sitä ja kappas, siellä näkyy kansiopuu sivuista, joilla olin käynyt! Koska kuvat ovat ZAPin mielestä usein vain "kohinaa", se piilottaa ne oletuksena historiasta, mutta tallentaa ne silti sivupuuhun.

<img width="982" height="520" alt="portswig image" src="https://github.com/user-attachments/assets/02f90d4c-0ed8-4544-8fb3-31de4ace0c47" />


- Etsin sieltä PortSwiggerin kansion ja vuhuu, mä sain kuvan löydettyä!

## Cross Site Scripting (XSS)

### C) Reflected XSS into HTML context with nothing encoded

<img width="1725" height="566" alt="cross-site scripting" src="https://github.com/user-attachments/assets/fdf3f3f5-7e36-4895-93dd-65776e5ad644" />

### D) Stored XSS into HTML context with nothing encoded

<img width="767" height="657" alt="cross-site scripting2" src="https://github.com/user-attachments/assets/fa852d17-80e9-41e2-9eed-5b167e630c18" />

<img width="1527" height="511" alt="cross-site scripting2 passed" src="https://github.com/user-attachments/assets/5bc4b112-1fbf-43e3-9c1b-abfbf8eb407a" />

- Kummassakaan tehtävässä sovellus ei puhdistanut syötteitä (esim. muuttanut < ja > -merkkejä turvalliseen muotoon). Syötin hakukenttään ja kommentteihin koodin <script>alert(1)</script>. Selain luuli syötettäni oikeaksi koodiksi ja suoritti ponnahdusikkunan onnistuneesti.

- Vaarattoman ponnahdusikkunan avaaminen osoittaa kiistattomasti, että järjestelmä on haavoittuvainen ja hyökkääjä voisi yhtä helposti ajaa sivustolla oikeaa haittakoodia (esimerkiksi varastaa muiden käyttäjien istuntoevästeitä).

## Path traversal

### F) File path traversal, simple case. Laita tarvittaessa Zapissa kuvien sieppaus päälle. 

<img width="1708" height="911" alt="manipulate" src="https://github.com/user-attachments/assets/02627526-7d25-4d98-ae20-317449284acb" />


- Tässä tehtävässä hyödynsin aiemmin oppimaani tekniikkaa. Etsin ZAPin "Sites"-puusta Labin kansion. Sen sisältä löytyi kuvan hakeva GET-pyyntö, joka muodostui, kun avasin PortSwiggerin tuotesivun. Seuraavaksi tehtävänä oli manipuloida tätä pyyntöä (Manual Request Editorissa) siten, että saadaan tärkeitä palvelimen tiedostoja näkyviin

- Muokkasin pyynnön filename-parametria asettamalla sen arvoksi haavoittuvuutta hyödyntävän polun ../../../etc/passwd

- Nuo ../ -merkit (eli hakemistopolun peruutusmerkit) käskevät palvelinta "nousemaan yhden kansion verran ylöspäin" hakemistopuussa. Koska laitoin niitä kolme kappaletta peräkkäin (../../../), pakotin palvelimen peruuttamaan kuvakansiosta aina kiintolevyn juurihakemistoon (/) saakka. Kun olin juurihakemistossa, reitti jatkui suoraan /etc/passwd -tiedostoon.

- Koska koodissa ei ollut mitään suodattimia tai palomuureja tarkistamassa näitä merkkejä, palvelin totteli käskyäni kiltisti ja palautti minulle kuvatiedoston sijasta palvelimen salasanatiedoston. Tämä oli täydellinen esimerkki siitä, mitä tapahtuu, kun käyttäjän syötteitä ei sanitoida ollenkaan!


<img width="1553" height="270" alt="get image customization" src="https://github.com/user-attachments/assets/07fa5431-ff23-4788-9317-24a7659aca3f" />


<img width="768" height="177" alt="select text" src="https://github.com/user-attachments/assets/e6d39eef-8adb-4e41-8ad4-7a65fe7e8ded" />


- Koska alkuperäinen pyyntö haki kuvaa, ZAP yritti näyttää hyökkäyksen vastauksen oletuksena kuvana (mikä näyttää tyhjältä/rikkinäiseltä). Jotta näemme onnistuneen hyökkäyksen palauttaman tiedoston sisällön raakatekstinä, "Body"-näkymä piti vaihtaa asennosta "Image" asentoon "Text".

<img width="877" height="422" alt="it displays all the info needed" src="https://github.com/user-attachments/assets/a6affb70-1972-48a7-872a-91ea300e96d4" />


- Tämän jälkeen palvelimen /etc/passwd -tiedoston sisältö pärähti ruudulle!

<img width="1001" height="452" alt="lab solveeeed" src="https://github.com/user-attachments/assets/28a823c3-6126-4d3a-b22c-5d1db7d0876d" />

## G) File path traversal, traversal sequences blocked with absolute path bypass
<img width="1418" height="246" alt="same thing just a bit different" src="https://github.com/user-attachments/assets/936ae46e-dc45-4666-90bb-5b4bc08622e8" />

<img width="565" height="533" alt="response of the same thing" src="https://github.com/user-attachments/assets/691b1904-f991-4d59-a60a-07c0569105bb" />

- Toisessa tehtävässä vihje kuului näin: "The application blocks traversal sequences but treats the supplied filename as being relative to a default working directory."

- Tästä vihjeestä päättelin, että kehittäjät olivat laittaneet palvelimelle yksinkertaisen filtterin, joka pysäyttää pyynnön heti, jos se huomaa ../ -merkkejä tiedostonimessä. Tajusin, että minun ei siis kannata edes yrittää käyttää perinteistä reittiä "taaksepäin" kansioissa.

- Jos ../ on kerran estetty, päätin ohittaa koko filtterin antamalla suoraan täydellisen eli absoluuttisen polun. Vaihdoin filename-parametriksi yksinkertaisesti /etc/passwd. Koska koodissani ei ollut yhtäkään ../-pätkää, palvelimen palomuuri ei edes reagoinut hyökkäykseeni, vaan meni suoraan järjestelmän juurihakemistoon ja sylki salasanatiedoston suoraan ruudulleni!

<img width="1712" height="783" alt="file path traversal2" src="https://github.com/user-attachments/assets/3f93ec97-ac1b-415f-9ff6-3407c12a927f" />


## H) File path traversal, traversal sequences stripped non-recursively

<img width="1425" height="135" alt="same thing just a bit different 2" src="https://github.com/user-attachments/assets/ecdfab59-b057-4558-a47c-622cd43f4e32" />
<img width="515" height="560" alt="same thing response 2" src="https://github.com/user-attachments/assets/becee4ec-d279-4474-9124-40c9407ffb67" />
<img width="1715" height="791" alt="theres no i in team" src="https://github.com/user-attachments/assets/c897b0e5-583e-4810-b803-60f6a8d5f890" />

- Toisessa labrassa vihje oli hieman erilainen: "The application strips path traversal sequences from the user-supplied filename before using it."

- Tämä paljasti, että tällä kertaa sovellus ei suoraan estä pyyntöä, vaan se yrittää puhdistaa sen poistamalla (strippaamalla) kaikki ../ -merkit pyynnöstä. Tästä tuli heti mieleen klassinen kehittäjän virhe: mitä jos tuo puhdistusskripti ajetaan vain kerran (ei-rekursiivisesti)?

- Päätin testata tätä huijaamalla järjestelmää "sisäkkäisellä" rakenteella. Annoin tiedostonimeksi ....//....//....//etc/passwd.
- Kun lähetin tämän palvelimelle, sen filtteri teki juuri sen mitä pitikin: se etsi ../-pätkät ja poisti ne. Mutta heti kun sisempi osa poistettiin, jäljelle jääneet ulommat merkit loksahtivat yhteen muodostaen täydellisen, uuden ../-koodin! Järjestelmä itse siis vahingossa rakensi minulle toimivan hyökkäyksen filtterin ajon jälkeen, ja pääsin jälleen käsiksi haluamaani tiedostoon.

<img width="1670" height="775" alt="view transcript" src="https://github.com/user-attachments/assets/7a439ebc-1d5d-41ae-a5ae-2d5658588fc0" />

<img width="1447" height="342" alt="manipulated the file number" src="https://github.com/user-attachments/assets/8bf75e1a-6697-4d68-8400-2363e012ccbc" />

<img width="1018" height="551" alt="password acquired" src="https://github.com/user-attachments/assets/10d4afd7-574c-423a-b7cb-6c68852a544c" />

<img width="1702" height="636" alt="and we are in!" src="https://github.com/user-attachments/assets/42743971-ff3e-48a7-914d-76bd7e65f689" />


- Neljännessä haasteessa (jossa minun piti kaivaa salasana), huomasin erikoisen yksityiskohdan. Kun latasin oman chat-lokini, sen URL-osoitteessa luki yksinkertaisesti 2.txt

- Kokeilin ihan tuurilla ja vaihdoin ZAPin kautta tiedoston numeroksi ykkösen (1.txt). Sieltä paljastuikin Hal Plinen loki, jonne hän oli mennyt möläyttämään salasanansa selkokielellä. Tämän jälkeen homma olikin pelkkää sisäänkirjautumista!


## Miten chat-lokien kaappaus toimi?

- Kehittäjä meni sieltä mistä aita on matalin ja tallensi chat-lokit suoraan palvelimen kiintolevylle yksinkertaisilla, juoksevilla numeroilla (1.txt, 2.txt, 3.txt). Kun huomasin url-osoitteessa tiedoston 2.txt, järjestelmän logiikka paljastui heti. Jos tiedostot olisi nimetty pitkillä satunnaisilla merkkijonoilla (esim. log_8f92A4bB.txt), toisen käyttäjän tiedoston nimeä olisi ollut lähes mahdotonta arvata.

- Puuttuva käyttöoikeuksien tarkistus (Insecure): Tämä on se todellinen kohtalokas virhe. Kun pyysin palvelimelta manuaalisesti tiedostoa 1.txt, palvelin tarkisti vain kaksi asiaa: olenko kirjautunut ylipäätään sisään järjestelmään, ja onko kyseinen tiedosto olemassa kiintolevyllä. Palvelin ei kuitenkaan missään vaiheessa tarkistanut, onko minulla (nykyisellä käyttäjällä) oikeutta lukea juuri tuota kyseistä tiedostoa.



## Lähteet

- <a href="https://terokarvinen.com/tunkeutumistestaus/#taysin-laillinen-sertifikaatti">Tero Karvinen</a>
- <a href="https://owasp.org/Top10/A01_2021-Broken_Access_Control/">A01:2021 – Broken Access Control</a>
- <a href="https://portswigger.net/web-security/access-control/idor">PortSwigger Academy: Insecure direct object references (IDOR)</a>
- <a href="https://portswigger.net/web-security/file-path-traversal">PortSwigger Academy: Path traversal</a>
- <a href="https://portswigger.net/web-security/cross-site-scripting">PortSwigger Academy: Cross-site scripting</a>
- <a href="https://portswigger.net/web-security/cross-site-scripting/reflected/lab-html-context-nothing-encoded">Reflected XSS into HTML context with nothing encoded</a>
- <a href="https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded">Stored XSS into HTML context with nothing encoded</a>
- <a href="https://portswigger.net/web-security/file-path-traversal/lab-simple">File path traversal, simple case</a>
- <a href="https://portswigger.net/web-security/file-path-traversal/lab-absolute-path-bypass">File path traversal, traversal sequences blocked with absolute path bypass</a>
- <a href="https://portswigger.net/web-security/file-path-traversal/lab-sequences-stripped-non-recursively">File path traversal, traversal sequences stripped non-recursively</a>
- <a href="https://portswigger.net/web-security/access-control/lab-insecure-direct-object-references">Insecure direct object references</a>

