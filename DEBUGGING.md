# DEBUGGING – Booking overlap validation

## 1. Vilket fel undersökte jag?

Jag undersökte felet där applikationen inte alltid upptäckte överlappande bokningar för samma rum och datum. Problemet kunde leda till dubbelbokningar eftersom vissa nya bokningar godkändes trots att rummet redan var upptaget under en del av tiden.

## 2. Hur återskapade jag felet?

Jag använde en befintlig bokning i rummet Aspen mellan 09:00 och 11:00. Därefter testade jag nya bokningar med tiderna:

- 08:00–10:00
- 10:00–12:00
- 09:30–10:30
- 08:00–12:00

Alla dessa tider överlappar den befintliga bokningen.

## 3. Vilket resultat fick jag?

Vissa bokningar nekades korrekt, men andra godkändes felaktigt. Exempelvis kunde 08:00–10:00 godkännas trots att den överlappar den befintliga bokningen mellan 09:00 och 10:00.

## 4. Vilket resultat skulle jag ha fått?

Alla bokningar som överlappar en aktiv bokning för samma rum och datum skulle nekas. En bokning mellan 11:00 och 12:00 skulle däremot godkännas eftersom den börjar exakt när den tidigare bokningen slutar.

## 5. Vilken del av koden orsakade felet?

Felet fanns i BookingService, i Validate-metoden.

Den gamla kontrollen var:


booking.StartTime >= existing.StartTime &&
booking.StartTime < existing.EndTime


## 6. Varför gav koden ett felaktigt resultat?

Kontrollen tittade bara på den nya bokningens starttid. Den fungerade om den nya bokningen började inuti den befintliga tiden, men inte om den började tidigare och ändå överlappade.

För 08:00–10:00 mot 09:00–11:00 blev:

08:00 >= 09:00

false, trots att tiderna överlappar mellan 09:00 och 10:00.

## 7. Vad ändrade jag?

Jag ändrade kontrollen till:

booking.StartTime < existing.EndTime &&
booking.EndTime > existing.StartTime

Den nya kontrollen jämför hela tidsintervallet och upptäcker därför fler typer av överlappning.

## 8. Hur kontrollerade jag att ändringen fungerade?

Jag testade samma tider igen och alla överlappande bokningar nekades korrekt. Jag testade även 11:00–12:00 och den godkändes.

Efter detta körde jag alla automatiserade tester och samtliga gick igenom.

## 9. Vilket automatiserat test skyddar mot att felet återkommer?

Testet Create_WhenBookingOverlapsExistingBooking_ReturnsFailure kontrollerar flera typer av överlappningar.

Testet Create_WhenBookingStartsWhenExistingBookingEnds_ReturnsSuccess kontrollerar att en bokning som börjar exakt när en tidigare bokning slutar fortfarande tillåts.

Dessa tester gör att samma fel snabbt upptäcks om overlap-logiken skulle ändras felaktigt i framtiden.