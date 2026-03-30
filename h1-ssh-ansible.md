x) -Yksinkertaiset ja selkeät/suoraviivaiset ohjeet SSH palvelimen asennukseen sekä julkisen avainparin luomiseen

a) -SSH palvelimen asennus onnistui yksinkertaisesti "sudo apt-get -y install ssh" komennolla
   -Yhteyden testaus onnistui "ssh localhost" komennolla

b) -"ssh-keygen" komennolla luotiin julkinen avainpari palvelimelle
   -"ssh-copy id localhost" komennolla kopioitiin avainpari palvelimelle niin, että yhteys ei vaadi enää salasanaa

c) -Hieman pidempi vaiheinen tehtävä mutta yksinkertaisilla ja suoraviivaisilla ohjeilla sai helposti toimimaan
   -"sudo apt-get install ansible" komento asensi ansiblen
   -ansiblelle luotiin muutama eri kansio sekä tiedosto johon halutut hallittavat koneet (localhost) sijoitettiin ja annettiin tehtäviä mitä halutaan ansiblen automatisoivan kaikille hallituille laitteille
   -Python varoitus oli ärsyttävä mutta onneksi siihen oli myös kätevä lisäys "hosts.ini" kansioon jotta varoituksen sai pois
   -copy dest=/tmp kohta jäi hieman epäselväksi että mitä sillä haettiin, mutta halutun lopputuloksen sain kuitenkin toimimaan.
