---
title: Mapping
draft:
tags:
---


### Map
- `map()` działa **tylko na pojedynczej kolumnie (Series)**. Można używać:
	- funkcji
	- słownika (do zamiany wartości)
	- obiektów typu `str.format`
```python
df = pd.DataFrame({'grade': ['A', 'B', 'A', 'C']})

# Mapa zamieniająca litery na liczby
df['grade_num'] = df['grade'].map({'A': 5, 'B': 4, 'C': 3})
```

```python
# Zastosowanie funkcji
df['grade_upper'] = df['grade'].map(lambda x: x.lower())
```

### Apply
- `apply()`
- Gdy stosowane do **Series** → działa jak `map()`, ale z większą elastycznością (np. dostęp do wielu kolumn)
- Gdy stosowane do **DataFrame** → działa **na wierszach lub kolumnach**
#### ✅ Przykłady dla kolumny:
```python
df['score'] = [10, 20, 30, 40]
df['score_squared'] = df['score'].apply(lambda x: x**2)
```
#### ✅ Przykład dla całego DataFrame (kolumny):
```python
df[['score', 'score_squared']].apply(np.log1p)  # log(1 + x) dla każdej komórki
```
#### ✅ Przykład dla wierszy:
```python
def row_sum(row):
    return row['score'] + row['score_squared']

df['total'] = df.apply(row_sum, axis=1)
```

### Applymap
- `applymap()`
- Stosuje funkcję **do każdej pojedynczej komórki** w całym DataFrame.
#### ✅ Przykład:
```python
df_numeric = pd.DataFrame([[1, 2], [3, 4]])
df_numeric.applymap(lambda x: x * 10)
```