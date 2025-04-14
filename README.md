# Kniffel2090

Kniffel auf dem Busch Microtronic 2090

Doku wird noch ergänzt... Wer´s trotzdem schon testen mag, nur zu! :)

Die Kniffelregeln zu erklären, spare ich mir. Wer´s nicht kennt, möge einschlägig nachsehen... 

## tl;dr

Du willst einfach nur spielen? Dann hier die Kurzfassung: 

- Der erste Wurf startet automatisch.
- Mit den Tasten 1-5 wählst du die Würfel, die zurück in den Becher sollen.
- Mit der Taste 0 startet du den nächsten Wurf.
- Nach dem dritten Wurf trägst du das Ergebnis in den Block ein:
  - Mit den Tasten C-F wird die aktive Blockseite gewechselt
  - Mit den Tasten 1-4 wird das Feld zum Eintragen ausgewählt.
- Wenn alle Felder auf dem Kniffelblock voll sind, wird das Gesamtergebnis angezeigt.

Nächstes Spiel mit Taste Reset (Speicherregister müssen auch gelöscht werden).



## Kniffelblock

Der Kniffelblock wird durch die LEDs an den Ausgängen und den jeweiligen Buchstaben im Display repräsentiert. 

```

Kniffelblock       LED an Ausgang        Blockseite

1er                      1                    C
2er                      2                    "
3er                      3                    "
4er                      4                    "

5er                      1                    D
6er                      2                    "
3-Pasch                  3                    "
4-Pasch                  4                    "

Full House               1                    E
Kl. Straße               2                    "
Gr. Straße               3                    "
Kniffel                  4                    "

Chance                   1                    F
```

Eine leuchtende LED bedeutet, das entsprechende Feld im Block ist noch frei. Wenn die entsprechende LED nicht mehr leuchtet, wurde die *Figur* schon eingetragen oder gestrichen.

Die aktive Blockseite kann jederzeit und beliebig mit den Tasten C, D, E und F gewechselt werden (also während der Würfel- oder Eintrag-Phase).

## Harte Regeln

Mit den Ziffern 1-5 werden die Würfel ausgewählt, die zurück in den Becher, also erneut geworfen werden sollen. Wenn ein Würfel in den Becher gelegt wird, wird seine Augenzahl im Display durch eine 0 ersetzt. 

Aber Vorsicht - einmal einen Würfel gewählt gibt´s kein Zurück. Für eine Undo-Funktion war einfach kein Platz - weder Programmschritte noch Register.

## Nächster Wurf

Der nächste Wurf wird mit der Ziffer 0 gestartet.

## Alea iacta est

Nach dem dritten Wurf wird das Ergebnis in den Block eingetragen. Mit den Tasten C-F kann die aktive Blockseite geändert werden. Leuchtende LEDs an den Ausgängen zeigen an, welche Felder auf der aktuellen Blockseite noch frei sind. Dazu ein Beispiel: 

Nach dem dritten Wurf zeigt das Display
```
2 3 4 5 6 C
```
An den Ausgängen leuchten die LEDs
```
1 2 3 4
x - x x 
```
Die aktive Blockseite ist C, also wird das erste Segment des Kniffelblocks (1er, 2er, 3er, 4er) durch die Ausgänge dargestellt. Durch die LEDs erfahren wir, dass die 2er schon eingetragen wurden, die anderen Felder sind offenbar noch frei. 

Wir wechseln die aktive Blockseite, weil wir ja eine *Große Straße* gewürfelt haben. Dazu drücken wir die Taste E. Dann zeigt das Display
```
2 3 4 5 6 E
```
und die LEDs an den Ausgänge zeigen
```
1 2 3 4
- x - x
```
Also sind *Full House* und *Große Straße* schon eingetragen (oder gestrichen), weil die LEDs an den Ausgängen 1 und 3 nicht mehr leuchten. Ausgänge 2 und 4 leuchten, also sind *Kleine Straße* und *Kniffel* noch frei.

Einen *Kniffel* haben wir nicht getroffen, eine *Große Straße* können wir nicht mehr eintragen, allerdings wissen Kniffel-Profis: eine *Große Straße* enthält immer auch eine *Kleine Straße*.  

Mit den Tasten 1-4 wird das Ergebnis des Wurfs auf der aktiven Blockseite eingetragen. Testhalber drücken wir die Taste 1 und stellen fest, dass das Display sich nicht verändert - denn das Feld für *Full House* ist im Block schon belegt, daher wird die Auswahl nicht akzeptiert.

Mit der Taste 2 wählen wir schließlich die *Kleine Straße* aus. Daraufhin geht die LED an Ausgang 2 aus, der Microtronic blinkt und rechnet ein bisschen herum, und anschließend wird ein neuer Wurf gestartet. 

Intern hat der Microtronic die 30 Punkte für eine *Kleine Straße* zur Gesamtpunktzahl addiert - allerdings nur, wenn die geforderte Figur auch gewürfelt wurde, ansonsten würde das Feld gestrichen (also eingetragen, aber mit null Punkten).

## Figur

Bei Kniffel kommt es darauf an, mit fünf Würfeln und drei Würfen "Figuren" zu erzielen. Die beliebteste Figur ist natürlich der Kniffel selbst, also fünfmal die gleiche Augenzahl auf allen Würfeln.

Der Microtronic soll nicht nur den Prozess des Würfelns unterstützen, sondern am Ende auch erkennen, ob und welche Figuren getroffen wurden, damit die Punkte dafür korrekt in den Block eingetragen werden.

Die getroffenen Figuren werden als Bitmuster in den Registern FIGUR1 und FIGUR2 gespeichert. 


Bitmuster bieten sich dafür gut an, weil eine getroffene Figur mehrere andere Figuren beinhalten kann. Wenn zum Beispiel ein Kniffel gewürfelt wurde, dann wurden gleichzeitig auch ein Drilling, ein Vierling und ein Full House getroffen, also müssen die Bits für diese Figuren ebenfalls gesetzt werden.

```
              Figur 1                         Figur 2
nicht verw.      -              Full House       x
nicht verw.      -              Kl. Straße       -  
Drilling         x              Gr. Straße       -
Vierling         x              Kniffel          x
```

Nicht zufällig entspricht die Reihenfolge der Bits in diesen Registern auch der Reihenfolge der Figuren auf dem Kniffelblock. 



# Besonderheiten

## Bubblesort reloaded

Nachdem ich im Vorgängerprojekt "Mensch ärgere dich nicht" zunächst erwartungsfroh einen Bubblesort-Algorithmus implementiert hatte, nur um dann festzustellen, dass die volle Sortier-Leistung gar nicht benötigt wird, musste es diesmal einfach eine echte Herausforderung sein. Also Kniffel.

Hier kann der Microtronic endlich die geballte Rechenpower seines 500 kHz-Elektronengehirns entfesseln, denn nach jedem Wurf liegen die Würfel garantiert vollständig unsortiert auf dem Tisch. Für alle weiteren Berechnungen und Auswertungen brauchen wir diesmal den echten Bubblesort. Fünf Würfel, vier Durchläufe. Plus ein Bonus-Durchlauf für allerlei Dies und Das. Juhuuu.

## Volles Haus?

Das hat mir lange Kopfzerbrechen bereitet. Ich habe eine ganze Weile mit zusätzlichen Registern und Zählern experimentiert, nur um die Übersicht über die verschiedenen Paschs (Päsche? Paschas? Paschanten?) zu behalten. Das wurde nix, oder es wurde was, war aber viel zu lang.

Im Prinzip ist ein Full House (FH) ja nichts anderes als ein Dreier mit Zusatzpasch. Dieser zusätzliche Pasch kann aber "vorne" oder "hinten" sein. Wenn die Würfel sortiert vorliegen, sind somit diese beiden Varianten möglich:

```
PP DDD     (P - Pasch, D - Dreier)
DDD PP
```

Nehmen wir nun an, wir wüssten bereits, dass im Wurf ein Dreier vorliegt. Wie erkennen wir jetzt am einfachsten, ob auch ein FH vorliegt?

Letztlich dann doch trivial. Der erste und zweite Würfel sowie der vierte und fünfte Würfel müssen gleich sein - dann ist der gesamte Wurf ein FH.

```
ADDI #4,FIGUR1       Dreier wurde erkannt, Bit setzen
CMP W2,W1            W2 kleiner als W1?
BRC Eintrag          dann kein Pasch "vorne", kein FH
CMP W5,W4            W5 kleiner als W4?
BRC Eintrag          dann kein Pasch "hinten", kein FH
ADDI #1,FIGUR2       Full House erkannt, Bit setzen
```

Muss man aber auch erstmal drauf kommen... 😂

## Voller Block? 

Der Kniffelblock besteht aus vier Segmenten (Seiten), die jeweils ein (Speicher-)Register belegen. Im Laufe eines Spiels werden die einzelnen Felder (Bits) jeder Blockseite nach und nach "ausgeknippst". Dadurch wird protokolliert, ob in das jeweilige Feld des Kniffelblocks schon etwas eingetragen wurde.

Wenn in alle Felder einer Blockseite ein Wert eingetragen wurde, dann sind alle Bits dieses Registers nicht mehr gesetzt, also ist der Wert im Register dann 0. Sind alle Blockseiten gefüllt, dann haben alle Register den Wert 0. 

Wie testen wir nach jedem Wurf/Eintrag, ob das Spiel zuende ist?

```
OR BLOCK1,Z1
OR BLOCK2,Z1
OR BLOCK3,Z1
OR BLOCK4,Z1
BRZ Schluss
GOTO Beginn
```

Wir nehmen ein leeres Register (Z1) und verodern dieses Register nacheinander mit allen vier Block-Registern. Wenn am Ende dieser Operation immer noch eine Null im Testregister Z1 steht, wissen wir, dass auch alle Block-Register leer sind (also kein Bit unsere schöne Anfangs-Null verschmutzt hat). Das bedeutet dann, dass alle Felder im Kniffel-Block ausgefüllt sind, also die Schlussabrechnung erfolgen kann.


## Voller Bonus?

Wenn im oberen Block 63 Punkte oder mehr erzielt werden, erhält man 35 Punkte als Bonus. Wie kann man diese Prüfung mit möglichst wenig Programmschritten durchführen?

Die Summe der oben erzielten Punkte ist hexadezimal in den Speicherregistern 1 und 2 abgelegt. Wenn dort 3F hex oder mehr steht, dann soll der Bonus zur Gesamtsumme addiert werden.

Die intuitive, aber weniger ökonomische Lösung wäre: Zunächst die 10er-Stelle prüfen (>=3?), und danach - falls die 10er-Stelle genau 3 ist - auch die 1er-Stelle prüfen (=F?). Unterm Strich viele Vergleiche und bedingte Sprünge für beide Register, bis der Microtronic entscheiden kann, ob er die 35 Zusatz-Pöngs ausspucken soll oder nicht.

Da die oberen Punkte bereits zur Gesamtpunktzahl addiert worden sind, können wir mit diesen Registern ein bisschen spielen, ohne was kaputt zu machen. Wir wollen am Ende erreichen, dass nur ein Register geprüft werden muss.

```
ADDI #1,OBEN1
ADC OBEN2
SUBI #4,OBEN2
BRC ZeigeGesamt
MOVI #2,SUMME2
MOVI #3,SUMME1
CALL AddiereAugen

```

Also addieren wir zum Wert im 1er-Register einfach eine elegante 1. Einen Übertrag, sofern vorhanden, addieren wir zum 10er-Register. Für den Schwellenwert 3F ergäbe sich dann hexadezimal 3F + 1 = 40.

Anschließend muss nur noch geprüft werden, ob die 10er-Stelle einen Wert größer als 3 enthält. Falls ja, gibt's den Bonus, falls nein, Pech gehabt. 

Diese Prüfung erledigen wir durch Subtraktion des Wertes 4. Wenn dabei ein Unterlauf entsteht, also weniger als 4 im 10er-Register war, wird das Carry-Flag gesetzt und ohne Bonus zur Anzeige der Gesamtsumme gesprungen. Ansonsten wird der Bonus gegeben (unter Verwendung des bereits vorhandenen Additions-Unterprogramms).

Goldkörnchen zum Mitschreiben: Alle Schwellenwerte, die sich über mehrere Register erstrecken, können auf diese Weise einstellig (*"ein-registerig"*) überprüfbar gemacht werden. Wir addieren genau einen solchen Wert, mit dem das jeweils höchstwertige Register kippen würde. Dann muss nur noch dieses Register überprüft werden.




## Bedingtes Unterprogramm

Keine Ahnung, ob es sowas gibt. Falls nicht, hab ich's gerade erfunden...

Bestimmte Programmabschnitte lösen ähnliche Aufgaben, dann ist es sinnvoll, zu überlegen, ob diese Abschnitte in ein Unterprogramm ausgelagert werden können. Beim Microtronic stellen sich dann allerdings zwei potentielle Probleme: 

- Unterprogramme dürfen keine weiteren Unterprogramme aufrufen
- ein RETURN ohne vorheriges CALL ist unzulässig

Im Anleitungsbuch 1. Teil, S. 74 heißt es dazu: **Wichtig: In einem Unterprogramm darf kein CALL-Befehl vorhanden sein, weil sonst eine "Endlosschleife" entsteht.**

Dieser Warnhinweis schien mir ausreichend bedrohlich - wer will schon gefangen sein in einer *Endlosschleife*?! 😱 Womöglich stürzte ich das Universum ins Nichts nur durch einen unbedachten CALL? Zu viel Verantwortung für die schmalen Schultern eines jugendlichen Programmierschülers. 

Damals weder angezweifelt noch je wissentlich übertreten, sehe ich diese Gebote heutzutage - nach vielen undeklarierten Variablen, verbogenen Pointern und übergelaufenen Arrays - einen Tick gelassener...

Und hab's einfach mal riskiert. What can possibly go wrong? 

...



