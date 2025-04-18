---
title: Concatenate
draft:
tags:
---
- Jeżeli dane mają tę samą strukturę (takie same kolumny), można je połączyć pionowo
    - Do tego służy funkcja `pd.concat()`
    - Domyślnie dokleja po indeksie (`axis=0`)
    - Można resetować indeks lub zachować oryginalne indeksy
    - Warto podać `ignore_index=True`, jeśli chcemy uzyskać nowy ciągły indeks
    - `keys=` pozwala śledzić pochodzenie wierszy
```python
# podstawowe połączenie - row bind
pd.concat([df1, df2])

# nowy ciągły indeks
pd.concat([df1, df2], ignore_index=True)

# dodanie etykiety źródłowej
pd.concat([df1, df2], keys=['Q1', 'Q2'])
```

- Jeżeli kolumny się różnią, brakujące wartości zostaną uzupełnione `NaN`
```python
pd.concat([df1, df2], ignore_index=True)
```