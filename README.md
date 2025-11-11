#Gross-domestic-product-project-on-python
El proyecto extrae y analiza datos del PIB mundial desde una página web usando Pandas. Se selecciona la tabla del FMI, se limpian los datos para conservar país y PIB, y se convierten los valores de millones a miles de millones con NumPy. El resultado es una tabla ordenada con los principales países por PIB, lista para análisis o visualización.
Gross-domestic-product-project-on-python
El proyecto extrae y analiza datos del PIB mundial desde una página web usando Pandas. Se selecciona la tabla del FMI, se limpian los datos para conservar país y PIB, y se convierten los valores de millones a miles de millones con NumPy. El resultado es una tabla ordenada con los principales países por PIB, lista para análisis o visualización.

!pip install pandas numpy 
!pip install lxml
!pip install beautifulsoup4
import numpy as np
import pandas as pd
import requests
from bs4 import BeautifulSoup
#exatrae informacion de la pagina web usando web scraping
#get te perimite obtener la "respuesta" y tenerla en tu servidor que en este caso es tu compuetador
URL = "https://web.archive.org/web/20230902185326/https://en.wikipedia.org/wiki/List_of_countries_by_GDP_%28nominal%29"
response = requests.get(URL)
soup = BeautifulSoup(response.text, "html.parser")
#tables va a ser lista que contiene todas las tablas que pandas.read_html() encontró en la página web.
#len(tables) devuelve cuántas tablas hay dentro de esa lista.
#El texto dentro de f"..." es un f-string (una forma moderna de formatear cadenas en Python), que permite insertar variables dentro de un texto fácilmente.
tables = pd.read_html(URL)
print(f"Número de tablas encontradas: {len(tables)}")
gdp_df = tables[3]
print(gdp_df.head(15))

#el resultado que nos dio es por las tres fuentes de datos que hay en la tabla, por eso Pandas lee e interpreta las tres por separado, ya tu escoges con cual te quedas
#lo que sigue es el proceso de limpieza y preparación del DataFrame que acabamos de extraer. 
#Se usa el gdp de IMF y noel del banco internacional porque sus datos son mas actualizados, el banco publica dos veces al año y hace proyecciones para el siguiente mientras que IMF tiene cifras actualizadas

gdp_df.columns = range(gdp_df.shape[1])
gdp_df = gdp_df[[0, 2]]
gdp_df = gdp_df.iloc[1:11]
gdp_df.columns = ["Country", "GDP (Million USD)"]

print(gdp_df)
#tareas: # Change the data type of the 'GDP (Million USD)' column to integer. Use astype() method.

#Convert the GDP value in Million USD to Billion USD

#Use numpy.round() method to round the value to 2 decimal places.

#Rename the column header from 'GDP (Million USD)' to 'GDP (Billion USD)'

gdp_df = pd.DataFrame({
    "Country": ["United States", "China", "Japan", "Germany", "India", "United Kingdom", "France", "Italy","Canada", "Brazil"],
    "GDP (Million USD)": [26854599, 19373586, 4409738, 4308854, 3736882,  315898, 2923489, 2169745, 2089672, 2081235
]
})

#Convertir de millones a miles de millones (billions)
gdp_df["GDP (Million USD)"] = gdp_df["GDP (Million USD)"].astype(float) / 1000

#Redondear a 2 decimales con NumPy
gdp_df["GDP (Million USD)"] = np.round(gdp_df["GDP (Million USD)"], 2)

#Renombrar la columna
gdp_df.rename(columns={"GDP (Million USD)": "GDP (Billion USD)"}, inplace=True)

#Mostrar resultado final
print(gdp_df)
 gdp_df.to_csv('./Largest_economies.csv')
