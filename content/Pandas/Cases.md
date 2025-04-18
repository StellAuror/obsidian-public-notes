---
title: Cases
draft:
tags:
---
- **Pandas** nie posiada czytelnego rozwiązania w tym zakresie, warto użyć tutaj `select()` z **NumPy**
	- Najpierw definiuje się listę **warunków**,
	- Następnie definiuje się listę **opcji**, gdzie kolejność odpowiada warunkom
```python
# słownik, w którym odwołujemy się do kolumn ramki danych
conditions = [
	(df['Discount'] <= .05),  # zniżka ≤ 5%
	(df['Discount'] > .05) & (df['Discount'] <= 10),  # zniżka > 5% i ≤ 10%
	(df['Discount'] > .1)  # zniżka > 10%
]

# liczba wynikowa, która odpowiada warunkom zdefiniowanym w słowniku (kolejność)
options = ['Low', 'Medium', 'High']
```
    
- i użycie `select()` 
```python
np.select(conditions, options, default='Unexpected Value')
```


### BDSM version
```python

# Przykładowe dane
df = pd.DataFrame({
    'Discount': [0.03, 0.07, 0.12, 0.15, 0.05]
})

# Określanie przedziałów
bins = [0, 0.05, 0.10, np.inf]
labels = ['Low', 'Medium', 'High']

# Przypisanie kategorii
pd.cut(df['Discount'], bins=bins, labels=labels, right=True)


```