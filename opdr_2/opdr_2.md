# Sql basis

### Database gegevens aanpassen
Maak gebruik van het sql-bestand reisbureau.sql en voer hierop onderstaande opdrachten uit.
Theorie kun je vinden op: https://www.edutorial.nl/dbq/introductie/

### Opdracht 1
* Geef de query om alle tabellen in de database 'reisbureau' weer te gegeven

```sql
SHOW TABLES;
```

### Opdracht 2
* Voeg 2 nieuwe klanten toe aan de tabel 'customers' (je mag de waarden zelf bedenken)

```sql
INSERT INTO `customers` (`first_name`, `last_name`, `email`, `address`, `postal_code`, `city`, `country_code`, `phone`, `created_at`, `updated_at`)
VALUES
  ('Jan', 'de Vries', 'jan.devries@gmail.com', 'Kerkstraat 12', '1234 AB', 'Amsterdam', 'nl_NL', '+31612345678', NOW(), NULL),
  ('Lisa', 'Bakker', 'lisa.bakker@hotmail.com', 'Hoofdweg 5', '5678 CD', 'Rotterdam', 'nl_NL', '+31687654321', NOW(), NULL);
```

### Opdracht 3
* Geef de query om de eerste 10 boekingen te verwijderen (reservations)

```sql
DELETE FROM `reservations`
ORDER BY `id` ASC
LIMIT 10;
```

### Opdracht 4
* De klant met id 13 is verhuist naar 'De van der veldensteeg 81' in 'Apeldoorn'.
* Pas het record aan en geef de query
* Toon het record en controleer of de gegevens correct zijn.

```sql
UPDATE `customers`
SET `address` = 'De van der veldensteeg 81', `city` = 'Apeldoorn'
WHERE `id` = 13;

SELECT * FROM `customers` WHERE `id` = 13;
```
