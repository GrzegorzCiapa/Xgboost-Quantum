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

| Trend | Klasyczny XGBoost | Symulator | Odra 5 QPU |
| --- | --- | --- | --- |
| Wzrostowy | 17.75 | 24.90 | 19.72 |
| Spadkowy | 18.93 | 19.26 | 18.48 |
| Boczny | 37.17 | 40.09 | 38.83 |

## Wnioski

* Różnicowanie pierwszego rzędu jest kluczowe dla stabilności modeli drzewiastych.
* Szum sprzętowy Odra 5 QPU działa jak naturalny regularizator, pozwalając w określonych warunkach (np. trend spadkowy) pobić wyniki symulatora i klasyki.
* Niskowymiarowe dane i rozłączny trening sprawiają, że klasyczny XGBoost pozostaje silnym punktem odniesienia.
