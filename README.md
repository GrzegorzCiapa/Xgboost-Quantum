# Xgboost-Quantum

Projekt badawczy łączący XGBoost z wariacyjnymi obwodami kwantowymi (VQC) w architekturze Sim-to-QPU przy użyciu PennyLane, PyTorch oraz komputera kwantowego Odra 5 (IQM).

## Wymagania

* Python ze środowiskiem skonfigurowanym dla bibliotek xgboost, pennylane, pennylane-qiskit, qiskit, torch, scikit-learn, pandas i numpy.



## Architektura Pipeline'u

* **Różnicowanie:** Zastosowanie `diff(1)` w celu eliminacji efektu sufitu i poprawy ekstrapolacji.
* **Ekstrakcja cech:** 5-kubitowy obwód VQC (AngleEmbedding + StronglyEntanglingLayers), generujący 15 cech kwantowych (bazy X, Y, Z).
* **Transfer Learning (Sim-to-QPU):** Trening wag na symulatorze `default.qubit` z przeniesieniem na fizyczny QPU Odra 5.
* **Regresja:** Redukcja wymiarowości przez PCA do 3 składowych i predykcja za pomocą XGBoost.

## Wyniki (RMSE)

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
