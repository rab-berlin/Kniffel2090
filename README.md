# Kniffel2090

Kniffel auf dem Busch Microtronic 2090

## Bedingtes Unterprogramm

Keine Ahnung, ob es sowas gibt. Falls nicht, hab ich's gerade erfunden...

Bestimmte Programmabschnitte lösen ähnliche Aufgaben, dann ist es sinnvoll, zu überlegen, ob diese Abschnitte in ein Unterprogramm ausgelagert werden können. Beim Microtronic stellen sich dann allerdings zwei potentielle Probleme: 

- Unterprogramme dürfen keine weiteren Unterprogramme aufrufen
- ein RETURN ohne vorheriges CALL ist unzulässig

Im Anleitungsbuch 1. Teil, S. 74 heißt es dazu: **Wichtig: In einem Unterprogramm darf kein CALL-Befehl vorhanden sein, weil sonst eine "Endlosschleife" entsteht.**

Dieser Warnhinweis schien mir ausreichend bedrohlich - wer will schon gefangen sein in einer *Endlosschleife*?! 😱 Womöglich stürzte ich das Universum ins Nichts nur durch einen unbedachten CALL? Zu viel Verantwortung für die schmalen Schultern eines jugendlichen Programmierschülers. 

Damals weder angezweifelt noch je wissentlich übertreten, sehe ich diese Gebote heutzutage - nach vielen undeklarierten Variablen, verbogenen Pointern und übergelaufenen Arrays - einen Tick gelassener...

Und hab's einfach mal riskiert. What can possibly go wrong? 
