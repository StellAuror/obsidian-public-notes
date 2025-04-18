---
title: Filtering
draft:
tags:
---


- Użycie `loc` na wierszach - najbardziej intuicyjne
	- działa na zasadzie tworzenia wektorów logicznych
	- w `loc` pierwszy argument to wiersze, więc nie trzeba już podawać następnego argumentu z kolumnami - domyślnie będą wybierane wszystkie
```python
# Standardowe wybranie wierszy spełniających warunek logiczny
df.loc[(df['Sales'] >= 50) & (df['Sales'] != 70)]

# Albo trochę bardziej czytelnie...
cond1 = df['Sales'] >= 50 
cond2 = df['Sales'] != 70

df.loc[cond1 & cond2]

# Albo tak
selection = df['Product'].isin(['Apple', 'Banana'])

df.loc[cond1 & cond2 & selection]
```

- `query()` pozwala na filtrację danych przy użyciu wyrazu logicznego, podobnego do SQL
	- jako, że zapytanie jest tekstem, aby odwołać się do zmiennej środowiskowej należy użyć `@`
	- zwykle bardziej wydajne niż `loc`
```python
df.query('Sales >= 50 & Sales != 70')  # wybór wierszy z `Sales` >= 50, ale różnym od 70

# Użycie zmiennej środowiskowej
threshold=100
df.query('Sales < @threshold') # znak @ przed zmienną
```