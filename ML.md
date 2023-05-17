# ML

## Val av Algoritmer

Baserat på problemtypen Klassificering valdes nedan algoritmer:

1. __Algoritm 1: K-Nearest Neighbors (KNN) Classifier__

   - Huvudalgoritm: Eftersom det tvättade datasetet vare relativt litet (820 reader) och icke-linjärt problem av typ Klassificering ansågs KNN vara en lämplig algoritm.
2. __Algoritm 2: Random Forest__

   - Jämförelsealgoritm till algoritm 1. Även lämpad för klassificering av icke-linjära problem och hantera fall då data saknas.

### Resultat 1:

Efter första omgången fick ett Accuarcy score på ca 68% för bägge modeller (avrundat) samt liknande (men inte identiska) confusion matrix (CM). Utifrån CM kunde observerad att bägge modeller hade problem med att klassificera snarlika typer, e.g. olika typer av regn.



### Förbättring av resultat, resultat 2:

Försök att förbättra resultat genom ytterligare tvättning av data. Hypotes att aggregering av "regn" och "snö" typ skulle leda till färre "falska" resultat (e.g, att modellerna klassificerade det som regn när det egentligen var duggregn).

* Då KNN gav snarlikt resultat som Random Forest sparades bara en omträning på det uppdaterde datasetet på denna modell.
* Accuracy ökade från ca 70 % --> ca 90%, samma model, features, test_size, etc. Enbart uppdatering gjord på datanivå (tydligare kluster pga färre klasser)


### Övriga observationer:

Under testning av att tweaka `test_size` och `random_state`  kunde bl.a observeras att Random Forest i vissa fall presterade bättre än KNN (1-2%) för både mindre och större test size - även om ingen konsekvent trend kunde observeras för detta fenomen. Detta kan vara relateras till att datasetet var relativt litet och obalanserat - vissa klasser hade välidgt få datapunkter vilket ledde till att random state/test size blev olika bra fördelat för olika värden. Random forest är en passande algoritm för att hantera fall då data saknas, vilket kan ha gynnat den i vissa fall av "ofördelaktig fördeling av test/tränings data". Då bägge undersökta algoritmer gav snarlika resultat hade det varit intressant att jämföra ytterligare alternativ samt se hur samtliga påverkas av ett utökat och mer balanserat dataset.
