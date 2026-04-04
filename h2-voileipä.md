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
