#Instalación de librerías necesarias
!pip install pandas numpy lxml beautifulsoup4 seaborn matplotlib

#Importar librerías
import numpy as np
import pandas as pd
import requests
from bs4 import BeautifulSoup
import seaborn as sns
import matplotlib.pyplot as plt


URL = "https://web.archive.org/web/20230902185326/https://en.wikipedia.org/wiki/List_of_countries_by_GDP_%28nominal%29"
response = requests.get(URL)
soup = BeautifulSoup(response.text, "html.parser")

tables = pd.read_html(URL)
print(f"Número de tablas encontradas: {len(tables)}")

gdp_df = tables[3]
print(gdp_df.head(15))

#Nuevo dataframe con datos propios
gdp_df = pd.DataFrame({
    "Country": ["United States", "China", "Japan", "Germany", "India", 
                "United Kingdom", "France", "Italy", "Canada", "Brazil"],
    "GDP (Million USD)": [26854599, 19373586, 4409738, 4308854, 3736882, 
                          3158980, 2923489, 2169745, 2089672, 2081235]
})

#Convertir millones a miles de millones
gdp_df["GDP (Billion USD)"] = np.round(gdp_df["GDP (Million USD)"].astype(float) / 1000, 2)

#Eliminar la columna anterior si quieres
gdp_df.drop(columns=["GDP (Million USD)"], inplace=True)

print(gdp_df)

#Ordenar de mayor a menor PIB
gdp_df = gdp_df.sort_values(by="GDP (Billion USD)", ascending=False)

plt.figure(figsize=(10,6))
sns.barplot(
    x="GDP (Billion USD)",
    y="Country",
    data=gdp_df,
    hue="Country",        # <- Se agrega hue para aplicar la paleta correctamente
    palette="coolwarm",   # <- Mantiene los colores
    legend=False          # <- Evita que se muestre la leyenda innecesaria
)

plt.title("Top 10 Economies by GDP (Billion USD)", fontsize=14)
plt.xlabel("GDP (Billion USD)")
plt.ylabel("Country")
plt.tight_layout()
plt.show()
