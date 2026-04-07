# H3 Täysin Laillinen Sertifikaatti | Nathaniel Ssendagire 07.04.2026
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

Avattuani ZAP sovelluksen, varmistin että proxy oli localhost ja portissa 8080.

Sen jälkeen tallensin sertifikaatin koneelle ja importtasin sen firefoxiin.

<img width="751" height="577" alt="save the cert" src="https://github.com/user-attachments/assets/ca20b064-34dd-4088-82b1-d7812e23c30e" />

<img width="628" height="356" alt="save the cert into a folder" src="https://github.com/user-attachments/assets/d091dc6d-909e-4fcc-aea0-e246203f3e4e" />

<img width="761" height="477" alt="view cert" src="https://github.com/user-attachments/assets/fef3c9d5-56d3-4575-8d35-3351d5514585" />

<img width="672" height="468" alt="import cert" src="https://github.com/user-attachments/assets/785ff4f5-1d90-43d6-bbd2-65ed76ef43f6" />

<img width="752" height="352" alt="selecting the cert" src="https://github.com/user-attachments/assets/17bf5ad5-9327-4fdd-b621-a7dc32eb84f5" />

<img width="751" height="287" alt="trust is selected" src="https://github.com/user-attachments/assets/c52ac556-6fcd-4e6c-8249-ab77d3f395fa" />

Seuraavaksi testattiin että ilmestyykö mitään tuonne history terminaliin.

<img width="1305" height="473" alt="select manual explore" src="https://github.com/user-attachments/assets/5b437eed-99d5-4d0f-a485-dc7bc02402f5" />

<img width="1308" height="481" alt="select launch browser" src="https://github.com/user-attachments/assets/f603b479-fc74-4293-8357-eacf3a60c68d" />

<img width="1275" height="398" alt="example domain" src="https://github.com/user-attachments/assets/588d9bdc-efca-4228-baf6-01bbbbb743dd" />

<img width="1722" height="188" alt="proxy result" src="https://github.com/user-attachments/assets/938b4f1c-23b8-4f08-a191-3d23531efd84" />

Kuten huomaatte, terminalissa näkyy juuri haettua sivua eli http://example.com/

## b) Kettumaista. Asenna "FoxyProxy Standard" Firefox Addon, ja lisää ZAP proxyksi siihen

FoxyProxyn asennus oli tuskallista 😂, meni hetki tajuta mitä tekee mikäkin, mutta päästiin kuitenkin maaliin.

Ensiksi piti lisää se FoxyProxy firefoxiin 
<img width="1267" height="448" alt="foxyproxy" src="https://github.com/user-attachments/assets/4240f015-f3b3-41aa-816e-14978783c86b" />

Seuraavaksi avasin FoxyProxyn asetukset
<img width="295" height="328" alt="foxyproxy select options" src="https://github.com/user-attachments/assets/a0e8edcc-0717-481c-aa38-581b97ae4892" />

Sitten muokkasin sitä proxyä antamalla sille samat asetukset mitkä oli zapissa eli localhost ja portti 8080 että kaikki mitä foxyproxy hakee menee zappiin.

<img width="748" height="536" alt="foxyproxy config" src="https://github.com/user-attachments/assets/54bb0472-e66e-4641-b4f6-eb72fc038fe6" />

Seuraavaksi piti määritellä Proxy by Pattern tämä siis filteröi kaiken muun liikenteen pois paitsi ne jotka on määritelty tänne. Testien vuoksi olin laittanut aika monta eri osoitetta.

<img width="1160" height="227" alt="foxyproxyyyyy" src="https://github.com/user-attachments/assets/537eb1cf-e2da-44a7-8334-f83c2ca86eec" />

Valitsin tuon "Proxy by Patterns"

<img width="311" height="327" alt="select that one" src="https://github.com/user-attachments/assets/21ec8367-71f2-4fb5-9973-5ef9c3be5f90" />

Sitten testasin toimiiko meidän määritelty filtteri. Etsin hakukoneesta portswiggerin sekä google.com:in

<img width="760" height="907" alt="portswig search" src="https://github.com/user-attachments/assets/6cf6dd4d-dac2-48bb-ab03-d67af882754e" />

Terminalissa ei näkynyt google.com:ia joten voidaan todeta että filtteri toimii kuten pitääkin 👍.

<img width="756" height="302" alt="no google" src="https://github.com/user-attachments/assets/a951e505-baa5-482b-9971-e97a70a04ae5" />








