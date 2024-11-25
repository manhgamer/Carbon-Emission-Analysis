# Carbon-Emission-Analysis

## Data Structure
### Table product_emissions
```sql
SELECT * FROM product_emissions LIMIT 5;
```
Result

| id           | company_id | country_id | industry_group_id | year | product_name                                                    | weight_kg | carbon_footprint_pcf | upstream_percent_total_pcf | operations_percent_total_pcf | downstream_percent_total_pcf | 
| -----------: | ---------: | ---------: | ----------------: | ---: | --------------------------------------------------------------: | --------: | -------------------: | -------------------------: | ---------------------------: | ---------------------------: | 
| 10056-1-2014 | 82         | 28         | 2                 | 2014 | Frosted Flakes(R) Cereal                                        | 0.7485    | 2                    | 57.50                      | 30.00                        | 12.50                        | 
| 10056-1-2015 | 82         | 28         | 15                | 2015 | "Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)" | 0.7485    | 2                    | 57.50                      | 30.00                        | 12.50                        | 
| 10222-1-2013 | 83         | 28         | 8                 | 2013 | Office Chair                                                    | 20.68     | 73                   | 80.63                      | 17.36                        | 2.01                         | 
| 10261-1-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1488                 | 30.65                      | 5.51                         | 63.84                        | 
| 10261-2-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1818                 | 25.08                      | 4.51                         | 70.41                        | 

### Table industry_groups
```sql
SELECT * FROM industry_groups LIMIT 5
```
Result

| id | industry_group                                                         | 
| -: | ---------------------------------------------------------------------: | 
| 1  | "Consumer Durables, Household and Personal Products"                   | 
| 2  | "Food, Beverage & Tobacco"                                             | 
| 3  | "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | 
| 4  | "Mining - Iron, Aluminum, Other Metals"                                | 
| 5  | "Pharmaceuticals, Biotechnology & Life Sciences"                       | 

### Table companies
```sql
SELECT * FROM companies LIMIT 5
```
Result
| id | company_name                  | 
| -: | ----------------------------: | 
| 1  | "Autodesk, Inc."              | 
| 2  | "Casio Computer Co., Ltd."    | 
| 3  | "Cisco Systems, Inc."         | 
| 4  | "CNX Coal Resources, LP"      | 
| 5  | "Coca-Cola Enterprises, Inc." | 

### Table countries
```sql
SELECT * FROM countries LIMIT 5
```
Result
| id | country_name | 
| -: | -----------: | 
| 1  | Australia    | 
| 2  | Belgium      | 
| 3  | Brazil       | 
| 4  | Canada       | 
| 5  | Chile        | 

## Answer the Question
### 1. Which products contribute the most to carbon emissions?
```SQL
SELECT  
     product_name, 
     avg(carbon_footprint_pcf) AS Avg_PCF
FROM product_emissions
GROUP BY product_name
ORDER BY avg(carbon_footprint_pcf) DESC
LIMIT 10
```
Result
| product_name                                                                                                                       | Avg_PCF      | 
| ---------------------------------------------------------------------------------------------------------------------------------: | -----------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | 3718044.0000 | 
| Wind Turbine G132 5 Megawats                                                                                                       | 3276187.0000 | 
| Wind Turbine G114 2 Megawats                                                                                                       | 1532608.0000 | 
| Wind Turbine G90 2 Megawats                                                                                                        | 1251625.0000 | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | 191687.0000  | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | 167000.0000  | 
| TCDE                                                                                                                               | 99075.0000   | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | 91000.0000   | 
| Mercedes-Benz S-Class (S 500)                                                                                                      | 85000.0000   | 
| Mercedes-Benz SL (SL 350)                                                                                                          | 72000.0000   | 
### Answer: 
Wind Turbine, Steel Sheet Piles, Car Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV, Mercedes-Benz contribute the most to carbon emissions.

### 2. What are the industry groups of these products?
```sql
SELECT  
	 industry_group,
     product_name, 
     avg(carbon_footprint_pcf) AS Avg_PCF
FROM product_emissions prd_em
JOIN industry_groups ind_gr 
    ON prd_em.industry_group_id = ind_gr.id
GROUP BY industry_group, product_name
ORDER BY avg(carbon_footprint_pcf) DESC
LIMIT 10
```
Result
| industry_group                     | product_name                                                                                                                       | Avg_PCF      | 
| ---------------------------------: | ---------------------------------------------------------------------------------------------------------------------------------: | -----------: | 
| Electrical Equipment and Machinery | Wind Turbine G128 5 Megawats                                                                                                       | 3718044.0000 | 
| Electrical Equipment and Machinery | Wind Turbine G132 5 Megawats                                                                                                       | 3276187.0000 | 
| Electrical Equipment and Machinery | Wind Turbine G114 2 Megawats                                                                                                       | 1532608.0000 | 
| Electrical Equipment and Machinery | Wind Turbine G90 2 Megawats                                                                                                        | 1251625.0000 | 
| Automobiles & Components           | Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | 191687.0000  | 
| Materials                          | Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | 167000.0000  | 
| Materials                          | TCDE                                                                                                                               | 99075.0000   | 
| Automobiles & Components           | Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | 91000.0000   | 
| Automobiles & Components           | Mercedes-Benz S-Class (S 500)                                                                                                      | 85000.0000   | 
| Automobiles & Components           | Mercedes-Benz SL (SL 350)                                                                                                          | 72000.0000   | 

### Answer: Industry group of these products are Electrical Equipment and Machinery, Materials, Automobiles & Components        
### 3. What are the industries with the highest contribution to carbon emissions?
```SQL
SELECT  
	 industry_group,
     ROUND(AVG(carbon_footprint_pcf),2) AS Avg_PCF
FROM product_emissions prd_em
JOIN industry_groups ind_gr 
    ON prd_em.industry_group_id = ind_gr.id
GROUP BY industry_group
ORDER BY avg(carbon_footprint_pcf) DESC
LIMIT 10
```
Result
| industry_group                                   | Avg_PCF   | 
| -----------------------------------------------: | --------: | 
| Electrical Equipment and Machinery               | 891050.73 | 
| Automobiles & Components                         | 35373.48  | 
| "Pharmaceuticals, Biotechnology & Life Sciences" | 24162.00  | 
| Capital Goods                                    | 7391.77   | 
| Materials                                        | 3208.86   | 
| "Mining - Iron, Aluminum, Other Metals"          | 2727.00   | 
| Energy                                           | 2154.80   | 
| Chemicals                                        | 1949.03   | 
| Media                                            | 1534.47   | 
| Software & Services                              | 1368.94   | 
### Answer: the industries with the highest contribution to carbon emissions are Electrical Equiqment and Machinery.

### 4. What are the companies with the highest contribution to carbon emissions?
```sql
SELECT  
	 company_name,
     ROUND(AVG(carbon_footprint_pcf),2) AS Avg_PCF
FROM product_emissions prd_em
JOIN companies cpns
    ON prd_em.company_id = cpns.id
GROUP BY company_name
ORDER BY avg(carbon_footprint_pcf) DESC
LIMIT 10
```
Result
| company_name                           | Avg_PCF    | 
| -------------------------------------: | ---------: | 
| "Gamesa Corporación Tecnológica, S.A." | 2444616.00 | 
| "Hino Motors, Ltd."                    | 191687.00  | 
| Arcelor Mittal                         | 83503.50   | 
| Weg S/A                                | 53551.67   | 
| Daimler AG                             | 43089.19   | 
| General Motors Company                 | 34251.75   | 
| Volkswagen AG                          | 26238.40   | 
| Waters Corporation                     | 24162.00   | 
| "Daikin Industries, Ltd."              | 17600.00   | 
| CJ Cheiljedang                         | 15802.83   | 

### Answer: the companies with the highest contribution to carbon emissions are 1st. Gamesa Corporación Tecnológica, 2nd S.A, Hino Motors, Ltd., 3rd Arcelor Mittal 

### 5. What are the countries with the highest contribution to carbon emissions?
```sql
SELECT  
	 country_name,
     ROUND(AVG(carbon_footprint_pcf),2) AS Avg_PCF
FROM product_emissions prd_em
JOIN countries ctrs
    ON prd_em.company_id = ctrs.id
GROUP BY country_name
ORDER BY avg(carbon_footprint_pcf) DESC
LIMIT 10
```
Result
| country_name | Avg_PCF    | 
| -----------: | ---------: | 
| Germany      | 2444616.00 | 
| Greece       | 191687.00  | 
| Colombia     | 17600.00   | 
| Lithuania    | 13251.00   | 
| Italy        | 10000.00   | 
| India        | 9328.00    | 
| South Africa | 3550.50    | 
| China        | 3499.50    | 
| Canada       | 3025.00    | 
| Belgium      | 2344.00    | 

### Answer: Germany is one of the countries contribute highest carbon emissions, 2nd is Greece and 3rd iss Colombia

### 6. What is the trend of carbon footprints (PCFs) over the years?
```sql
SELECT
       ind_gr.industry_group AS 'Industry Group',
       ROUND(AVG(CASE WHEN pr_em.year = 2013 THEN pr_em.carbon_footprint_pcf ELSE 0 END), 2) AS '2013 Emission',
       ROUND(AVG(CASE WHEN pr_em.year = 2014 THEN pr_em.carbon_footprint_pcf ELSE 0 END), 2) AS '2014 Emission',
       ROUND(AVG(CASE WHEN pr_em.year = 2015 THEN pr_em.carbon_footprint_pcf ELSE 0 END), 2) AS '2015 Emission',
       ROUND(AVG(CASE WHEN pr_em.year = 2016 THEN pr_em.carbon_footprint_pcf ELSE 0 END), 2) AS '2016 Emission',
       ROUND(AVG(CASE WHEN pr_em.year = 2017 THEN pr_em.carbon_footprint_pcf ELSE 0 END), 2) AS '2017 Emission'
FROM product_emissions pr_em
JOIN industry_groups ind_gr ON ind_gr.id = pr_em.industry_group_id
GROUP BY ind_gr.industry_group
ORDER BY ind_gr.industry_group ASC;
```
