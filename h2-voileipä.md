x) "Karvinen 2026: Sudo without password" sivulla kerrotaan yksinkertaisesti miten sudo komennosta poistetaan jatkuva salasanojen kysely. Nopeuttaa huomattavasti toimintaa, kun ei tarvitse aina kirjoitella salasanoja. 
Hyvänä vinkkinä oli avata root shell toiseen ikkunaan jos sattuukin käymään pieni lapsus. Olisin ehkä kaivannut demoa, että mitä root shellillä voi tehdä jos sudo oikeudet meneekin rikki.
"https://terokarvinen.com/passwordless-sudo-with-ansible/" sivu oli mielestäni liian suppea, kun mitään ei varsinaisesti ole selitetty.
Tunnillakin asia käytiin muutamassa hassussa minuutissa nopeasti läpi, niin mitään ei jäänyt päähän. Laitettua itse tiedostot "kuntoon" sivun mukaisesti sain vain virheilmoitusta, enkä tiedä miten lähteä asiaa purkamaan.

<img width="680" height="461" alt="image" src="https://github.com/user-attachments/assets/f925d8a2-98a1-4732-9562-fe16daa8d656" />

Pienen troubleshoottamisen jälkeen (chatgpt) sain selville että ongelma olikin komennossa, piti olla iso -K eikä pieni -k, jonka jälkeen playbook komento toimi taas.

<img width="800" height="518" alt="image" src="https://github.com/user-attachments/assets/7834fc11-49cc-42d7-baf1-2a3bce593231" />

Suoriettua komennon uudestaan huomasin sen olevan idempotentti, eli muutoksia ei enää tullut.

Ansible-docit on erittäin kattavia ohjeita ansiblen käyttöön. Kertoo komennot tai tiedot mitä voi antaa, ja lyhyet mutta ytimekkäät selitykst jokaiselle kohdalle.
Luettavaa riittäisi vaikka viikoiksi.

a) Tämä onkin tuossa ylempänä jo hieman tehtynä, mutta tässä vielä testaus toimivuudesta.

<img width="411" height="182" alt="image" src="https://github.com/user-attachments/assets/37642141-f58b-4291-828b-388ba070a784" />

b) Tämäkin tuli tuossa ylempänä jo tehtyä, tässä vielä puurakenne ja main.yml tiedosto missä vaaditut tiedot:

<img width="622" height="607" alt="Näyttökuva 2026-04-04 143659" src="https://github.com/user-attachments/assets/d2cc3871-4b0a-43c3-8219-ff7f2605bbd5" />

<img width="539" height="634" alt="image" src="https://github.com/user-attachments/assets/84ee33f1-b7c9-47e5-94d0-0a46de26acb9" />

c) Pakettien asennusta varten piti mennä leksa roolin main.yml tiedostoon ja lisätä sinne uusi kohta, joka kertoo että mitä paketteja asennetaan.
Siihen lisättiin vielä state: present joka asentaa paketin jos sitä ei ole, ja jos paketit löytyy, niitä ei asenneta.

<img width="656" height="464" alt="Näyttökuva 2026-04-04 144637" src="https://github.com/user-attachments/assets/242c7e64-940f-4490-920c-a16f4d6ed567" />

Suoritin playbook komennon taas kaksi kertaa, ensimmäisellä kerralla muutoksia tuli kun paketteja asennettiin, ja toisella kerralla changed kohta muuttui odotetusti nollaksi.

<img width="656" height="225" alt="image" src="https://github.com/user-attachments/assets/ab778e01-47bd-4c08-ac4b-660929230083" />

d) Tässä kohdassa lisäsin "leksa" roolin main.yml tiedostoon kohdan, joka luo uuden tiedoston omilla oikeuksillaan, ja kokeilin tässä yksinkertaista teksitiedostoa.

<img width="542" height="384" alt="image" src="https://github.com/user-attachments/assets/697b83c1-de07-442f-a29c-d503954eb849" />

"0600" oikeudet meinaa sitä, että 0: erikoisoikeuksia ei ole, 6: omistajalla on oikeus lukea ja kirjoittaa tiedosto, mutta ei ajaa, ja loput 00: ryhmällä ja muilla käyttäjillä ei ole mitään oikeuksia. "ls -l" komennolla saa tiedostosta oikeudet erimuodossa, eli "rwx" muodossa, vasemmalta oikealle alkaen ensin tulee erikousoikeudet, sitten omistajan, sen jälkeen ryhmän ja lopuksi muut käyttäjät. Yksinkertaisuudessaan "r" meinaa read oikeutta, "w" write oikeutta sekä "x" ajo oikeutta.

<img width="514" height="118" alt="image" src="https://github.com/user-attachments/assets/91677a27-8268-4e28-aca5-9738ab6685ab" />

e) Viimeiseen kohtaan kysyin chatgpt, mikä olisi hyvä uusi asia oppia ansiblesta, ja sain vinkiksi asentaa nginx web-palvelin ohjelman, ja kirjoittaa taskeihin kohdan joka tarkistaa ja pitää huolen että nginx on päällä. Lopputulos näyttää yksinkertaisesti tältä. Tämä eroaa aikaisemmasta kohdasta siten, että tämä hallitsee palveluiden tilaa, kun taas apt moduuli asentaa paketteja.

<img width="598" height="362" alt="image" src="https://github.com/user-attachments/assets/a3137c94-ae75-4890-b1d3-60edadbd4478" />

<img width="746" height="459" alt="image" src="https://github.com/user-attachments/assets/f570435a-6093-4ad9-b81e-476caad601f8" />



