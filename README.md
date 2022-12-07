## Descrição

![GitHub top language](https://img.shields.io/github/languages/top/hbatistuzzo/Case-Itau-Henrique)
![GitHub commit activity](https://img.shields.io/github/commit-activity/m/hbatistuzzo/Case-Itau-Henrique)
![GitHub code size in bytes](https://img.shields.io/github/languages/code-size/hbatistuzzo/Case-Itau-Henrique)
![GitHub last commit](https://img.shields.io/github/last-commit/hbatistuzzo/Case-Itau-Henrique)

Project status: In Progress

# Project Summary

Given that it rains today, will it rain again tomorrow?
Real-time accurate rainfall prediction is challenging due to the inherent non-linear nature of weather. Using a database of meteorological variables in different cities of
Australia, the goal of this project is to train a model on short-term rainfall prediction (forecasting range up to 72 hours).---

# Technologies

- Python 3.8.13
	- Pandas 1.4.4
	- Numpy 1.21.5
	- Pycaret 2.3.10
	- Seaborn 0.11.2
	- Matplotlib 3.5.2
	- Scikit-learn 1.1
	- Seaborn 0.11.2
- Tableau 2022.3

---

## The Original Case (in portuguese):

Este case consiste em um exercício prático de construção de um modelo e quais insights você consegue extrair dos dados.

Desenhamos o case para que você possa mostrar as suas habilidades como cientista de dados.

O conjunto de dados que fornecemos contém observações diárias do clima de algumas estações meteorológicas na Austrália.
 
Os dados estão organizados em duas tabelas:

- `rain_data_aus.csv`: Contém a maior parte das informações, já centralizadas, de todas as estações.

- `wind_table_01.csv a wind_table_08.csv`: Contém informações sobre velocidade e orientação de ventos.

As tabelas estão apartadas, pois são originadas de um outro instrumento e salvas em um sistema apartado.

Submeta os arquivos em um repositório no git e nos envie o link para avaliação.

Idealmente, queremos poder replicar sua análise a partir dos códigos enviados. Portanto, especifique as versões das ferramentas e pacotes que você está usando.

---

## Data Description

Variable | Description
---------|------------
Date   |  The date of observation
Location   |  The common name of the location of the weather station
MinTemp   |  The minimum temperature in degrees celsius
MaxTemp   |  The maximum temperature in degrees celsius
Rainfall   |  The amount of rainfall recorded for the day in mm
Evaporation   |  The so-called Class A pan evaporation (mm) in the 24 hours to 9am
Sunshine   |  The number of hours of bright sunshine in the day.
WindGustDir   |  The direction of the strongest wind gust in the 24 hours to midnight
WindGustSpeed   |  The speed (km/h) of the strongest wind gust in the 24 hours to midnight
WindDir9am   |  Direction of the wind at 9am
WindDir3pm   |  Direction of the wind at 3pm
WindSpeed9am   |  Wind speed (km/hr) averaged over 10 minutes prior to 9am
WindSpeed3pm   |  Wind speed (km/hr) averaged over 10 minutes prior to 3pm
Humidity9am   |  Humidity (percent) at 9am
Humidity3pm   |  Humidity (percent) at 3pm
Pressure9am   |  Atmospheric pressure (hpa) reduced to mean sea level at 9am
Pressure3pm   |  Atmospheric pressure (hpa) reduced to mean sea level at 3pm
Cloud9am   |  Fraction of sky obscured by cloud at 9am. This is measured in "oktas", which are a unit of eigths. It records how many eigths of the sky are obscured by cloud. A 0 measure indicates completely clear sky whilst an 8 indicates that it is completely overcast.
Cloud3pm | Fraction of sky obscured by cloud (in "oktas": eighths) at 3pm. See Cload9am for a description of the values
Temp9am |  Temperature (degrees C) at 9am
Temp3pm |  Temperature (degrees C) at 3pm
Precipitation9am |  The amount of rain in mm prior to 9am
Precipitation3pm |  The amount of rain in mm prior to 3pm
AmountOfRain |  The amount of rain in mm
Temp |  Temperature (degrees C)
Humidity |  Humidity (percent)
RainToday |  Boolean: 1 if precipitation (mm) in the 24 hours to 9am exceeds 1mm, otherwise 0
RainTomorrow |  The target variable. Did it rain tomorrow?

---
## Data Inspection and Pre-processing: Rain data

|   |       date | location | mintemp | maxtemp | rainfall | evaporation | sunshine | humidity9am | humidity3pm | pressure9am | pressure3pm | cloud9am | cloud3pm | temp9am | temp3pm | raintoday | amountOfRain | raintomorrow |  temp |  humidity | precipitation3pm | precipitation9am | modelo_vigente |
|---|-----------:|---------:|--------:|--------:|---------:|------------:|---------:|------------:|------------:|------------:|------------:|---------:|---------:|--------:|--------:|----------:|-------------:|-------------:|------:|----------:|-----------------:|-----------------:|---------------:|
| 0 | 2008-12-01 |   Albury |    13.4 |    22.9 |      0.6 |         NaN |      NaN |        71.0 |        22.0 |      1007.7 |      1007.1 |      8.0 |      NaN |    16.9 |    21.8 |        No |          0.0 |           No | 29.48 | 28.400000 |               12 |         5.115360 |       0.089825 |
| 1 | 2008-12-02 |   Albury |     7.4 |    25.1 |      0.0 |         NaN |      NaN |        44.0 |        25.0 |      1010.6 |      1007.8 |      NaN |      NaN |    17.2 |    24.3 |        No |          0.0 |           No | 32.12 |  2.208569 |               10 |        21.497100 |       0.023477 |
| 2 | 2008-12-03 |   Albury |    12.9 |    25.7 |      0.0 |         NaN |      NaN |        38.0 |        30.0 |      1007.6 |      1008.7 |      NaN |      2.0 |    21.0 |    23.2 |        No |          0.0 |           No | 32.84 | 38.000000 |               17 |        20.782859 |       0.027580 |
| 3 | 2008-12-04 |   Albury |     9.2 |    28.0 |      0.0 |         NaN |      NaN |        45.0 |        16.0 |      1017.6 |      1012.8 |      NaN |      NaN |    18.1 |    26.5 |        No |          1.0 |           No | 35.60 | 21.200000 |                8 |        12.028646 |       0.023962 |
| 4 | 2008-12-05 |   Albury |    17.5 |    32.3 |      1.0 |         NaN |      NaN |        82.0 |        33.0 |      1010.8 |      1006.0 |      7.0 |      8.0 |    17.8 |    29.7 |        No |          0.2 |           No | 40.76 | 41.600000 |                9 |        11.883546 |       0.220164 |

**insights**
- _raintoday_ is either Yes or No, based on _rainfall_. Yes if _rainfall_ > 1.0, NO if _rainfall_ < 1.0
- _amountOfRain is the variable _rainfall_ with a 1-day lag. This column will be dropped to avoid data leaking.
- _modelo-vigente_ is an artificial column left in the case without further explanations. Will also be dropped (but let's keep an eye on it).
- _date_ is an object. Convert to datetime.
- No duplicates here.
- Several NaNs in several columns. Investigate.

<p align="right"><img src="images/rain_nans.png" width="100%" alt="rn"></p>

- pd.describe is always an useful tool to get an initial bird-eye view of the dataset:

|       |       mintemp |       maxtemp |      rainfall |  evaporation |     sunshine |   humidity9am |   humidity3pm |   pressure9am |   pressure3pm |     cloud9am |     cloud3pm |       temp9am |       temp3pm |          temp |      humidity | precipitation3pm | precipitation9am |
|-------|--------------:|--------------:|--------------:|-------------:|-------------:|--------------:|--------------:|--------------:|--------------:|-------------:|-------------:|--------------:|--------------:|--------------:|--------------:|-----------------:|-----------------:|
| count | 141556.000000 | 141871.000000 | 140787.000000 | 81350.000000 | 74377.000000 | 140419.000000 | 138583.000000 | 128179.000000 | 128212.000000 | 88536.000000 | 85099.000000 | 141289.000000 | 139467.000000 | 141871.000000 | 138583.000000 |    142193.000000 |    142193.000000 |
|  mean |     12.186400 |     23.226784 |      2.349974 |     5.469824 |     7.624853 |     68.843810 |     51.482606 |   1017.653758 |   1015.258204 |     4.437189 |     4.503167 |     16.987509 |     21.687235 |     28.505419 |     61.991179 |        10.014164 |        10.000748 |
|   std |      6.403283 |      7.117618 |      8.465173 |     4.188537 |     3.781525 |     19.051293 |     20.797772 |      7.105476 |      7.036677 |     2.887016 |     2.720633 |      6.492838 |      6.937594 |     10.237506 |     26.649111 |         3.169832 |         4.997908 |
|   min |     -8.500000 |     -4.800000 |      0.000000 |     0.000000 |     0.000000 |      0.000000 |      0.000000 |    980.500000 |    977.100000 |     0.000000 |     0.000000 |     -7.200000 |     -5.400000 |     -3.760000 |      2.000000 |         0.000000 |       -17.739346 |
|   25% |      7.600000 |     17.900000 |      0.000000 |     2.600000 |     4.900000 |     57.000000 |     37.000000 |   1012.900000 |   1010.400000 |     1.000000 |     2.000000 |     12.300000 |     16.600000 |     22.520000 |     44.000000 |         8.000000 |         6.650238 |
|   50% |     12.000000 |     22.600000 |      0.000000 |     4.800000 |     8.500000 |     70.000000 |     52.000000 |   1017.600000 |   1015.200000 |     5.000000 |     5.000000 |     16.700000 |     21.100000 |     28.520000 |     63.200000 |        10.000000 |        10.000009 |
|   75% |     16.800000 |     28.200000 |      0.800000 |     7.400000 |    10.600000 |     83.000000 |     66.000000 |   1022.400000 |   1020.000000 |     7.000000 |     7.000000 |     21.600000 |     26.400000 |     35.480000 |     80.000000 |        12.000000 |        13.389306 |
|   max |     33.900000 |     48.100000 |    371.000000 |   145.000000 |    14.500000 |    100.000000 |    100.000000 |   1041.000000 |   1039.600000 |     9.000000 |     9.000000 |     40.200000 |     46.700000 |     59.720000 |    122.000000 |        26.000000 |        32.478590 |

- I find that a "describe heatmap" is also useful to catch discrepancies in the data.

<p align="right"><img src="images/describe_heatmap.png" width="100%" alt="rn"></p>

**Additional info**

- rain_new['location'].unique().size yields 49 unique locations (cities).
- rain_new['location'].value_counts() shows that the input for each city is not equal. Some cities (e.g. Canberra, Sydney, Perth) have over 3.000 data points, while others
(e.g. Nhil, Katherine, Uluru) have less than 1.600 data points.
- rain_new['date'].unique() yields 3436 distinct dates. The dataset ranges from '2007-11-01' to '2017-06-25', about 9 years and a half of data.
- some preprocessing fits here: Yes and No to 1s and 0s respectively (the model appreciates!)

```
rain_new.loc[rain_new['raintoday'] == 'No','raintoday'] = 0
rain_new.loc[rain_new['raintoday'] == 'Yes','raintoday'] = 1

rain_new.loc[rain_new['raintomorrow'] == 'No','raintomorrow'] = 0
rain_new.loc[rain_new['raintomorrow'] == 'Yes','raintomorrow'] = 1
```

- Finally, our dataset shape is now 142193 rows × 21 columns

---

## Data Inspection: Wind data 1-8

Other than the rain data, we are also given 8 separate csv files with wind data. Let's import, concatenate, inspect and pre-process it.
<p align="right"><img src="images/wind_nans.png" width="100%" alt="wn"></p>

---

<p align="right"><img src="images/wind_nans_correto.png" width="100%" alt="wnc"></p>

---

<p align="right"><img src="images/merged_nans.png" width="100%" alt="mn"></p>

---

<p align="right"><img src="images/pairplot.png" width="100%" alt="pp"></p>

---

<p align="right"><img src="images/corr_heatmap.png" width="100%" alt="ch"></p>

---

<p align="right"><img src="images/distplot.png" width="100%" alt="dp"></p>