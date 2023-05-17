# ML

## Val av Algoritmer

Baserat på problemtypen __klassificering__ valdes nedan algoritmer:

1. __Algoritm 1: K-Nearest Neighbors (KNN) Classifier__

   - Huvudalgoritm: Eftersom det tvättade datasetet vare relativt litet (820 reader) och icke-linjärt problem av typ Klassificering ansågs KNN vara en lämplig algoritm.
2. __Algoritm 2: Random Forest__

   - Jämförelsealgoritm till algoritm 1. Även lämpad för klassificering av icke-linjära problem och hantera fall då data saknas.

### Resultat 1:

Efter första omgången ficks ett __accuarcy score__ på ca 68% för bägge modeller (avrundat) samt liknande (men inte identiska) confusion matrix (CM). Utifrån CM kunde observerad att bägge modeller hade problem med att klassificera snarlika typer, e.g. olika typer av regn.


### Förbättring av resultat, resultat 2:

Försök att förbättra resultat genom ytterligare tvättning av data. Hypotes att en aggregering av "regn" och "snö" typ skulle leda till färre "falska" resultat (e.g, att modellerna klassificerade det som regn när det egentligen var duggregn). 

Hagel och snöblandat regn delades upp i egna aggregeringar på följande grunder:
- `Hagel` p.g.a "outliner data" i förhållande till både snö och regn, få datapunkter. 
- `Sböblandat regn` p.g.a mellanting av snö och regn, relativt stor mängd datapunkter (i förhållande till snö), samt "tydligt" om än överlappande kluster med regn och snö. 

* Då KNN gav snarlikt resultat som Random Forest sparades bara en omträning av det uppdaterade datasetet för denna modell.
* Accuracy ökade från ca 70 % --> ca 90%, samma model, features, test_size, etc. Enbart uppdatering gjord på datanivå (tydligare kluster p.g.a färre (överlappande) klasser). Även en tydlig förbättring av Confusion Matrix resultat kunde observeras, men som förväntat hade moddellerna fortsatta problem med att korrekt klassificera snöblandat regn och hagel:

1. Mindre dataset (speciellt för hagel)
2. Naturligt överlapp mellan kluster (e.g. snöblandat regn = blandning av snö och regn till sin natur) 

Man kan fråga sig huruvida 90% är ett bra reultat med ett så obalanserat dataset (är modellan bara bra på att förutse regn?), men då en tydlig korrelation med luftfuktighet, temperatur och SNÖ/REGN kunde observeras kan slutsatsen dras att modellen med stor sannolikt är bra på att avgöra skillnaden på snö och regn utanför "snöblandat regn" intervallet kring 0 grader.  

### Övriga observationer:

Under testning av att tweaka `test_size` och `random_state` kunde bl.a observeras att Random Forest i vissa fall presterade bättre än KNN (1-2%) för både mindre och större test size - även om ingen konsekvent trend kunde observeras för detta fenomen. Detta kan vara relateras till att datasetet var relativt litet och obalanserat - vissa klasser hade välidgt få datapunkter vilket ledde till att random state/test size blev olika bra fördelat för olika värden. Random forest är en passande algoritm för att hantera fall då data saknas, vilket kan ha gynnat den i vissa fall av "ofördelaktig fördeling av test/tränings data". Då bägge undersökta algoritmer gav snarlika resultat hade det varit intressant att jämföra dessa mot resultatet från ytterligare klassificerings algoritmer samt se hur samtliga påverkas av ett utökat och mer balanserat dataset.


