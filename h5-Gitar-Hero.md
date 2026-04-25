# Gitar Hero

## x) Lue ja tiivistä

Annetut lähteet kertovat kattavasti Gitin tarkoituksen, ja antavat ohjeita sen kanssa toimimiseen.
Pää linkistä "Pro Git, 2ed" löytyy kaikki tarvittava kätevästi järjesteltynä, mitä gitin käyttöön saattaa tarvita.
Git on siis versionhallinta järjestelmä joka erottuu joukosta niin, että se tallentaa tiedostot snapshotteina "erikseen".
Git tallentaa niinsanotun screenshotin tiedostoista sillä hetkellä kun sinä niin päätät, ja tallentaa sen yhtenä versiona. Git ei myöskään tallenna tiedostoa uusiksi, jos siihen ei ole tullut muutoksia.
Git tallentaa lähes kaiken lokaalisti omalle laitteellesi, joten pystyt käymään läpi, muokkaamaan tai jopa aloittamaan uusia projekteja täysin lennossa ilman verkkoyhteyttä. Verkkoyhteyden tarvii tosin sitten, kun haluat "työntää" tehdyt muutokset muiden nähtäväksi.
Herää kysymys, että miten kriittisillä vesillä liikutaan jos laite sattuu hajoamaan ennenkuin saat työnnettyä paikalliset tiedostot vaikkapa githubin palvelimelle?

## a) Online

Tehtävänanto on yksinkertainen ja selkeä. Tämä on suora toisto siitä mitä tunnilla jo kerettiinkin tekemään. Dokumentaation vuoksi, teen saman uudestaan annetun tunnisteen mukaisesti!
Repoa tehdessä täytyy muistaa lisätä joukkoon readme tiedosto, kukaan ei tiedä miksi mutta näin se vaan on.
Lisenssiksi valitsin GNU General Public License v3.

<img width="843" height="786" alt="image" src="https://github.com/user-attachments/assets/6fd4eca6-d233-48a1-a309-e4d91b4ea701" />

<img width="972" height="533" alt="image" src="https://github.com/user-attachments/assets/59dd2b5a-7983-4067-a7ce-df4d9582559f" />


## b) Dolly

Edellisessä tehtävässä tehty repositorio täytyy nyt yhdistää Gittiini, jotta voin oman linuxini komentoriviltä muokata repoa suoraan SSH yhteyden avulla.
Ensin haetaan oman laitteen SSH avaimen julkinen osuus ssh kansiosta. Tämä tapahtuu kotihakemistosta komennolla "cd .ssh", jonka jälkeen näet julkisen ja yksityisen avaimen tiedostot.
"cat id_rsa.pub" komennolla saat tulostettua oman julkisen avaimen puolikkaasi, jonka jälkeen se on kopioitava.

<img width="1308" height="214" alt="image" src="https://github.com/user-attachments/assets/aff5eb23-b7f3-40ec-a7a8-6351743d6d2d" />


Tämän jälkeen siirrytään githubissa oman käyttäjän asetuksiin.

<img width="432" height="910" alt="image" src="https://github.com/user-attachments/assets/f58eb42d-1ce5-4096-840e-b3410964e5eb" />


Ja vielä eteenpäin SSH avain kohtaan.

<img width="462" height="817" alt="image" src="https://github.com/user-attachments/assets/ba07461a-46ab-4076-91e5-9ff15ebaba9a" />


Täällä painetaan "Add new SSH key" painiketta, ja päästään kohtaan missä voit nimetä yhteyden ja lisätä äsken kaivamamme julkisen avaimen puolikkaan tilillesi.

<img width="1276" height="659" alt="image" src="https://github.com/user-attachments/assets/49e5c1c2-d1fa-42ce-a87b-dde9c2583924" />


Kun tämä on tehty, voidaan palata terminaaliin ja ottaa yhteys suoraan äsken luomaamme "Sunshine" repositorioon. Tämä tapahtuu "kloonaamalla".
Mennään githubin puolelle, ja avataan aikaisemmin luotu uusi repomme. Repossa painamme vihreää "Code" nappia, ja edelleen SSH "välilehteä". Sieltä saamme avaimen jolla yhdistää terminaalista githubiin.
Avain kopioidaan, ja mennään terminaaliin. Terminaalissa otetaan suora yhteys äsken luomaamme repoon komennolla "git clone <äsken kopioitu avain>", ja tämä luo meille suoraa tiedoston, joka tässä tapauksessa on nimeltään "Sunshine".

<img width="1168" height="632" alt="image" src="https://github.com/user-attachments/assets/31e31ad2-e49a-4aa4-a8f0-8e8ac2cfb4c3" />


Siirryin Sunshine kansioon "cd Sunshine" komennolla, ja loin sinne testausta varten tekstitiedoston komennolla "nano sunshine.txt", ja kirjoitin sinne jotain tunnistettavaa tekstiä.
Gittiin tehdyt muutokset tallenetaan ja viedään seuraavilla komennoilla:
1. git add -all = lisää kaikki tekemäsi muutokset "valmiustilaan"
2. git commit = vie tiedostot paikalliseen tallenukseen, näyttää myös muutoksien määrän tässä vaiheessa. Tässä vaiheessa annetaan myös kommentti tekemistäsi muutoksista
3. git pull = tuo uusimman version repositoriosta, ja näyttää onko siellä tällä hetkellä tullut muutoksia. Tämä kannattaa tehdä myös ennen git add -all komentoa, jos on useampia muokkaajia repossa.
4. git push = vie tiedostot githubiin
5. git log --patch = antaa tarkemmat tiedot viimeisimmistä muutoksista

<img width="1896" height="696" alt="Näyttökuva 2026-04-25 113440" src="https://github.com/user-attachments/assets/4a59872a-c6a0-4c6b-a77d-4732974456ac" />

<img width="1282" height="690" alt="image" src="https://github.com/user-attachments/assets/28826eeb-4386-44f7-8c98-1005b247c1cf" />


## c) Doh!




## Lähteet
https://git-scm.com/book/en/v2
https://git-scm.com/book/en/v2/Getting-Started-What-is-Git%3F
