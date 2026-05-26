# Retrouver les requêtes SQL 2
## LES COMMANDES

### 1/Obtenir l’utilisateur ayant le prénom “Muriel” et le mot de passe “test11”, sachant que l’encodage du mot de passe est effectué avec l’algorithme Sha1
```sql
SELECT prenom, client.password
FROM client
WHERE prenom = 'Muriel' AND password = SHA1('test11')
```

### 2/Obtenir la liste de tous les produits qui sont présent sur +ieurs commandes

```sql
SELECT commande_ligne.nom, COUNT(*) AS Produits_repeter
FROM commande_ligne
GROUP BY commande_ligne.nom
HAVING COUNT(*)> 1
```

### 3/Obtenir la liste de tous les produits qui sont présents sur +ieurs commandes et y ajouter une colonne qui liste les ids des commandes associées

```sql
SELECT commande_ligne.nom, commande_ligne.commande_id, COUNT(*) AS Produits_repeter
FROM commande_ligne 
GROUP BY commande_ligne.nom 
HAVING COUNT(*)> 1
```

### 4/Enregistrer le prix total à lintérieur de chaque ligne des commandes, en fonction de prix unitaire et de la quantité

```sql
UPDATE commande_ligne
SET commande_ligne.prix_total = (`prix_unitaire` * `quantite`)
```

### 5/Obtenir le montant total pour chaqque commande et y voir facilement la date associée à cette commande ainsi que le prénom et nom du client associé

```sql
SELECT client.nom, client.prenom, commande.date_achat, commande_id, SUM(`prix_total`) AS Montant_Total
FROM commande_ligne
INNER JOIN client ON client.id = commande_ligne.commande_id
INNER JOIN commande ON commande_ligne.commande_id = commande.client_id
GROUP BY client.nom, client.prenom, commande.date_achat, commande_id
```

### 6/Enregistrer le montant total de chaque commande dans le champs intitulé "cache_prix_total"

SELECT commande_ligne.commande_id, ROUND(SUM(commande_ligne.prix_total), 2) AS cache_prix_total
FROM commande_ligne
GROUP BY commande_ligne.commande_id

```sql
UPDATE commande
INNER JOIN (SELECT commande_ligne.commande_id, ROUND(SUM(commande_ligne.prix_total), 2) AS cache_prix_total
FROM commande_ligne
GROUP BY commande_ligne.commande_id) AS Result_total ON commande.id = Result_total.commande_id

SET commande.cache_prix_total = Result_total.cache_prix_total
```

### 7/Obtenir le montant global de toutes les commandes, pour chaque mois

```sql
SELECT DATE(`date_achat`) AS Group_Date_Achat, ROUND(SUM(commande.cache_prix_total), 2) AS Montant_Global
FROM commande
GROUP BY YEAR(commande.date_achat), MONTH(commande.date_achat)
ORDER BY YEAR(`date_achat`), MONTH(`date_achat`)
```

### 8/Obtenir la liste des 10 clients qui ont effectué le +> montant de commandes, et obtenir ce montant total pour chaque client

```sql
SELECT client.id, client.nom, client.prenom, ROUND(SUM(`cache_prix_total`), 2) AS Montant_Total
FROM commande
INNER JOIN client ON client.id = commande.client_id
GROUP BY commande.client_id
ORDER BY Montant_Total DESC
LIMIT 10
```

### 9/Obtenir le montant total des commandes pour chaque date

```sql
SELECT commande.date_achat, ROUND(SUM(commande.cache_prix_total), 2) AS Montant_total
FROM commande
GROUP BY commande.date_achat
```

### 10/Ajouter une colonne intitulée "Category" à la table contenant les commandes. Cette colonne contiendra une valeur numérique

```sql
ALTER TABLE `commande` ADD `category` INT UNSIGNED NOT NULL AFTER `cache_prix_total`
```

### 11/Enregistrer la valeur de la catégorie, en suivant les règles donnés :

```sql
UPDATE commande
SET commande.category = 
CASE 
WHEN commande.cache_prix_total < 200 THEN 1
WHEN commande.cache_prix_total BETWEEN 200 AND 500 THEN 2
WHEN commande.cache_prix_total BETWEEN 500 AND 1000 THEN 3
ELSE 4
END
```