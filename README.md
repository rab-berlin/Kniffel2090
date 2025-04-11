# Kniffel2090

Kniffel auf dem Busch Microtronic 2090

Doku wird noch ergänzt... Wer´s trotzdem schon testen mag, nur zu! :)

Die Kniffelregeln zu erklären, spare ich mir. Wer´s nicht kennt, möge einschlägig nachsehen... 

## Bubblesort reloaded

Nachdem ich im Vorgängerprojekt "Mensch ärgere dich nicht" zunächst erwartungsfroh einen Bubblesort-Algorithmus implementiert hatte, nur um dann festzustellen, dass die volle Sortier-Leistung gar nicht benötigt wird, musste es diesmal einfach eine echte Herausforderung sein. Also Kniffel.

Nur hier kann der Microtronic die geballte Rechenpower seines 500 kHz-Elektronengehirns endlich entfesseln, denn nach jedem Wurf liegen die Würfel garantiert vollständig unsortiert auf dem Tisch. Für alle weiteren Berechnungen und Auswertungen brauchen wir diesmal den echten Bubblesort. Fünf Würfel, vier Durchläufe. Plus ein Bonus-Durchlauf für allerlei Dies und Das. Juhuuu.


## Kniffelblock

Der Kniffelblock wird durch die LEDs an den Ausgängen und den jeweiligen Buchstaben im Display repräsentiert. 

```
  Aktive
Blockseite          Kniffelblock

    C               1er, 2er, 3er, 4er
    D               5er, 6er, 3-Pasch, 4-Pasch
    E               Full House, kl. Str., gr. Str., Kniffel
    F               Chance
```

Eine leuchtende LED bedeutet, das entsprechende Feld im Block ist noch frei. Wenn die entsprechende LED nicht mehr leuchtet, wurde die *Figur* schon eingetragen oder gestrichen.

Die aktive Blockseite kann jederzeit mit den Tasten C, D, E und F gewechselt werden (also während der Würfel- oder Eintrag-Phase).

## Harte Regeln

Mit den Ziffern 1-5 werden die Würfel ausgewählt, die zurück in den Becher, also erneut geworfen werden sollen. Wenn ein Würfel in den Becher gelegt wird, wird seine Augenzahl im Display durch eine 0 ersetzt. 

Aber Vorsicht - einmal einen Würfel gewählt gibt´s kein Zurück. Für eine Undo-Funktion war einfach kein Platz - weder Programmschritte noch Register.

## Nächster Wurf

Der nächste Wurf wird mit der Ziffer 0 gestartet.

## Alea iacta est

Nach dem dritten Wurf wird das Ergebnis in den Block eingetragen. Mit den Tasten C-F kann die aktive Blockseite beliebig oft gewechselt werden. Leuchtende LEDs an den Ausgängen zeigen an, welche Felder auf der aktuellen Blockseite noch frei sind. Dazu ein Beispiel: 

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

Einen *Kniffel* haben wir nicht getroffen, eine *Große Straße* können wir nicht mehr eintragen, allerdings ist eine *Große Straße* gleichzeitig auch immer eine *Kleine Straße*.  

Mit den Tasten 1-4 wird das Ergebnis des Wurfs auf der aktiven Blockseite eingetragen. Testhalber drücken wir die Taste 1 und stellen fest, dass das Display sich nicht verändert - denn das Feld für *Full House* ist im Block schon belegt, daher wird die Auswahl nicht akzeptiert.



# Besonderheiten

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


## Voller Bonus?

Wenn im oberen Block 63 Punkte oder mehr erzielt werden, erhält man 35 Punkte als Bonus. Wie kann man diese Prüfung mit möglichst wenig Programmschritten durchführen?

Die Summe der oben erzielten Punkte ist hexadezimal in den Speicherregistern 1 und 2 abgelegt. Wenn dort 3F hex oder mehr steht, dann soll der Bonus zur Gesamtsumme addiert werden.

Die intuitive, aber weniger ökonomische Lösung wäre: Zunächst die 10er-Stelle prüfen (>=3?), und danach - falls die 10er-Stelle genau 3 ist - auch die 1er-Stelle prüfen (=F?). Unterm Strich viele Vergleiche und bedingte Sprünge für beide Register, bis der Microtronic entscheiden kann, ob er die 35 Zusatz-Pöngs ausspucken soll oder nicht.

Da die Punkte oben bereits in der Gesamtpunktzahl enthalten sind, können wir mit diesen Registern ein bisschen spielen, ohne was kaputt zu machen. Wir wollen am Ende erreichen, dass nur ein Register geprüft werden muss.

```
ADDI #1,OBEN1
ADC OBEN2
SUBI #4,OBEN2
BRC ZeigeGesamt
MOVI #2,SUMME2
MOVI #3,SUMME1
CALL AddiereAugen

```

Also addieren wir zum Wert im 1er-Register einfach eine elegante 1. Einen Übertrag, sofern vorhanden, addieren wir zum 10er-Register. Für den Schwellenwert ergäbe sich dann hexadezimal 3F + 1 = 40.

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



