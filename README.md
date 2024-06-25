## Project: Analyzing Electric Vehicle Charging Habits (SQL Beginner)
Link to the workbook: https://www.datacamp.com/datalab/w/b92031af-6bdb-49fc-99e0-075f7a70d92e/edit

When factoring heat generation required for the manufacturing and transportation of products, _Greenhouse gas emissions attributable to products, from food to sneakers to appliances, make up more than 75% of global emissions._ [The Carbon Catalogue](https://www.nature.com/articles/s41597-022-01178-9)

Our data, which is publicly available on [nature.com](https://www.nature.com/articles/s41597-022-01178-9), contains product carbon footprints (PCFs) for various companies. PCFs are the greenhouse gas emissions attributable to a given product, measured in CO<sub>2</sub> (carbon dioxide equivalent).

This data is stored in a PostgreSQL database containing one table, `product_emissions`, which looks at PCFs by product as well as the stage of production that these emissions occurred. Here's a snapshot of what `product_emissions` contains in each column:

### `product_emissions`

| field                              | data type |
|------------------------------------|-----------|
| `id`                                 | `VARCHAR`   |
| `year`                               | `INT`       |
| `product_name`                       | `VARCHAR`   |
| `company`                            | `VARCHAR`   |
| `country`                            | `VARCHAR`   |
| `industry_group`                     | `VARCHAR`   |
| `weight_kg`                          | `NUMERIC`   |
| `carbon_footprint_pcf`               | `NUMERIC`   |
| `upstream_percent_total_pcf`         | `VARCHAR`   |
| `operations_percent_total_pcf`       | `VARCHAR`   |
| `downstream_percent_total_pcf`       | `VARCHAR`   |

You'll use this data to examine the carbon footprint of each industry in the dataset! 

### Answering questions
1. Using the product_emissions table, find the number of unique companies and their total carbon footprint PCF for each industry group, filtering for the most recent year in the database. The query should return three columns: industry_group, num_companies, and total_industry_footprint, with the last column being rounded to one decimal place. The results should be sorted by total_industry_footprint from highest to lowest values.

```sql
-- Finding the most recent year first
SELECT *
FROM product_emissions
ORDER BY year DESC;
```

| id           | year | product_name                | company                                  | country | industry_group               | weight_kg | carbon_footprint_pcf | upstream_percent_total_pcf | operations_percent_total_pcf | downstream_percent_total_pcf |
|--------------|------|-----------------------------|------------------------------------------|---------|------------------------------|-----------|----------------------|----------------------------|------------------------------|-----------------------------|
| 12134-4-2017 | 2017 | Amines                      | Mitsubishi Gas Chemical Company, Inc.    | Japan   | Materials                    | 9         | 8                    | 63.68%                     | 33.73%                       | 2.60%                       |
| 12134-6-2017 | 2017 | Super-pure hydrogen peroxide| Mitsubishi Gas Chemical Company, Inc.    | Japan   | Materials                    | 17736     | 6469                 | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) |
| 12134-2-2017 | 2017 | Methacrylic acid            | Mitsubishi Gas Chemical Company, Inc.    | Japan   | Materials                    | 20        | 71                   | 64.44%                     | 34.13%                       | 1.43%                       |
| 12134-3-2017 | 2017 | Hydrogen peroxide           | Mitsubishi Gas Chemical Company, Inc.    | Japan   | Materials                    | 20        | 4                    | 65.22%                     | 34.54%                       | 0.24%                       |
| 10261-3-2017 | 2017 | Multifunction Printers      | Konica Minolta, Inc.                     | Japan   | Technology Hardware & Equipment | 110       | 2274                 | 20.05%                     | 3.61%                        | 76.34%                      |
| 12134-5-2017 | 2017 | Peroxides                   | Mitsubishi Gas Chemical Company, Inc.    | Japan   | Materials                    | 25        | 42                   | 63.82%                     | 33.80%                       | 2.38%                       |
| 10261-1-2017 | 2017 | Multifunction Printers      | Konica Minolta, Inc.                     | Japan   | Technology Hardware & Equipment | 110       | 1488                 | 30.65%                     | 5.51%                        | 63.84%                      |
| 12134-1-2017 | 2017 | Paraformaldehyde            | Mitsubishi Gas Chemical Company, Inc.    | Japan   | Materials                    | 500       | 200                  | 56.50%                     | 29.93%                       | 13.57%                      |
| 10261-2-2017 | 2017 | Multifunction Printers      | Konica Minolta, Inc.                     | Japan   | Technology Hardware & Equipment | 110       | 1818                 | 25.08%                     | 4.51%                        | 70.41%                      |
| 12134-7-2017 | 2017 | ELM                         | Mitsubishi Gas Chemical Company, Inc.    | Japan   | Materials                    | 160       | 139                  | 7.13%                      | 3.77%                        | 89.10%                      |


```sql
-- Writing the query according to requirements
SELECT industry_group, 
	COUNT(DISTINCT company) AS num_companies, 
	ROUND(SUM(carbon_footprint_pcf), 1) AS total_industry_footprint
FROM product_emissions
WHERE YEAR >= 2017
GROUP BY industry_group
ORDER BY total_industry_footprint DESC;
```

| industry_group                        | num_companies | total_industry_footprint |
|---------------------------------------|---------------|--------------------------|
| Materials                             | 3             | 107129                   |
| Capital Goods                         | 2             | 94942.7                  |
| Technology Hardware & Equipment       | 4             | 21865.1                  |
| Food, Beverage & Tobacco              | 1             | 3161.5                   |
| Commercial & Professional Services    | 1             | 740.6                    |
| Software & Services                   | 1             | 690                      |
