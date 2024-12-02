# TP SQL Chats

1.Ajout des données

```sql
DROP DATABASE IF EXISTS cats;

CREATE DATABASE cats CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

CREATE TABLE cats (
    id INT NOT NULL AUTO_INCREMENT,
    nom VARCHAR(255) NOT NULL,
    yeux VARCHAR(255) NOT NULL,
    age TINYINT NOT NULL
) ENGINE=InnoDB; 

USE cats;

INSERT INTO chats (id, nom, yeux, age) VALUES
(1, 'Maine coon', 'marron', 20),
(2, 'Siamois', 'bleu', 15),
(3, 'Bengal', 'marron', 18),
(4, 'Scottish Fold', 'marron', 10);

```

2.Afficher le chat avec l'id :2 

```sql
SELECT *
FROM cats
WHERE cats.id = 2
```

3.Trier les chats par nom et par age

```sql
SELECT *
FROM cats
ORDER BY nom ASC, age ASC;
```

4.Afficher les chats qui vive entre 11 et 19 ans  

```sql
SELECT *
FROM cats
WHERE age BETWEEN 11 AND 19;
```

5.Afficher le ou les chats dont le nom contient 'sia'  

```sql
SELECT *
FROM cats
WHERE nom LIKE '%sia%';
```

6.Afficher le ou les chats dont le nom contient 'a'  

```sql
SELECT *
FROM cats
WHERE nom LIKE '%a%'
```

7.Afficher la moyenne d'age des chats  

```sql
SELECT AVG(age) AS moyenne_age 
FROM chats;
```

8.Afficher le nombre de chats dans la table

```sql
SELECT COUNT(*) AS nombre_chats 
FROM chats;
```

9.Afficher le nombre de chat avec couleur d'yeux marron  

```sql
SELECT COUNT(*) AS chats_yeux_marron 
FROM chats 
WHERE yeux = 'marron';
```

10.Afficher le nombre de chat par couleur d'yeux

```sql
SELECT yeux, COUNT(*) AS nombre_chats_par_couleur 
FROM chats 
GROUP BY yeux;

```

[BONUS] - Ajouter les données à partir d'un fichier excel  
