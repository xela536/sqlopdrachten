# Sql basis

### Database
Maak gebruik van het sql-bestand flits.sql en voer hierop onderstaande opdrachten uit.
Theorie kun je vinden op: https://www.edutorial.nl/dbq/introductie/

### Queries flitspaal
* Welke cameras zijn er en waar staan ze (id, address, city, max_speed).

```sql
SELECT id, address, city, max_speed FROM cameras;
```

* Overzicht van boetes op 50km wegen

```sql
SELECT fl.license, fl.speed, c.max_speed,
       (fl.speed - c.max_speed) AS overschrijding_kmh,
       f.fine AS boete_euro
FROM flashes fl
JOIN cameras c ON fl.camera_id = c.id
JOIN fines f ON f.speed_limit = c.max_speed
           AND f.speed_excess = (fl.speed - c.max_speed)
WHERE c.max_speed = 50;
```

* Overzicht van overtredingen van 1 kenteken.

```sql
SELECT fl.id, fl.license, fl.speed, c.address, c.city, c.max_speed, fl.expdate
FROM flashes fl
JOIN cameras c ON fl.camera_id = c.id
WHERE fl.license = 'CR-52-52';
```

* N.a.w.-gegevens van de hardrijders die het meest in de fout zijn gegaan. (top 10)

```sql
SELECT l.first_name, l.last_name, l.address, l.postal_code, l.city,
       COUNT(*) AS aantal_overtredingen
FROM flashes fl
JOIN licenses l ON fl.license = l.license
GROUP BY fl.license, l.first_name, l.last_name, l.address, l.postal_code, l.city
ORDER BY aantal_overtredingen DESC
LIMIT 10;
```

* Welke camera's (id, address, city) meten de meeste snelheidsovertredingen (top 10)

```sql
SELECT c.id, c.address, c.city, COUNT(*) AS aantal_overtredingen
FROM flashes fl
JOIN cameras c ON fl.camera_id = c.id
GROUP BY c.id, c.address, c.city
ORDER BY aantal_overtredingen DESC
LIMIT 10;
```

* Welke auto's (kenteken, merk, type) zijn het meeste geflitst

```sql
-- Let op: de database bevat geen merk/type kolommen; alleen kenteken is beschikbaar.
SELECT fl.license, COUNT(*) AS aantal_keer_geflitst
FROM flashes fl
GROUP BY fl.license
ORDER BY aantal_keer_geflitst DESC
LIMIT 10;
```

* Welke flitspaal (=camera met id, address, city) flitst het meest (top 10)

```sql
SELECT c.id, c.address, c.city, COUNT(*) AS aantal_flitsen
FROM flashes fl
JOIN cameras c ON fl.camera_id = c.id
GROUP BY c.id, c.address, c.city
ORDER BY aantal_flitsen DESC
LIMIT 10;
```

* Kentekens van auto's die het hoogste bedrag aan boetes hebben gekregen (top 10)

```sql
SELECT fl.license, SUM(f.fine) AS totaal_boetes
FROM flashes fl
JOIN cameras c ON fl.camera_id = c.id
JOIN fines f ON f.speed_limit = c.max_speed
           AND f.speed_excess = (fl.speed - c.max_speed)
GROUP BY fl.license
ORDER BY totaal_boetes DESC
LIMIT 10;
```

* Overzicht van auto's (kenteken, merk, type) waarvan het kenteken overeenkomt met sitecode 10 (zoals X-999-XX) https://nl.wikipedia.org/wiki/Nederlands_kenteken.

```sql
-- Sitecode 10: formaat X-999-XX (1 letter, 3 cijfers, 2 letters)
SELECT license
FROM licenses
WHERE license REGEXP '^[A-Z]-[0-9]{3}-[A-Z]{2}$';
```
