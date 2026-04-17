# H4 Pizza Fantasia

## x) Lue ja tiivistä

Artikkelin kohta 4.12 (sivu 109) alkaa tutulla termillä "idempotentti", ja korostaa sen tärkeyttä että systeemi tai järjestelmä on muuuttumaton.
Kohdassa 4.12.1 vertaillaan erilaisia hallintajärjestelmiä lyhyesti mutta teknisellä tasolla. Hieman hirvitti Salt DSL dokumentoinnin määrä.
Kohdassa 4.12.2 sukellettiin todella tekniseksi, ja itsellä meni jopa hieman ohi mitä haettiin. Tekstiä oli vaikea "tavallisen pulliaisen" sisäistää. 
Jos oikein ymmärsin, siinä vertailtiin eri hallintajärjestelmien toimintatapoja, ja jopa pohdittiin olisiko sellainen vaikea koodata itse.
Kohdassa 4.12.3.1 syvennettiin Conftero hallintajärjestelmään, ja vaikutti jopa tarpeeksi yksinkertaiselta. Tärkein ominaisuus on, että tällä järjestelmällä on tärkeää olla idempotentti.
Conftero toimii if-rakenteella, eli tekee muutoksia vain jos järjestelmä ei ole idempotentti. Se tarkistaa if-rakenteella, onko jokin paketti tms ennallaan, if not, muutos tapahtuu.

## a) Räpylä

Tehtävässä tuli asentaa demoni, jota ei ole vielä opetuksen yhteydessä käyty. Valitsin itselleni tähän tehtävään "Redis" demonin, joka on pieni tietokanta. 
Redis tallentaa dataa RAM muistiin, ja data jota tallenetaan on yleensä käyttäjäsessioita yms. Esimerkkinä tästä olisi vaikkapa verkkosivujen kirjautumistiedot, eli hyvin päivittäinen asia.
Koska Redis tallentaa datan RAM muistiin, on se huomattavasti nopeampi kuin levylle tallennettu tietokanta. 
Redis kuuntelee kahta tietokannoille tärkeää komentoa, eli "set" ja "get", jolla annetaan parametreille arvoja.

Demoni oli yksinkertainen asentaa komennolla "sudo apt install redis-server", ja tämän jälkeen testattiin systemctl komennolla, onko palvelu päällä.

<img width="622" height="426" alt="image" src="https://github.com/user-attachments/assets/ecebb8ee-067e-4d00-86a6-13c0ae8134bb" />

Redisin toimintaa voi myös testata pingillä, ja jos kaikki toimii niin saat vastaukseksi PONG.

<img width="262" height="80" alt="image" src="https://github.com/user-attachments/assets/3e83cea1-ffca-48ea-87c1-70ba8e743604" />

## b) Automaatti

Seuraavassa tehtävässä Redisin asennus tulisi automatisoida Ansiblella. 
Lähden tähän samalta pohjalta kuin aikaisempiinkin automatisointeihin
Aluksi luon uuden roolin, ja laitan muut roolit suorituksessa kommenteiksi joten testaus nopeutuu.

<img width="237" height="266" alt="image" src="https://github.com/user-attachments/assets/b90523f3-509a-44b0-bdba-7fc91b722805" />

<img width="315" height="300" alt="image" src="https://github.com/user-attachments/assets/d9db6abd-ab0f-4c4a-9858-0f15a23d47e4" />

Tähän väliin kysyin tekoälyltä (chatgpt) oikeaa syntaksia main.yml filuun, ja lopputulos näytti tältä joka hieman epäilytti minua.

<img width="467" height="515" alt="image" src="https://github.com/user-attachments/assets/6e2f8a45-ce22-4021-bf18-79c3c8bd1e09" />

Ja oikeassa olin, päästiin ensimmäisen virheilmoituksen kimppuun.

<img width="507" height="228" alt="image" src="https://github.com/user-attachments/assets/64d47a85-52ea-41ba-b30b-6c80dc453c51" />

Koska tein tätä hieman laput silmillä kipeänä, enhän minä huomannut että tekoäly tunki playbook kohdan myös tuohon roolin sisäisen taskin alkuun.
Korjasin main.yml rakenteen tutumman näköiseksi, ja poistin update_cache kohdan, sillä se oli uutta minulle enkä ole sitä aikaisemminkaan tarvinnut. Pitäisi ehkä tutustua.

<img width="633" height="605" alt="image" src="https://github.com/user-attachments/assets/7a5a27b2-1f1f-42f4-9e14-03a83e09d0a9" />

Tämän jälkeen suoritin playbookin kahteen kertaan, todeten että playbook toimii ja on idempotentti.

<img width="631" height="501" alt="image" src="https://github.com/user-attachments/assets/d34f8ce5-719a-43fd-a681-512feadf041c" />

## c) Asetus

Tässä kohdassa päätin muuttaa Redisille tärkeää asetusta, eli salasanaa tietokannalle. Onhan se tärkeää suojata kriittinen tietokanta salasanalla (sitä tämä ei ole onneksi.)
Ansiblella voidaan viitata suoraan sisäistettyyn config tiedostoon "lineinfile" kohdalla, joka varmistaa että tiedot eivät mene muiden asetusten kanssa sekaisin.
Regexp kohta puolestaan etsii halutun kohdan filestä.
Requirepass onkin yksinkertainen asia, eli halutaan määrätä salasana.
(Teoria luettu virallisista dokumentaatioista, oikea syntaxi kysytty tekoälyltä).

<img width="622" height="639" alt="image" src="https://github.com/user-attachments/assets/ba3c3f3c-8a56-4eec-b98a-8442b5e06b69" />

Tämän jälkeen ajoin playbookin ensimmäisen kerran, ja muutokset tulivat selkeästi voimaan.

<img width="622" height="603" alt="image" src="https://github.com/user-attachments/assets/b7389234-8b20-488a-a219-387c097b24b8" />

Ajoin playbookin vielä kerran, jonka jälkeen muutoksia ei tullut = idempotentti.
Tämän jälkeen restarttasin systemctl komennolla redisin, sillä halusin testata että toimiihan salasana asetus varmasti.
Restartin jälkeen varmistin redisin olevan päällä, jonka jälkeen siirryin redisin omaan komentoriviin "redis-cli" komennolla. 
Siellä voin tetata salasana vaatimusta pelkällä PING komennolla, enkä varsinaisesti yllättynyt kun vastauksena ei tullutkaan PONG.

<img width="330" height="113" alt="image" src="https://github.com/user-attachments/assets/55fa27f6-2934-42ad-9dbd-194fd7f7cf17" />

Tämän jälkeen syötin äärimmäisen salaisen salasanani, ja kokeilin pingausta uudestaan (älkää varastako tietokantaa).

<img width="366" height="211" alt="image" src="https://github.com/user-attachments/assets/f9a83604-a009-4a48-96b7-67bcb360d4a1" />

Tähän restartiin olisi voinut luoda myös handlerin edelliseen tehtävään viitaten, mutta tällä kertaa (kipeänä) laiskotti niin että jäi tekemättä.

## d) Paikka remonttiin

Lähdin tähän tehtävänantoon ensimmäisen vihjeen perusteella, eli päätin poistaa käsin demonin ja asetustiedostos.
Tämä kohta tehtävästä meneee aika yksiselitteisesti, ja pelkillä kuvilla joten nauttikaa kyydistä.
Poisto tapahtui yksinkertaisella komennolla "sudo apt-get purge redis-server -y"

<img width="603" height="447" alt="image" src="https://github.com/user-attachments/assets/14e504b7-0d76-4b16-aedf-d6a4d2e00083" />

Testasin vielä ottaa yhteyttä redisin komentoriviin.

<img width="551" height="108" alt="image" src="https://github.com/user-attachments/assets/49066598-5e5b-49cf-8c03-391ec90ca0f7" />

Ajoin playbook komennon, ja odotetusti muutoksia tuli kaksin kappalein.

<img width="650" height="594" alt="image" src="https://github.com/user-attachments/assets/0022e1f4-49de-4c18-9caf-11b76f6bbb70" />

Toisen ajon jälkeen muutoksia oli jälleen nolla, eli idempotentissa tilassa ollaan.
Tähän väliin handler olisi ollut kätevä luoda, ettei olisi tarvinnut manuaalisesti restarta, ja mietinkin että onko salasana vaatimus turha, jos se ei tule edes käyttöön ilman manuaalista restarttia.
Playbookin ajettuani kokeilin pingata redisin komentorivillä, ja se meni mutkitta läpi mikä ei ole haluttu lopputulos.
Systemctl restartin jälkeen komentorivillä pingaus vaati taas erittäin salaisen salasanani.

<img width="500" height="416" alt="image" src="https://github.com/user-attachments/assets/dcd2407c-eb12-45c3-b368-3f0523cdb343" />

## Lähteet

https://www.ibm.com/think/topics/redis (luettu 17.4.2026)
https://westminsterresearch.westminster.ac.uk/download/4cc417566aa9af60fe3826d690719e390abdb7a3c8672f3d51b1eb4ca75e7624/1427236/karvinen-2023-configuration-management-of-distributed-systems.pdf (Kohdat 4.12.1, 4.12.2 ja 4.12.3 luettu 17.4.2026)
https://redis.io/docs/latest/operate/oss_and_stack/management/config/ (Luettu 17.4.2026)
https://docs.ansible.com/projects/ansible/latest/collections/ansible/builtin/lineinfile_module.html (Luettu 17.4.2026)
