# Sql basis

### Database 
Maak gebruik van het sql-bestand reisbureau.sql en voer hierop onderstaande opdrachten uit.
Theorie kun je vinden op: https://www.edutorial.nl/dbq/introductie/

### Queries reisbureau
* Geef de namen van de klanten die in Rotterdam wonen

```sql
SELECT Naam FROM klanten WHERE Plaats = 'Rotterdam';
```

* Geef de namen van de klanten die geen reis geboekt hebben.

```sql
SELECT Naam FROM klanten
WHERE Klantnummer NOT IN (SELECT Klantnummer FROM boekingen);
```

* Hoeveel klanten komen er niet uit Rotterdam

```sql
SELECT COUNT(*) AS aantal FROM klanten WHERE Plaats != 'Rotterdam';
```

* Hoeveel reizen hebben als bestemming Afrika?

```sql
SELECT COUNT(*) AS aantal
FROM reizen r
JOIN bestemmingen b ON r.Bestemmingcode = b.Bestemmingcode
WHERE b.Werelddeel = 'Afrika';
```

* Hoeveel moeten de klanten, die naar Azië gaan, in totaal betalen?

```sql
SELECT SUM(bo.`Betaald bedrag`) AS totaal_te_betalen
FROM boekingen bo
JOIN reizen r ON bo.Reisnummer = r.Reisnummer
JOIN bestemmingen b ON r.Bestemmingcode = b.Bestemmingcode
WHERE b.Werelddeel = 'Azie';
```

* Geef de namen van de klanten die met kinderen op reis gaan.

```sql
SELECT k.Naam
FROM klanten k
JOIN boekingen bo ON k.Klantnummer = bo.Klantnummer
WHERE bo.`Aantal kinderen` > 0;
```

* Hoeveel verschillende reizen kun je boeken bij dit reisbureau?

```sql
SELECT COUNT(*) AS aantal_reizen FROM reizen;
```

* Geef de naam en postcode van de klanten die in een postcode gebied wonen dat begint met een 9.

```sql
SELECT Naam, Postcode FROM klanten WHERE Postcode LIKE '9%';
```

* Groepeer de klanten op woonplaats. Geef de woonplaats en het aantal klanten per woonplaats.

```sql
SELECT Plaats, COUNT(*) AS aantal_klanten
FROM klanten
GROUP BY Plaats
ORDER BY aantal_klanten DESC;
```

* Geef naam en datum van de klanten die voor de maand April een reis hebbben geboekt.

```sql
SELECT k.Naam, bo.Boekdatum
FROM klanten k
JOIN boekingen bo ON k.Klantnummer = bo.Klantnummer
WHERE MONTH(bo.Boekdatum) < 4;
```

* Geef de namen van klanten, het werelddeel van de bestemming en het aantal dagen van de reis voor boekingen van minimaal 15 dagen.

```sql
SELECT k.Naam, b.Werelddeel, r.`Aantal dagen`
FROM klanten k
JOIN boekingen bo ON k.Klantnummer = bo.Klantnummer
JOIN reizen r ON bo.Reisnummer = r.Reisnummer
JOIN bestemmingen b ON r.Bestemmingcode = b.Bestemmingcode
WHERE r.`Aantal dagen` >= 15;
```
