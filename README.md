# Kniffel2090

Kniffel auf dem Busch Microtronic 2090

## Bedingtes Unterprogramm

Keine Ahnung, ob es sowas gibt. Falls nicht, hab ich's gerade erfunden...

Bestimmte Programmabschnitte lösen ähnliche Aufgaben, dann ist es sinnvoll, zu schauen, ob diese Abschnitte in ein Unterprogramm ausgelagert werden können. Beim Microtronic stellen sich dann allerdings zwei Probleme: 

- Unterprogramme können keine weiteren Unterprogramme aufrufen
- ein RETURN ohne vorheriges CALL ist unzulässig

Im Anleitungsbuch 1. Teil, S. 74 heißt es dazu **Wichtig: In einem Unterprogramm darf kein CALL-Befehl vorhanden sein, weil sonst eine "Endlosschleife" entsteht.**

Als jugendlicher Programmierschüler schien mir diese Warnung ausreichend bedrohlich - wer will schon in einer **Endlosschleife** 😱 gefangen sein?! Damals nahezu das elfte Gebot, heutzutage - nach vielen mutwillig verbogenen Pointern und übergelaufenen Arrays - sehe ich das etwas gelassener...

Und hab's einfach mal ausprobiert.
