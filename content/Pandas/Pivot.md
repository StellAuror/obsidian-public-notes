---
title: Pivot
draft:
tags:
---


- **Tworzenie szerszego pivotu** pozwala na przekształcenie danych z formatu długiego na szeroki
    - ułatwia analizę porównawczą między kategoriami
    - wyniki są bardziej czytelne dla ludzi, ale trudniejsze do dalszych przekształceń
    - znacznie ogranicza redundancję danych (nie powtarza kategorii)
	- Metoda przyjmuje następujące **argumenty**:
	    - `index='Date'` - kolumna, która stanie się indeksem w nowej tabeli
	    - `columns='Product'` - kolumna, której unikalne wartości staną się nowymi kolumnami
	    - `values='Sales'` - kolumna, której wartości wypełnią komórki nowej tabeli
	    - `reset_index()` - opcjonalne, pozwala na przekształcenie indeksu w standardową kolumnę
```python
df_wide = df.pivot(index='Date', columns='Product', values='Sales').reset_index()
```

- **Tworzenie dłuższego pivotu** (`melt()`) to odwrotna operacja do pivot
    - przekształca dane z formatu szerokiego na długi
    - przydatne do analizy statystycznej, która często wymaga formatu długiego
    - zwiększa redundancję danych, ale ułatwia filtrowanie i grupowanie
    - Kluczowe **argumenty** dla `melt()`:
	    - `id_vars='Date'` - kolumny, które pozostaną niezmienione
	    - `var_name='Product'` - nazwa nowej kolumny przechowującej nagłówki kolumn z tabeli źródłowej
	    - `value_name='Sales'` - nazwa nowej kolumny przechowującej wartości
```python
df_long = df_wide.melt(id_vars='Date', var_name='Product', value_name='Sales')
```

Pewnie! Oto krótkie, uporządkowane notatki w stylu pasującym do Twoich wcześniejszych — dotyczące wczytywania i zapisywania plików w pandas, w tym również z GitHuba: