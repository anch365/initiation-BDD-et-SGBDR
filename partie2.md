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