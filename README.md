# Segmentacja klientów i predykcja rezygnacji — Online Retail II

*[English version](README.en.md)*

Kod źródłowy do pracy magisterskiej *Zastosowanie metod uczenia maszynowego
w analizie zachowań zakupowych klientów handlu elektronicznego*
(Politechnika Gdańska, 2026).

Analiza obejmuje dwa zadania na zbiorze transakcyjnym **Online Retail II**
(801 291 transakcji, 5 859 klientów, lata 2009–2011):

- **segmentację klientów** metodami grupowania ($k$-średnich, DBSCAN)
  na cechach modelu RFM rozszerzonego o pięć cech wzorca zachowania,
- **predykcję rezygnacji klienta** (regresja logistyczna, las losowy)
  wraz z analizą ważności cech metodą SHAP.

## Dane wejściowe

Repozytorium **nie zawiera danych** — surowy plik ma ok. 45 MB,
a zbiór po czyszczeniu ok. 78 MB.

Pobierz `online_retail_II.xlsx` z repozytorium UCI Machine Learning Repository:

> Chen, D. *Online Retail II*. UCI Machine Learning Repository, 2019.
> https://doi.org/10.24432/C5CG6D

Umieść plik w katalogu głównym repozytorium, obok notatników.

## Uruchomienie

```bash
pip install -r requirements.txt
jupyter lab
```

Notatniki należy wykonać **w podanej kolejności** — każdy korzysta
z plików zapisanych przez poprzednie:

| Notatnik | Zakres | Zapisuje |
|---|---|---|
| `01_eda.ipynb` | wczytanie danych surowych, eksploracyjna analiza, czyszczenie | `online_retail_clean.csv` |
| `02_features.ipynb` | model RFM, pięć cech wzorca zachowania, transformacja logarytmiczna i standaryzacja | `rfm_features.csv`, `rfm_scaled.csv` |
| `03_segmentacja.ipynb` | $k$-średnie, DBSCAN, rzut PCA, rysunki rozdz. 6 | `cluster_labels.csv` |
| `04_churn.ipynb` | definicja rezygnacji, oba klasyfikatory, wartości SHAP, analizy diagnostyczne | `churn_predictions.csv`, `churn_results.csv`, `churn_diagnostyka.csv` |

Notatniki są zapisane **wraz z wynikami**, więc kod i otrzymane liczby
można obejrzeć bez uruchamiania czegokolwiek.

## Powtarzalność

Wszystkie procedury losowe korzystają z ustalonego ziarna
(`RANDOM_STATE = 42`): podział na zbiór uczący i testowy, inicjalizacja
$k$-średnich, las losowy oraz próbka obserwacji użyta do wyznaczenia
wartości SHAP. Ponowne wykonanie notatników na wersjach bibliotek
podanych w `requirements.txt` odtwarza liczby przedstawione w pracy.

## Wyniki dołączone do repozytorium

Dwa pliki wynikowe zostały dołączone, ponieważ odpowiadają bezpośrednio
tabelom w pracy:

- `churn_results.csv` — metryki obu modeli na zbiorze testowym,
- `churn_diagnostyka.csv` — warianty oceny pozwalające oddzielić zdolność
  predykcyjną modelu od definicyjnej zależności między cechą *Recency*
  a zmienną docelową.

## Uwaga metodologiczna

Cechy wykorzystane w zadaniu predykcji rezygnacji wyznaczono **wyłącznie
z transakcji sprzed przyjętej daty odcięcia** (2011-09-01), niezależnie
od cech użytych do segmentacji, które obliczono na pełnym okresie
obserwacji. Oba potoki przygotowania danych są rozdzielone celowo:
wspólne cechy prowadziłyby do wykorzystania informacji o przyszłym
zachowaniu klienta.
