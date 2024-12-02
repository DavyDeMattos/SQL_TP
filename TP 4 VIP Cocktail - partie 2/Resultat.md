```sql
DROP DATABASE IF EXISTS invitation;

CREATE DATABASE invitation CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

USE invitation;

CREATE TABLE inv_personne (
    id INT NOT NULL AUTO_INCREMENT,
    prenom VARCHAR(255) NOT NULL,
    nom VARCHAR(255) NOT NULL,
    age TINYINT NOT NULL,
    inscription DATE NOT NULL,
    etat BOOLEAN NOT NULL,
    statut VARCHAR(255) NOT NULL,
    cv VARCHAR(255) NOT NULL,
    salaire INT NOT NULL,
    PRIMARY KEY (id)
) ENGINE=InnoDB;

INSERT INTO inv_personne (prenom, nom, age, inscription, etat, statut, cv, salaire) VALUES
('Brad', 'PITT', 60, '1970-01-01', 1, 'non membre', 'lorem ipsum', 2000000),
('George', 'CLONEY', 62, '1999-01-01', 1, 'membre', 'juste beau', 4000000),
('Jean', 'DUJEARDIN', 51, '1994-01-01', 0, 'membre', 'brice de nice', 1000000);

```

1.Ajouter les données

```sql
INSERT INTO inv_personne (id, prenom, nom, age, inscription, etat, statut, cv, salaire) VALUES
(1, 'Brad', 'PITT', 60, '1970-01-01', 1, 'non membre', 'lorem ipsum', 2000000),
(2, 'George', 'CLONEY', 62, '1999-01-01', 1, 'membre', 'juste beau', 4000000),
(3, 'Jean', 'DUJARDIN', 51, '1994-01-01', 0, 'membre', 'brice de nice', 1000000);
```

2.Afficher le plus gros salaire (avec MAX)  [doc max w3](https://www.w3schools.com/sql/func_mysql_max.asp)

```sql
SELECT MAX(salaire) AS plus_gros_salaire FROM inv_personne;
```

3.Afficher le plus petit salaire (avec MIN)  [doc min w3](https://www.w3schools.com/sql/func_mysql_min.asp)

```sql
SELECT MIN(salaire) AS plus_petit_salaire FROM inv_personne;
```

4.Afficher le nom de l'acteur (et son salaire) qui a le plus petit salaire avec `LIMIT` & `ORDER BY`  

```sql
SELECT prenom, nom, salaire
FROM inv_personne
ORDER BY salaire ASC
LIMIT 1;
```

5.Afficher le nom de l'acteur (et son salaire) qui a le plus gros salaire avec `LIMIT` & `ORDER BY`

```sql
SELECT prenom, nom, salaire
FROM inv_personne
ORDER BY salaire DESC
LIMIT 1;
```

6.Afficher le salaire moyen  

```sql
   SELECT AVG(salaire) AS salaire_moyen FROM inv_personne;
```

7.Afficher le nombre de personnes  

```sql
SELECT COUNT(*) AS nb_personnes FROM inv_personne;
```

8.Afficher les inv_personne avec un salaire entre 1 000 000 et 4 000 000 avec `BE

```sql
SELECT id, prenom, nom, salaire
FROM inv_personne
WHERE salaire BETWEEN 1000001 AND 3999999;
```

9.Proposer une requete avec  `UPPER()` & `LOWER()`  

```sql
SELECT id, UPPER(prenom) AS prenom, LOWER(nom) AS nom FROM inv_personne;
```

10.Afficher les personnes dont le prénom contient 'bra'  

```sql
SELECT id, prenom, nom
FROM inv_personne
WHERE prenom LIKE '%bra%'
```

12.Trier par age les membres

```sql
SELECT prenom, nom, age
FROM inv_personne
WHERE statut = 'membre'
ORDER BY age ASC
```

13.Afficher le nombre d'acteurs "membre"

```sql
SELECT COUNT(*) AS nb_membres
FROM inv_personne
WHERE statut = 'membre';
```

14.Afficher le nombre d'acteurs "membre"

```sql
SELECT statut AS membre, COUNT(*) AS nb_acteur
FROM inv_personne
GROUP BY statut
```
