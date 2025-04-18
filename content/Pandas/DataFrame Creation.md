---
title: DataFrame Creation
draft:
tags:
---

- Najprostszym rozwiązaniem jest utworzenie **słownika**.
- **Słowniki** można bezpośrednio przekształcać w ramki danych - są wspierane przez pandas
- `split()` rozbija ciągi tekstowe, domyślnie `spacja`, ale można zdefiniować własny argument, np. `split(';')` 
```python
data = {
    'Date': '2025-03-01 2025-03-01 2025-03-02 2025-03-02 2025-03-03 2025-03-03'.split(),
    'Product': 'Apple Banana Orange Apple Banana Orange Apple Banana Orange'.split(),
    'Sales': [50, 30, 20, 60, 40, 10, 55, 25, 15],
    'Price': [0.5, 0.2, 0.7, 0.5, 0.2, 0.7, 0.5, 0.2, 0.7],
    'Region': 'North North North South South South North North North'.split(),
    'Discount': '5% 30% 15% 10% 20% 25% 5% 30% 15%'.split()
}
```

- Teraz można zamienić słownika `data` na ramkę danych z **pandas** za pomocą `DataFrame()`
- A następnie wyczyścić dane, np.
	- zamieniając **Date** na rzeczywisty format daty funkcją `to_datetime()` i argumentem `format`
	- **Discount** zmienić z znaków tekstowych na floating metodami `replace()` oraz `astype(float)`

```python
# Sprawdzanie, czy długości odpowiadają sobie w każdej kolumnie - ramki danych muszę mieć tyle samo wierszy w każdej z kolumn
for key, value in data.items():
    print(len(value))
    
# i utworzenie ramki danych pandas
df = pd.DataFrame(data)


# funkcja to_datetime() konwertuje tekst na datę po dostarceniu odpowiedniego foramtu daty
df['Date'] = pd.to_datetime(df['Date'], format='%Y-%m-%d')

# najpierw metoda replace() do usunięcia procentów, następnie astype(float) do zmiany foramtu
df['Discount'] = df['Discount'].replace('%', '', regex=True).astype(float) / 100

```