# Xgboost-Quantum

Projekt badawczy łączący XGBoost z wariacyjnymi obwodami kwantowymi (VQC) w architekturze Sim-to-QPU przy użyciu PennyLane, PyTorch oraz komputera kwantowego Odra 5 (IQM). Algorytm uczy się na 3 data setach: 
* **AirPassengers.txt**: Klasyczny, realny zbiór szeregów czasowych przedstawiający miesięczną liczbę międzynarodowych pasażerów linii lotniczych w latach 1949–1960, charakteryzujący się silnym trendem wzrostowym oraz wyraźną roczną sezonowością.


* **Trend_Spadkowy.txt**: Syntetyczny zbiór danych modelujący miesięczną liczbę pasażerów w latach 2010–2019, w którym występuje wyraźny, długoterminowy trend malejący (liczba pasażerów systematycznie spada z ponad 1000 na początku do ok. 350-400 pod koniec) przy zachowaniu cykliczności.


* **Trend_Boczny.txt**: Syntetyczny zbiór danych z lat 2010–2019 symulujący rynek o trendzie bocznym (stacjonarnym), gdzie wartości fluktuują w zbliżonym, powtarzalnym przedziale (głównie między 370 a 650) bez wyraźnego wzrostu lub spadku w ujęciu wieloletnim.

## Wymagania

* Python ze środowiskiem skonfigurowanym dla bibliotek xgboost, pennylane, pennylane-qiskit, qiskit, torch, scikit-learn, pandas i numpy.



## Architektura

* **Różnicowanie:** Zastosowanie `diff(1)` w celu eliminacji efektu sufitu i poprawy ekstrapolacji.
* **Ekstrakcja cech:** 5-kubitowy obwód VQC (AngleEmbedding + StronglyEntanglingLayers), generujący 15 cech kwantowych (bazy X, Y, Z).
* **Transfer Learning (Sim-to-QPU):** Trening wag na symulatorze `default.qubit` z przeniesieniem na fizyczny QPU Odra 5.
* **Regresja:** Redukcja wymiarowości przez PCA do 3 składowych i predykcja za pomocą XGBoost.

## Wyniki (RMSE)

RMSE (Root Mean Square Error) - miara oceny jakości modeli regresji i predykcji. Określa ona, jak duże są średnie odchylenia między wartościami przewidywanymi przez model a rzeczywistymi danymi.

### 50 epok

| Trend | Klasyczny (RMSE) | Symulator (RMSE) | Odra 5 QPU (RMSE) |
| --- | --- | --- | --- |
| Wzrostowy | 17.75 | 19.62 | 17.18 |
| Spadkowy | 18.93 | 20.06 | 19.21 |
| Boczny (Mieszany) | 37.17 | 37.42 | 37.17 |

### 300 epok

| Trend | Klasyczny (RMSE) | Symulator (RMSE) | Odra 5 QPU (RMSE) |
| --- | --- | --- | --- |
| Wzrostowy | 17.75 | 24.90 | 18.35 |
| Spadkowy | 18.93 | 19.26 | 18.37 |
| Boczny (Mieszany) | 37.17 | 40.09 | 38.19 |

### 600 epok

| Trend | Klasyczny (RMSE) | Symulator (RMSE) | Odra 5 QPU (RMSE) |
| --- | --- | --- | --- |
| Wzrostowy | 17.75 | 18.10 | 19.33 |
| Spadkowy | 18.93 | 20.11 | 17.57 |
| Boczny (Mieszany) | 37.17 | 34.95 | 40.50 |

## Wnioski

* Różnicowanie pierwszego rzędu jest kluczowe dla stabilności modeli drzewiastych.
* Szum sprzętowy Odra 5 QPU działa jak naturalny regularizator, pozwalając w określonych warunkach (np. trend spadkowy) pobić wyniki symulatora i klasyki.
* Niskowymiarowe dane i rozłączny trening sprawiają, że klasyczny XGBoost pozostaje silnym punktem odniesienia.
