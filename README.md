# data-project
Eget projekt i kursen "Pythonprogrammering för AI-utveckling" vid IT-Högskolan VT23 med syftet att använda maskininlärning/AI till att lösa ett på förhand
givet problem.

---

## STEG 1
Den första delen av projektet fokuserar på att undersöka förutsättningarna för det egna projektet, detta har gjorts i fyra delsteg: 

### 1. Problemställning 
_Beskrivning av problemet och mål._ 

#### Snöfalls förutsägelse för Stockholm. 
Årligen lamslås Stockholms kollektivtrafik av snöfall vilket medför negativa konsekvens för invånarna och viktiga samhällsfunktioner (utryckningsfordon, etc.). Idealt hade snöfalls förutsägelser kunnat användas för att bättre planera behov av jourhavande snöröjning.



### 2. Identifiering av relevant data för att lösa problemet 

#### Identifiera förutsättningar för snö, [källa](https://nsidc.org/learn/parts-cryosphere/snow/science-snow):

- För att det ska snöa måste det finnas fuktighet i atmosfären.
- Snö bildas när den atmosfäriska temperaturen är vid eller under fryspunkten (0°C) .
- Generell regel: snö bildas inte om marktemperaturen är minst 5°C. 
- För marktemperaturen vid eller under fryspunkten kan snö nå marken, under optimala förhållanden kan snö nå marken vid högre temperaturer.
- Det kan inte vara för kallt för att det ska snöa, men det kan vara för torrt.
- De flesta kraftiga snöfall inträffar när det finns relativt varm luft nära marken (typiskt -9°C eller varmare) eftersom varmare luft kan hålla mer vattenånga. 
- Väldigt kalla men torra förhållanden ger sällan snöfall. 

#### Identifiera nödvändig data transformation: snö till vatten (nederbörd) förhållande, [källa](https://nsidc.org/learn/parts-cryosphere/snow/science-snow):


Snö består av frusna vattenkristaller omgiven av luft vilket medför att luft utgör den störst volymen i ett snölager. Ett vanligt (men inte alltid korrekt) antagande är ett tio-till-ett-förhållande (10:1) mellan snö och vatten. Majoriteten av nysnöfall i USA innehåller ett vatten-till-snö-förhållande mellan 0,04 (4 procent) och 0,10 (10 procent), beroende på de meteorologiska förhållandena som är förknippade med snöfallet. 

I detta projekt kommer det generella 10:1 snö-vatten förhållandet att användas.


#### --> Data som behövs:

- Historisk väderdata från Stockholm innehållande:
    - Nederbörd (mm)
    - Datum
    - Temperatur (°C)
	    - Idealt: Atmosfärisk temperatur 
	    - Annars: Marktemperatur
    - Luftfuktighet 

Datakälla: [SMHI - Meteorologiska observationer](https://www.smhi.se/data/meteorologi/ladda-ner-meteorologiska-observationer/#param=airtemperatureInstant,stations=core,stationid=98210)

### 3. Inledande dataanalys
- Komplett dataset?
- Null-värden?
- Extremvärden?
- Förekommande datatyper?
- Relevanta datafält?
- Kan relevanta fält konverteras till ett numeriskt format?

### 4. Identifiering av typ av problem
Vilken typ av problem ska lösas? - Doukumentera 
- Klassificerings- eller regressionsproblem? 
- Labeled/unlabled data - behov av att själv skapa labels med hjälp av unsupervised learning (klassificeringsproblem)?
- Reinforcement Learning och deep neural networks? - typ av data, datastruktur, utformning av belönings/bestraffningssystem, tolkning av output från nätverket, träningsplattform för nätverk?

Regressionsproblem: förutse ett numeriskt värde (mängd nederbörd) baserat på historiskt data (datum, nederbörd, temperatur, luftfuktighet).

---
#### Bilagor:
- Om du använder dataset - beskriv det data du hittat. Hur många rader har du? Hur är kvaliteten på datat.
- Frivilligt: lämna in Jupyter Notebooks som visar hur du har undersökt ditt data (om du inte skall arbeta med Reinforcement
Learning).

Potentiella datakällor:

- SMHI
- Stockholms free data


