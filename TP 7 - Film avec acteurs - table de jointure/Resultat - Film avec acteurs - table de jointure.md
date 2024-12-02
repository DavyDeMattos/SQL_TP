# Les données

```sql
DROP DATABASE IF EXISTS netflix;

CREATE DATABASE netflix CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

USE netflix;

CREATE TABLE film (
    id INT NOT NULL AUTO_INCREMENT,
    nom VARCHAR(100) NOT NULL,
    PRIMARY KEY (id)
) ENGINE=InnoDB;

INSERT INTO film (nom) VALUES
('Fight Club'),
('One Upon the time');


CREATE TABLE acteur (
    id INT NOT NULL AUTO_INCREMENT,
    prenom VARCHAR(100) NOT NULL,
    nom VARCHAR(100) NOT NULL,
    PRIMARY KEY (id)
) ENGINE=InnoDB;

INSERT INTO acteur (prenom, nom) VALUES
('Brad', 'PITT'),
('Léonardo', 'DICAPRIO');

CREATE TABLE film_has_acteur (
    film_id INT NOT NULL,
    acteur_id INT NOT NULL,
    PRIMARY KEY (film_id, acteur_id)
) ENGINE=InnoDB;

ALTER TABLE film_has_acteur
ADD CONSTRAINT fk_film FOREIGN KEY (film_id) REFERENCES film(id),
ADD CONSTRAINT fk_acteur FOREIGN KEY (acteur_id) REFERENCES acteur(id);

INSERT INTO film_has_acteur (film_id, acteur_id) VALUES
(1,1),
(2,1),
(2,2);

```

## 1 - Afficher tous les films de Léonardo DI CAPRIO

```sql
SELECT film.nom, acteur.prenom AS acteur_prenom, acteur.nom AS acteur_nom
FROM film
INNER JOIN film_has_acteur  ON film.id = film_has_acteur.film_id
INNER JOIN acteur ON acteur.id = film_has_acteur.acteur_id
WHERE acteur.prenom = 'Léonardo' AND acteur.nom = 'DICAPRIO'
```

## 2 - Afficher le nombre de films par acteur

```sql
SELECT acteur.prenom AS acteur_prenom, acteur.nom AS acteur_nom, COUNT(film.nom) AS nb_films
FROM film
INNER JOIN film_has_acteur  ON film.id = film_has_acteur.film_id
INNER JOIN acteur ON acteur.id = film_has_acteur.acteur_id
GROUP BY acteur.nom
```

## 3 - Ajouter un film :TITANIC

```sql
INSERT INTO film (nom) VALUES
('Titanic');

```

## 4 - Trouver le film qui n'a pas d'acteur

```sql
SELECT film.nom
FROM film
LEFT JOIN film_has_acteur ON film.id = film_has_acteur.film_id
WHERE film_has_acteur.film_id IS NULL
```
