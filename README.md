# carbon-emission-analysis.md

## Data structure
### Table product_emission
``` sql
select * from product_emission limit 9;
```
Result
| id           | company_id | country_id | industry_group_id | year | product_name                                                    | weight_kg | carbon_footprint_pcf | upstream_percent_total_pcf | operations_percent_total_pcf | downstream_percent_total_pcf | 
| -----------: | ---------: | ---------: | ----------------: | ---: | --------------------------------------------------------------: | --------: | -------------------: | -------------------------: | ---------------------------: | ---------------------------: | 
| 10056-1-2014 | 82         | 28         | 2                 | 2014 | Frosted Flakes(R) Cereal                                        | 0.7485    | 2                    | 57.50                      | 30.00                        | 12.50                        | 
| 10056-1-2015 | 82         | 28         | 15                | 2015 | "Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)" | 0.7485    | 2                    | 57.50                      | 30.00                        | 12.50                        | 
| 10222-1-2013 | 83         | 28         | 8                 | 2013 | Office Chair                                                    | 20.68     | 73                   | 80.63                      | 17.36                        | 2.01                         | 
| 10261-1-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1488                 | 30.65                      | 5.51                         | 63.84                        | 
| 10261-2-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1818                 | 25.08                      | 4.51                         | 70.41                        |

## the most to carbon emissions produtcs
```sql
select 
	prd_em.product_name,
	Round(avg(prd_em.carbon_footprint_pcf),2) as Avg_PCF
from product_emissions as prd_em
group by prd_em.product_name
order by avg(prd_em.carbon_footprint_pcf) desc
limit 10;
```
| product_name                                                                                                                       | Avg_PCF    | 
| ---------------------------------------------------------------------------------------------------------------------------------: | ---------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | 3718044.00 | 
| Wind Turbine G132 5 Megawats                                                                                                       | 3276187.00 | 
| Wind Turbine G114 2 Megawats                                                                                                       | 1532608.00 | 
| Wind Turbine G90 2 Megawats                                                                                                        | 1251625.00 | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | 191687.00  | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | 167000.00  | 
| TCDE                                                                                                                               | 99075.00   | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | 91000.00   | 
| Mercedes-Benz S-Class (S 500)                                                                                                      | 85000.00   | 
| Mercedes-Benz SL (SL 350)                                                                                                          | 72000.00   | 
## The industry groups of these products
```sql
select 
	prd_em.product_name, ind_gr.industry_group,
	Round(avg(prd_em.carbon_footprint_pcf),2) as Avg_PCF
from product_emissions as prd_em
join industry_groups as ind_gr on prd_em.industry_group_id = ind_gr.id
group by prd_em.product_name, ind_gr.industry_group
order by avg(prd_em.carbon_footprint_pcf) desc
limit 10;
```
| product_name                                                                                                                       | industry_group                     | Avg_PCF    | 
| ---------------------------------------------------------------------------------------------------------------------------------: | ---------------------------------: | ---------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | Electrical Equipment and Machinery | 3718044.00 | 
| Wind Turbine G132 5 Megawats                                                                                                       | Electrical Equipment and Machinery | 3276187.00 | 
| Wind Turbine G114 2 Megawats                                                                                                       | Electrical Equipment and Machinery | 1532608.00 | 
| Wind Turbine G90 2 Megawats                                                                                                        | Electrical Equipment and Machinery | 1251625.00 | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | Automobiles & Components           | 191687.00  | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | Materials                          | 167000.00  | 
| TCDE                                                                                                                               | Materials                          | 99075.00   | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | Automobiles & Components           | 91000.00   | 
| Mercedes-Benz S-Class (S 500)                                                                                                      | Automobiles & Components           | 85000.00   | 
| Mercedes-Benz SL (SL 350)                                                                                                          | Automobiles & Components           | 72000.00   | 

## The industries with the highest contribution to carbon emissions
```sql
select 
	ind_gr.industry_group,
	Round(avg(prd_em.carbon_footprint_pcf),2) as Avg_PCF
from product_emissions as prd_em
join industry_groups as ind_gr on prd_em.industry_group_id = ind_gr.id
group by ind_gr.industry_group
order by avg(prd_em.carbon_footprint_pcf) desc
limit 10;
```
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

## What are the companies with the highest contribution to carbon emissions?
```sql
select 
	prd_em.company_id, c.company_name,
	Round(avg(prd_em.carbon_footprint_pcf),2) as Avg_PCF
from product_emissions as prd_em
join companies as c on prd_em.company_id = c.id
group by c.company_name
order by avg(prd_em.carbon_footprint_pcf) desc
limit 10;
```

| company_id | company_name                           | Avg_PCF    | 
| ---------: | -------------------------------------: | ---------: | 
| 10         | "Gamesa Corporación Tecnológica, S.A." | 2444616.00 | 
| 11         | "Hino Motors, Ltd."                    | 191687.00  | 
| 34         | Arcelor Mittal                         | 83503.50   | 
| 141        | Weg S/A                                | 53551.67   | 
| 61         | Daimler AG                             | 43089.19   | 
| 71         | General Motors Company                 | 34251.75   | 
| 139        | Volkswagen AG                          | 26238.40   | 
| 140        | Waters Corporation                     | 24162.00   | 
| 7          | "Daikin Industries, Ltd."              | 17600.00   | 
| 53         | CJ Cheiljedang                         | 15802.83   | 

## What are the countries with the highest contribution to carbon emissions?
```sql
select 
	prd_em.country_id, cou.country_name,
	Round(avg(prd_em.carbon_footprint_pcf),2) as Avg_PCF
from product_emissions as prd_em
join countries as cou on prd_em.country_id = cou.id
group by cou.country_name
order by avg(prd_em.carbon_footprint_pcf) desc
limit 10;
```

| country_id | country_name | Avg_PCF   | 
| ---------: | -----------: | --------: | 
| 23         | Spain        | 699009.29 | 
| 18         | Luxembourg   | 83503.50  | 
| 10         | Germany      | 33600.37  | 
| 3          | Brazil       | 9407.61   | 
| 22         | South Korea  | 5665.61   | 
| 16         | Japan        | 4600.26   | 
| 20         | Netherlands  | 2011.91   | 
| 12         | India        | 1535.88   | 
| 28         | USA          | 1332.60   | 
| 21         | South Africa | 1119.27   | 


## What is the trend of carbon footprints (PCFs) over the years?

