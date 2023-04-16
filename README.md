# data-project
Eget projekt i kursen "Pythonprogrammering för AI-utveckling" vid IT-Högskolan VT23 med syftet att använda maskininlärning/AI till att lösa ett på förhand
givet problem.

---

## STEG 1
Den första delen av projektet fokuserar på att undersöka förutsättningarna för det egna projektet, detta har gjorts i fyra delsteg: 

### 1. Problemställning 
_"Börja med att i text beskriva problemet och vad det är du vill åstadkomma."_ 

#### Snöfalls förutsägelse för Stockholm. 
Årligen lamslås Stockholms kollektivtafik av snöfall vilket medför negativa konsekvense för invånarna och viktigtiga sammhällsfuntioner (uttrykningsfordon, etc.). Idealt hade snöfalls förutsägelser kunnat användas för att bättre planera behov av jourhavande snöröjning.



### 2. Identifierinng av relevant data för att lösa problemet 
_"Om du inte kommer över lämpligt data får du tänka
om. Kan du göra en variant på ditt problem som kan lösas med det data du hittat? Om inte, definiera
ett nytt problem och försök igen.
Tänk på att datat kan komma från olika källor. Om du hämtar data från olika källor måste du
undersöka hur kompatibelt det är."_

Data som behövs:

- Historisk väderdata från Stockholm
    - Nederbörd (mm)
    - Dag (steglängd)
    - Temperatur?
        - Ta hänsyn till temperaturökning per år? / klimatförändringarna

- [Klimat- och väderstatistik från Stockholms Stad](https://miljobarometern.stockholm.se/klimat/klimat-och-vaderstatistik/)
    - [Årsnederbörd](https://miljobarometern.stockholm.se/klimat/klimat-och-vaderstatistik/arsnederbord/)
        - _Nederbörden registreras av SMHI i Observatorielunden i centrala Stockholm. Mätningarna omfattar all nederbörd, både regn och snö. Definitionen på nederbörd är att det uppmätts minst 0,1 mm på ett dygn. Statistik över dygnsvärden kan laddas hem från SMHI Öppna data från 1961, vilket denna indikator baseras på._
- [SMHI - Meteorologiska observationer](https://www.smhi.se/data/meteorologi/ladda-ner-meteorologiska-observationer/#param=airtemperatureInstant,stations=core,stationid=98210)

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

Regressionsproblem: förutse ett numeriskt värde (mängd nederbörd) baserat på historiskt data (nederbörd, dag). Om temperatur understiger 0 kan antagandet göras att nederbörden fås i form av snö. 

---
#### Bilagor:
- Om du använder dataset - beskriv det data du hittat. Hur många rader har du? Hur är kvaliteten på datat.
- Frivilligt: lämna in Jupyter Notebooks som visar hur du har undersökt ditt data (om du inte skall arbeta med Reinforcement
Learning).

Potentiella datakällor:

- SMHI
- Stockholms free data
