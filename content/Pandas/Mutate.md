---
title: Mutate
draft:
tags:
---


- Oczywiście można tworzyć bardzo proste operacje na kolumnach - czyli wykorzystanie klasy series z pandas
	- bardzo wygodne do przekształceń jednej kolumny
	- natomiast nie sprawdza się podczas zmieniania całej serii kolumn
```python
df['Sales'] = df['Sales'] * 100  # przykładowa operacja na kolumnie - przemnożenie przez 100

# oczywiście można dodać nową kolumnę
df['SalesDistribution'] = df['Sales'] / df['Sales'].sum() 
```

- Z kolei metoda `eval()` pozwala na bardzo wydajne przekształcanie danych wykorzystując wyrażenie, które
	- działa na kolumnach - bardzo kompaktowy zapis, szczególnie przy skomplikowanych obliczeniach
	- jest wydajne, zalecane do pracy z dużymi tabelami
	- ze względu na formę tekstową łatwo o błędy - brak podkreślania syntaxu
	- można używać `@` lub `f{}` do przekazywania zmiennych środowiskowych
	- `inplace=True` pozwala na nadpisanie istniejącej ramki danych, domyślnie `False` 
	- obsługuje również metody, stosując je na kolumnach
```python
# Utworzenie nowej kolumny dzięki inplace=True
df.eval('NetValue = (Sales - Discount) * (1 - .23)', inplace=True)

# Metoda ze zmienną środowiskową, tym razem nie zmieni danych, jedynie wyświetli wynik (inplace=False)
tax = 1 - .23
df.eval('Net Value = (Sales - Discount) * @tax')
df.eval(f'Net Value = (Sales - Discount) * {tax}')
# I z użyciem metody...
df.eval('SalesDistribution = Sales / Sales.sum()')
```
    
- Do przekształceń o charakterze **piepline**, najlepiej stosować metodę`assign()`
	- bardzo dobre do wielu przekształceń
	- wykorzystując `lambda` można odwoływać się do wcześniej utworzonych kolumn - w innym wypadku odwołanie się w drugim argumencie do kolumny wynikowej z pierwszego argumentu będzie skutkować błędem
	- konstruktor wyrażeń posiada standardową składnie - bardzo intuicyjne
	- działa z `grupby` - metodą grupowania kolumn
```python
tax=(1-.23)
# Podstawowe przekształcenia
df=df.assign(Profit = (df['Sales'] - df['Discount']) * tax)
df=df.assign(Profit = lambda x: (x['Sales'] - x['Discount']) * tax)  # użycie lambdy

# Pipeline z wykorzystaniem lambdy w celu odwołania się do wcześniejszego wyniku
# wykorzystanie piepline wymaga nawiasów wokół całego wyrażenia
(
    df:=df # := powoduje, że wynik jest zarówno przypisany, jak i wyświetlony w konsoli
	.assign(SalesAfterDiscount = df['Sales'] - df['Discount'])
	.assign(
        Profit = lambda x: x['SalesAfterDiscount'] * tax,
        TaxPaid = lambda x: x['Sales'] * (1-tax)
    )
)
```
### Grupowanie    
- W przypadku bardziej zaawansowanych obliczeń na grupach, można zastosować kombinację 
  `groupby() apply() i assign() z lambdą`
	- `groupby()` grupuje kolumny, np, `groupby('Product')` lub dla wielu `groupby(['Product' 'Region'])`
		- argument `as_index` pozwala na utworzenie nowego indexu, z którego składają się użyte zmienne, domyślniej `True`
		- utworzony index można zrzucić używając metody `reset_index(drop=True)`
	- `apply()` stosuje dla każdej grupy zadane przekształcenie
	- `assign()` odwołuje się do tabeli utworzonej w `lambda` - dzięki temu może pracować na zgrupowanej tabeli
```python
# Przypisanie nowych wartości, nie tworzy indexu
(
	df:=df.groupby(['Region', 'Product'], as_index=False).apply(
		lambda x: x.assign(
			MeanSales = (x['Sales'] * 100).mean(),  # przykładowe obliczenie
			MedianSales = x['Sales'].median(),
			q2Sales = x['Sales'].quantile(.25)
		).round(2)
	)
)

# A tak wygląda z indexem
df.groupby(['Region', 'Product'], as_index=True).apply(
	lambda x: x.assign(
		MeanSales = (x['Sales'] * 100).mean(),  # przykładowe obliczenie
		MedianSales = x['Sales'].median(),
		q2Sales = x['Sales'].quantile(.25)
	).round(2)
)


# można też ręcznie utworzyć klaster przypominający index
df.groupby(['Region', 'Product'], as_index=True).apply(
	lambda x: x.assign(
	Cluster = 
	'Region: ' +  x['Region'] +
	' Selling: ' + x['Product']
	)
)
```

- Jeżeli skomplikowanie `assign()` nie jest wymagane, można uprościć proces stosując `transform()`
	- w tym wypadku tworzymy `groupby`, a następnie wyciągamy z niego agregowaną kolumnę
```python
# średnia sprzedaży dla każdego regionu
df = df.groupby(['Region'])['Sales'].transform('mean')

# średnia sprzedaży dla regionu i produktu
df = df.groupby(['Region', 'Product'])['Sales'].transform('mean')  
```