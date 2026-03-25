# SQL - Étude de Cas : Gestion d'une Entreprise Informatique

## Partie A --- Structure

3.  ```sql
    CREATE database techdistrib ;
    ```

    ```sql
    CREATE TABLE Client (
        id_client INT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
        nom VARCHAR(50) NOT NULL,
        prenom VARCHAR(50) NOT NULL,
        ville VARCHAR(50),
        annee_naissance YEAR
    );
    ```

4.  ```sql
    CREATE TABLE Produit (
        id_produit INT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
        description VARCHAR(100),
        prix DECIMAL(10,2) NOT NULL,
        stock INT UNSIGNED DEFAULT 0
    );
    ```

5.  ```sql
    CREATE TABLE Commande (
        id_commande INT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
        date_commande DATE NOT NULL,
        id_client INT UNSIGNED
    );
    ```
6.  ```sql
    -- 1. Supprimer la contrainte existante
    ALTER TABLE Commande
    DROP FOREIGN KEY fk_commande_client;

    -- 2. Ajouter la nouvelle contrainte avec le Delete Cascade
    ALTER TABLE Commande
    ADD CONSTRAINT fk_commande_client
    FOREIGN KEY (id_client)
    REFERENCES Client (id_client)
    ON DELETE CASCADE;
    ```

7.  ```sql
    CREATE TABLE LigneCommande (
        id_ligne INT UNSIGNED PRIMARY KEY AUTO_INCREMENT,
        id_commande INT UNSIGNED REFERENCES Commande(id_commande),
        id_produit INT UNSIGNED,
        quantite INT UNSIGNED NOT NULL CHECK(quantite > 0)

    );
    ```

8.  ```sql
    -- Ajouter les clés étrangères sur LigneCommande
    ALTER TABLE LigneCommande
    ADD CONSTRAINT fk_ligne_commande FOREIGN KEY (id_commande) REFERENCES Commande(id_commande),
    ADD CONSTRAINT fk_ligne_produit FOREIGN KEY (id_produit) REFERENCES Produit(id_produit);

    -- Ajouter la contrainte UNIQUE sur la combinaison des deux colonnes
    ALTER TABLE LigneCommande
    ADD CONSTRAINT U_LigneCommande UNIQUE (id_commande, id_produit);
    ```

9.  ```sql
    -- Création d'index
    CREATE INDEX idx_name_client
    ON Client (nom);
    ```

10. ```sql
    ALTER TABLE Produit
    ADD COLUMN categorie ENUM('PC','Accessoire','Logiciel');
    ```

11. ```sql
    ALTER TABLE Produit
    MODIFY description VARCHAR(150);
    ```

12. ```sql
    -- Rename table
    ALTER TABLE Commande
    RENAME TO Commandes;
    ```

13. ```sql
    DROP TABLE IF EXISTS Temporaire;
    ```

## Partie B --- Manipulation des données

1. ```sql
   -- memo : INSERT INTO table VALUES ('valeur 1', 'valeur 2', ...)
   INSERT INTO Client (nom, prenom, ville, annee_naissance)
   VALUES
       ('Dupont', 'Jean', 'Saint-Etienne','1980'),
       ('Martin', 'Bob', 'Givors','2005'),
       ('Fontava', 'Theo', 'Lorette','1978'),
       ('Garcia', 'Erwan', 'Saint-Etienne','1962'),
       ('Lafontaine', 'Greg', 'Lyon','2001');
   ```

2. ```sql
   INSERT INTO Produit (description, prix, stock, categorie)
   VALUES
       ('Chaise de bureau cuire', 56.99, 26,'Accessoire'),
       ('Souris corsair-X546', 156.23, 12,'Accessoire'),
       ('PC gamer predator-2T', 1599.99, 5,'PC'),
       ('Antivirus 1an', 149.99, 465,'Logiciel'),
       ('Clavier SteelSeries apexpro TKL Wireless', 200, 7,'Accessoire');
   ```

3. ```sql
   INSERT INTO Commandes (date_commande, id_client)
   VALUES
       ('2026-03-24', 1),
       ('2026-03-20', 3),
       ('2026-01-03', 2);
   ```
