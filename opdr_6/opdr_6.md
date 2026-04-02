# Sql basis

### Database gegevens aanpassen
Maak gebruik van het sql-bestand idscan.sql en voer hierop onderstaande opdrachten uit.
Theorie kun je vinden op: https://www.edutorial.nl/dbq/introductie/

### Queries winkelketen
* Welke medewerkers (id, voornaam, achternaam) zijn er in dienst van de winkelketen.

```sql
SELECT id, firstname, lastname FROM persons;
```

* Hoeveel medewerkers hebben dezelfde functie (jobtitle)

```sql
SELECT jobtitle, COUNT(*) AS aantal
FROM persons
GROUP BY jobtitle
ORDER BY aantal DESC;
```

* Hoeveel medewerkers zijn professor of ingenieur (title = prof, ir of ing)

```sql
SELECT COUNT(*) AS aantal
FROM persons
WHERE title IN ('prof.', 'ir.', 'ing.');
```

* Overzicht van medewerkers (id, voornaam, tussenvoegsel, achternaam) per gebouw (buildingname, street en buildingnumber)

```sql
-- Let op: de persons-tabel heeft geen aparte tussenvoegsel-kolom.
SELECT DISTINCT p.id, p.firstname, p.lastname,
       b.buildingname, b.street, b.buildingnumber
FROM persons p
JOIN scans s ON p.id = s.person_id
JOIN buildings b ON s.building_id = b.id
ORDER BY b.buildingname, p.lastname;
```

* Overzicht van medewerkers (id, voornaam, tussenvoegsel, achternaam) die op een *  * bepaalde datum in gebouw met id 1 waren (buildingname, 

```sql
SELECT DISTINCT p.id, p.firstname, p.lastname, b.buildingname
FROM persons p
JOIN scans s ON p.id = s.person_id
JOIN buildings b ON s.building_id = b.id
WHERE s.building_id = 1
  AND s.scandate = '2023-09-13';
```

* Overzicht van medewerkers die op diezelfde datum vergeten zijn om uit te checken

```sql
SELECT DISTINCT p.id, p.firstname, p.lastname
FROM persons p
JOIN scans s ON p.id = s.person_id
WHERE s.building_id = 1
  AND s.scandate = '2023-09-13'
  AND s.in_out = 'in'
  AND p.id NOT IN (
      SELECT person_id FROM scans
      WHERE building_id = 1
        AND scandate = '2023-09-13'
        AND in_out = 'out'
  );
```

* Overzicht van het aantal medewerker per gebouw op 13 september 2023.

```sql
SELECT b.buildingname, COUNT(DISTINCT s.person_id) AS aantal_medewerkers
FROM scans s
JOIN buildings b ON s.building_id = b.id
WHERE s.scandate = '2023-09-13'
GROUP BY b.id, b.buildingname;
```

* Overzicht van medewerkers en het aantal uur dat ze op 15 september 2023 hebben gewerkt.

```sql
SELECT p.id, p.firstname, p.lastname,
       ROUND(TIME_TO_SEC(TIMEDIFF(
           MAX(CASE WHEN s.in_out = 'out' THEN s.scantime END),
           MIN(CASE WHEN s.in_out = 'in'  THEN s.scantime END)
       )) / 3600, 2) AS uren_gewerkt
FROM persons p
JOIN scans s ON p.id = s.person_id
WHERE s.scandate = '2023-09-15'
GROUP BY p.id, p.firstname, p.lastname;
```

* Medewerker van de maand! (De medewerker die het meeste uren heeft gemaakt van iedereen in de maand september)

```sql
SELECT p.id, p.firstname, p.lastname,
       ROUND(SUM(TIME_TO_SEC(TIMEDIFF(s_out.scantime, s_in.scantime))) / 3600, 2) AS totaal_uren
FROM persons p
JOIN scans s_in ON p.id = s_in.person_id AND s_in.in_out = 'in'
JOIN scans s_out ON p.id = s_out.person_id
    AND s_out.in_out = 'out'
    AND s_out.scandate = s_in.scandate
    AND s_out.building_id = s_in.building_id
WHERE MONTH(s_in.scandate) = 9
  AND YEAR(s_in.scandate) = 2023
GROUP BY p.id, p.firstname, p.lastname
ORDER BY totaal_uren DESC
LIMIT 1;
```
