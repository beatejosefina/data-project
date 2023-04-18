# data-project

Eget projekt i kursen "Pythonprogrammering för AI-utveckling" vid IT-Högskolan VT23 med syftet att använda maskininlärning/AI till att lösa ett på förhand
givet problem.

---

## STEG 1

Den första delen av projektet fokuserar på att undersöka förutsättningarna för det egna projektet, detta har gjorts i fyra delsteg:

### 1. Problemställning

_Beskrivning av problemet och mål._

#### Snöfalls förutsägelse för Stockholm.

Årligen lamslås Stockholms kollektivtrafik av snöfall vilket medför negativa konsekvens för invånarna och viktiga samhällsfunktioner (utryckningsfordon, etc.). Idealt hade dagens väder kunnat användas för förutsäga morgondagens ev. snöfall så att kommunen bättre hade kunnat planera jourhavande snöröjning eller på annat sätt anpassa sin kollektivtrafik.

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

Baserat på ovanstående "litteraturstudie" forutset en modell behöva nedanstående typ av data.

- Historisk väderdata från centrala Stockholm innehållande:

  - Datum - för att kunna genomföra jämförele av samma dag ifall data hämtas från olika källor.
  - Temperatur (°C)
    - Idealt: Atmosfärisk temperatur
    - Annars: Marktemperatur
  - Luftfuktighet
  - Nederbörd (mm) - för attt kunna fastslå om det snöat eller ej (nederbörd > 0.0 mm = neberbörd). Teoretisk kommer denna utifrån Temperatur och Kuftfuktighets data kunnas klassas som snö eller regn. Baserat på vatten-till-snö förhållandet kommer även mängden snö (volym) att kunna uppskattas.

Identfierad potentiell datakälla: [SMHI - Meteorologiska observationer](https://www.smhi.se/data/meteorologi/ladda-ner-meteorologiska-observationer/#param=airtemperatureInstant,stations=core,stationid=98210)

Vald väderstation: Stockholm-Observatoriekullen pga dess centrala läge.

### 3. Inledande dataanalys

_Under den inledande dataanalysfasen undersöktes data utifrån följande kriterier:_

- _Komplett dataset?_
- _Förekommer null-värden?_
- _Förekommer Extremvärden?_
- _Förekommande datatyper?_
- _Relevanta datafält?_
- _Kan relevanta fält konverteras till ett numeriskt format?_

Från Data Analysen utförd i _DataExploration.ipynb kunde följand oobservationer göras:_

- I SMHIs Metetologiska databas återfanns historikst data för __Nerderbörd__, __Temperatur__ _(på mätstation, vilket antas vara marknivå)_ samt __Relativ luftfuktighet__. Dessa dataset kunnde fås med varierat tidsintervall (15 min, 1h, 12h , 1 dygn, osv.) för olika dataset. Dataseten med mät-tidsintervallet 1h valdes för närmare analys då denna var den enda steglängd som förekomm för samtliga utvalda dataset.
- Preview a filerna visade att .csv filerna inte enbart innehåll tabelldata utan även inledande information, vilket behövde städas bort i Excel innan de kunde läsas in med hjälp av pandas.
- Utöver de initialt identifierade dataseten återfanns även data på __Nederbördstyp__, detta dataset förekom dock enbart med en tidsperiod på 12h för tisperioden 2019-10-07 till 2023-04-14. Eftersom detta data var labeled behövdes inte längre nederbörd klassificeras/uppdkattas som snö utifrån den vetenskapliga "snöreceptet" ovan. Detta medförde att även att alla tillgängliga dataset med steglängden 12 h laddades ner (__Temperatur Min Max__) och undersöktes.
- Data analysen visade att alla numeriska dataset utöver två bortvalda dataset (Nederbördsmäng) höll övervägande god datakvalitet (baserat på SMHI's självuppskattning i kombination med att data ej fattades). Hos klassificerings datat (typ av nederbörd) var samtliga mätningar av sämre kvalitet (SMHIs självskattning), vilket kan vara relaterat till att klassifiering av olika typer av snö/regn inte är nummeriskt och utan snarare subjektivt och därmed inte alls lika konkret mätbart som t.ex. tempertur eller tid.
- Utifrån Data Analysen kunde följande relevanta data fält identiferades:

  - __Datum__ & __Tid__ (för jämförelse av samma tidpunkt)
  - __Lufttemperatur__ (Min och max = __"Lufttempertur__", "__Lufttempertur.1__")
  - __Nederbörd__ (Typ av nederbörd)
  - __Relativ Luftfuktighet__
- Då modellen kommer bygga på det lablade datat (nederbördstyp) är det detta tidsintervall samt steglängd (12h) som kommer att användas för samtliga dataset:

  - *smhi_nederbördstyp_12h.csv* - sammanslaget med *smhi_nederbördstyp_12h_last4months.csv*
  - *smhi_lufttemperatur_minMax_12h.csv* - kommer kompletteras med data från de senaste 3 månaderna
  - *smhi_relativ_luftfuktighet_h.csv* - där min och max för en 12h period kommer att användas. Datasetet kommer även att kompletteras med data från de senaste 3 månaderna.
  - Övriga dataset i repot kommer __INTE__ att användas.


### 4. Identifiering av typ av problem

_Vilken typ av problem ska lösas?_

I detta fall:

- Regressions och klassificierings problem
  - 1. Förutse nummeriska värden för morgondagens väder
    2. Klassificera detta som snöfall eller ej utifrån dessa värden.
- Labelad data --> Supervised learning

---
