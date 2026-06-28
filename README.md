# Projekt 21: Wyszukiwanie dni z podobną pogodą

## 1. Cel projektu
Głównym celem projektu jest pobranie historycznych danych pogodowych z REST API, przekształcenie ich w dzienne wektory cech oraz znalezienie dni historycznych, które są matematycznie najbardziej podobne do wybranego dnia referencyjnego przy użyciu techniki wyszukiwania najbliższych sąsiadów.

## 2. Architektura i przetwarzanie danych
Projekt działa w całości w środowisku lokalnym z wykorzystaniem bibliotek Data Science języka Python.
* **Źródło danych:** Współdzielone REST API pogodowe.
* **Pozyskiwanie danych:** Biblioteka `requests` służy do uwierzytelniania i pobierania danych bezpośrednio w notatniku.
* **Surowe dane:** Odpowiedź JSON jest zapisywana w lokalnym systemie plików w katalogu `raw_data/` przed wykonaniem jakichkolwiek przekształceń.
* **Przetwarzanie i analiza:** * `pandas` służy do rozpakowania zagnieżdżonego formatu JSON i agregacji pomiarów do wektorów dziennych (średnia temperatura, maksymalny wiatr, całkowity opad itp.).
    * `scikit-learn` służy do normalizacji wektorów, aby zmienne o dużym zakresie wartości nie dominowały nad zmiennymi o mniejszym zakresie.
    * `euclidean_distances` stanowi główną metodę analityczną do obliczania podobieństwa.
* **Prezentacja:** `matplotlib` służy do generowania wykresów porównawczych weryfikujących skuteczność algorytmu.

## 3. Kroki uruchomienia
Aby uruchomić projekt lokalnie:
1. Upewnij się, że masz zainstalowanego Pythona w wersji 3.9+.
2. Zainstaluj wymagane biblioteki w terminalu: `pip install requests pandas scikit-learn matplotlib`.
3. Otwórz `main.ipynb` w środowisku Jupyter Notebook lub PyCharm.
4. Uruchom wszystkie komórki sekwencyjnie. Pierwsze komórki odpowiadają za pobranie i zapis danych, kolejne za przetwarzanie, analizę i wizualizację.

## 4. Założenia, ograniczenia i możliwe ulepszenia
* **Ograniczenia:** Podczas etapu pozyskiwania danych stwierdzono, że źródłowe API nakłada sztywny limit, zwracając maksymalnie 500 rekordów na zapytanie. Ogranicza to pojedynczą paczkę danych do około 3-4 dni.
* **Założenia:** Przyjęto, że dzienna agregacja (średnia, min, max, suma) trafnie odzwierciedla profil pogodowy danego dnia, eliminując szum spowodowany krótkotrwałymi zmianami pogody.
* **Możliwe ulepszenia:** W przypadku obsługi ogromnych zbiorów danych, logikę przetwarzania można przenieść z lokalnej biblioteki pandas do klastra AWS EMR przy użyciu PySpark.