---
title: Fetching
draft:
tags:
---


- **CSV**
	- `read_csv()` to podstawowa funkcja do wczytywania danych CSV
	- obsługuje różne separatory, kodowania i inne opcje formatowania
	- wydajny nawet dla dużych plików dzięki optymalizacjom C
```python
# Podstawowe wczytanie pliku CSV
df = pd.read_csv('data.csv')

# Z dodatkowymi parametrami
df = pd.read_csv('data.csv', 
                sep=';',               # separator kolumn (domyślnie ',')
                decimal=',',           # separator dziesiętny
                encoding='utf-8',      # kodowanie znaków
                header=0,              # numer wiersza z nagłówkami (0-based)
                na_values=['NA', '-']  # wartości uznawane za braki danych
                )

# Wczytanie tylko wybranych kolumn
df = pd.read_csv('data.csv', usecols=['Date', 'Sales', 'Product'])
```

- **TXT**
	- również wczytywane przez `read_csv()` dzięki elastyczności tej funkcji
	- przydatne przy danych z nietypowymi separatorami (np. tabulatory, spacje)
```python
# Wczytanie pliku TXT z separatorem tab
df = pd.read_csv('data.txt', sep='\t')

# Dla plików z separatorem o stałej szerokości
df = pd.read_fwf('data.txt', widths=[10, 5, 8])  # szerokości kolumn
```

- **xlsx** 
    - wymaga zainstalowania biblioteki `openpyxl` lub `xlrd`
    - pozwala na wczytywanie konkretnych arkuszy lub zakresów komórek
    - wolniejszy od CSV przy dużych plikach
```python
# pip install openpyxl

# Podstawowe wczytanie
df = pd.read_excel('data.xlsx')

# Z dodatkowymi parametrami
df = pd.read_excel('data.xlsx',
                  sheet_name='Sales',     # nazwa arkusza (domyślnie pierwszy)
                  skiprows=2,             # pominięcie pierwszych wierszy
                  usecols='A:C,F',        # wybrane kolumny (notacja Excel)
                  na_values=['N/A']       # wartości uznawane za braki danych
                  )

# Wczytanie wielu arkuszy do słownika ramek danych
dfs = pd.read_excel('data.xlsx', sheet_name=None)  # None = wszystkie arkusze
```

- **Pliki JSON** 
    - `read_json()` obsługuje różne orientacje danych JSON
    - może bezpośrednio wczytać dane z API zwracających JSON
    - przydatny dla danych z sieci i web API

```python
# Podstawowe wczytanie
df = pd.read_json('data.json')

# Dla JSONów o różnych orientacjach
df = pd.read_json('data.json', orient='records')  # lista rekordów
df = pd.read_json('data.json', orient='split')    # osobne kolumny i wiersze

# Normalizacja zagnieżdżonych danych
df = pd.json_normalize(json_data)  # spłaszczenie zagnieżdżonych JSON
```

- **GitHub** - wczytywanie danych bezpośrednio z repozytoriów
    - przydatne dla otwartych zbiorów danych i notebooków
    - można wczytać bez pobierania pliku lokalnie
```python
# Bezpośrednie wczytanie z URL surowych danych z GitHub
url = 'https://raw.githubusercontent.com/username/repo/main/data.csv'
df = pd.read_csv(url)

# Używając biblioteki requests dla bardziej zaawansowanych przypadków
import requests
response = requests.get(url)
df = pd.read_csv(io.StringIO(response.text))
```

- **API internetowe** - dostęp do danych od zewnętrznych dostawców
    - często wymaga klucza API i autentykacji
    - zwykle zwraca dane w formacie JSON
```python
# Przykład pobrania danych z API
import requests

url = 'https://api.example.com/data'
params = {'key': 'your_api_key', 'format': 'json'}
response = requests.get(url, params=params)

# Konwersja odpowiedzi na ramkę danych
df = pd.DataFrame(response.json()['results'])
```