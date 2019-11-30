`pretazeno z pmwiki, pravdepodobne se spatne renderuje`

# IETF 68th Meeting
* Prague, 2007, Hotel Hilton
* 19. - 23. 3. 2007
* ''... mel sem se fajne ;) ...''
* http://www3.ietf.org/meetings/68-IETF.html

## TODO
* {-krb generally rfc4120.txt-}
* ghost ???
* iakerb ????
* kerb fast vs. startls - larry zhou, Sam Hartman
* {-gss-api-}
* threath models ...
* cram, digest-md5 i kdyz ho nikdo nepouziva
* scram
* channel binding (nico williams)
* ULP, ESP, AH, SPD ? some IPsec stuff
* krb preautentizace


## co je to: MAC vs. HMAC?
oboje jsou "kontrolni soucty", ktere jsou nakonec zasifrovane nejakym heslem (zpravidla symetricky) slouzi k overeni pravosti a nezmenenosti zpravy po ceste mezi odesilatelem a prijemcem. rekl bych ze H znemoznuje utok na danou symetrickou sifru.

## co je to: GSSAPI?
standardizovana specifikace API rozhrani pro autentizacni ukony zleva uzivana aplikacemi a zprava vyvojari/implementatory autentizacnich mechanismu

* co je to: "TLS vs. SASL"
* co je to: EAP, EAP vs. TLS?;)) .. PEAP ...
* co je to: preauthentication - preautentizace


## Makaci
* Larry Zhu
* Jeffrey Hutzelman
* Sam Hartman
* Nico Williams

## backlog
### prndeli

stredne zlutej pes. 

=JeffH chodi na meetingy oblecenej normalne jako ja do prace akorat bez ponozek ;)

"IETF Meeting" neni konference, je to setkani lidi, kteri pracuji na standartech/RFC pomoci mailing listu. Meetingy jsou pomuckou jak si najit nejake kolegy a upresnit si s nimi nazory a myslenky rychleji nezli psanim. Je to hodne o soušl iventu. Najdou se zde zastupci jednotlivych firem a skupin, ktere dane tema pouzivaji. Pokud clovek na nejakou WG jde, muze se dozvedet co se v nejblizsi dobe v dane oblasti upece, nicmene z prime diskuse nic nevytezi pokud
nezna soucasna temata diskuse v mailech nebo necetl propirane dokumenty.

Zajimave bylo prime zprcani =Jeff(a)H od predsedy krb-wg za lehke invektivy na stranu zastupce Microsoftu (asiata s prisernou vyslovnosti nicmene ti zbyli 2 recnici mu rozumeli dost dobre) s tim ze MS prispiva do WG docela otevrene a hodne takze se tu zadny napadani trpet nebudou ... Mozna tak vyjel spis proto ze v sale sedelo asi 30 lidi, ale ti co znali problematiku byli 3 ;)

Posledni zajimavosti prndeli bylo, ze kazda WG ma na meetingu svoji jabber room,kam se prepisuji hrube myslenky o cem se jedna a audiostream prispivajicich recniku.

### utery
malo kafibrku ;)

no na TLS sem sice tusil o co se jedna nicmene moderator .. um .. mluvil ..um.. tak pekelne ..um.. ze nechapu ..um.. ze ..um. siumelobjednatbyt..um..jen kafe..um 

no asi nejvyznamejsi skupinoou byla krb-wg jak jinak. Pekne nam diskutoval tlustej predseda JeffryH se slepym SamemH s hezkym NicemX a asijskym LarrymZ z Microsoftu ;) .. vysledkem bude zrejme nejaka dohoda o sifrovani komunikace mezi klientem a KDC a snad se dohodnou i na standartu s MS. zrejme se i dockame HW preautentizace ale o te se v zasade stejne nehovorilo ...

dohadovani o polickach v certifikatech moc zajimave nebylo, ale aspon sem si rozbehal acpi_sleep na T43 ;)))


### streda a ctvrtek
zrovna nic moc zajimaveho, nicmene sem nasel E&A, rekl bych ze celkem pobral, a vecer sme meli jednu stihu za druhou. Nejdrif cajti a potom cernej pes ..

 
#### Athena a Euripides
 
* http://web.mit.edu/Kerberos/dialogue.html
* http://www.faqs.org/faqs/kerberos-faq/general/index.html
 
* resi vzajemne overeni/prokazani identity 2 principlu (ucastniku|(uzivatel,sluzba)) pomoci '''symetricke kryptografie'''. To '''za pomoci duveryhodne 3ti strany''', ktere veri oba subj. a spolecnych tajemstvi K_svc(svc,KDC) a K_c(c,KDC)
 
* resi zabezpeceni proti odposlechu neposilanim techto tajemstvi po dratech pri procesu autentizace
 
* zabranuje replay utoku a podvrzeni identity zachycenim prenasenych dat

* dale pripravi vzajemne sdilene tajemstvi pouzite pro dokonceni autentizace a zabezpeceni dalsi komunikace mezi subj.
 
 0) terminology
```
    K_c := hash(passwd)
    TKT := {u,svc,ip,time,SK}K_svc
    AUT := {u,ip,one-time,X}SK
    cred := SK,TKT,svc,u,ip
 ```

 1) obtaining service ticket and session key
```
    c -> kdc: u,ip,svc
         V5: some preauth cause of offline dict attack do K_(c|svc)
    
    kdc -> c: {TKT, SK}K_c >>> TKT can't be stolen on the wire
              V5: TKT,{SK}K_c
```
    
 2) mutual authentication by the ticket
```
    c generates one-time AUT
    c -> svc: TKT, AUT 
      svc: 
        - decrypting TKT => SK
        - decrypting AUT prooves that he's who he caims to be
           and that he's a legitimate owner of the ticket
           (SK was given to him only by KDC)
        - use of AUT cache prevents replay
        
    svc -> c: { Y=X+1 [, TKT1] }SK 
      c: Y-1=X => auth server
```

#### co je to: S/KEY
* metoda autentizace uzivatele pomoci jednorazoveho hesla vyuzivajici '''sily jednosmernych hash funkci'''
* neresi problem MIM, auth neni vzajemna, uzivatel nevi jestli posila data spravnemu serveru << zarucuje se pouzitim certifikatu
  
* predpripravi se data pro budouci pouziti
  `init -> H(init) = p_1 -> H(H(w)) = p_2 -> ... -> H^n(W) = p_n`
 
* uzivatel dostane: `p_n, p_(n-1) ... p_1`
* server si necha pouze: p_n
* init nesmi byt prozrazeno
 
* k overeni se pouzije draha znalost p_(n-1) ze strany uzivatele. Spocitat H(p_(n-1)) = p_n je levne .. spocitat H^-1(p_n) = p_(n-1) je pro utocnika drahe

* uzivatel tedy posle p_(n-1)
* server overi ze H(p_(n-1)) = p_n; potom ulozi p_(n-1) jako nove heslo


#### co je to:Diffie-Hellmans?
* protokol/technika pro ustaveni bezpecneho komunikacniho kanalu ("bezpecnym" generovanim sdileneho tajemstvi/klice) mezi 2ma subjekty (nikoliv autentizace nektere ze stran => neresi MIM)
````
  0) Y = g^x mod p

  1) K = A^b mod p = (g^a mod p)^b mod p = (g^ab mod p) mod p = (g^b mod p)^a mod p = B^a mod p

  DK) g^(a*b) = (g^a)^b != g^(a+b) || g^a*g^b
````

** a,b jsou tajemstvi
** p,g jsou zakladem soustavy ???
** K je vysledny sdileny klic
** A,B jsou verejne casti klicovych parecku

* ??he?? -- ''asociativita umocnovani umozni vymenu struhly na ktere je po celou dobu  privesen alespon jeden zamek a vypocet spravne odmocniny je omezen modulem p ?? .. resp. rozsekani prostoru do nekolika intervalu ze kterych si dost dobre nejde vybrat (dostatecne rychle) pokud budou p,a,b dostatecne velike?'' -- ?he?
* {-ve skutecnosti se metoda opira o problem '''vypoctu diskretniho logaritmu Y vzhledem k modulu p''' (viz. [0]), kde DL je cislo x ktere neni diky modulu jednoznacne urceno -}
* zakladem uspechu algoritmu je vhodne voleni cisel ''g'' a ''p''

* na tomto principu [0] je zalozena sifra RSA (ktera ale uziva narocnosti rozkladu na prvocisla;he? moc nechapu zatim rozdil s DH ;( ) a DSA, ktera zase nekde uprostred zamestnava hashovaci fci. ''[0] je zakladem vsech asymetrickych sifrovacich mechanismu.''

#### co je to: SSH protokol?
* DH + auth?

 
#### co je to: SRP?
* protokol pro prokazani identity uzivatele za pomoci zero-knowledge dukazu
* zaroven ustavi mezi subjekty sdilene tajemstvi kterym muzou zabezpecit dalsi komunikaci
* stoji to na DH a jeste se tam ruzne delaji dalsi sachymace pri vymene zprav a by byla zarucena lepsi bezpecnost a vetsi rychlost

#### co je to: challenge-response
jsou to metody autentizace, kdy dochazi prokazani znalosti urciteho hesla pomoci vyzvy a vypocitani spravne odpovedi (na zaklade zname vypocetni fce/algoritmu). Ve vypoctu obvykle figuruje heslo. Utocnik muze nicmene pouzit utok typu slovnik ci hrubou silu. Heslo se neprenasi 

#### co je to: problem batohu
do batohu dame x zavazi, ktere ma kazde jinou hmotnost a potom se snazime zpetne urcit ktera zavazi jsme tam dali >> problem neni jednoznacne resitelny
* http://cs.wikipedia.org/wiki/Probl%C3%A9m_batohu
 vektor x = [x1, x2, .. xn], mnozina A e (a1, a2, .. an)
 M = x1a1 + x2a2 + .. xnan


### Orákulum
http://crypto-world.info/klima/2005/ST_2005_07_14_15.pdf

Z hlediska bezpecnosti bychom byli rádi, kdyby se hasovaci funkce chovala jako náhodné orákulum, tj. stroj, ktery na novy vstup odpovídá náhodnym vyberem z mnoziny moznych vystupu (jeden vystup se tedy muze i opakovat), pricemz na tentyz (stary, opakovany) vstup odpovídá tímtz vystupem. I kdyz z teoretického hlediska nejsou hasovací funkce náhodná orákula,
prakticky se tak jeví. Kazdá zmena, byt i jednoho bitu na vstupu, má za následek nepredikovatelnou náhodnou zmenu vsech bitu na výstupu s pravdepodobností 1/2. A naopak jakákoliv zmena na vystupu by mela vést k nepredikovatelné a náhodné zmene na vstupu (vstupech).
