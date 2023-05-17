# data-project

*Eget projekt i kursen "Pythonprogrammering för AI-utveckling" vid IT-Högskolan VT23 med syftet att använda maskininlärning/AI till att lösa ett på förhand
givet problem. Projektet var uppdelat i två steg: 1 och 2, där det första steget var fouserat på problemställning och mer high-level data exploration och andra steget bestod av data cleaning följt av träning, testing och utvärdering av ML.*

Avslutande del av projektet återfinns i `Del2_Datahandling.ipynb`

## STEG 1

Den första delen av projektet fokuserar på att undersöka förutsättningarna för det egna projektet, detta har gjorts i fyra delsteg:

### 1. Problemställning

_Beskrivning av problemet och mål._

#### Snöfalls förutsägelse för Stockholm.

Årligen lamslås Stockholms kollektivtrafik av snöfall vilket medför negativa konsekvens för invånarna och viktiga samhällsfunktioner (utryckningsfordon, etc.). Idealt hade dagens väder kunnat användas för förutsäga morgondagens ev. snöfall så att kommunen bättre skulle kunna planera jourhavande snöröjning eller på annat sätt anpassa sin kollektivtrafik.

### 2. Identifiering av relevant data för att lösa problemet

#### Identifiera förutsättningar för snö, [källa](https://nsidc.org/learn/parts-cryosphere/snow/science-snow):

- För att det ska snöa måste det finnas luftfuktighet i atmosfären.
- Snö bildas när den atmosfäriska temperaturen är vid eller under fryspunkten (0°C) .
- Generell regel: snö bildas inte om marktemperaturen är minst 5°C.
- För marktemperaturen vid eller under fryspunkten kan snö nå marken, under optimala förhållanden kan snö nå marken vid högre temperaturer.
- Det kan inte vara för kallt för att det ska snöa, men det kan vara för torrt.
- De flesta kraftiga snöfall inträffar när det finns relativt varm luft nära marken (typiskt -9°C eller varmare) eftersom varmare luft kan hålla mer vattenånga.
- Väldigt kalla men torra förhållanden ger sällan snöfall.

#### Identifiera nödvändig data transformation: snö till vatten (nederbörd) förhållande, [källa](https://nsidc.org/learn/parts-cryosphere/snow/science-snow):

Snö består av frusna vattenkristaller omgiven av luft vilket medför att luft utgör den störst volymen i ett snölager. Ett vanligt (men inte alltid korrekt) antagande är ett tio-till-ett-förhållande (10:1) mellan snö och vatten. Majoriteten av nysnöfall i USA innehåller ett vatten-till-snö-förhållande mellan 0,04 (4 procent) och 0,10 (10 procent), beroende på de meteorologiska förhållandena som är förknippade med snöfallet.

I detta projekt kommer det generella 10:1 snö-vatten förhållandet att användas.

#### Data som behövs:

Baserat på ovanstående "litteraturstudie" förutses en modell behöva historisk väderdata från centrala Stockholm innehållande:

- Datum - för att kunna genomföra jämförelse av samma dag ifall data hämtas från olika källor.
- Temperatur (°C)
  - Idealt: Atmosfärisk temperatur
  - Annars: Marktemperatur
- Luftfuktighet
- Nederbörd (mm) - för att kunna fastslå om det snöat eller ej (nederbörd > 0.0 mm = nederbörd). Teoretisk kommer denna utifrån Temperatur och Luftfuktighets data kunna klassas som snö eller regn. Baserat på vatten-till-snö förhållandet kommer även mängden snö (volym) att kunna uppskattas.

Identifierad potentiell datakälla: [SMHI - Meteorologiska observationer](https://www.smhi.se/data/meteorologi/ladda-ner-meteorologiska-observationer/#param=airtemperatureInstant,stations=core,stationid=98210)

Vald väderstation: Stockholm-Observatoriekullen p.g.a. dess centrala läge.

### 3. Inledande dataanalys

_Under den inledande dataanalys-fasen undersöktes data utifrån följande kriterier:_

- _Komplett dataset?_
- _Förekommer null-värden?_
- _Förekommer Extremvärden?_
- _Förekommande datatyper?_
- _Relevanta datafält?_
- _Kan relevanta fält konverteras till ett numeriskt format?_

Från data analysen utförd i _`Del1_Datahandling/DataExploration.ipynb` kunde följande observationer göras:_

- __Dataset__: I SMHIs Meteorologiska databas återfanns historiskt data för __Nerderbörd__, __Temperatur__ _(på mätstation, vilket antas vara marknivå)_ samt __Relativ luftfuktighet__.
  - Dessa dataset kunde fås med varierat tidsintervall (15 min, 1h, 12h , 1 dygn, osv.) för olika dataset.
  - Dataseten med mät-tidsintervallet 1h valdes för närmare analys då denna var den enda steglängd som förekom för samtliga utvalda dataset.
- __Data cleaning__: Preview av filerna visade att .csv-filerna inte enbart innehöll tabelldata utan även inledande information, vilket behövde städas bort i Excel innan de kunde läsas in med hjälp av pandas.
- __Labled data__: Utöver de initialt identifierade dataseten återfanns även data på __Nederbördstyp__, detta dataset förekom dock enbart med tidsintervallet 12h för tidsperioden 2019-10-07 till 2023-04-14.
  - Eftersom detta data var labeled drogs slutsatsen att nederbörd inte längre behövdes klassificeras/uppskattas som snö utifrån den vetenskapliga "snöreceptet" ovan. Detta medförde även att alla tillgängliga dataset med steglängden 12 h laddades ner (__Temperatur Min Max__) och undersöktes.
- __Datakvalitet / komplett dataset__: Data analysen visade att alla numeriska dataset utöver två bortvalda, tomma dataset (Nederbördsmängd) höll övervägande god datakvalitet (baserat på SMHIs självuppskattning i kombination med att data ej fattades). Hos klassificerings data (typ av nederbörd) var samtliga mätningar av sämre kvalitet (SMHIs självskattning), vilket kan vara relaterat till att klassificering av olika typer av snö/regn inte är numeriskt och utan snarare subjektivt och därmed inte alls lika konkret mätbart som t.ex. temperatur eller tid.

Utifrån Data Analysen kunde följande relevanta data fält identifierades:

- __Datum__ & __Tid__ (för jämförelse av samma tidpunkt)
- __Lufttemperatur__ (Min och max = __"Lufttempertur__", "__Lufttempertur.1__")
- __Nederbörd__ (Typ av nederbörd)
- __Relativ Luftfuktighet__

Då modellen kommer bygga på det lablade data (nederbördstyp) är det detta tidsintervall samt steglängd (12h) som kommer att användas för samtliga dataset:

- *smhi_nederbördstyp_12h.csv* - sammanslaget med *smhi_nederbördstyp_12h_last4months.csv*
- *smhi_lufttemperatur_minMax_12h.csv* - kommer kompletteras med data från de senaste 3 månaderna
- *smhi_relativ_luftfuktighet_h.csv* - där min och max för en 12h period kommer att användas. Datasetet kommer även att kompletteras med data från de senaste 3 månaderna.

Övriga dataset i repot kommer __INTE__ att användas.

### 4. Identifiering av typ av problem

_Vilken typ av problem ska lösas?_

I detta fall:

- Regressions och klassificeringsproblem
- 1. Förutse numeriska värden för morgondagens väder
  2. Klassificera detta som snöfall eller ej utifrån dessa värden.
- Labelad data --> Supervised learning

---

## STEG 2

Under Steg 2 uppdaterades scopet för projektet. Istället för att enbart fokusera på en model som kunde förutse snöfall, lades fokus på en modell som kunde förutse all typ av nederbörd som förekom i SMHIs dataset. Detta på grund av obalans i datasetet (övervägande datapunkter för regn jämfört med snö) samt att redan identifierade "relevanta data fält" för snö-klassificering även ansågs relevanta för annan typ av nederbörd. Vidare lades fokus på att enbart lösa klassificerings delen av projektet då denna bedömdes som mest intressant.

 Steg 2 utfördes i `Del2_Datahandling.ipynb` och kan sammanfattas i följande steg:

##### **Data Pre-Processing/Cleaning:**
- Sammanslagning av dataframes av samma typ men för olika tidsperioder
  - Borttagning av dubbletter efter sammanslagning pga tidsöverlapp mellan datafilerna.
- Konvertering av datatyp: fältet 'Datum' konverterades till typen datetime för att möjliggöra datumbaserad analys
- Sammanslagning av data för **Nederbördstyp** (från nu refererat till som 'typ/type'), **Luftfuktighet** och **Lufttemperatur** ('temp') till en dataframe,  'Datum' och 'Tid (UTC)' användes som nyckel.

**Data Exploration**
- Plottning av den sammanslagna dataframen för att säkerställa att data inte innehöll outliners eller att data fattades.
- Pllottning av data för att identifiera potentiella kluster och korrelation/relation mellan data variabler.

##### **ML Training, Testing and Validation**
- Uppdelning av dataset i tränings och validerings data
- Träning, test och utvärdering av olika modeller

##### **Förbättringsåtgärder**
- Iterering, förbättringsåtgärder baserat på utfall av föregående steg

##### Future steps
- Förslag på framtid aktiviteter för att förbättra projketet som helhet samt framtagna modell
