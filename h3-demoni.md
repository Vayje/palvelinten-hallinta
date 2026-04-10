# H3 Demonit

## x) Lue ja tiivistä

Teron sivulla on selkeät ja nopeat ohjeet apachen asennukseen, sekä sen tiedostojen konfigurointiin.
Tero on myös antanut kuvat konfiguraatioista, joilta lopputulosten tulisi näyttää. Kaipaisin hieman enemmän avausta miksi mikäkin konfiguraatio on sitä mitä ne on.
Tunnilla ne oli hieman nopeasti käyty, eikä kaikkea kerennyt sisäistämään.

Ansible-docin ohjeet ovatkin sitten huomattavasti kattavammat (tietysti), ja joka ongelmaan varmasti löytyy vastaus.
Handler käsitteenä avattiin myös kivasti heti alkuun. Tunnilla meni ehkä hieman ohi mitä handler meinaa. Handler siis on "palvelu", joka toimii ansiblessa vaan jos muutoksia tapahtuu.
Esimerkiksi handlerit restarttaa vaikkapa nyt kyseessä olevan apachen, jos sen konfiguraatioihin tulisi muutoksia.
Notify parametrillä käsketään handleria tekemään tietty asia, vaikkapa restart. Handlerit myös välttävät useaa restarttausta vaikka muutoksia tulisi useampia monesta eri lähteestä.

Ansible-doc service manuaali avaa hieman servicen tarkoitusta. Serviceillä hallitaan tietokoneita etänä heterogeenisessä ympäristössä.
Name parametriin annetaan nimensä mukaisesti nimi servicelle, joka kuvaa "käskyn" sisältöä ja version numeroa.
State parametriin annetaan tieto, mitä halutaan tapahtuvan. Install = asentaa paketin ja present = pitää huolen että paketti on päällä ja käytössä.


## a) Apassi

Apache2 asennettaan komennolla "sudo apt-get apache2". Asennuksen jälkeen "sudo systemctl status apache2" komennolla voidaan tarkistaa,
onko ohjelma päällä, ja oman kotisivun tulisi näkyä heti http://localhost osoitteessa.

<img width="749" height="505" alt="image" src="https://github.com/user-attachments/assets/2842fdad-b845-4a82-9f76-ca8bc2c4a73f" />

<img width="609" height="901" alt="image" src="https://github.com/user-attachments/assets/784b216f-cdc6-4536-b8ab-fe20439126eb" />

Luotuani uuden roolin, sekä konfiguroituani tiedostot Teron ohjeiden mukaisesti pääsin idempotenttiin tilaan, jolloin playbook komento toimii muuttumattomana.

<img width="769" height="622" alt="image" src="https://github.com/user-attachments/assets/42c5430f-7a87-4016-8625-1facc1030352" />

Loin vielä erikseen kotihakemistoon publicsite kansion, ja playbook komento ei muuttunut senkään jälkeen. Ei virheilmoitusta = toimii?.
Varmistin vielä restarttaamalla apachen manuaalisesti komennolla "sudo systemctl restart apache2" ja suoritin playbook komennon uudestaan, muutoksia ei tapahtunut.

## b) Moottorix

Aloitin tehtävän sammuttamalla apachen komennolla "sudo systemctl stop apache2" ja varmistin vielä erikseen status komennolla että käsky varmasti meni perille.
Apache2 suljettiin onnistuneesti, ja kotisivuun ei enää saada yhteyttä.

<img width="771" height="719" alt="image" src="https://github.com/user-attachments/assets/0ff71aff-7158-4822-99d3-c59b197d1467" />

Asennettuani nginxin ja varmistettuani että se oli käynnissä, kokeilin ottaa yhteyttä kotisivuuni. Yllätyin kun näytölle pompppasi taas apachen kotisivu.
Varmistin että apache oli varmasti pois päältä komennolla "sudo systemctl status apache2", ja huomasin että apache oli inactive (dead).
Hämmästeltyäni hetken kysyin tekoälyltä että mikä mättää promptilla "olen sammuttanut ja varmistanut että apache2 ei ole päällä ja olen asentanut ngixn, mutta kotisivuni on edelleen apache2, miksi?".
Tekoäly neuvoi, että molemmat sovellukset käyttävät samaa /var/www/html hakemistoa, ja että minun tulisi poistaa index.html ja luoda uusi.
Näin tein, ja lisäsin indexiin yksinkertaisen otsikon jotta saan selville toimiiko se. Toimii!

<img width="458" height="334" alt="image" src="https://github.com/user-attachments/assets/842d6873-0887-4391-901a-318cb87f3cee" />

Äskeisellä promptilla, mitään kysymättä tekoäly antoi minulle myös käskyn muokata oikeuksia html tiedostoon, ja kuinkas sattuikaan, niin oli tarkoitus tehdä jokatapauksessa.
Näillä kolmella komennolla annoin oikeudet muokata sivua ilman sudo tai root oikeuksia, ajo-oikeudet että sivun saa auki sekä tarkistin että oikeudet varmasti meni perille asti.

<img width="568" height="170" alt="image" src="https://github.com/user-attachments/assets/47df962f-d84d-4473-b5d2-b5d02ffad54a" />

## c) Automoottorix

Tein aikaisemmin jo saman, kuin tunnilla eli tein osasta rooleista kommentteja jotta playbookin suorittaminen nopeutuisi.
Aloitin automatisoinnin luomalla uuden roolin nginxille.

<img width="507" height="273" alt="image" src="https://github.com/user-attachments/assets/928e0ad8-105c-4a3a-befc-ca3033cb94ab" />

Tehtävä oli sinänsä helppo. Riitti kun teki uuden rooli nginxille, ja vastaavat asetukset kun apachella oli.
Täytyi vaan tarkistaa syntaxi, joka on hieman erilainen kuin apachella. Tähän käytin avuksi tekoälyä, jolta kysyin että onko apachen ja nginxin konfiguraatiota eriävät.
Lopullinen puu näytti tältä.

<img width="414" height="281" alt="image" src="https://github.com/user-attachments/assets/b8752a42-970e-4dc2-91de-ae7ed95803ec" />

Lopulliset konfiguraatiot näytti tältä.

<img width="623" height="632" alt="image" src="https://github.com/user-attachments/assets/11c0fc8b-95a1-4b98-8ce9-67caf8f0e85a" />

<img width="278" height="198" alt="image" src="https://github.com/user-attachments/assets/85f74c4c-b371-442c-a918-8785cf3414bc" />


## Lähteet
1. https://terokarvinen.com/apache-ansible/ (Luettu 10.4.2026)
2. https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_handlers.html (Luettu 10.4.2026)
3. Chatgpt
