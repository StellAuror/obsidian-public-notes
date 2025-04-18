---
title: Remaining
draft:
tags:
---


1. **`dropna()`** – usuwa wiersze lub kolumny zawierające brakujące wartości  
   `df.dropna()`  
   `df.dropna(axis=1)`  

2. **`fillna()`** – uzupełnia brakujące wartości podaną wartością lub metodą  
   `df.fillna(0)`  
   `df.fillna(method='ffill')`  

3. **`isna()` / `isnull()`** – zwraca maskę wartości brakujących (`NaN`)  
   `df.isna()`  
   `df['col'].isnull()`  

4. **`notna()` / `notnull()`** – zwraca maskę wartości niebrakujących  
   `df.notna()`  
   `df['col'].notnull()`  

5. **`replace()`** – zamienia wskazane wartości na inne  
   `df.replace({'unknown': np.nan})`  
   `df['col'].replace(0, np.nan)`  

6. **`drop_duplicates()`** – usuwa zduplikowane wiersze  
   `df.drop_duplicates()`  
   `df.drop_duplicates(subset=['col1', 'col2'])`  

7. **`duplicated()`** – zwraca maskę zduplikowanych wierszy  
   `df.duplicated()`  
   `df.duplicated(subset=['col'])`  

8. **`astype()`** – konwertuje kolumnę do innego typu danych  
   `df['col'].astype('int')`  
   `df.astype({'col1': 'float', 'col2': 'string'})`  

9. **`str` accessor** – operacje tekstowe na kolumnach typu string  
    `df['col'].str.lower()`  
    `df['col'].str.replace(' ', '_')`  

10. **`rename()`** – zmienia nazwy kolumn lub indeksów  
    `df.rename(columns={'old': 'new'})`  
    `df.rename(index={0: 'first'})`  

11. **`set_index()` / `reset_index()`** – ustawia kolumnę jako indeks lub przywraca indeks do kolumny  
    `df.set_index('id')`  
    `df.reset_index()`  

12. **`value_counts()`** – zlicza wystąpienia unikalnych wartości w serii  
    `df['col'].value_counts()`  

13. **`cut()` / `qcut()`** – dzieli dane na przedziały (cut) lub kwantyle (qcut)  
    `pd.cut(df['age'], bins=[0,18,65,100])`  
    `pd.qcut(df['score'], q=4)`  

14. **`pd.to_datetime()`** – konwertuje dane do formatu datetime  
    `pd.to_datetime(df['date'])`  

15. **`pd.to_numeric()`** – konwertuje dane do liczb, opcjonalnie ignorując błędy  
    `pd.to_numeric(df['col'], errors='coerce')`  

16. **`pd.get_dummies()`** – koduje zmienne kategoryczne jako zmienne zero-jedynkowe  
    `pd.get_dummies(df['category'])`  

17. **`interpolate()`** – uzupełnia brakujące dane przez interpolację  
    `df['value'].interpolate()`  
    `df.interpolate(method='linear', limit_direction='forward')`
