---
title: Column Selection
draft:
tags:
---

### Podstawowe
- Najszybsza metoda to po prostu wrzucenie listy z nazwami kolumn `df[['Date', 'Product']]`
- Bardziej wygodną opcją jest metoda `loc`, czyli **location**
	- obsługuje zarówno wybór wierszy, jak i kolumn `loc[wiersze,kolumny]`
	- znak `:` oznacza, że wybieramy wszystkie kolumny lub wiersze, np. `df.loc[:, :]` - czyli de facto cała tabela
	- `:` oznacza od - do, czyli `df.loc[:, 'Date':'Price']` wybiera wszystkie kolumny od `Sales` do `Price`
	- można też po prostu wybrać kolumny ręcznie `df.loc[:, ['Date', 'Price']`
	- albo wybrać kolumny poza tymi zdefiniowanymi
- alternatywą jest `iloc`, czyli **indexed location**, w tym przypadku wystarczy podać index - wartość numeryczną
- Albo zrzucić za pomocą `drop()` podając argument `columns`
```python
# Standard
df[['Date', 'Product', 'Sales']]  # wybór tylko tych trzech kolumn

# loc & iloc
df.loc[:, 'Date':'Price']  # wybiera wszystkie kolumny od `Date` do `Price`
df.loc[:, ['Price', 'Date']]  # wybór kolumn `Price` i `Date`
df.loc[:, df.columns != 'Date']  # usuwanie kolumny 'Date'
df.iloc[:, 0:2] # wybiera kolumny od 0 i 1

# drop
df.drop(columns=['Date'])# usunięcie kolumny 'Date'  
```
### Zaawansowane
- Istnieje też bardziej zaawansowana metoda `filter()`
	- `items` - po prostu wybieranie konkretnych kolumn
	- `like` - czyli elementy zawierające określony znak
	- `regex` - konstruktor zaawansowanych wyrażeń (regular expressions), warto korzystać z https://regex101.com
	- `axis` - wybieranie osi tabeli: `0` to wiersze, `1` to kolumny, domyślnie `1`
```python
df.filter(items=['Product', 'Sales'])  # wybór kolumn 'Product' i 'Sales'
df.filter(like='P', axis=1) # wybór wszystkich kolumn na 'P'
df.filter(regex='^[PS]', axis=1) # wybór wszystkich kolumn na 'P' LUB 'S'
```

- Inne metody
```python
# Wybieranie określonego typu kolumny
df.select_dtypes(include=['number'])

# wybieranie kolumn z kardynalnością większą niż 5
selection = [col for col in df.columns if df[col].nunique() > 5]
df[selection]
```