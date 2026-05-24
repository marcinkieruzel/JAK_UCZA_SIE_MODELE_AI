# Jak uczą się modele AI — gradient descent na regresji liniowej

Interaktywny notatnik dydaktyczny, który **od zera** pokazuje, jak komputer „uczy się" parametrów modelu — używając najprostszego możliwego przykładu: regresji liniowej i ok. 30 linii kodu w NumPy.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/marcinkieruzel/JAK_UCZA_SIE_MODELE_AI/blob/main/simple_example.ipynb)
![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)
![License](https://img.shields.io/badge/license-CC%20BY--NC%204.0-lightgrey)

---

## Co znajdziesz w notatniku

Na przykładzie **200 studentów** (czas nauki vs wynik egzaminu) przechodzimy przez:

1. **Intuicję** — historia Jasia, który zgaduje współczynniki $a$ i $b$ pod okiem nauczyciela
2. **Model regresji liniowej** — co geometrycznie oznaczają parametry $a$ (nachylenie) i $b$ (przesunięcie)
3. **Funkcję kosztu MSE** — policzona ręcznie krok po kroku na 3 punktach + wizualizacja reszt
4. **Rozwiązanie analityczne** (metoda najmniejszych kwadratów) — i odpowiedź na pytanie *„skoro mamy wzór, po co GD?"*
5. **Powierzchnię kosztu** — kontury 2D + widok 3D paraboloidy
6. **Implementację gradient descent** — od zera, w czystym NumPy
7. **🎛️ Interaktywne suwaki** — ręczne strzelanie i pełna piaskownica GD (`learning_rate`, `n_iter`, punkt startowy)
8. **Podsumowanie** + propozycja rozszerzenia na regresję wielowymiarową

Cel: po pracy z notatnikiem zrozumiesz **mechanikę**, na której opierają się wszystkie współczesne sieci neuronowe.

---

## Jak uruchomić — wybierz wariant

### 🚀 Wariant A — Google Colab (zalecany na start, **bez instalacji**)

Najprostsza droga: kliknij badge na górze albo bezpośrednio link:

> **[Otwórz notebook w Colab →](https://colab.research.google.com/github/marcinkieruzel/JAK_UCZA_SIE_MODELE_AI/blob/main/simple_example.ipynb)**

Następnie:
1. Zaloguj się kontem Google (jeśli jeszcze nie jesteś).
2. Wybierz w menu: **Runtime → Run all** (Ctrl/⌘ + F9), żeby wykonać wszystkie komórki.
3. Przewiń do sekcji 3 i 8 — przesuń suwakami i obserwuj, co się dzieje.

**Co dostajesz w Colabie:**
- gotowe środowisko Python z NumPy, pandas, Matplotlib, ipywidgets
- dane (`student_exam_scores.csv`) pobierane automatycznie z tego repozytorium
- darmowe CPU (a opcjonalnie GPU — ale tutaj zbędne)

**Wada Colaba:** każda sesja jest tymczasowa — jeśli zamkniesz kartę, trzeba ponownie wykonać wszystkie komórki.

---

### 💻 Wariant B — Lokalnie na komputerze (dla zaawansowanych)

Lepsze do dłuższej pracy, własnych eksperymentów i pracy bez internetu.

**Wymagania:** Python 3.10+, `git`.

```bash
# 1. Sklonuj repozytorium
git clone https://github.com/marcinkieruzel/JAK_UCZA_SIE_MODELE_AI.git
cd JAK_UCZA_SIE_MODELE_AI

# 2. Utwórz i aktywuj środowisko wirtualne
python -m venv .venv
source .venv/bin/activate          # Linux / macOS
# .venv\Scripts\activate           # Windows (PowerShell)

# 3. Zainstaluj zależności
pip install -r requirements.txt

# 4. Uruchom Jupyter
jupyter notebook simple_example.ipynb
# lub w VSCode: po prostu otwórz simple_example.ipynb i wybierz interpreter .venv
```

Jeśli `requirements.txt` instaluje za dużo (zawiera m.in. PyTorch + CUDA), wystarczy **minimalny zestaw**:

```bash
pip install numpy pandas matplotlib ipykernel ipywidgets jupyter
```

Pełna, krok po kroku instalacja Pythona dla **Windows / Linux / macOS** — wraz z troubleshootingiem — jest w [`INSTALATION.md`](INSTALATION.md).

---

## Struktura repozytorium

```
.
├── simple_example.ipynb              ← główny notatnik
├── student_exam_scores.csv           ← dane: 200 studentów, 5 kolumn
├── requirements.txt                  ← pełne zamrożone zależności
├── INSTALATION.md                    ← szczegółowa instalacja Pythona (Win/Lin/Mac)
├── scenariusz_lekcji_45min_regresja.md      ← scenariusz dla nauczyciela (45 min)
├── scenariusz_projektu_10h_regresja.md      ← scenariusz projektu (~10 h)
└── README.md                         ← ten plik
```

---

## Materiały dla nauczycieli

Notatnik został przygotowany z myślą o szkole ponadpodstawowej (liceum / technikum). W repo znajdziesz dwa gotowe scenariusze:

- **[`scenariusz_lekcji_45min_regresja.md`](scenariusz_lekcji_45min_regresja.md)** — pojedyncza lekcja informatyki: intuicja → dane → MSE → GD → piaskownica
- **[`scenariusz_projektu_10h_regresja.md`](scenariusz_projektu_10h_regresja.md)** — projekt grupowy z eksperymentami nad `lr`, standaryzacją, rozszerzeniem na regresję wielowymiarową, prezentacjami i peer review

Oba zawierają cele zajęć, metody pracy, kryteria oceny i wskazówki dydaktyczne.

---

## Częste problemy

**Suwaki ipywidgets nie reagują w VSCode**
→ Upewnij się, że wybrany kernel to ten z `.venv` (prawy górny róg notatnika). Czasem trzeba odświeżyć przez **Reload Window**.

**`ModuleNotFoundError: No module named 'ipywidgets'`**
→ Pakiet jest, ale w innym środowisku. Sprawdź `which python` i porównaj z kernelem notatnika.

**Czas wykonania kilku komórek > 1 minuty na CPU**
→ Najpewniej powierzchnia kosztu (sekcja 4 i 7) — to dwa zagnieżdżone `for` po 80×80 punktów. Można zmniejszyć siatkę z `np.linspace(..., 80)` na `np.linspace(..., 40)`.

---

## Licencja

[CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/) — Uznanie autorstwa, użycie niekomercyjne. Możesz dowolnie wykorzystywać, modyfikować i udostępniać materiały w celach edukacyjnych, z zachowaniem informacji o autorze.

---

## Dalsza eksploracja

- 🎥 [3Blue1Brown — Gradient descent, how neural networks learn](https://www.youtube.com/watch?v=IHZwWFHWa-w) (PL napisy)
- 📘 *Generatywna AI. Wszystko, co warto wiedzieć* — dobry pomost do tego, jak GD działa w LLM-ach
- 🔬 Spróbuj rozszerzenia z sekcji 9 notatnika: dodaj drugą cechę (`sleep_hours`) i zobacz, czy poprawia dopasowanie
