# H4 Täysin Laillinen Sertifikaatti | Nathaniel Ssendagire 07.04.2026
## Ympäristö
### OS: Kali Linux

### Browser: Firefox 128.3.1esr (64-bit)

### Hardware Model: VMWare Workstation Pro Memory: 4.0 GB

### Processor: AMD Ryzen 3 7320U - 4 cores used

### Disk: 35 GB

### Network: NAT / Host-Only

## Tiivistelmä

## a) Totally Legit Sertificate. Asenna OWASP ZAP, generoi CA-sertifikaatti ja asenna se selaimeesi.

<img width="751" height="573" alt="proxy config" src="https://github.com/user-attachments/assets/62f9abc3-3ab1-46ad-b5e1-76ec809014f2" />

<img width="751" height="577" alt="save the cert" src="https://github.com/user-attachments/assets/ca20b064-34dd-4088-82b1-d7812e23c30e" />

<img width="628" height="356" alt="save the cert into a folder" src="https://github.com/user-attachments/assets/d091dc6d-909e-4fcc-aea0-e246203f3e4e" />

<img width="761" height="477" alt="view cert" src="https://github.com/user-attachments/assets/fef3c9d5-56d3-4575-8d35-3351d5514585" />

<img width="672" height="468" alt="import cert" src="https://github.com/user-attachments/assets/785ff4f5-1d90-43d6-bbd2-65ed76ef43f6" />

<img width="752" height="352" alt="selecting the cert" src="https://github.com/user-attachments/assets/17bf5ad5-9327-4fdd-b621-a7dc32eb84f5" />

<img width="751" height="287" alt="trust is selected" src="https://github.com/user-attachments/assets/c52ac556-6fcd-4e6c-8249-ab77d3f395fa" />

Avattuani ZAP-sovelluksen, varmistin, että proxy kuunteli localhostia portissa 8080.
Sen jälkeen tallensin ZAPin juurisertifikaatin (CA Certificate) koneelle ja toin (import) sen Firefoxin varmenteisiin.

<img width="1305" height="473" alt="select manual explore" src="https://github.com/user-attachments/assets/5b437eed-99d5-4d0f-a485-dc7bc02402f5" />

<img width="1308" height="481" alt="select launch browser" src="https://github.com/user-attachments/assets/f603b479-fc74-4293-8357-eacf3a60c68d" />

<img width="1275" height="398" alt="example domain" src="https://github.com/user-attachments/assets/588d9bdc-efca-4228-baf6-01bbbbb743dd" />

<img width="1722" height="188" alt="proxy result" src="https://github.com/user-attachments/assets/938b4f1c-23b8-4f08-a191-3d23531efd84" />

Seuraavaksi testattiin, ilmestyykö liikennettä ZAPin History-välilehdelle.

Kuten huomaatte, terminalissa näkyy juuri haettua sivua eli http://example.com/

## b) Kettumaista. Asenna "FoxyProxy Standard" Firefox Addon, ja lisää ZAP proxyksi siihen

FoxyProxyn asennus oli tuskallista 😂, meni hetki tajuta mitä tekee mikäkin, mutta päästiin kuitenkin maaliin.

<img width="1267" height="448" alt="foxyproxy" src="https://github.com/user-attachments/assets/4240f015-f3b3-41aa-816e-14978783c86b" />

Ensiksi lisäsin FoxyProxy-lisäosan Firefoxiin.


<img width="295" height="328" alt="foxyproxy select options" src="https://github.com/user-attachments/assets/a0e8edcc-0717-481c-aa38-581b97ae4892" />

Seuraavaksi avasin FoxyProxyn asetukset.

<img width="748" height="536" alt="foxyproxy config" src="https://github.com/user-attachments/assets/54bb0472-e66e-4641-b4f6-eb72fc038fe6" />

Määritin uuden välityspalvelimen antamalla sille samat asetukset, jotka olivat ZAPissa (localhost ja portti 8080). Näin kaikki liikenne, jonka FoxyProxy nappaa, menee suoraan ZAPiin.

<img width="1160" height="227" alt="foxyproxyyyyy" src="https://github.com/user-attachments/assets/537eb1cf-e2da-44a7-8334-f83c2ca86eec" />

Seuraavaksi piti määritellä "Proxy by Patterns". Tämä ominaisuus suodattaa kaiken muun liikenteen pois paitsi ne osoitteet, jotka on erikseen määritelty sallituksi. Testien vuoksi olin laittanut sääntöihin muutamia eri osoitteita.

<img width="311" height="327" alt="select that one" src="https://github.com/user-attachments/assets/21ec8367-71f2-4fb5-9973-5ef9c3be5f90" />

Valitsin laajennuksesta päälle tilan "Proxy by Patterns".

<img width="760" height="907" alt="portswig search" src="https://github.com/user-attachments/assets/6cf6dd4d-dac2-48bb-ab03-d67af882754e" />

Sitten testasin, toimiiko määritelty filtteri. Etsin selaimella PortSwiggerin sivuston sekä google.com:in.

<img width="756" height="302" alt="no google" src="https://github.com/user-attachments/assets/a951e505-baa5-482b-9971-e97a70a04ae5" />

ZAPin historiassa ei näkynyt Googlea, joten voidaan todeta, että filtteri toimii kuten pitääkin 👍.

<img width="416" height="378" alt="SITES" src="https://github.com/user-attachments/assets/37554fde-ef7d-4908-9dac-29ed535a4721" />

Nyt tulee päivän vaikein osuus (tai no, minulla ainakin kesti noin tunti tämän ratkaisemisessa 😅). Tehtävänannossa luki, että piti näyttää ZAPin löytävän kuvatiedostoja. Kun katsoin History-välilehteä, siellä ei ollut yhtään mitään. Luulin, että FoxyProxyssa oli jokin asetusvirhe ja testasin koko prosessin uudestaan, mutta mikään ei auttanut...

Kunnes katsoin ZAPin vasenta sivupalkkia, jossa lukee "Sites". Klikkasin sitä ja kappas, siellä näkyy kansiopuu sivuista, joilla olin käynyt! Koska kuvat ovat ZAPin mielestä usein vain "kohinaa", se piilottaa ne oletuksena historiasta, mutta tallentaa ne silti sivupuuhun.

<img width="982" height="520" alt="portswig image" src="https://github.com/user-attachments/assets/02f90d4c-0ed8-4544-8fb3-31de4ace0c47" />

Etsin sieltä PortSwiggerin kansion ja vuhuu, mä sain kuvan löydettyä!

## Cross Site Scripting (XSS)

### C) Reflected XSS into HTML context with nothing encoded

<img width="1725" height="566" alt="cross-site scripting" src="https://github.com/user-attachments/assets/fdf3f3f5-7e36-4895-93dd-65776e5ad644" />

### D) Stored XSS into HTML context with nothing encoded

<img width="767" height="657" alt="cross-site scripting2" src="https://github.com/user-attachments/assets/fa852d17-80e9-41e2-9eed-5b167e630c18" />

<img width="1527" height="511" alt="cross-site scripting2 passed" src="https://github.com/user-attachments/assets/5bc4b112-1fbf-43e3-9c1b-abfbf8eb407a" />

## Path traversal

### F) File path traversal, simple case. Laita tarvittaessa Zapissa kuvien sieppaus päälle. 

<img width="1708" height="911" alt="manipulate" src="https://github.com/user-attachments/assets/02627526-7d25-4d98-ae20-317449284acb" />


Tässä tehtävässä hyödynsin aiemmin oppimaani tekniikkaa. Etsin ZAPin "Sites"-puusta Labin kansion. Sen sisältä löytyi kuvan hakeva GET-pyyntö, joka muodostui, kun avasin PortSwiggerin tuotesivun. Seuraavaksi tehtävänä oli manipuloida tätä pyyntöä (Manual Request Editorissa) siten, että saadaan tärkeitä palvelimen tiedostoja näkyviin

Muokkasin pyynnön filename-parametria asettamalla sen arvoksi haavoittuvuutta hyödyntävän polun ../../../etc/passwd

<img width="1553" height="270" alt="get image customization" src="https://github.com/user-attachments/assets/07fa5431-ff23-4788-9317-24a7659aca3f" />


<img width="768" height="177" alt="select text" src="https://github.com/user-attachments/assets/e6d39eef-8adb-4e41-8ad4-7a65fe7e8ded" />

Koska alkuperäinen pyyntö haki kuvaa, ZAP yritti näyttää hyökkäyksen vastauksen oletuksena kuvana (mikä näyttää tyhjältä/rikkinäiseltä). Jotta näemme onnistuneen hyökkäyksen palauttaman tiedoston sisällön raakatekstinä, "Body"-näkymä piti vaihtaa asennosta "Image" asentoon "Text".

<img width="877" height="422" alt="it displays all the info needed" src="https://github.com/user-attachments/assets/a6affb70-1972-48a7-872a-91ea300e96d4" />

Tämän jälkeen palvelimen /etc/passwd -tiedoston sisältö pärähti ruudulle!

<img width="1001" height="452" alt="lab solveeeed" src="https://github.com/user-attachments/assets/28a823c3-6126-4d3a-b22c-5d1db7d0876d" />


<img width="1418" height="246" alt="same thing just a bit different" src="https://github.com/user-attachments/assets/936ae46e-dc45-4666-90bb-5b4bc08622e8" />

<img width="565" height="533" alt="response of the same thing" src="https://github.com/user-attachments/assets/691b1904-f991-4d59-a60a-07c0569105bb" />
<img width="1712" height="783" alt="file path traversal2" src="https://github.com/user-attachments/assets/3f93ec97-ac1b-415f-9ff6-3407c12a927f" />



<img width="1425" height="135" alt="same thing just a bit different 2" src="https://github.com/user-attachments/assets/ecdfab59-b057-4558-a47c-622cd43f4e32" />
<img width="515" height="560" alt="same thing response 2" src="https://github.com/user-attachments/assets/becee4ec-d279-4474-9124-40c9407ffb67" />
<img width="1715" height="791" alt="theres no i in team" src="https://github.com/user-attachments/assets/c897b0e5-583e-4810-b803-60f6a8d5f890" />


<img width="1670" height="775" alt="view transcript" src="https://github.com/user-attachments/assets/7a439ebc-1d5d-41ae-a5ae-2d5658588fc0" />

<img width="1447" height="342" alt="manipulated the file number" src="https://github.com/user-attachments/assets/8bf75e1a-6697-4d68-8400-2363e012ccbc" />

<img width="1018" height="551" alt="password acquired" src="https://github.com/user-attachments/assets/10d4afd7-574c-423a-b7cb-6c68852a544c" />

<img width="1702" height="636" alt="and we are in!" src="https://github.com/user-attachments/assets/42743971-ff3e-48a7-914d-76bd7e65f689" />


