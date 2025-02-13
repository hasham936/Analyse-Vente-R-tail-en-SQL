# Projet d'Analyse de Ventes Retail en SQL

### Intro

```sql
CREATE DATABASE sql_project;

CREATE TABLE retails_sales
(
    transactions_id INT PRIMARY KEY,
    sale_date DATE,	
    sale_time TIME,
    customer_id INT,	
    gender VARCHAR(10),
    age INT,
    category VARCHAR(35),
    quantity INT,
    price_per_unit FLOAT,	
    cogs FLOAT,
    total_sale FLOAT
);
```
### Analyse des Données et Résultats

Requêtes SQL répondant à des questions concrètes :

1. **Récupérer toutes les ventes du '2022-11-05'**:
```sql
SELECT *
FROM retails_sales
WHERE sale_date = '2022-11-05';
```

2. **Transactions avec la categorie 'Clothing' avec +4 unités vendues en Novembre 2022**:
```sql
SELECT * 
FROM retails_sales 
WHERE category = 'Clothing' 
AND DATE_FORMAT(sale_date, '%Y-%m') = '2022-11' 
AND quantity > 4;
```

3. **Calcule le total des ventes pour chaque catégorie**:
```sql
SELECT 
    category,
    SUM(total_sale) as net_sale,
    COUNT(*) as total_orders
FROM retails_sales
GROUP BY 1
```

4. **Âge moyen des clients de la catégorie 'Beauty'**: 
```sql
SELECT
    ROUND(AVG(age), 2) as avg_age
FROM retails_sales
WHERE category = 'Beauty'
```

5. **Transactions avec totale des ventes > 1000**: 
```sql
SELECT * FROM retails_sales
WHERE total_sale > 1000
```

6. **Nombre de transactions faite par chaque genre et chaque catégorie**:
```sql
SELECT 
    category,
    gender,
    COUNT(*) as total_trans
FROM retails_sales
GROUP BY 
    category,
    gender
ORDER BY 1
```

7. **Trouver le top 5 des meilleurs client basé sur la vente totale**:
```sql
SELECT 
    customer_id,
    SUM(total_sale) as total_sales
FROM retails_sales
GROUP BY 1
ORDER BY 2 DESC
LIMIT 5
```

8. **Trouver le nombre client unique qui à acheté par catégorie**:
```sql
SELECT 
    category,    
    COUNT(DISTINCT customer_id) as cnt_unique_cs
FROM retails_sales
GROUP BY category
```

9. **Créer 3 shift and nombre de commande (Matin < 12, Midi entre 12 et 17, Soire > 17)**:
```sql
WITH hourly_sale AS (
SELECT
*,
CASE
WHEN HOUR(sale_time) < 12 THEN 'Morning'
WHEN HOUR(sale_time) BETWEEN 12 AND 17 THEN 'Afternoon'
ELSE 'Evening'
END AS shift
FROM retails_sales
LIMIT 20
)
SELECT
shift,  -- Utilisation du nom correct
COUNT(*) AS total_orders
FROM hourly_sale
GROUP BY shift;
```

