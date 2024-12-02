
1. Création de la base de données **spa**  
2. Création de la table **chat**  
3. Création de la table **couleur**  
4. Insérer  les données  

```sql
DROP DATABASE IF EXISTS spa;

CREATE DATABASE spa CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

USE spa;

CREATE TABLE couleur (
    id INT NOT NULL AUTO_INCREMENT,
    nom VARCHAR(30) NOT NULL,
    PRIMARY KEY (id)
) ENGINE=InnoDB;

INSERT INTO couleur (id, nom) VALUES
(1, 'marron'),
(2, 'bleu'),
(3, 'vert');

CREATE TABLE chat (
    id INT NOT NULL AUTO_INCREMENT,
    nom VARCHAR(30) NOT NULL,
    couleur_id INT,
    PRIMARY KEY (id),
    FOREIGN KEY (couleur_id) REFERENCES couleur(id)
) ENGINE=InnoDB;

INSERT INTO chat (nom, couleur_id) VALUES
('Maine coon', 1),
('Siamois', 2),
('Bengal', 1),
('Scottish Fold', 1),
('domestique', NULL);

```

```sql
DROP DATABASE IF EXISTS spa;

CREATE DATABASE spa CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

USE spa;

CREATE TABLE couleur (
    id INT NOT NULL AUTO_INCREMENT,
    nom VARCHAR(30) NOT NULL,
    PRIMARY KEY (id)
) ENGINE=InnoDB;

INSERT INTO couleur (id, nom) VALUES
(1, 'marron'),
(2, 'bleu'),
(3, 'vert');

CREATE TABLE chat (
    id INT NOT NULL AUTO_INCREMENT,
    nom VARCHAR(30) NOT NULL,
    couleur_id INT,
    PRIMARY KEY (id)
) ENGINE=InnoDB;

ALTER TABLE chat
ADD FOREIGN KEY (couleur_id) REFERENCES couleur(id)

INSERT INTO chat (nom, couleur_id) VALUES
('Maine coon', 1),
('Siamois', 2),
('Bengal', 1),
('Scottish Fold', 1),
('domestique', NULL);

```