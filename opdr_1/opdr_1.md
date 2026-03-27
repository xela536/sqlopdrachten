# Sql basis

### Database gegevens filteren
Maak gebruik van het sql-bestand school.sql en voer hierop onderstaande opdrachten uit.
Theorie kun je vinden op: https://www.edutorial.nl/dbq/introductie/

### Opdracht 1
* Geef de query voor een overzicht van de naam en roepnaam van alle cursisten die in Oosterhout wonen.

```sql
SELECT naam, roepnaam
FROM cursist
WHERE plaats = 'Oosterhout';
```

### Opdracht 2
* Geef de query voor een overzicht van het cursistnummer en de roepnaam van alle cursisten die niet in Oosterhout wonen.

```sql
SELECT cursistnr, roepnaam
FROM cursist
WHERE plaats <> 'Oosterhout';
```

### Opdracht 3
* Geef de query voor een overzicht van alle cursisten die vrouw zijn.

```sql
SELECT *
FROM cursist
WHERE geslacht = 'V';
```

### Opdracht 4
* Geef de query voor een overzicht van alle cursisten die niet man zijn. (dit is een andere query dan de vorige vraag, maar met hetzelfde resultaat)

```sql
SELECT *
FROM cursist
WHERE NOT geslacht = 'M';
```

### Opdracht 5
* Geef de query voor een overzicht van alle cursisten die in Breda wonen en vrouw zijn.

```sql
SELECT *
FROM cursist
WHERE plaats = 'Breda' AND geslacht = 'V';
```

### Opdracht 6
* Geef de query voor een overzicht van alle cursisten die in Oosterhout of Made wonen.

```sql
SELECT *
FROM cursist
WHERE plaats IN ('Oosterhout', 'Made');
```

### Opdracht 7
* Geef de query voor een overzicht van alle plaatsen waar cursussen worden gegeven.

```sql
SELECT DISTINCT curs_plts
FROM cursus;
```
