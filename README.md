# Kniffel2090

Kniffel auf dem Busch Microtronic 2090

Die Kniffelregeln zu erklären, spare ich mir. Wer´s nicht kennt, möge einschlägig nachsehen... 

## tl;dr

Du willst einfach nur spielen? Dann hier die Kurzfassung: 

- Der erste Wurf startet automatisch.
- Mit den **Tasten 1-5** wählst du die Würfel, die **zurück in den Becher** sollen.
- Mit der **Taste 0** startet du den **nächsten Wurf**.
- Nach jedem Wurf kannst du, spätestens aber nach dem dritten Wurf musst du das Ergebnis in den Block eintragen:
  - Mit den **Tasten C-F** kann die **aktive Blockseite gewechselt** werden.
  - Mit den **Tasten 8-B** wird das **Feld zum Eintragen** ausgewählt.
- Wenn alle Felder auf dem Kniffelblock voll sind, zeigt der Microtronic die Gesamtpunktzahl.

Das nächste Spiel startest du mit der Reset-Taste (denn die Speicherregister müssen auch gelöscht werden).

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

Mit den Tasten 8-B wird das Ergebnis des Wurfs auf der aktiven Blockseite eingetragen. Testhalber drücken wir die Taste 8 und stellen fest, dass das Display sich nicht verändert - denn das Feld für *Full House* ist im Block schon belegt, daher wird die Auswahl nicht akzeptiert.

Mit der Taste 9 wählen wir schließlich die *Kleine Straße* aus. Daraufhin geht die LED an Ausgang 2 aus, der Microtronic blinkt und rechnet ein bisschen herum, und anschließend wird ein neuer Wurf gestartet. 

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

Wenn mehrere Programmabschnitte ähnliche Aufgaben lösen, liegt ein Unterprogramm nahe. Beim Microtronic gibt es dabei aber zwei Haken:

- Ein Unterprogramm darf kein weiteres Unterprogramm aufrufen.
- Ein `RET` ohne vorheriges `CALL` ist unzulässig.

Im Anleitungsbuch 1. Teil, S. 74 heißt es dazu: **Wichtig: In einem Unterprogramm darf kein CALL-Befehl vorhanden sein, weil sonst eine "Endlosschleife" entsteht.**

Dieser Warnhinweis schien mir ausreichend bedrohlich – wer will schon gefangen sein in einer *Endlosschleife*?! 😱 Womöglich stürzte ich das Universum ins Nichts nur durch einen unbedachten CALL? Zu viel Verantwortung für die schmalen Schultern eines jugendlichen Programmierschülers.

Damals weder angezweifelt noch je wissentlich übertreten, sehe ich diese Gebote heutzutage – nach vielen undeklarierten Variablen, verbogenen Pointern und übergelaufenen Arrays – einen Tick gelassener. Der Grund ist ganz einfach: Der Microtronic merkt sich genau **eine** Rücksprungadresse. Ein zweites `CALL` überschreibt sie, und das äußere `RET` springt danach zurück ins innere Unterprogramm statt zum Aufrufer.

### Augen zählen

Für die obere Hälfte des Blocks braucht der Microtronic die Augensumme einer bestimmten Zahl, also alle Einser, alle Zweier und so weiter. Für Dreierpasch, Viererpasch und Chance braucht er die Summe aller Augen.

Eine Schleife über die fünf Würfel gibt es dafür nicht. Programm- und Registerspeicher sind getrennt, und ein Register lässt sich nur über seine feste Nummer im Befehl ansprechen. Es gibt keinen Index, der von Würfel zu Würfel weiterzählt, also kein „Array“. Bleiben zwei teure Wege:

- jeden Würfel einzeln hart codiert prüfen und addieren, das kostet viele `ADD`- und Vergleichsbefehle,
- oder jeden Würfel nacheinander in ein festes Register schieben und immer dieses eine Register auswerten, das kostet viele `MOV`-Befehle.

### Die Lösung: Sortieren rechnet mit

Genau dieses Schieben passiert aber ohnehin schon: Der Bubblesort holt jeden Würfel nacheinander in die Vergleichsregister WL und WH. Also rechnet die Sortierroutine gleich mit. Jeder Würfel, der durch WL läuft, wird zur Summe addiert:

- `EINGABE` = 0: alle Augen zählen (Gesamtsumme für Pasch und Chance).
- `EINGABE` = 1 bis 6: nur die Würfel mit genau dieser Augenzahl zählen.

```
TauscheNicht   CMPI #0,EINGABE      alle Augen zählen?
               BRZ Dazu
               CMP WL,EINGABE       oder nur diese Augenzahl?
               BRZ Dazu
               GOTO NichtDazu
Dazu           ADD WL,SUMME1        Summe bilden
               ADC SUMME2
NichtDazu      ...
```

Die Sortierroutine ruft selbst das Unterprogramm `Sort_WH_WL` auf. Ein `CALL Sortiere` ist deshalb verboten, siehe oben. Also wird sie für die Augensumme mit `GOTO` angesprungen. Für den „Rücksprung“ sorgt eine Bedingung am Ende des ersten Durchlaufs:

```
Return?        CMPI #0,EINGABE      Augensumme 1-6 gefragt?
               BRC PlusOben         ja: "Rücksprung" zum Eintragen
               SUBI #F,Z2           nein: normal weitersortieren
               BRC Sortiere
```

Steht in `EINGABE` eine Zahl von 1 bis 6, verlässt das Programm die Sortierroutine schon nach dem ersten Durchlauf mit der fertigen Summe. Die Würfel sind zu diesem Zeitpunkt längst sortiert. Steht dort eine 0, läuft der normale Ablauf nach dem Wurf weiter: alle fünf Durchläufe, Figuren erkennen, Gesamtsumme bilden.

Das ist ein Unterprogramm ohne `CALL` und ohne `RET`, mit einem Rücksprung, der vom Inhalt eines Registers abhängt. Ein **bedingtes Unterprogramm** eben. Das Universum ist außer Gefahr.

## Das Problem mit dem Glück

> Das Glück ist eine leichte Dirne,\
> Und weilt nicht gern am selben Ort;\
> Sie streicht das Haar dir von der Stirne\
> Und küßt dich rasch und flattert fort.
>
> Frau Unglück hat im Gegentheile\
> Dich liebefest an’s Herz gedrückt;\
> Sie sagt, sie habe keine Eile,\
> Setzt sich zu dir an’s Bett und strickt.
>
> — *Heinrich Heine*

Im Emulator mit echtem Zufall lief alles prima. Auf dem echten Microtronic dagegen spuckte der Würfelbecher bereits im ersten Wurf großzügig Kniffel, Vierlinge und Drillinge aus, aber nie eine Straße. Der Grund: `RND` ist auf dem 2090 gar kein echter Zufallsgenerator, sondern ein Zähler. 

Höchste Zeit, den Mechanismus dieses "Zufallszählers" mal genauer unter die Lupe zu nehmen. Durch Testprogramme und Versuche hab ich schließlich herausgefunden:

- Jeder ausgeführte Befehl erhöht den Zähler um +3 (wenige Ausnahmen).
- Solange der Microtronic auf eine Taste wartet (`KIN`), zählt er mit etwa 205 Schritten pro Sekunde weiter (gemessen).
- `RESET` setzt ihn zurück auf 000.
-  Einige Befehle benutzen ihn als Zwischenspeicher und setzen ihn auf einen festen Wert, zum Beispiel `HXDZ`.
-  Der Display-Modus (`DISP`, `DISOUT`) hat keinen Einfluss auf den Zähler.

Zufällig ist also nur der Zeitpunkt des Tastendrucks. Alles danach lässt sich vorausberechnen.

### Was in der alten Version passierte

Die alte Würfelroutine:

```
Würfel   CMPI #0,AUGEN
         BRC Zurück
Zufall   RND
         CMPI #5,AUGEN        nur Register D wird geprüft
         BRC Zufall           3 Befehle je Runde = +9
         ADDI #1,AUGEN
Zurück   RET
```

Die Routine schaut nur auf Register D, also die letzte Stelle des Zählers (0 bis F). Ist D 0 bis 5, wird D + 1 der Würfel, sonst liest sie den Zähler neu ab. Weil der Zähler stur weiterzählt, gelten für D zwei feste Regeln:

- **Neuer Versuch:** 3 Befehle, also +9 (nach F geht es bei 0 weiter).
- **Nächster Würfel:** Zwischen dem gültigen `RND` und dem `RND` für den nächsten Würfel liegen 10 Befehle, also +30. Für D heißt das: −2.

Schon ein einzelner Würfel ist dabei nicht gerecht, selbst wenn D beim ersten `RND` völlig zufällig ist. Ein ungültiger Wert bringt keinen neuen Zufall, denn der nächste Versuch liegt immer genau 9 höher. D läuft also immer dieselbe Runde ab:

```
0 → 9 → 2 → B → 4 → D → 6 → F → 8 → 1 → A → 3 → C → 5 → E → 7 → (0)
```

Gültig sind nur die Werte 0 bis 5. Alle ungültigen Werte folgen dieser immer gleichen Abfolge und enden schließlich in einem gültigen Wert:

| Augen | D | erreicht von Startwert D | Wahrscheinlichkeit |
|---|---|---|---|
| 1 | 0 | 0, 7, E | 3/16 = 19 % |
| 2 | 1 | 1, 6, 8, D, F | 5/16 = 31 % |
| 3 | 2 | 2, 9 | 2/16 = 12,5 % |
| 4 | 3 | 3, A | 2/16 = 12,5 % |
| 5 | 4 | 4, B | 2/16 = 12,5 % |
| 6 | 5 | 5, C | 2/16 = 12,5 % |

Die 2 kommt zweieinhalbmal so oft wie die 6. Wo bleibt da die Gerechtigkeit, fragt man sich.

Im Emulator mit echtem Zufall ist dieselbe Routine gerecht, weil dort jeder neue Versuch einen frischen Wert liefert. Ein anderer Schleifenabstand hilft auf dem Gerät nicht: 16 Werte lassen sich nicht in sechs gleiche Teile teilen.

#### Es kommt noch schlimmer

Ein einzelner Würfel ist also schon nicht gerecht. Beim ganzen Wurf kommt es aber noch schlimmer, weil die fünf Würfel nicht unabhängig voneinander sind. Der nächste Würfel startet nicht bei einem neuen, zufälligen Zählerstand, sondern immer genau 10 Befehle nach dem letzten gültigen Wert, also bei D − 2. Damit legt der erste Würfel alle weiteren fest.

Zufällig ist nur D beim allerersten `RND` des Wurfs. Ein Beispiel mit D = 4 (gewürfelt wird von W5 nach W1):

| Würfel | D | Ergebnis |
|---|---|---|
| W5 | 4 | 5 |
| W4 | 4 − 2 = 2 | 3 |
| W3 | 2 − 2 = 0 | 1 |
| W2 | 0 − 2 = E, +9 → 7, +9 → 0 | 1 |
| W1 | wieder E → 7 → 0 | 1 |

Angezeigt wird 1 1 1 3 5. Sobald eine 1 oder 2 fällt, dreht sich D im Kreis: Nach einer 1 landet D immer auf E, die Schleife läuft E → 7 → 0, und das ist wieder eine 1. Nach einer 2 läuft sie F → 8 → 1, und das ist wieder eine 2. Aus den 16 möglichen Startwerten werden so genau 6 Würfe:

| D beim 1. RND | Wurf |
|---|---|
| 0, 7, E | 1 1 1 1 1 |
| 1, 6, 8, D, F | 2 2 2 2 2 |
| 2, 9 | 1 1 1 1 3 |
| 3, A | 2 2 2 2 4 |
| 4, B | 1 1 1 3 5 |
| 5, C | 2 2 2 4 6 |

Die Hälfte aller Würfe war ein Kniffel. Eine Straße war unmöglich.

Hilft ein anderer Abstand zwischen den Würfeln? Nein. Wenn ein Wert akzeptiert wird, steht in D immer genau sein Wert minus 1, egal wo der Zähler vorher stand. Der nächste Würfel hängt also nur vom vorigen ab, und der erste Würfel legt den ganzen Wurf fest. Sechs mögliche erste Würfel ergeben sechs mögliche Würfe. Ein anderer Abstand ändert nur, *welche* sechs: Bei 11 Befehlen wäre jeder Wurf eine große Straße, bei 13 jeder ein Kniffel. Helfen kann nur etwas, das beim Akzeptieren nicht verloren geht und von Würfel zu Würfel weitergetragen wird.

Dazu kam ein zweites Problem: `Sortiere` löscht D, E und F mit `MOVI #F,GLEICH1` und `HXDZ`. Ich fand das elegant und nützlich, weil es einen Befehl spart (Wert über 3E7, und schon wird alles 0), aber `HXDZ` setzt dabei auch den "Zufallszähler" auf einen festen Wert - weil der TMS1600 sich die Freiheit nimmt, die internen Register des Zählers für seine Konvertierungsberechnungen zu benutzen. 

Nach dem Sortieren eines Wurfs schadet das nicht: Danach wartet `Auswahl` auf eine Taste, und die Wartezeit macht den Zählerstand wieder zufällig. Anders bei einem Eintrag oben (1er bis 6er). Für die Augensumme springt das Programm mit `GOTO Sortiere` zurück ins bedingte Unterprogramm und damit direkt in das `HXDZ`. Danach geht es ohne Tastendruck über `PlusOben`, `WurfEnde` und `Beginn` bis zum nächsten Wurf, der damit schon feststand. Die anderen Felder (Pasch, Full House, Straßen, Kniffel, Chance) brauchen `Sortiere` nicht noch einmal, weil die Gesamtsumme schon vorliegt. Sie sind deshalb nicht betroffen.

### Die neue Würfelroutine

```
Würfel   CMPI #0,AUGEN        Würfel im Becher?
         BRC Zurück
         RND                  D, E, F = Zählerstand
         ADD rE,WL            mittleres und oberes Nibble in den Akku WL
         ADD rF,WL            (WL wird erst beim Sortieren gebraucht)
Zufall   ADDI #D,WL           x = 17 (x + D), x = AUGEN:WL (8 Bit)
         ADC AUGEN
         ADD WL,AUGEN
         CMPI #5,AUGEN        0-5 gültig, sonst nächster Schritt
         BRC Zufall
         ADDI #1,AUGEN        Augen 1-6
Zurück   RET
```

- Alle drei Register des Zählers fließen ein, nicht nur das unterste.
- Ungültige Werte laufen nicht mehr über den Zähler, sondern über einen kleinen Zufallsgenerator.
- WL sammelt über alle fünf Würfel eines Wurfs. Dadurch hängen die Würfel kaum noch voneinander ab.

**Der Generator.** AUGEN und WL bilden zusammen eine 8-Bit-Zahl x: AUGEN ist die obere Stelle, WL die untere (x = 16 · AUGEN + WL). Jeder Schritt rechnet x' = 17 · (x + D), modulo 256.

Warum ausgerechnet 17? Weil es auf einem 4-Bit-Rechner fast nichts kostet: 17 · x = x + 16 · x, und mal 16 heißt nur „eine Stelle nach links“. Bei 8 Bit bedeutet das: die untere Stelle auf die obere addieren, also genau ein `ADD WL,AUGEN`. Jeder andere Multiplikator bräuchte Schiebebefehle über beide Stellen samt Überträgen.

Gut ist die 17 auch: Ein Generator der Form x' = a · x + c läuft nur dann durch alle 256 Werte, bevor er sich wiederholt, wenn c ungerade ist und a − 1 durch 4 teilbar. Mit a = 17 und c = 17 · D = DD hex ist beides erfüllt. Ohne diese volle Runde gäbe es kleine Kreise, aus denen manche Startwerte nie zu jeder Augenzahl fänden.

**Die Verteilung.** Die Routine akzeptiert einen Wert, wenn AUGEN 0 bis 5 ist. Ist er ungültig, rechnet der Generator so lange weiter, bis ein gültiger Wert kommt. Jeder der 256 möglichen Startwerte landet dabei bei genau einer Augenzahl. Die 256 Startwerte werden also in sechs Klassen aufgeteilt (eine Partition), und die Klassen sind fast gleich groß:

| | Klassengrößen (Augen 1 bis 6) | von |
|---|---|---|
| alte Routine | 3, 5, 2, 2, 2, 2 | 16 |
| neue Routine | 40, 43, 41, 45, 43, 44 | 256 |
| bestmöglich | 43, 43, 43, 43, 42, 42 | 256 |

Ein einzelner Würfel zeigt damit jede Augenzahl mit 15,6 % bis 17,6 % statt 16,7 %. Ganz gleich geht es nicht, weil 256 nicht durch 6 teilbar ist.

Außerdem:

- `Sortiere` löscht D, E und F jetzt mit drei `MOVI #0` statt mit `HXDZ`. Der Zähler läuft ungestört weiter, und der Zeitpunkt jedes Tastendrucks zählt mit, auch beim Zurücklegen der Würfel und beim Blättern im Block.

### Wie gut ist der neue Zufall?

Häufigkeiten bei Würfen mit allen 5 Würfeln im Becher (hier durfte mein bester Freund Claude helfen):

| | alt | neu | fair |
|---|---|---|---|
| verschiedene Würfe | 6 | rund 2000 | 7776 |
| Kniffel | 53 % | 0,09 % | 0,08 % |
| Vierling | 75 % | 2,2 % | 2,0 % |
| Drilling | 100 % | 21,0 % | 21,3 % |
| Full House | 50 % | 3,9 % | 3,9 % |
| Große Straße | 0 % | 3,3 % | 3,1 % |
| Kleine Straße | 0 % | 15,6 % | 15,4 % |

Was bleibt:

- Die Augenzahlen sind nicht ganz gleich wahrscheinlich, sondern liegen je nach Spielsituation zwischen etwa 15 % und 18 % statt bei 16,7 %. Ganz gleichmäßig geht es mit 8 Bit grundsätzlich nicht, weil 256 nicht durch 6 teilbar ist.
- Mehr als 4096 verschiedene Würfe sind nicht drin, denn das ist alles, was der "Zufallszähler" hergibt.
- Ein Wurf dauert etwas länger, im Mittel gut 2 Sekunden, selten bis zu 4,5 Sekunden. Die Würfel rollen also ein bisschen länger über den Tisch. Unendliche Spannung!


# Was fehlt oder anders ist...

## Bonuspunkte für weitere Kniffel

Laut der Spielanleitung von Schmidt Spiele bringt jeder zweite und weitere Kniffel 50 Zusatzpunkte. Sie werden auf der Rückseite des Blocks notiert. Beim amerikanischen Yahtzee sind es sogar 100 Punkte.

Der Microtronic kennt diese Regel nicht: Ein weiterer Kniffel bringt nur die Punkte des Feldes, in das er eingetragen wird.

## Kniffel als Joker

Die offiziellen Regeln sagen: Zusätzlich zu den 50 Punkten darf ein weiterer Kniffel als Joker in ein beliebiges freies Feld, und zwar mit der Höchstpunktzahl dieses Feldes:

- **oben:** in jedes freie Zahlenfeld. Fünf Vieren ergeben zum Beispiel 25 Punkte im Fünfer-Feld.
- **unten:** in jedes freie Feld. Es gibt zum Beispiel 30 Punkte für die Kleine Straße oder 40 Punkte für die Große Straße.

Ältere Ausgaben waren strenger. Dort musste ein weiterer Kniffel zuerst ins passende Zahlenfeld oben, dann gab es auch die 50 Zusatzpunkte. Als Joker unten war er erst erlaubt, wenn sowohl das Kniffelfeld als auch dieses Zahlenfeld schon belegt waren. War auch unten nichts mehr frei, wurde oben ein Feld gestrichen.

So haben wir das aber **niemals nie nicht** gespielt. Wenn man einen weiteren Kniffel gewürfelt hatte, wurde der dort eingetragen, wo es noch reinpasste. Wenn er nicht mehr unterzubringen war, griff die berühmte _"Schade, schade, leider Pech gehabt"-Regel_.

Genau so rechnet auch Kniffel2090:

- Ein Kniffel gilt zugleich als Dreierpasch, Viererpasch und Full House. Dort zählt er seine Augensumme beziehungsweise 25 Punkte.
- Oben zählt er nur im Feld seiner eigenen Augenzahl.
- Für die Straßen taugt er nicht. Wer ihn dort einträgt, streicht das Feld.

## Kniffel ist (auch) ein Full House!

In meiner Einfalt nahm ich an, darüber müsse gar nicht diskutiert werden. 

Es liegt ja auf der Hand: 5 Gleiche sind - mathematisch - auch 3 Gleiche und 2 Gleiche.

Allerdings musste ich bei meinen Recherchen feststellen, dass es eine Fraktion gibt, die diese unumstößliche Tatsache einfach gänzlich anders sieht. Mit denen würde ich prinzipiell nicht mehr spielen. Vielleicht gibt es auch einen besonderen Platz in der Hölle für "Kniffel ist Full House"-Leugner. Zu meiner Zerknirschung habe ich aber auch erfahren müssen, dass das offizielle Regelwerk diesen Häretikern recht gibt. Tatsächlich muss ein Full House aus zwei unterschiedlichen Paschs bestehen. 

Ich weigere mich aber, das zu programmieren, weil sich jede Faser in mir dagegen sträubt. Trotzdem ein Hinweis zur Güte - Wer Kniffelse und Full Häuser getrennt bewertet wissen will, möge an dieser Stelle eine kleine Anpassung vornehmen:

```
Kniffel        ADDI #8,FIGUR2
Vierer         MOVI #8,FIGUR1
Dreier         ADDI #4,FIGUR1
               CMP W2,W1
               BRC Auswahl
               CMP W5,W4
               BRC Auswahl
               CMP W1,W5             (für Full-House-Leugner)
               BRZ Auswahl           (für Full-House-Leugner)
FullHouse      ADDI #1,FIGUR2
```

Wenn erster und fünfter Würfel gleich sind, dann wird mit dieser Änderung kein Full House gewertet. Verwendung auf eigene Gefahr.



