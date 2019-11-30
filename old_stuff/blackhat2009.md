`pretazeno z pmwiki, pravdepodobne se spatne renderuje`

# Black Hat Europe 2009 Amsterdam

konferka je to celkem mastna (vlez za 25kkc), ale rekl bych ze hutna a clovek na ni opravdu potka chytre lidi z oblasti bezpecnosti. je to luxus, ale stalo to za to ... muj rodokmen se timto rozrostl: Aina2006, IETF2007, BlackHat2009 .. co sem delal v roce 2008 se ani neptejte ;)

## Poznamky z konference 

Nasledujici serie budiz pouze hloupym prehledem mych nizkych chapani prezentovaneho materialu, rozhodne byste si meli precist cele whitepapery ....

http://www.blackhat.com/html/bh-europe-09/bh-eu-09-archives.html


----

### Fun and Games with Mac OS X and iPhone Payloads
''Charlie Miller, Vincenzo Iozzo''

http://www.blackhat.com/presentations/bh-europe-09/Miller_Iozzo/BlackHat-Europe-2009-Miller-Iozzo-OSX-IPhone-Payloads-whitepaper.pdf

* Technika jak na MACu a i iPhone spoustet jakekoliv programy tak, aby nezanechavali stopy na disku, aneb co se vsechno da udelat v userlandu po uspesne exploitaci (get ring0 - the lord of the ring ;)).

* ASLR je na macu pekne zprasena, protoze sice randomizuje knihovny, ale uz ne dynamicky linker ze ktereho pak lze vsechny potrebne informace ziskat ...

* mprotect je dobre chraneny, nemuzu si vytvorit vlastni stranku a oznacit +x, protoze jsou vsechny casti binarek podepsany applem, nemuzu si ani pujcit cizi stranku pro prepis pac by podpis nesedel ... ale muzu si udelat vlastni kopii nejake jine sdilene stranky ktera uz RWX je ;]]]]

* velka spousta kodu je mezi MacOS a iPhone sdilena (az 80%), castecne lze tedy analyzovat sw iPhone analyzou MacOS X ;)   

* Vyse uvedenou technikouo lze: spustit jakoukoliv binarku, nezanechat zadne stopy na disku, nezavolat jediny execve() (tj. obejit kernel)

* ''notes''
  * libSystem (MacOSX) == libc (linux)
  * pagezero
  * cavity infector
* ''apps'': BinDiff


 
### SAP Penetration Testing
''Mariano Nunez Di Croce''

http://www.blackhat.com/presentations/bh-europe-09/DiCroce/BlackHat-Europe-2009-DiCroce-CYBSEC-Publication-SAP-Penetration-Testing.pdf

* prezentace popisovala ruzne veci kolem sapu, jak je postaveny, z ceho se sklada a jak jsou jednotlive systemy propojeny, co se da crackovat, co se da scanovat ...
* zakladni komunikacni protokol RFC je navrzeny jako plaintextovy, malo lidi to zavira do tunelu
* pro sdileni informaci ma kazdy SAP system sdileny disk (CommonTransportDirectory - nfs, cifs, ...) na kterem se nachazeji uzitecne informace
* pouzivana hesla jsou vzdycky v DB USR02 a daji se crackovat upravenym johnem
* SAP <> Oracle (vetsinou port 1527), user OPS$<SID>ADM
* SAPy umeni vetsinou spoustet nejake davky v hostitelskem operacnim systemu takze: ziskat heslo >> ziskat vic hesel >> projit do OS

* ''note'': hack.lu
* ''apps'': sapyto, BurpSuite/w3af, nmap -T3


### Stripping SSL To Defeat HTTPS In Practice 
''Moxie Marlinspike''


http://www.thoughtcrime.org/software/sslstrip/
http://www.blackhat.com/presentations/bh-dc-09/Marlinspike/BlackHat-DC-09-Marlinspike-Defeating-SSL.pdf
https://media.blackhat.com/bh-dc-09/video/Marlinspike/blackhat-dc-09-marlinspike-slide.mov


* rekl bych ze nejhezci prezentace, nekdo rekne nic noveho, ja rikam chytre, jednoduche, vychcane jak mraky

* 2002 - implementace overovani certifikatu v prohlizecich je totalne mizerna. Nic nezabrani pridat do chainu SSL certifikatu jeste jednu nodu kterou podepisu svym vlastnim, legalnim certifikatem, vytvareny koncovy leaf si udelam na jakekoliv jmeno a prohlizec jej zvaliduje, nekontroluje ani basicConstraints. Nektere implementace SSL to maji spatne dodnes ... 

`root.CRT -> inter1.CRL -> inter2.CRT -> srv1.CRT ... -> srvWhatever.CRT`

* 200x - implementace overovani certifikatu v prohlizecich je totalne mizerna. Staci online vygenerovat certifikat na jakekoliv jmeno s platnosti ktera skoncila pred hodinou. Vetsina prohlizecu zobrazivavala (jak je to dnes musim vyzkouset) dialog o tom ze je certifikat vyprseny ne ze je podpis neplatny !!! pripadne k te informaci uzivatel uz nedojde ... staci ze to vyprselo pred hodinou

* 2009 - sslstrip - lidi nepisou https:// ale opouze domenove jmeno, to znamena ze na HTTPS se dostanou vetsinou az pomoci 302ky nebo kliknutim na nejaky odkaz. Staci byt tedy uprostred a modifikovat traffic (mitm) stranek tak, ze z nej vyhazime vsechny https a nahradime http. Stranka v prohlizeci vypada UPLNE stejne, akorat formularove policko action= nemiri tam kam by melo. Pro zobrazeni zdrojaku stranky je udelat zpravidla druhy request, a tam uz to muze byt zase v cajku

* 200x - udelat si domenu ktera zni china.cn a na ni mit subdomeny jako `www.webkdc.zcu.cz/RT?d2e3ecasde123e132312ec.china.cn` tak aby china uz nebylo v adresbaru videt, specialni znaky se daji poskladat z i18n, vypadaji jako URL ale je to porad domenove jmeno ;)) a tohle dokazou podepsat i velke CA - certifikat nerve, url vypada normalne - vsechno by melo bejt v cajku ne

* lidi davaji take na to jak stranka vypada, kolik je na ni ikonek zamecku a kdesi cosi, co brani nekomu zamenit favico.ico za obrazek zamecku ???

* problemem zbyvaji veci jako content-encoding (gzip, ...), secure-cookies, cachech pages ... to se da vyresit hlubsi manipulaci s prochazejicim trafficem, vhodym zahazovanim cookies/sezeni (posilanim del-cookie...) tim se musi nekdy uzivatel znovu prihlasit.

* v kazdem pripade jde o ziskani jmena a hesla, ktere predstavuji dlouhodoby pristup k prostredkum. o nejaky session hijacking se nikdo nestara protoze je to kratkodobe/kratkozrake. 

* nacvicovat se to da TORem ;) je treba si udelat nekolik nezavislych tunelu pres TOR sit a porovnavat ziskana data. pokud se stranka/pohled pochazejci z nejake exitnody lisi od tech ostatnich je tato brana zrejme evil ;)))

* vsechniy zminovane triky utoci na slabe misto, ktere predstavuje most mezi plainem a ssl. vychcane jak mraky.

* lokalni obranou je VPNka

* ''apps'': sslstrip
* ''note'': homograph attack




### Advanced SQL Injection Exploitation to Operating System Full Control
''Bernardo Damele Assumpcao Guimaraes''

http://www.blackhat.com/presentations/bh-europe-09/Guimaraes/BlackHat-Europe-09-Damele-A-G-Advanced-SQL-injection-whitepaper.pdf

* types: boolean, union, batched

* fs read access: load_file (mysql), copy (postgres), bulk insert (mssql)
** informace se daji sosat i s mezikrokem nalevani souboru do tabulek s naslednym ctenim 

* fs write access: into dumpfile (mysql), lo_export (postgres), exec xp_cmdshell (mssql)

* os access:
** vetsina db muze tak ci onak spoustet programy a to bud primo nebo UDF
** k urychlovani/zmensovani dat potrebnych pro prenos pouzij UPX/strip


* ''apps'': sqlmap, smbrelay, churrasco
* ''notes'': 
  * windows debug script
  * SMB Authentication Relay Attack




### Taming the Beast : Assess Kerberos-Protected Networks 
''Emmanuel Bouillon''

http://www.blackhat.com/presentations/bh-europe-09/Bouillon/BlackHat-Europe-09-Bouillon-Taming-the-Beast-Kerberous-whitepaper.pdf

* AS_REP KDC spoofing na lokalnim segmentu
  * pamy delaji nekdy jenom pulku prace, tj. spokoji se s uspesnym desifrovanim AS-REP a uz neoveruji platnost protoze k tomu by potrebovali jeste hostovsky key (keytab) 
* replay 
  * tehle casti moc nehovim, to musim poradne dostudovat

* nicmene tam byl kombinovany utok s vyse uvedenymi technikami, jenze tomu sem taky moc neporozumel, francouzska anglictina je fakt peklo

* ve Win LSA Secrets je ulozen TGT, stejne jako v Unixu v /tmp/krb.cache 
* Vista pouzivaji TCP pro krb, coz velmi ztezuje tyhle triky

* ? preautentizace ?
 
* obranou je plny IPsec mezi strojem a routerem

* ''apps'': scapy, PSHTK (nejaky toolkit), fgdump, Cain, Ettercap



### Alice in User-Land: Hijacking the Linux Kernel via /dev/mem
''Anthony Lineberry''

http://www.blackhat.com/presentations/bh-europe-09/Lineberry/BlackHat-Europe-2009-Lineberry-code-injection-via-dev-mem.pdf

* tohle bylo dost low level o starsich vecech, typek co vypadal jako hipik vypravel o trikach jak:
  * se dostat do pameti v Linuxu pres /dev/mem
  * jak najit IDT a jine dulezite tabulky a potrebne informace
  * jak upravovat sys_call_table
  * jak najit bezpecne misto v pameti kernelu pro svuj kod aniz by to masinu zborilo
* defaultni kernel /dev/mem nechrani, nastesti grsec&pax, SELinux jo ;]
* obranou obecne je:
  * kontrola tabulek
  * podepisovani (kernelu, modulu, binarek)
  * hashovani (tripwire, aide, ...)
  * zakazat moduly

* ''apps'': libmemrk, libcc



### Integrating Maltego with Offensive and Defensive Open Source Tools  
Roelof Temmingh and Chris Bohme

* na tohle sem prisel na poslednich 5 minut, ale z toho co sem videl je Maltego nastroj pro integraci nasosanych, vygooglenych, vyfacebookovanych, vysniffovanych a jinak podloudne ziskanych dat k ziskani vazeb mezi jednotlivymi subjekty a vubec o dolovani informaci z cehokoliv (vcetne online utoku po zachyceni webove sesny)

* je to nekde k mani na webu a asi by to stalo za to to vyzkouset 
* hodne se to libilo hlavounum a sarzim okolo CIO/CSO

* http://ctas.paterva.com/view/MTG_Files
* http://www.paterva.com/maltego/community-edition/


----


### Stack Smashing as of Today: A State-of-the-Art Overview on Buffer Overflow Protections on linux_x86_64 
''Hagen Fritsch''

http://itooktheredpill.dyndns.org/stuff/studium/bh09/blackhat-linux64-stacksmashing.pdf

http://www.blackhat.com/presentations/bh-europe-09/Fritsch/Blackhat-Europe-2009-Fritsch-Buffer-Overflows-Linux-whitepaper.pdf

http://www.blackhat.com/presentations/bh-europe-09/Fritsch/Blackhat-Europe-2009-Fritsch-Bypassing-aslr-whitepaper.pdf


* buffer overflow, klasicky vysmahnout zasobnik 

http://www.phrack.org/archives/49/p49_0x0e_Smashing%20The%20Stack%20For%20Fun%20And%20Profit_by_Aleph1.txt

* obrana:
  * naucit programatory delat kod spravne
  * vyrabet bezpecne jazyky ktere neobsahuji pointery (Java, Python, Ruby, ...)
  * kontrolovat progrmay za behu (bounds checking) - bohuzel se znacnou vykonnostni penalizaci
  * staticka kontrola, valgrind
  * kombinace predchozich
  * bodik: no ja bych asi jeste doplnil i harwardskou architekturu nicmene (http://planete.inrialpes.fr/~ccastel/PAPERS/CCS08.pdf)

* soucasne ploty a jejich obchazeni:
 
  * NX/XD/XN - rozsirit pristupove flagy stranek na rwx
    * Return-to-libc (ret2libc) \\
Knihovny vsak obsahuji dostatek kodu ktery je jiz pripraven, oznacen jako spustitelny, staci pouze pripravit stack a skakat do hotovych shellcodu ;)  Priprava stacku muze vyzadovat zvetseni potrebnych argumentu (///////////////./////////.////bin/sh) \\
Nekdy (x86_64) je potreba dostat argumenty do registru < ret code chunking < je potreba skakat pres ruzna mista v pameti na vhodne instrukce tak abychom pripravili registry misto stacku, tj. pouzit ret-into-cokoliv (treba i nekolikrat) k priprave vhodnych podminek pro finalni skok ... (phrack 58x04 - Advanced return into libc ...)

    * nektere softwary potrebuji manipulovat se zasobnikem a spoustet z nej kod (java, ...) pripadne vytvaret trampoliny

    * procesory z doby <2004 nemaji potrebnou podporu v jednotce pro spravu pameti a proto se tyto flagy museji/li emulovat softwarove (PaX, W^X), coz slo vypinat odskokem pres mprotect ;)

  * ASLR - vetsina exploitu potrebuje znat dulezite adresy kam se da skakat, randomizace adresniho prostoru mela znemoznit odhadnuti techto potrebnych adres. Od jadra 2.6.12 jadro randomizuje cast VA (8b na x86_32, 28b na x86_64). \\
(bodik: windows delaji podobne veci, randomizuji v rozsahu 2MB, (Dowd/Sotirov  2008) \\
(bodik: asi by bylo hodne zajimave zeptat se Brada Spenglera co si o tom mysli a jestli je PaX a GRSec v cajku nebo ma stejne trable)

    * brute force - nahodnost adresniho prostoru lze bruteforcovat (2004 Schacham)

    * spraying - nahodnost lze obejit alokovanim velkeho prostoru (100vky MB) do ktereho je umisten cilovy shellkod. JavaScript, ActiveX, ... (Dowd/Sotirov 2008)

    * partial overwrites/information leak - pomoci castecnych prepisu se muze skakat relativne, pomoci format-string-vulnerabilities je mozne ziskat informace o rozlozeni dat v pameti (Durden 2002)

    * control the environment - Fritsch vyzkoumal, ze randomizace v linuxu zavisi nahodnost "na case" (jiffies) a PID procesu. Je mozne spoustet dalsi procesy (execve) ktere v casovem okne az dvou minut, sdileji stejnou nahodnost (small_jiffies + big_pid == bigger_jiffies + smaller_pid) \\
(bodik: pouzite jiffies je int, ktery jednou za cas wrapuje kolem nuly) \\
Navic se nahodny generator mel regenerovat kazdou sekundu, bug v jadre byl ze to delal pouze v 5ti minutovych intervalech... \\
Pomoci spousteni novych procesu a cteni jejich informaci lze odhadnout nahodnost v adresnim prostoru a tim ziskat spravne adresy pro skoky potrebne pro exploitaci ... \\
(bodik: asi to budu muset jeste promyslet) 

    * static pages - i ASLR prochazi vyvojem a chybami, jeste v nedavne dobe nebyla randomizovana adres pro linux-gate.so (< 2.6.20) << ret2libc. \\
Navic ani umisteni kodu binarky neni dnes randomizovan takze je pripraven pro ret-code-chunking ;( i kdyz \0 tam delaji bordel. \\
V x86_64 je prozmenu stranka s vsyscall, nezna se to moc uzitelne, protoze je tam malo vhodnych instrukci, ale prej na tom jeste zapracuje ;)


  * Smashing stack protection - stack cookies - cannary
    * Cowan 1998

    * vlozeni magickeho kanarky mezi lokalni promene a navratovou adresu. kanarka kontrolovat v epilogu funkce

    * obchvat prepsanim frame pointeru (Richarte 2002)
    * bodik: na windowsech prepsat exception handler (Dowd/Sotirov 2008)

    * obchvat prepsanim pouze lokalnich promennych, nejlepe pointeru aby utocnik mohl prepisovat libovolne misto v pameti (nejlepe GOT, asi karel ;) (kil3r, bulba 2000; Anonymous 2001; kaempf 2001)

    * vylepseni (IBM 2005), ProPolice - preskladavani promennych stak aby muselo dojit k prepisu kanarka (resp. aby nemohlo dojit k prepsani jinych lokalnich promennych)

    * obchvat: implementace SSP chranila pouze buffery vetsi nez 4b ;(

    * obchvat: pomoci frame pointeru ktery nemusi byt ve vcsech implementacich chranen muze dojit k uniku informaci o stacku, samotneho kanarka nebo vyzradit jej pomoci format-string-bug

    * obchvat: odhad pomoci side-channel timing attack (Hawkes 2006 - RuxCon)

    * obchvat: kernel spatne inicializuje kanarka nekdy na statickou pripadne odhadnutelnou hodnotu (Poor man&#8217;s randomization hack by Jakub Jelinek)

    * obchvat: prepsat samotneho master kanarka ;)

* podle Hagena jsou cookies zatim nejlepsi obranou. Nicmene je nutne rici ze neumi poznat dobrou diskoteku od spatny, ani rozeznat 14ctku od 19ctky ;)) Take je zvlastni ze nikdy neslysel o GrSecu, pricemz o PaXu vi .. inu nemec.



### WiShMaster - WIndows SHellcode MASTERy 
''Benjamin Caillat''

http://www.blackhat.com/presentations/bh-europe-09/Caillat/BlackHat-Europe-09-Caillat-Wishmaster-whitepaper.pdf

http://www.blackhat.com/presentations/bh-europe-09/Caillat/BlackHat-Europe-09-Caillat-Wishmaster-slides.pdf

* dnesni malware musi:
  * byt sifrovany tak aby se spatne detekoval (staci XOR, na 3 radky v C)
  * mel by byt pouze v pameti aby nesel nalezt signaturovym antivirakem (polymorfismus)
  * mel by se umet propagovat pripojovanim k binarce nebo bezicim procesum (patching entry points, patching functions table, dll injection, direct code injection )

* shellkod musi byt specialni tak aby sel pouzivat genericky bez nutnosti pouzit knihovny ci jiny kod z hostitele, bez potreby mit hardcodovane jakekoliv adresy << wishmaster je nastroj pro vytvareni takovychto shellkodu z libovolneho zdrojaku v C

* na cilovy stroj je potreba dostat loader, ktery se uhnizdi v cizim procesu a pote sestavi z uznych kousku vysledny malware (kazdy kousek muze prijit jinou cestou: prohlizecem, USB klicem, ...) 

* wishmaster dokaze z libovolneho programu v C pripravit takovy virus: nejdrive je kod prelozen, vysledky kod je upraven tak aby nepotreboval linker a obsahoval vsechny potrebne fce, potom je poupraven tak aby pouzival spravne pointery a nebyl adresne nezavisly (pouziti globalnich pointeru) a nepouzival zadne hardcoded_adresses (shellcodisation)

* i presto ze wishmaster provadi nejakou normalizaci dulezitych GLOBAL_DATA je presto velmi obtizne jednotlive casti nejak fingerprintovat, maximalne podle nejake magicke konstanty jenze ta se da snadno zmenit

* je mozne sifrovat jednotlive casti sdilenym klicem a ke kazde z nich prilozit pouze jeho cast, k desifrovani dojde pouze pokud jsou slozeny vsechny potrebne casti viru/klice

* bodik: rekl bych ze podobne modularni je StormWorm 

* ''apps'': wishmaster, scapy, shellknife, DebugView, WebDoor, Shellforge
* ''notes'': shellcodisation


### Tactical Fingerprinting Using Metadata, Hidden Info and Lost Data
''Chema Alonso, Enrique Rando''

http://www.blackhat.com/presentations/bh-europe-09/Alonso_Rando/Blackhat-Europe-09-Alonso-Rando-Fingerprinting-networks-metadata-whitepaper.pdf

* v dnes pouzivanych dokumentech se skryva mnoho uzitecnych informaci, ktere nejsou na prvni pohled tak uplne videt
  * metadata (exif, autor, linky, templaty  ...)
  * hidden data (revize, verze sw, tiskarny, sql selecty pro cteni z externich zdroju, sitova ACLka)
  * lost data

* sbiranim techto dat z verejne dostupnych dokumentu muze clovek nasbirat velke mnozstvi informaci i o interni strukture intranetu dane firmy/subjektu
  * jmena uzivatelu, cesty k jejich domacim adresarum
  * UNC cesty sdilenych adresaru
  * adresy tiskaren
  * ...

* sbirat tyto data lze i pomoci GYM(google/msn/yahoo)
* fotky krome datumu nekdy obsahuji i GPS souradnice
* existuji i nastroje pro cisteni techto dat, nicmene to vetsinou nedelaji moc kvalitne

* typkove vytvorili soft jmenem FOCA ktery dokaze dolovat data z beznych dokumentu (doc,odt,pdf ...). Ukazka byla velmi hezka, existuje online verze, le jenom pro jeden dokument. Prej tam nemame posilat PDF ktere obsahuji viry ;]

* ''apps'': FOCA, notepad++, BinText, rhdtool.exe, oometaextractor, metagoofil


### Detecting "Certified Pre-owned" Software and Devices
''Chris Wysopal''

http://www.blackhat.com/presentations/bh-europe-09/Wysopal/BlackHat-Europe-2009-Wysopal-Certified-Pre-Owned-1.00-wp.pdf

* prezentace se zabyvala detekci backdoru pomoci staticke analyzy programu. Jake jsou zhruba typy backdoru, jak se je pokouset nalezt, po cem patrat v pripade rootkitu, a ktere jine zajimave konstrukce viry/rootkity obsahuji.  Prezentace se soustredila na statickou analyzu.

* backdoory
  * systemove (viry)
  * kryptograficke (umyslna chyba v kryptoalgoritmech)
  * aplikacni (hacky zanechane autory aplikace, treba i s dobrym umyslem; Ken Thompson - /bin/login)

* typy a detekce:

  * specialni hesla (vetsinou hardocodeovana)
    * vyhledavaji se napriklad kontrolou, zda nejsou v kryptofunkcich pouzivany staticka data
    * vyhledavaji se jako retezce ktere pripominaji hashe
    * take je zajimave hledat obfuscovane veci (bodik:r57 web counter) nebo deobfuscovaci rutiny (base64_decode, eval, exec, system, ...)
    * vyhledavat veci ktere porovnavaji cosi s IP, DNS, porty, ...
    * vyhledavat kusy kodu ktere generuji (odchozi) traffic (connect(), bind(), mail())

  * invisible web parameters (admin=1) (wordpress 2007)
     * porovnavat vstupy programu (formulare) s daty ktere aplikace zpracovava (GET...)

  * undocumented commands (quake rcon 1998)

  * debug commands


* detekce rootkitu: 

  * vyhledavat podezrele fce ktere naznacuji nestandardni chovani (SetWindowsHookEx, regSet, regGet, CreateRemoteThread, SuspendThread, WriteProcessMemory, iat hook)

  * detekce debugeru - anti debuging kodu
    * windows API (IsDebugerPresent, OutputDebugString)
    * pomoci FindWindow zkouset najit OllyDbg
    * vyhledat v registrech jestli je olly nainstalovan ;)
    * pokusit se debugovat sam sebe (windows dovoluji mit na proces privesen pouze jeden debuger)
    * vyhodit vyjimku, jestli ji nekdo (debuger) chytne

* detekce casovych bomb
  * vyhledavani porovnani nejen s casem, ale vubec praci s casem
  * vyheldavat kod ktery kontroluje delku behu programu (difftime)

* ostatni:
  * self modifying code
  * unreachable code
  * encryptet data blocks

* Ken Thompson: Reflections on Trusting Trust \\
http://www.ece.cmu.edu/~ganger/712.fall02/papers/p761-thompson.pdf \\
typek to teda prezentoval trosku jinak nez clanek, hlavni rozdil byl v tom ze Ken pry takhle atakoval i disassembler, aby ochranil puvodni blob. ;) 

* ''notes'': http://attrition.org/errata/cpo/


### Masibty: a Web Application Firewall Based on Anomaly Detection
''Stefano Zanero & Claudio Criscione''  

http://www.blackhat.com/presentations/bh-europe-09/Zanero_Criscione/BlackHat-Europe-2009-Zanero-Criscione-Masibty-Web-App-Firewall-wp.pdf

* Masibty je webovy aplikacni firewall, ktery pouziva metody detekce anomalii

* mozne typy waf
  * blacklisty - neucinne, spatne
  * whitelisty - velmi obtizne implementovatelne, nemozne na realnou udrzbu

* resenim je Masibty, waf pro detekci anomalii
  * automaticky whitelist
  * defaultni deny na vsechny ostatni veci
  * unsupervised learning  

* vstupnimy daty jsou URL. Ta jsou prohnana analyzatorem rekvestu, nasledne je shlukuji a srovnavaji jejich vlastnosti a "vzdalenost" od ostatnich. Parametry se zkoumaji na ruzne (statisticke) ukazatele
  * delka
  * character distribution
  * "slovnikovani" ... (token engine)
  * ..

* pro analyzu pouzivaji nekolik enginu, ktery kazdy z nich zkouma jine vlastnosti
  * Number Engine
  * Token Engine
  * Length Engine
  * Presence Engine/Alien Engine
  * Distribution Engine
  * Order Engine

* analyzuji i DOM strom generovanych stranek, ten posleze generalizuji >. z toho procesu vypadne template stromu. Pokud je stranka podobna s templatou je to ok, pokud se nejak lisi je to spatne. Strom funguje podobne jako regularni vyraz/akceptor. Pri kontstrukci stromu se zajimaji hlavne o JavaScript (staticke html neni prilis zajimave)  

* na podobnem principu (generovani stromu) podchycuji i SQL (rozkladaji prikaz podle operatoru). V pripade injection je strom radikalne odlisny od toho na ktery je aplikace "zvykla" 


----

## Co jsem nestihl ...

... a mluvilo se o tom v zakulisi

* Jeroen van Beek - Passports Reloaded Goes Mobile 
* Enno Rey and Daniel Mende - All Your Packets Are Belong to Us
* Erez Metula - .NET Framework Rootkits: Backdoors Inside Your Framework 
* Roberto Gassira' and Roberto Piccirillo - Hijacking Mobile Data Connections 

----

## Ostatni odkazy

* http://www.endhack.com
* http://www.thoughtcrime.org/
* http://www.biomimicryinstitute.org/
* http://c22blog.wordpress.com/ 

* http://www.chaosreigns.com/code/springgraph/3d.html


----


## Cesta do zeme zaslibene

Nasledujici radky byste nemeli cist.

### Centraal

... inu je treba uznat ze ac byla tahle cesla lemovana vselijakymi nastrahami osudu, byla nakonec velmi vydarena ...

Zaclo to uz cestou v praze, kdy sem jel na Kacerov za Emylem prespat protoze mi letadlo letelo v sedum rano. Vylezu na Kacerove z metra a hups, stoji tam chaos ;] inu nahoda. Rano vezmu tago, specham na letiste, odbavim se .. vsechno jako na dratkach.

Letiste na Sipcholu jedna basen, kam se hrabou obrovske haly na Ruzyni, 3 patrova budovka nahore odlety, uprostred jidlo a stanky a primo pod letistem velka zeleznice. Lupen si koupis v automatu a vlaky tu jezdej na vsechny smery kazdej 10 minut. Mazec, zadny chozeni sem a tam a hledani spravnejch vchodu a vychochu, bloudeni po rozhlehlych halach a podobne. Tady to maj vsechno celkem kompaktni.  

Zadnej problem se nekam dostat, zadny fronty. Tady vubec ty transporty frcej jako na dratkach (teda krom toho ze se strana nastupiste na kterou prijede vas vlak zmeni 2x behem 2 minut cekani, takze pri nastupovani je opravdu dobre se presvedcit ze to je ten spravny vlak ;)

Lupen mezi Sipcholem a Amsterdam Centraal me stal 3.9e a cesta byla hezka jenom par minut. Vylez na Centraale stejnej jako na letisti, prakticky zadna hala, jenom velkej podchod se stankama mezi schodistema na perony, zadny kasy, jenom automaty na listky, koupit si listek na kartu .. zadnej problem. 

{{ :oldstuff:zhotelu.p.jpg?direct&400 |}}

Hotel byl situovanej prakticky v centru, na okraju prumysloveindustrialni zony, ktera se v amstru tahne podle celyho zalivu, podel baraky a fabriky, lode co vozej lidi a veci. Pesky trvala cesta asi 10 minut a uz sem stal na mole pred hotelem jako kraaava. Nejakejch 5 hvezdicek, vsichni maj kvadra a rikaj ti "Sir".

Pokoj totalne luxusni s vlastnim zehlicim prknem a zehlickou (krome telky, postele, lednicky a osobnim trezorem). Hned pod hotelem je pujcovna kol, ktera se z pocatku zdala jako fajn, nakonec se ukazalo ze sou stejne lemplove,
no mozna sem lempl ja ;] 

Kola maj teeeda, kola maj vsude. Uz primo pred Centraalem stalo molo, 30x20 metru, a na nem asi 400 kol, nekecam ve 2 radach nad sebou, kolo vedle kola nalepeny na sebe, to sem jeste chapal, je to jejich hlavni nadrazi, ale stejne ... 

{{ :oldstuff:chopper.p.jpg?direct&400 |}}

Jejich kola sou spis jako chopper, vsechno takovy damskoholandsky s velkejma riditkama a sedackou proklate nizko, nektery vic nektery min, je to takovej korab kterym se clovek loooooowdaaaaaaa mestem. Mistni se teda neloudaj a celej ten traffic na ulicich je plnej cyklistu, ne jako u nas, tam jezdi vic kol nez aut, a vsichni to perfektne zvladaj, protoze infrastruktura silnic ma v sobe pruhy pro kola uplne vsude, jako mazec, vubec sem z toho nestihal. A kola vsude. U kazdickyho klandru stoji zpravidla nejake kolo, nekdy 2, nekdy 10, nekdy 200 na jednom fleku. Inu jako cesky zacinajici cyklista sem z toho byl mirne perplex. 

{{ :oldstuff:kanaly.p.jpg?direct&400 |}}

Stary centrum je cely postaveny na husty siti kanalu (ktery se teda tahnou asi pocely zemi v ruznych velikosti). Baraky nemaj omitku a vsechny vypadaj dost stejne, posleze pri dalsich projizdkach sem zjistil ze asi vsechny baraky v holandu vypadaj uplne stejne  ... ;) 

{{ :oldstuff:staremesto.p.jpg?direct&400 |}}

No a co cert nechtel najednou sem se potacel mezi tunou turistu, jak sem zjistil druhej den dojel sem do nejvic nejcentrovatejsiho centra aniz sem ho hledal. Vypada to celkem dost jako praha, spousta turistu, vsichni se motaj, vsude samy kramy s cingrlatkama a tak .. normalni centrum hlavnich mest. Nastesti sem se celkem rychle dostal na misto kterymu se rika Waag a nemaje co delat hadej co .. zasel sem na kavu.

{{ :oldstuff:joker.p.jpg?direct&400 |}}


Byl krasnej jarni den a uvitala me hrozne prijemna teta ktera jiste nebyla o moc starsi nez-li. Toz lamanou anglictinou sem ji vysvetlil ze nejsem misti a vubec nevim jak to tam chodi, takze se o me hezky postarala a udelala mi ukazku ktery vsechny druhy maji. Vzal sem si nejslabsi bilou s bilou kavou ;)) ... pohoda, je to trosku zvlastni pocit ale bylo to dobry. Nabidl sem ji, abych tam nebyl jako koren sam (bylo 11 dopo) jenze oni to tam takhle nemaj. Anzto to tam vnimaji stejne jako pivo tak miva vetsinou kazdej svoji kavu (inu kdyz si clovek u nas objedna pivko taky ho neposila ze ;) tak sem se aspon snazil konverzovat. Po chvili prisel jeji kamos Jerry, kterej je spisovatel a z toho sem vytahl vsechny dulezity veci, jako kam se ve meste podivat a kam jit nakupovat (Albert Hejin), a kde najit nejakej obchod s bubnama (chtel sem najit puvodne ten Kenecuv, jenze to bylo 10 let zpatky ze ;)

Tak sme tam chvili posedeli a ja mel to s testi ze me Jerry vzal s sebou a protoze nemel co delat ukazal mi projizdku meste, tim mi dal zakladni trenink jizdy v jejich provozu. No mel sem takovej pocit ze ty jejich semafory jsou tam jenom pro ozdobu a pro vyhoveni predpisum unie. Jezdi se tam na cervenou, na zelenou, na orandzovou, je to jedno, prislo mi to jako kdyz Dolf vypravel o thajsku, tam se jezdi na plynulost provozu ne na nejaky predjizdeni ... Proste jedes a dokud do nikoho nevrazis tak muzes jet i stredem krizovatky .. uzasnej fenomen ;) to se mi moc libilo ... 

{{ :oldstuff:opera.p.jpg?direct&400 |}}

S Jerrym sme obrazili 2 mistni parky, nejdriv me vzal do takovy ho mestskyho, abych videl Operu a Van Goghovo muzeum, dali sme si frisbie a pak pokracovali do vetsiho parku, cestou vzali pivko a koupili mapu, protoze ta co sem dostal v hotelu mi uz po 3 hodinach byla mala ;) Parky tam teda maj, sou docela hezky i kdyz jsou v centru mesta a narozdil od nas tam maj vsude samy rybnicky (to asi maj tak jako tak po cely zemi park nepark, ale nevim, byl sem jenom v tom Amstru). Je to tam narvany turistama ale po case sme nasli takovej volnejsi flek. No a protoze sme byli v Amstru tak se za chvili priritil Juil (Zil nebo jak se to francouzsky pise) .. byl to zongler a prijel asi ze sem se mu libil protoze sem na nej ukazoval zonglovani kdyz sme projizdeli parkem, tak to pochopil a prisel si k nam posedet, pokourit a pokecat. 

{{ :oldstuff:juil.p.jpg?direct&400 |}}

Skoda jen ze me jenom jednu sadu kuzelek a 5 micku, ale i tak to stacilo aby me naucil krasnej novej jednoduchej trik, kterej nemel jmeno, ale zahy jej dostal .. Juil ;) Odpoledne v parku probehlo paradne a nez sme se rozesli tak sem si stihl i zahrat na kytarku kterou sem si prozmenu pujcil od jinyho typka.

No a protoze se mi jeste nechtelo domu zase sem se vydal na projizku mestem. Podle slunce sem jel az nekam daleko na jih, myslim ze sem dojel az nekam do Amstelparku, ale rict to nemuzu, telefon sem nemel takze bez fotek a pak sem se nasel nekde v Zuideramstelu, kde sem se teda zeptal jak se dostat zpatky do centra.

Vyjizdka to byla nadherna, na otm jejich chopperu se jezdi jedna basen, staci nasednout, 3x slapnout a jedes hodinku, dve, tri. Vsechno je tam placaty takze se clovek ani moc nenadre. Zpatky sem se vratil az dllooouho za tmy a pak sem stejne jeste drandil po pristavu a prohlizel si svitici lodicky s turistama na palube ;)    

{{ :oldstuff:vpristavu.p.jpg?direct&400 |}}

## Day 1

Konferka celkove probihala jednoduse, na konferencnim patre 2 tracky plny chytrejch typku. Prezentace byly hezky a celkem sem jim i rozumel, coz se nedalo rict o rozhovorech ve foyer ;) ... Na objede kde sem se dal do reci s nemcema sem si prisel jako totalni amater, protoze tam byl kazdej desnej pentester, kterej cely dny pentestuje nebo konzultuje jiny peny a nikdo nepracuje pro firmu mensi nez Ernst&Young nebo je pri nejhorsim alespon nejaky CSO v jiny maly firme co stejne dela pro velky banky ;]

Vecer sem se na brifingu moc nesocializoval, takovy normalni se rozprchli a zustavali jenom hlavouni v sakach a ja mezi nima jako hipik ;) Toz sem se zase jal jezdit mestem, zalej sem do Waagu, akorat ta hodna teta uz tam nebyla takze sem sel zase korzovat. Korzoval sem dlouho a protoze sem vyjel nekdy v sedum tak brzo padla tma a nez sem se ve 12 dostal zpatky tak sem se 3x ztratil ;] nastesti sem si poridil dobrou mapu tak sem se dycky nasel, nicmene v noci to mesto vypada uplne jinak nez ve dne (kdo by to cekal ze, nicmene v te chvili sem byl uz vtazen do zpomalene nalady, ktera se asi tahne celym holandskem a je notne podporovana tichem ktery se mimo nejcentrovatejsi centrum dost znat. Vubec mi ta zeme prisla dost klidna, v noci tam postavaj misto houmlesaku akorat tichy kola u klandru a sumi tam voda v kanalech .... relax jako blazen ...

{{ :oldstuff:relax.p.jpg?direct&400 |}}

### Day 2

Probehl v hodne stejnem duchu, ale uz sem tam mel nejaky tipy z obeda ke kterejm se clovek mohl pritocit a prohodit aspon par slov, pripadne si nechat vysvetlit co sem z prezentace nepochopil. Jazykova bariera mi davala teda kurevsky zabrat ( s tim ze s tim musim neco jiste udelat). Asi 2 nejzajimavejsi veci ktery sem tam videl byl Moxie kterej ukazoval ze si cely SSL muzeme strcit vitekam, prednaska o utocich na Kerberos, ze ktery sem pobral jenom pulku ale rozhodne si ty veci musim vyzkouset a tak posledni myslenka z uplne posledniho sezeni kdy typek vzpominal jak Ken Thompson, udelal prekladac s backdorem, zabackdoroval disasembler aby to nikdo nemohl videt a protoze tim prekladacem prekladali krome normalnich programu i dalsi/nove prekladace casem mohl backdoor ze zdrojaku odstranit ale dira tam porad zustavala a propagovala se dal a dal a dal ;)

Tak rekl bych ze tohle byly asi 3 myslenky/prezentace ktery mi utkveli v pameti az do navratu, teda krome zapisku ktery sem si ke kazdy prednasce udelal, zkusim je sepsat down below ... 

{{ :oldstuff:party1.p.jpg?direct&400 |}}


Vecer, krome obvykle projizdky, kterou jsem notne zkratil, sem se vydal na After Party do klubu Panama. Pekne si dal do nosku jejich pivko (ktery je teda dost limonadoidni na cesky pomery), pokecal s Bradem kterej to tam asi dost sefoval a pracoval na nejakejch projektech simulovani zivota. Rekl bych ze jmeno Brad Smith budu muset jeste trosku progooglit, byl veselej, povidavej a asi i celkem chytrej ;]]]]

{{ :oldstuff:party2.p.jpg?direct&400 |}}

Nakonec sme dam dali dohromady bandicku, ktera nechtela jit spat kdyz zavreli. Podarilo se mi presvedcit jednoho mistniho co byl s nama na konferenci (jako rad bych si pamatoval jmena ale to je dost tezky i tady v cechach, natoz tam ...)

{{ :oldstuff:redlight.p.jpg?direct&400 |}}

... no a ten nas vzal kam ? no prece do Red Light District. Puvodne sem teda myslel ze to bude nejaky vetsi halo, ale nic moc. par uzkejch ulicek jako na stary praze (nektery teda cca 50 cisel siroky ;), a kazdejch par metru velky okno a za nim usmevava paneka ktera vypada ze prave vypadla z Adobe Photoshop 7.0(tm). 

Nic moc, to ostatni holandanky sou moc hezky holky, asi tim ze sou vsechny hubeny jak jezdej porad na kole, nosej hodne barevny obleceny a spoooustu sukni. Jo a taky satky, asi jak tam fouka tak je satek teda nejenom modni doplnek. Nejvic mi pripominali irky, hubeny a svetly. Jak ruzne vypadaj holky tak chlapi myslim ze tam sou takovy stredne unifikovany, myslim (podle toho co sem videl) nosi prumernej holandan kosili, svetrik a kdyz fouka tak nejaky sacko. Vetsinou kratky vlasy a podlouhlejsi hlavicku, proste takovej svihak. 

Zbytek lidi je tam hodne z arabie nebo z afriky, ale jak rikam, byl sem akorat par dni v hlavnim meste ...  

{{ :oldstuff:vydrzeli.p.jpg?direct&400 |}}

No nakonec se skupinka turistu z RLD zacala pomalu rozpadat protoze se trosku rozfoukalo, ale ti nejstatecnejsi tj. ja, typek z nemecka co prezentoval ochranu pameti a borka z israele, ktera byla zrejme uz dost nalita a unavena ;) sme zakotvili nekde v miste ktery se jmenuje "Studenten diskoteke" coz byla podprumerna dira, do ktery bylo 5 Ecek vstup. Uvnitr nebyl nikdo, krome bandy nejakejch 14ctek co si hraly na 17ctky, akorat ten nemec co byl s nama porad rikal ze jim je minimalne 19 spis vic, tak sme se mu smali ;] 

{{ :oldstuff:diskoteka.p.jpg?direct&400 |}}

Bylo to tam priserny a navic s pekelnou muzikou, chtel sem jungle ale ksiltofkar poustel 2step tak sem zase zmizel do mesta a pak pomalu na hotel. Nasel sem jadro BH kalit, vsechno americani a bavil se snama akorat The Nurse, typek kterej to tam dost sefoval a rikal ze pracuje na nejakej projektech s biomimicry ;) Byl fajn a veselej ... Tak sem si dal posledni pivko, nejaky cigo pred hotelem a zasel spat ...

Rano sem se sbalil, dal si veci do uschovny a sel sem se jeste projet a sehnat darky, ty dny predtim sem se hodne dival kolem ale obchody sem nejak prehlizel,nastesti mi opet (jako skoro cely ten vylet) bylo nesmirne prano a nasel sem proslulej blesi trh ;) .. krasa, koupil sem satky, jednu plachtu (ktera mela teda doma po rozbaleni jinej vzor nez sem chtel, ale vypada to taky africky tak co se da delat) koupil sem si mikinku, podle ktery me kluci po nawratu do prace hned zacali prezdivat El Gobelin ;) LOL

{{ :oldstuff:guatemalec.p.jpg?direct&400 |}}

{{ :oldstuff:nakole.p.jpg?direct&400 |}}

{{ :oldstuff:obchod.p.jpg?direct&400 |}}

Dokonce sem tam potkal guatemalce z konference tak sme pak nakupovali spolu, zasli zase do parku, dali si pivko a pohoda. Nakonec me doprovodil do hotelu, ja si vyzvedl veci, hodinu sem cekal na typka od kol i kdyz mel mit normalne otevreno abych se dozvedel ze moje obcanka je proste fuc. Nojo, tak co se da delat. Dali sme si s Camiem posledni kavicku, ja mu tam nechal zbytek co sem mel abych nemel na letisti nejaky honky ze me vyhmatnou a mastil sem zpatky na Centraal ... 

... inu co rici zaverem. Cesty jsou nevyzpytatelne pane a i presto ze sem se to tyhle casti sveta dostal rekneme velmi komplikovanym zpusobem a to dokonce az na druhy pokus a s jinym itinerarem nez ktery jsem puvodne planoval, vylet to byl krasny, plny zivota a stejne s jakym odporem a nechuti jsem letel tam, tak jeste s vetsim sem se vracel zpatky. I z vlaku jsem vystoupil na posledni chvili a jeste zustal dalsi hodinu sedet na molu a koukal se na zaliv ... 

{{ :oldstuff:zaliv.p.jpg?direct&400 |}}

But nothing last forever, even cold november rain ... takze sbohem a satecek ... 

{{ :oldstuff:satecek.p.jpg?direct&400 |}}
