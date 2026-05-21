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
### 5/Obtenir la liste du nom de chaque département, associé à son code et du nombre de commune au sein de ces département, en triant afin d’obtenir en priorité les départements qui possèdent le plus de communes

```sql
SELECT departement.departement_nom, departement.departement_code, COUNT(villes_france_free.ville_commune) AS Nombre_de_commune

FROM departement 

JOIN villes_france_free ON villes_france_free.ville_departement = departement.departement_code 

GROUP BY villes_france_free.ville_departement

ORDER BY Nombre_de_commune DESC;
```

### 6/Obtenir la liste des 10 + > département, en terme de superficie

```sql
SELECT villes_france_free.ville_nom, villes_france_free.ville_departement, villes_france_free.ville_surface

FROM villes_france_free

ORDER BY ville_surface DESC

LIMIT 10
```

### 7/Compter le nombre de ville dont le nom commence par "Saint"

```sql
SELECT COUNT(villes_france_free.ville_nom) AS Nom_par_Saint
FROM villes_france_free
WHERE villes_france_free.ville_nom LIKE 'Saint%'
```

### 8/Obtenir la liste des villes qui ont un nom existants plusieurs fois, et trier afin d'obtenir en premier celles dont le nom est le souvent utilisé par plusieurs communes

```sql
SELECT villes_france_free.ville_nom, COUNT(*) AS Noms_pareils
FROM villes_france_free
GROUP BY villes_france_free.ville_nom
ORDER BY Noms_pareils DESC
```