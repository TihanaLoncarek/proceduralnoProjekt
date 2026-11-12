# Milestone 1

## Izrada kata

Za odabrani projekt bilo je potrebno izraditi model zgrade. U tu svrhu najprije je kreiran čvor "geometry" unutar kojeg se odvija cjelokupna izrada modela. Na njemu su definirani parametri "minWidth" i "maxWidth" za širinu, "minDepth" i "maxDepth" za dubinu te "minFloorHeight" i "maxFloorHeight" za visinu kata. Dimenzije svakog kata nasumično se odabiru unutar zadanih intervala. Dodatno je definiran parametar "floors" kojim se određuje broj katova zgrade.

Unutar čvora "geometry" postavljen je čvor "box", preimenovan u "Floor", koji služi kao osnova pojedinog kata. Zatim je dodana petlja "For-Each Number" koja se izvršava onoliko puta koliko zgrada ima katova. Izlaz čvora "Floor" povezan je s ulazom čvora "foreach_end1" kako bi se izgradilo više katova.

Kako bi svaki kat imao vlastite dimenzije i ispravan položaj, kreiran je čvor "Attribute Wrangle". On na ulazu prima redni broj trenutne iteracije te na temelju njega izračunava visinu na kojoj se kat nalazi. Isti čvor izračunava i visinu, širinu te dubinu kata, pri čemu se redni broj iteracije koristi i za osiguravanje različitog izgleda svakog kata.

Na kraju je izrađen istaknuti krov. U tu svrhu uvedeni su parametri "slabThickness" i "slabOverhang". Unutar čvora "geometry" postavljen je čvor "box", preimenovan u "slab". Njegova visina ovisi o parametru "slabThickness", dok su širina i dubina jednake onima čvora "Floor", uvećane za vrijednost parametra "slabOverhang".

Čvorovi "Floor" i "slab" povezani su čvorom "merge", čiji je izlaz spojen na ulaz čvora "foreach_end1".

<img width="894" height="1036" alt="image" src="https://github.com/user-attachments/assets/0c2120ab-079c-4121-90e4-52c40ca5b3fa" />


## Izrada prozora

Za izradu prozora korištena su četiri čvora "Grid", od kojih dva leže u XY ravnini, a preostala dva u YZ ravnini. Svaki je prozor postavljen na jednu od četiriju bočnih strana čvora "Floor", a njegova se visina i širina izračunavaju množenjem dimenzija te strane vrijednostima parametara "windowHeight" i "windowWidth". Ta dva novouvedena parametra mogu poprimiti vrijednosti između 0 i 1, pri čemu vrijednost 1 označava da je dimenzija prozora jednaka dimenziji kata, a vrijednost 0 da prozor ne postoji. Broj redaka iznosi 2, dok broj stupaca ovisi o veličini kata i parametru "windowsNum".

Svaki je prozor povezan sa zasebnim čvorom "polyextrude", koji ekstrudira rubove prema unutra za vrijednost parametra "windowDepth", dok se parametrom "windowInset" može odrediti uvlačenje (inset).

Kako bi prozori bili vidljivi, na njihovu je mjestu u čvoru "Floor" potrebno stvoriti otvor. Stoga se svi čvorovi "Grid" iz kojih su prozori izrađeni najprije spajaju čvorom "merge", a zatim se čvorom "boolean" taj dio oduzima od čvora "Floor".

Za izradu istaka (peraja) preko prozora korištena su dva čvora "box", preimenovana u "Fin" i "FinBottom". Uz čvor "FinBottom" dodan je čvor "Attribute Wrangle" koji donje dvije točke pomiče prema zidu, čime geometrija poprima oblik izduženog trokuta. Te se dvije geometrije zatim spajaju čvorom "merge". Kako bi se istaci rasporedili po prozoru, uveden je čvor "Attribute Wrangle" koji na ulazu prima spojene baze prozora (čvorove "Grid"), a na izlazu ostavlja samo normale u donjim točkama čiji je smjer okrenut prema vanjskoj strani zgrade. Naposljetku se čvorom "Copy to Points" istaci postavljaju na prozor. Dobiveni izlaz dodaje se čvoru "merge" zajedno s čvorovima "Floor" i "slab". Broj istaka ovisi o parametru "windowsNum".

<img width="494" height="1052" alt="image" src="https://github.com/user-attachments/assets/5658f49d-1cd3-4e00-b002-ce70c47246f8" />


## Izrada detalja

Za izradu detalja nasumično su odabrane točke na stranama svakog kata. To je ostvareno čvorom "Attribute Wrangle", preimenovanim u "wallsOnly", koji zadržava samo bočne strane čvora "Floor", nakon čega se čvorom "Scatter" nasumično odabire "plateCount" točaka. Te će točke služiti kao središta pojedinih ploča. Izlazi čvorova "Scatter" i "wallsOnly" povezuju se u još jedan čvor "Attribute Wrangle", koji nasumično određuje veličinu svake ploče unutar intervala "plateMinSize" i "plateMaxSize". Njegov izlaz, zajedno s izlazom geometrije "box" preimenovane u "plate", povezuje se u čvor "Copy to Points" kako bi se na svakoj točki postavila geometrija ploče.

Zbog svoje veličine, geometrije ploča ne ostaju isključivo na stranama zidova, već djelomično strše prema van. Kako bi se to ispravilo, izlaz čvora "wallsOnly" spaja se na čvor "polyextrude", koji bočne zidove povećava za vrijednost parametra "plateThickness". Njegov se izlaz, zajedno s izlazom čvora "Copy to Points", zatim povezuje s čvorom "boolean" koji zadržava samo presjek tih dviju geometrija. Na taj se način dobivaju ploče priljubljene uz bočne strane zidova bez izbočenja izvan njih.

<img width="408" height="1068" alt="image" src="https://github.com/user-attachments/assets/ff7bc01b-b67c-4379-badc-ecf2821c9fb9" />


## Dodavanje dodatnog dijela kata

Glavni dio zgrade je središnji dio, a na njegove bočne strane moguće je postaviti ili prozore ili dodatni dio kata. Budući da su svi dijelovi kata međusobno slični, petlja se izvršava 5 * "floors" puta, odnosno pet puta za svaki kat. U svakoj se iteraciji izračunava postoji li dodatni dio kata na pojedinoj strani. Ako postoji, prozori se na toj strani ne prikazuju. Kada se u iteraciji obrađuje dio kata koji nije središnji, provjerava se jesu li na toj strani već postavljeni prozori. To je moguće jer funkcija "rand" za iste ulazne parametre uvijek vraća isti izlaz.

Naposljetku se, nakon zadnjeg čvora "merge" u petlji, dodaje čvor "Attribute Wrangle" koji briše cijeli dio kata iz te iteracije ako on ne smije postojati.

<img width="692" height="1078" alt="image" src="https://github.com/user-attachments/assets/ec59d040-ebc8-4598-a3f4-318bfd95afbe" />
