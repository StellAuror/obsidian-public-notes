---
title: Sort
draft:
tags:
---


- Sortowanie ramki danych można realizować na dwa sposoby
	- poprzez wartości metodą `.sort_values()`
		- `axis` - *integer* wskazujący oś sortowania **0** - wiersze (default); **1** - kolumny
		- `by` - czyli *lista* kolumn, które będą sortowane
		- `ascending` - *boolean* wskazujący kolejność sortowania
		- `inplace` - *boolean* czy od razu nadpisać dane
		- `kind` - *string* wskazujący na typ sortowania: 
			- **quicksort**: szybki, ale niestabilny (może zmienić kolejność równych elementów).
			- **mergesort** / **stable**: zachowują kolejność elementów o tych samych wartościach
			- **heapsort**: kompromis między stabilnością a wydajnością, ale także niestabilny.
		- `na_position` - *string* wskazujący pozycję wartości *NA*: **first** lub **last**
	- poprzez index metodą `.sort_index()`, dodatkowe argumenty to
		- `level` - *integer* wskazujący poziom sortowania (dla multi-index), np. **1**
```python
df.sort_values(
	by='item_name item_price quantity'.split(),		# sexi zdefiniowanie kolumn do sortowania
	ascending=[False]*3 							# też sexi, powielanie działa tylko z listą!!!
)
```

### Manually Defined Criteria
- Do sortowania na podstawie własnej kolejności można użyć typu danych kategorycznych `Categorical()`
	- 1. **`values`** (wymagany):
	    - Lista, tablica **NumPy**, seria **Pandas** lub inny iterowalny obiekt, który zawiera dane do zaklasyfikowania jako kategorie.
	    - Przykład: **['a', 'b', 'a', 'c']**.
	2. **`categories`** (opcjonalny):
	    - Lista unikalnych wartości, które mają być traktowane jako kategorie. Jeśli nie zostanie podana, Pandas automatycznie wyciągnie unikalne wartości z `values`.
	    - Przykład: **['a', 'b', 'c', 'd']** (nawet jeśli `d` nie występuje w `values`, zostanie uwzględnione jako kategoria).
	3. **`ordered`** (opcjonalny, domyślnie **False**):
	    - Określa, czy kategorie mają być traktowane jako uporządkowane (np. `low < medium < high`).
	    - Jeśli **True**, można wykonywać operacje porównawcze, takie jak `<`, `>`, itp.
```python
# Predefiniowana lista kolejności
order = ['banana', 'cherry', 'apple']

# Zamiana na kolumnę typu Categorical
df['item_name'] = pd.Categorical(
	df['item_name'],
	categories=order,
	ordered=True
)

# Sortowanie wg zaaplikowanego orderu
df_sorted = df.sort_values('item_name')

```