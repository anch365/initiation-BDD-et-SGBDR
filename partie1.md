# Retrouver les requêtes SQL
## Les commandes :

### 1/ Obtenir la liste des 10 villes les plus peuplées en 2012
<p> SELECT * FROM villes_france_free ORDER BY ville_population_2012 DESC LIMIT 10 ; </p>

### 2/ Obtenir la liste des 50 villes ayant la plus faible superficie
<p> SELECT * FROM villes_france_free ORDER BY ville_surface ASC LIMIT 50</p>

### 3/ Obtenir la liste des départements d’outres-mer
<p>SELECT * FROM departement WHERE departement_id >= 97</p>