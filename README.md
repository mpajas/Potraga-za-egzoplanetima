# Potraga za egzoplanetima

## Sažetak
Egzoplaneti su planeti koji se nalaze izvan Sunčevog sustava. Oni su fascinantni astronomima i amaterima koji žele pronaći planete s izvanzemaljskim oblicima života. 
Snimanjem zvijezda pomoću satelitskih teleskopa, kao što su Kepler i TESS, pronašlo se puno egzoplaneta. 
Ručno analiziranje teleskopskih podataka vrlo je težak i dugotrajan posao.
Moguće je smanjiti broj kandidata za ručnu provjeru
primjenom strojnog učenja i drugih naprednih računalnih metoda. Koristili smo logističku regresiju i stabla odlučivanja za klasifikaciju zvijezda na one koje imaju i one koje nemaju egzoplanete.

## Skup podataka

Podaci su preuzeti sa web stranice https://www.kaggle.com/keplersmachines/kepler-labelled-time-series-data.

Podaci korišteni u ovom radu dio su treće kampanje druge Kepler misije nazvane po svemirskom teleskopu Kepler koji je NASA razvila u svrhu potrage za egzoplanetima. Misija se fokusirala na snimanje zvijezda u regiji Mliječne staze u kojoj se nalazi Zemlja, s ciljem pronalaska sličnih planeta koji bi potencijalno podržavali život. Svaka kampanja je zbog putanje teleskopa i kuta upada Sunca bila ograničena na snimanje zvijezda oko 80 dana ([Kepler](https://www.nasa.gov/mission_pages/kepler/overview/index.html)). Nakon micanja šuma u signalu, prikupljene podatke NASA objavljuje u arhivima koji su besplatno dostupni svima ([Caltech](https://exoplanetarchive.ipac.caltech.edu/), [Mikulski archive](https://archive.stsci.edu/missions-and-data/k2)). Skup podataka koji se koristi u radu preuzet je sa Kaggle web stranice na kojoj je korisnik izvdojio intenzitete svjetlosti zvijezda treće kampanje zajedno sa svim potvrđenim planetima ostalih kampanja iz Mikulski arhive ([Kaggle](https://www.kaggle.com/keplersmachines/kepler-labelled-time-series-data)). U tablici se nalazi 5657 redaka koji predstavljaju snimanje intenziteta svjetlosti pojedine zvijezde u periodu od 80 dana. U stupcima koji predstavljaju značajke zapisano je 3197 intenziteta svjetlosti svake zvijezde koji su snimljeni u jednakim vremenskim razmacima.  
Dio podataka prikazan je u Tablici 4.1.
Jedan stupac (LABEL u Tablici 4.1) sadrži oznake koje pokazuju ima li zvijezda planet u svojoj okolini (LABEL=2), ili ga nema (LABEL=1). U tablici se nalazi ukupno 42 potvrđena egzoplaneta, a podijeljena je na skup podataka za treniranje koji sadrži 37 potvrđenih planeta i 5087 zvijezda bez planeta, dok je u skupu za testiranje 5 zvijezda s planetom i 565 bez.

![image](https://user-images.githubusercontent.com/23265032/132749148-416ffac6-8c6f-471f-bf91-75c2d7f08146.png)


Ovako izgledaju neki podaci intenziteta svjetlosti zvijezdi bez egzoplaneta:

![flux_bez_exo_raw](https://user-images.githubusercontent.com/23265032/132750282-25c1be6d-2ba3-4be6-a508-054cd8511706.png)

Ovako s egzoplanetima:

![flux_s_exo_raw](https://user-images.githubusercontent.com/23265032/132750308-92afeb56-d2ee-47f2-814c-f1675b87c662.png)

## Obrada podataka
Obrada podataka izvršena je u četiri koraka koji su prikazani na idućoj slici. Nakon Fourierove transformacije kojom su se podaci prebacili u frekventnu domenu očekujemo da niske frekvencije sugeriraju prolazak planeta ispred zvijezde. Zašto? Zato što je prolazak planeta ispred zvijezde rijedak događaj. Prije Fourierove transformacije podaci su izglađeni Savitzky–Golayevim filtrom. Također su skalirani i normalizirani kako bi se vrijednosti na niskim frekvencijama bolje istaknuli.

Nakon obrade vidi se očita razlika između zvijezdi s planetima i bez planeta koju će machine learning algoritmi lakše "nanjušiti" nego time series podatke.

![image](https://user-images.githubusercontent.com/23265032/132750565-b65aa9d6-00aa-454b-9c31-8096ce1811c8.png)

Nakon obrade podaci intenziteta svjetlosti zvijezdi bez egzoplaneta izgledaju ovako:

![flux_bez_exo_obrađen](https://user-images.githubusercontent.com/23265032/132751524-fad70326-efab-4399-b605-c16df053d121.png)

S egzoplanetima ovako:

![flux_s_exo_obrađen](https://user-images.githubusercontent.com/23265032/132751544-122493ed-a64e-46cf-8ffd-e328438338ff.png)

## Rezultati
Na slikama su prikazane matrice zbunjenosti, redom, logističke regresije i stabla odlučivanja nakon ugađanja parametara. Logistička regresija pokazuje velik potencijal za traženje naše igle u plastu sijena, zvijezde koje imaju planete.

![logreg_confusion_matrix](https://user-images.githubusercontent.com/23265032/132751779-283e1c50-7256-4faa-a116-6f394d7520de.png)
![tree_confusion_matrix](https://user-images.githubusercontent.com/23265032/132751781-8ae5c3d8-412a-4aba-86a7-200c2a200def.png)

Zanimljiv je i rezultat ansambla oba algoritma koji ima nešto manju preciznost.

![ansambl_confusion_matrix](https://user-images.githubusercontent.com/23265032/132752274-0a09c952-cf4e-4e12-ad52-f3ed35e823a9.png)

