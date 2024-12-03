# Partie 1 - Création de la table

```sql
DROP DATABASE IF EXISTS location_ski;

CREATE DATABASE location_ski CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

USE location_ski;

CREATE TABLE clients (
    noCli INT NOT NULL AUTO_INCREMENT,
    nom VARCHAR(30) NOT NULL,
    prenom VARCHAR(30),
    adresse VARCHAR(120),
    cpo VARCHAR(5) NOT NULL,
    ville VARCHAR(80) NOT NULL,
    PRIMARY KEY (noCli)
) ENGINE=InnoDB;

CREATE TABLE fiches (
    noFic INT NOT NULL AUTO_INCREMENT,
    noCli INT NOT NULL,
    dateCrea DATE NOT NULL,
    datePaiement DATE,
    etat ENUM('SO', 'EC', 'RE'),
    PRIMARY KEY (noFic)
) ENGINE=InnoDB;

CREATE TABLE lignesFic (
    noLig INT NOT NULL,
    noFic INT NOT NULL,
    refart CHAR(8) NOT NULL,
    depart DATE NOT NULL,
    retour DATE,
    PRIMARY KEY (noLig, noFic)
) ENGINE=InnoDB;

CREATE TABLE articles (
    refart CHAR(8) NOT NULL,
    designation VARCHAR(80) NOT NULL,
    codeGam char(5),
    codeCate char(5),
    PRIMARY KEY (refart)
) ENGINE=InnoDB;

CREATE TABLE categories (
    codeCate char(5) NOT NULL,
    libelle VARCHAR(30) NOT NULL,
    PRIMARY KEY (codeCate)
) ENGINE=InnoDB;

CREATE TABLE gammes (
    codeGam char(5) NOT NULL,
    libelle VARCHAR(30) NOT NULL,
    PRIMARY KEY (codeGam)
) ENGINE=InnoDB;

CREATE TABLE grilleTarifs (
    codeCate char(5) NOT NULL,
    codeGam char(5) NOT NULL,
    codeTarif char(5),
    PRIMARY KEY (codeGam, codeCate)
) ENGINE=InnoDB;

CREATE TABLE tarifs (
    codeTarif char(5) NOT NULL,
    libelle VARCHAR(30) NOT NULL,
    prixJour DECIMAL(5,0) NOT NULL,
    PRIMARY KEY (codeTarif)
) ENGINE=InnoDB;

-- Clef secondaire

ALTER TABLE fiches
ADD CONSTRAINT fk_fiches_clients FOREIGN KEY (noCli) REFERENCES clients(noCli);

ALTER TABLE lignesFic
ADD CONSTRAINT fk_lignesFic_articles FOREIGN KEY (refart) REFERENCES articles(refart); 

ALTER TABLE articles 
ADD CONSTRAINT fk_articles_gammes FOREIGN KEY (codeGam) REFERENCES gammes(codeGam), 
ADD CONSTRAINT fk_articles_categories FOREIGN KEY (codeCate) REFERENCES categories(codeCate); 
 

ALTER TABLE grilleTarifs 
ADD CONSTRAINT fk_grilleTarifs_tarifs FOREIGN KEY (codeTarif) REFERENCES tarifs(codeTarif);
```

# Partie 2 - Requêtes

## **1** - Liste des clients (toutes les informations) dont le nom commence par un <code>D</code>

```sql
SELECT clients.noCli, clients.nom, clients.prenom, clients.adresse, clients.cpo, clients.ville
FROM clients
WHERE clients.nom LIKE 'D%'
```

|noCli|nom|prenom|adresse|cpo|ville|
|---|---|---|---|---|---|
|3|Dupond|Camille|Rue Crébillon|44000|Nantes|
|4|Desmoulin|Daniel|Rue descendante|21000|Dijon|
|9|Dupond|Jean|Rue des mimosas|75018 |Paris|

## **2** - Nom et prénom de tous les clients

```sql
SELECT clients.prenom, clients.nom
FROM clients
```

|prenom|nom|
|---|---|
|Albert|Anatole|
|Bernard|Barnab|
|Dupond|Camille|
|Desmoulin|Daniel|
|Ferdinand|François|
|Albert|Anatole|
|Dupond|Jean|
|Boutaud|Sabine|

## **3** - Liste des fiches (n°, état) pour les clients (nom, prénom) qui habitent en Loire Atlantique (44)

```sql
SELECT fiches.noFic, fiches.etat, clients.nom, clients.prenom
FROM fiches
LEFT JOIN clients ON fiches.noCli = clients.noCli
WHERE clients.cpo LIKE '44%'
```

| noFic | etat | nom       | prenom   |
| ----- | ---- | --------- | -------- |
| 1004  | EC   | Ferdinand | François |
| 1005  | EC   | Dupond    | Camille  |

## **4** - Détail de la fiche n°1002

```sql
SELECT fiches.noFic, clients.nom, clients.prenom, lignesFic.refart, articles.designation, lignesFic.depart, lignesFic.retour,  tarifs.prixJour, DATEDIFF(IFNULL(lignesFic.retour, NOW()), lignesFic.depart) * tarifs.prixJour AS montant
FROM lignesFic
JOIN fiches ON lignesFic.noFic = fiches.noFic 
JOIN clients ON fiches.noCli = clients.noCli 
JOIN articles ON lignesFic.refart = articles.refart 
JOIN grilleTarifs ON articles.codeGam = grilleTarifs.codeGam AND articles.codeCate = grilleTarifs.codeCate 
JOIN tarifs ON grilleTarifs.codeTarif = tarifs.codeTarif 
WHERE fiches.noFic = 1002;
```

|noFic|nom|prenom|refart|designation|depart|retour|prixJour|montant|
|---|---|---|---|---|---|---|---|---|
|1002|Desmoulin|Daniel|A03|Salomon 24X+Z12|2024-11-18|2024-11-22|10|50|
|1002|Desmoulin|Daniel|A04|Salomon 24X+Z12|2024-11-19|2024-11-24|10|60|
|1002|Desmoulin|Daniel|S03|Décathlon Apparition|2024-11-23|NULL|10|90|
