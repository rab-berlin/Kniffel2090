# Kniffel2090

Kniffel auf dem Busch Microtronic 2090

Doku wird noch ergänzt... Wer´s trotzdem schon testen mag, nur zu! :)

Die Kniffelregeln zu erklären, spare ich mir. Wer´s nicht kennt, möge einschlägig nachsehen... 

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

Mit den Ziffern 1-5 werden die Würfel ausgewählt, die zurück in den Becher, also erneut geworfen werden sollen. Wenn ein Würfel in den Becher gelegt wird, wird seine Augenzahl im Display durch eine 0 ersetzt. Aber Vorsicht - einmal einen Würfel gewählt gibt´s kein Zurück. Für eine Undo-Funktion war einfach kein Platz - weder Programmschritte noch Register.

## Nächster Wurf

Der nächste Wurf wird mit der Ziffer 0 gestartet.

## Alea iacta est

Nach dem dritten Wurf muss in den Block eingetragen werden. Mit der Taste 1-4 auf der jeweils aktiven Blockseite.

# Besonderheiten

## Bonus

Wenn im oberen Block 63 Punkte oder mehr erzielt werden, erhält man 35 Punkte als Bonus. Wie kann man diese Prüfung mit möglichst wenig Programmschritten durchführen?

Die Summe der oben erzielten Punkte ist hexadezimal in den Speicherregistern 1 und 2 abgelegt. Wenn dort 3F hex oder mehr steht, dann soll der Bonus zur Gesamtsumme addiert werden.

Die einfache, aber weniger ökonomische Lösung wäre: Zunächst die 10er-Stelle prüfen, danach - falls die 10er-Stelle genau 3 ist - auch die 1er-Stelle prüfen. Unterm Strich viele Vergleiche und bedingte Sprünge für beide Register.







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



