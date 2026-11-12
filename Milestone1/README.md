# Milestone 1

## Izrada kata

Za odabran projekt bilo je potrebno napraviti zgradu. Za pocetak, napravljen je node "geometry" u kojem ce se odvijati cijela izrada modela. Na njemu su dodani parametri "minWidth" i "maxWidth" za sirinu, "minDepth" i "maxDepth" za dubinu te "minFloorHeight" i "maxFloorHeight" za visinn kata. Za svaki kat su nasumicno odabrane dimenzije u tim intervalima. Uz to, dodan je parametar "floors" koji odreduje broj katova zgrade.

Unutar "geometry node" postavljen je node "box" preimenovan u "Floor" koji sluzi kao baza jednog kata. Zatim je dodana node petlja "For-Each Number" na kojoj je namjesteno da se ponavlja onoliko puta koliko ima katova. Na ulaz od "foreach_end1" je stavljen izlaz od "Floor" kako bi se izgradilo vise katova. 

Kako bi svaki kat imao svoje dimenzije i bio na dobrom mjestu, napravljen je node "Attribute Wrangle". Na ulazu dobiva koja iteracija se trenutno odvija te pomocu toga izracuna na kojoj visini se nalazi kat. Takoder, taj node izracunava i visinu, sirinu te dubinu kata, a koristi se i broj iteracije kako bi svaki kat izgledao drugacije. 

Na kraju se radi ispupceni krov. Dodaju se parametri "slabThickness" i "slabOverhang". U "geometry node" se postavlja node "box" preimenovan u "slab". Njegova visina ovisi o "slabThickness", a sirina i dubina su iste kao i kod "Floor", samo sto im je dodan i "slabOverhang". 

"Floor" i "slab" se povezu s "merge" node, ciji se izlaz zatim spaja na ulaz "foreach_end1".


## Izrada prozora

Za prozore su napravljene cetiri "Grid" node, dva leze u XY ravnini, a druga dva u YZ ravnini. Svaki od prozora je postavljen tako da lezi na jednoj od cetiri bocne strane "Floor", a njihova visina i sirina se izracunava pomocu dimenzija te strane pomnozenom s vrijednostima "windowHeight" i "windowWidth". Ta dva novouvedena parametra mogu biti vrijednosti izmedu 0 i 1, gdje je 1 kada je dimenzija prozora jednaka dimenziji kata, a 0 kada ne postoji prozor.
