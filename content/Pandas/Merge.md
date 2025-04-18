---
title: Merge
draft:
tags:
---


- Do łączenia dwóch tabel danych służy metoda `merge()` z `pandas`
    - Domyślnie wykonuje `inner join`
    - Możliwe typy złączeń `how`: `'left'`, `'right'`, `'outer'`, `'inner'`
    - Można łączyć po indeksach lub kolumnach
    - Możliwe jest także łączenie po wielu kolumnach
    - Warto korzystać z `reset_index()` lub `set_index()` dla większej kontroli
```python
# INDEX merge
pd.merge(df1, df2, left_index=True, right_index=True)

# COLUMN merge
pd.merge(df1, df2, on='sex smoker'.split())

# COLUMN merge (using index columns)
pd.merge(df1.reset_index(), df2.reset_index(), on='sex smoker'.split())

# METHODS
pd.merge(df1, df2, on='sex smoker'.split(), how='outer')
```
### Parametry wspomagające
- `validate=` — sprawdza spójność typów złączeń (np. `1:1`, `1:m`)
- `indicator=True` — tworzy kolumnę pokazującą pochodzenie wiersza (`both`, `left_only`, `right_only`)
```python
# walidacja relacji: one-to-one
pd.merge(df1.reset_index(), df2.reset_index(), on='sex smoker'.split(), validate='1:1')

# z kolumną pochodzenia
pd.merge(df1, df2, on='sex smoker'.split(), how='outer', indicator=True)
```