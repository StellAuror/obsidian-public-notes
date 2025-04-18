---
title: Summarize
draft:
tags:
---


- Użycie **funkcji agregujących** pozwala na wykonanie różnych obliczeń w ramach grup
    - metoda `agg()` umożliwia definiowanie wielu funkcji dla wielu kolumn
    - domyślnie tworzy wielopoziomowy nagłówek (MultiIndex) dla kolumn
    - najczęściej stosowana z `groupby()` w celu analizy danych
```python
df.groupby(['Region', 'Product']).agg(
    totalSales = ('Sales', 'sum'),
    avgDiscount = ('Sales', 'mean')
)
```

- Zastosowanie **niestandardowych funkcji z lambdą** rozszerza możliwości agregacji
    - pozwala na wykonanie dowolnego obliczenia na pogrupowanych danych
    - bardzo elastyczne, ale może wpływać na wydajność przy dużych zbiorach danych
    - szczególnie przydatne przy niestandardowych miarach statystycznych

```python
df.groupby('Region').agg({
    'Discount': lambda x: np.where(x<.1, 1, 0).mean(), # procent ofert w regionie z niską zniżką 
    'Sales': lambda x: x.sum() / x.count() # Średnia
}).round(1)

# TBH wygląda to strasznie, ale w rzeczywistości to zwykły słownik z funkcjami lambda
# także, upraczając, tworzymy słownik eval:
eval = {
    'Discount': lambda x: np.where(x<.1, 1, 0).mean(), # procent ofert w regionie z niską zniżką 
    'Sales': lambda x: x.sum() / x.count() # Średnia
}

# I stosujemy go w agg() i gites
df.groupby('Region').agg(eval).round(1)
```