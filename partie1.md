# Retrouver les requêtes SQL
## Les commandes :

### 1/ Obtenir la liste des 10 villes les plus peuplées en 2012

```sql
 SELECT * FROM villes_france_free ORDER BY ville_population_2012 DESC LIMIT 10 ;
```
### 2/ Obtenir la liste des 50 villes ayant la plus faible superficie

```sql
 SELECT * FROM villes_france_free ORDER BY ville_surface ASC LIMIT 50
 ```

### 3/ Obtenir la liste des départements d’outres-mer

```sql
SELECT * FROM departement WHERE `departement_code` LIKE '97%'
```

### 4/ Obtenir le nom des 10 villes + peuplées en 2012 & nom du département associé

```sql
 SELECT villes_france_free.ville_nom, departement.departement_nom, villes_france_free.ville_population_2012
 FROM villes_france_free
 JOIN departement ON departement.departement_code = villes_france_free.ville_departement
 ORDER BY ville_population_2012 DESC 
 LIMIT 10
 ```