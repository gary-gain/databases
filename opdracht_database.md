 # Verslag Gary Gain
 22/03/2017

 ## Oplossingen

 ### Oefening1:

 Geef een lijst met namen van alle tabellen die in de gamereview_example database te vinden zijn.

```sql
SHOW DATABASES;
```
```
+---------------------+
| Database            |
+---------------------+
| gamereviews_example |
| information_schema  |
| mysql               |
| performance_schema  |
| phpmyadmin          |
| test                |
+---------------------+
```

### Oefening2:

 Geef alle kolom namen met type voor de tabel games.

```sql
SHOW TABLES;
```
```
+-------------------------------+
| Tables_in_gamereviews_example |
+-------------------------------+
| game_genres                   |
| games                         |
| genres                        |
| publishers                    |
| reviews                       |
| user_games                    |
| users                         |
+-------------------------------+
```

### Oefening3:

Antwoord kort en bondig op volgende vragen over de users, antwoord niet met een query.

#### i.

Is het opgeven van een name verplicht?

```sql
Dit is verplicht omdat deze niet NULL mag zijn.
```

#### ii.

Is het opgeven van een lastname verplicht?

```sql
Nee dit is niet verplicht, het mag NULL zijn.
```

#### iii.

Is het opgeven van een email verplicht?

```sql
Nee dit is niet verplicht, het mag NULL zijn.
```

#### iv.

Wat is de maximale lengte van een password?

```sql
De maximale lengte is 32 karakters want, char(32).
```

### Oefening4:

Geef een lijst van alle namen van games .

```sql
SELECT name FROM games;
```
```
+------------------------------------------+
| name                                     |
+------------------------------------------+
| Resident Evil 4                          |
| Call of Duty 4: Modern Warfare           |
| Assassin's Creed                         |
| Prototype                                |
| No More Heroes                           |
| Burnout Paradise                         |
| Shadow of the Colossus                   |
| Rock Band                                |
| Halo 3                                   |
| Crysis                                   |
+------------------------------------------+
```

### Oefening5:

Geef een lijst van alle namen van users in alfabetische volgorde.

```sql
SELECT name FROM users ORDER BY name;
```
```
+----------+
| name     |
+----------+
| Amber    |
| Amber    |
| Anna     |
| Anna     |
| Anna     |
| Anouk    |
| Bas      |
| Britt    |
| Daan     |
| Emma     |
+----------+
```

### Oefening6:

Geef een lijst van de 10de tot de 20ste publishers die alfabetisch gerangschikt staan.


```sql
SELECT name FROM publishers ORDER BY name LIMIT 10,10;
```
```
+-------------------------------+
| name                          |
+-------------------------------+
| BAP Interactive               |
| BioWare                       |
| Black Isle Studios            |
| BMG Interactive Entertainment |
| Centron Software, Inc.        |
| ChessBase GmbH                |
| Crave Entertainment           |
| Croteam Ltd.                  |
| Cyan Worlds, Inc.             |
| Deep Silver                   |
+-------------------------------+
```

### Oefening7:

Wat is het aantal games dat in de database zit?

```sql
SELECT COUNT(name) AS 'games' FROM games;
```
```
+-------+
| games |
+-------+
|   100 |
+-------+
```

### Oefening8:

Wat is het aantal games dat in de database zit en een multiplayer game is?

```sql
SELECT COUNT(name) AS 'games' FROM games WHERE multiplayer = 1;
```
```
+-------+
| games |
+-------+
|    53 |
+-------+
```

### Oefening9:

Geef met 1 query het aantal users, aantal games, aantal reviews in de database.

```sql
SELECT
-> (SELECT count(*) FROM users) AS users,
-> (SELECT count(*) FROM games) AS games,
-> (SELECT count(*) FROM reviews) AS reviews;
```
```
+-------+-------+---------+
| users | games | reviews |
+-------+-------+---------+
|    75 |   100 |     534 |
+-------+-------+---------+
```

### Oefening10:

Hoeveel reviews hebben een score kleiner dan 2 gegeven?

```sql
SELECT count(score) AS 'score' FROM reviews WHERE score < 2;
```
```
+-------+
| score |
+-------+
|    75 |
+-------+
```

### Oefening11:

Geef de top 3 van beste reviews.

```sql
SELECT game_id,score FROM reviews ORDER BY score DESC LIMIT 3;
```
```
+---------+-------+
| game_id | score |
+---------+-------+
|   20961 |    10 |
|   20599 |    10 |
|   26706 |    10 |
+---------+-------+
```

### Oefening12:

Hoeveel reviews heef het game met id: 20626 gekregen?

```sql
SELECT game_id FROM reviews WHERE game_id = 20626;
```
```
+---------+
| game_id |
+---------+
|   20626 |
|   20626 |
|   20626 |
+---------+
```
#### Of:

```sql
SELECT COUNT(game_id) AS aantal_reviews
-> FROM reviews
-> WHERE game_id = 20626;
```
```
+----------------+
| aantal_reviews |
+----------------+
|              3 |
+----------------+
```

### Oefening13:

Wat is het id van de game met de meeste reviews?

```sql
SELECT game_id, COUNT(*) AS aantal_reviews
-> FROM reviews
-> GROUP BY game
-> ORDER BY aantal_reviews
-> DESC LIMIT 1;
```
```
+---------+----------------+
| game_id | aantal_reviews |
+---------+----------------+
|    4209 |             11 |
+---------+----------------+
```

### Oefening14:

Wat is de naam van de game met de meeste reviews?

```sql
SELECT id, name FROM games WHERE id = (SELECT game_id FROM reviews GROUP BY game_id ORDER BY COUNT(*) DESC LIMIT 1);
```
```
+------+-----------+
| id   | name      |
+------+-----------+
| 4209 | Prototype |
+------+-----------+
```

### Oefening15:

Wat is de gemiddelde score voor het game met id : 20991?

```sql
SELECT game_id, AVG(score) AS avg_score FROM reviews WHERE game_id = 20991;
```
```
+---------+-----------+
| game_id | avg_score |
+---------+-----------+
|   20991 |    4.4444 |
+---------+-----------+
```

### Oefening16:

 Wat is de gemiddelde score voor het game met name "Halo 3"?

```sql
SELECT name, AVG(score) AS avg_score FROM games, reviews WHERE name = 'Halo 3';
```
```
+--------+-----------+
| name   | avg_score |
+--------+-----------+
| Halo 3 |    5.1798 |
+--------+-----------+
```

### Oefening17:

Geef een lijst van game names die de tekst 'battle' bevatten.


```sql
SELECT name FROM games WHERE name LIKE '%batttle%';
```
```
Empty set (0.00 sec)
```

In de database staan geen gamenamen met 'battle' in, echter wel met 'Battle'.

```sql
SELECT name FROM games WHERE name LIKE '%Battle%';
```
```
+----------------------------+
| name                       |
+----------------------------+
| Battlefield: Bad Company   |
| Battlefield: Bad Company 2 |
| Battlefield 1943           |
+----------------------------+
```

### Oefening18:

Wat is de name van de user die de allereerste review geschreven heeft?

```sql
SELECT name, timestamp FROM users ORDER BY timestamp LIMIT 1;
```
```
+------+---------------------+
| name | timestamp           |
+------+---------------------+
| Finn | 2010-06-27 23:31:19 |
+------+---------------------+
```

### Oefening19:

Geef een lijst met namen en gemiddelde score's van de 10 beste games.

```sql

```
```

```

### Oefening20:

Geef de namen van de 3 duurste games, en geef ook de name van de publischer van die games .

```sql
SELECT name, price, publisher_id FROM games ORDER BY price DESC LIMIT 3;
```
```
+----------------------------+-------+--------------+
| name                       | price | publisher_id |
+----------------------------+-------+--------------+
| Halo 3                     | 59.83 |          102 |
| Uncharted 2: Among Thieves | 59.63 |           10 |
| Crysis                     | 59.06 |           83 |
+----------------------------+-------+--------------+
```
