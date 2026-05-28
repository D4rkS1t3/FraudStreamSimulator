# FraudStreamSimulator

Symulator systemu antyfraudowego przetwarzającego strumienie danych transakcyjnych w czasie rzeczywistym (Real-time Event Processing). Projekt implementuje architekturę detekcji anomalii opartą na koncepcji ruchomego okna czasowego (Sliding Window) przy użyciu zoptymalizowanych struktur pamięci podręcznej.

## Zaimplementowane Reguły Antyfraudowe

System analizuje każdą nową transakcję "w locie", porównując ją z historią aktywności karty z ostatnich 5 sekund:
1. **Niemożliwa Podróż (Velocity Fraud / Location Anomaly)**: Automatyczne blokowanie transakcji, jeśli karta została użyta w dwóch różnych lokalizacjach geograficznych (miastach) w czasie uniemożliwiającym fizyczne przemieszczenie się (okno < 5s).
2. **Atak Wolumetryczny (High-Frequency Flood)**: Wykrywanie i powstrzymywanie prób masowego bombardowania terminali (np. po kradzieży karty). System blokuje operacje, jeśli w oknie czasowym wykryje więcej niż 3 transakcje na jedno konto.

## Architektura i Optymalizacja Technologiczna

- **Python 3.x**
- **Przetwarzanie Strumieniowe (In-Memory Streaming)**: Zamiast obciążających baz danych SQL, system opiera się na strukturach in-memory, symulując wydajny mechanizm cachowania danych.
- **Optymalizacja Kolejek**: Wykorzystanie klasy `collections.deque(maxlen=5)` osadzonej wewnątrz `defaultdict`. Gwarantuje to automatyczne usuwanie przestarzałych zdarzeń z pamięci (złożoność O(1)) i zapobiega wyciekom pamięci przy milionach symulowanych rekordów.
- **CLI Dashboard (`colorama`)**: Interaktywny panel konsolowy wizualizujący w czasie rzeczywistym statusy weryfikacji (OK/ALERT) oraz aktualny stan bufora pamięci podręcznej dla każdego klienta.
