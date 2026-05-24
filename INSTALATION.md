# Instalacja — krok po kroku

![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?logo=python&logoColor=white)
![TensorFlow](https://img.shields.io/badge/TensorFlow-2.20%2B-FF6F00?logo=tensorflow&logoColor=white)
![PyTorch](https://img.shields.io/badge/PyTorch-2.x-EE4C2C?logo=pytorch&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-F37626?logo=jupyter&logoColor=white)

Przewodnik instalacji dla **Windows**, **Linux** i **macOS**.

---

## Spis treści

1. [Krok 1 — Instalacja Pythona](#krok-1--instalacja-pythona)
2. [Krok 2 — Utworzenie środowiska wirtualnego (venv)](#krok-2--utworzenie-środowiska-wirtualnego-venv)
3. [Krok 3 — Aktywacja środowiska](#krok-3--aktywacja-środowiska)
4. [Krok 4 — Instalacja pakietów](#krok-4--instalacja-pakietów)
5. [Krok 5 — Uruchomienie Jupyter Notebook](#krok-5--uruchomienie-jupyter-notebook)
6. [Weryfikacja instalacji](#weryfikacja-instalacji)
7. [Rozwiązywanie problemów](#rozwiązywanie-problemów)
8. [Alternatywa — Google Colab (bez instalacji)](#alternatywa--google-colab-bez-instalacji)

---

## Krok 1 — Instalacja Pythona

Potrzebujesz **Pythona 3.10 lub nowszego**. Sprawdź najpierw, czy masz już Pythona:

```bash
python --version
# lub
python3 --version
```

Jeśli wynik to `Python 3.10.x` lub wyższy — przejdź do kroku 2. Jeśli nie — zainstaluj według systemu poniżej.

---

### Windows

1. Wejdź na **https://www.python.org/downloads/windows/**
2. Kliknij **"Download Python 3.x.x"** (najnowsza stabilna wersja).

   ![Przycisk pobierania na python.org](https://www.python.org/static/img/python-logo.png)

3. Uruchom pobrany instalator `.exe`.

   > **WAŻNE:** Na pierwszym ekranie instalatora zaznacz ✅ **"Add Python to PATH"** — bez tego terminal nie znajdzie Pythona.

   ```
   ┌─────────────────────────────────────────────┐
   │  Install Python 3.x.x                       │
   │                                             │
   │  ☑ Add Python 3.x to PATH    ← zaznacz to! │
   │                                             │
   │  [ Install Now ]  [ Customize installation ]│
   └─────────────────────────────────────────────┘
   ```

4. Kliknij **"Install Now"** i poczekaj na zakończenie.
5. Zamknij i ponownie otwórz terminal (PowerShell lub Command Prompt).
6. Zweryfikuj:
   ```powershell
   python --version
   ```

---

### macOS

**Opcja A — oficjalny instalator (zalecana dla początkujących):**

1. Wejdź na **https://www.python.org/downloads/macos/**
2. Pobierz i uruchom pakiet `.pkg`.
3. Kliknij przez kolejne ekrany instalatora (Next → Agree → Install).
4. Po instalacji otwórz **Terminal** (`Cmd+Space` → wpisz „Terminal"):
   ```bash
   python3 --version
   ```

**Opcja B — przez Homebrew (zalecana jeśli masz już Homebrew):**

```bash
brew install python@3.12
python3 --version
```

> Na macOS komenda to zawsze `python3`, nie `python`.

---

### Linux (Ubuntu / Debian)

Python 3 jest zazwyczaj preinstalowany. Sprawdź wersję:

```bash
python3 --version
```

Jeśli wersja jest starsza niż 3.10 lub Pythona nie ma — zainstaluj:

```bash
sudo apt update
sudo apt install python3 python3-pip python3-venv -y
python3 --version
```

**Fedora / RHEL / CentOS:**
```bash
sudo dnf install python3 python3-pip -y
```

**Arch Linux:**
```bash
sudo pacman -S python python-pip
```

---

## Krok 2 — Utworzenie środowiska wirtualnego (venv)

Środowisko wirtualne izoluje pakiety tego projektu od reszty systemu — dzięki temu różne projekty mogą mieć różne wersje bibliotek bez konfliktów.

**Najpierw przejdź do folderu projektu:**

```bash
# Zmień ścieżkę na miejsce, gdzie masz ten projekt:
cd /ścieżka/do/NEURAL_NETWORK_MNIST
```

**Utwórz środowisko wirtualne o nazwie `.venv`:**

```bash
# Windows
python -m venv .venv

# macOS / Linux
python3 -m venv .venv
```

Po wykonaniu tej komendy pojawi się folder `.venv/` w katalogu projektu — to Twoje izolowane środowisko.

```
NEURAL_NETWORK_MNIST/
├── .venv/              ← nowo utworzone środowisko
├── neural_network.ipynb
├── README.md
└── ...
```

---

## Krok 3 — Aktywacja środowiska

Środowisko musisz **aktywować w każdym nowym oknie terminala** przed pracą z projektem.

### Windows — PowerShell

```powershell
.venv\Scripts\Activate.ps1
```

Jeśli pojawi się błąd o polityce wykonywania skryptów:
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
.venv\Scripts\Activate.ps1
```

### Windows — Command Prompt (cmd)

```cmd
.venv\Scripts\activate.bat
```

### macOS / Linux

```bash
source .venv/bin/activate
```

---

**Jak sprawdzić, że środowisko jest aktywne?**

Po aktywacji na początku linii pojawi się nazwa środowiska w nawiasach:

```
(.venv) C:\Users\TwojaNazwa\NEURAL_NETWORK_MNIST>    ← Windows
(.venv) kazik@komputer:~/NEURAL_NETWORK_MNIST$        ← Linux/macOS
```

**Dezaktywacja** (kiedy skończysz pracę):
```bash
deactivate
```

---

## Krok 4 — Instalacja pakietów

Upewnij się, że środowisko jest aktywne (widzisz `(.venv)` w terminalu), a następnie zainstaluj wszystkie wymagane pakiety.

### Aktualizacja pip (zalecane)

```bash
pip install --upgrade pip
```

### Instalacja pakietów dla notatnika Keras/TensorFlow (`neural_network.ipynb`)

```bash
pip install tensorflow numpy matplotlib pillow jupyter
```

### Instalacja pakietów dla notatnika PyTorch (`neural_network_pytorch.ipynb`)

```bash
pip install torch torchvision numpy matplotlib pillow jupyter
```

### Instalacja wszystkiego naraz (oba notatniki + analiza)

```bash
pip install tensorflow torch torchvision numpy matplotlib pillow jupyter scikit-learn
```

> **Uwaga:** Pakiety `tensorflow` i `torch` są duże (~500 MB–1 GB łącznie) — pobieranie może zająć kilka minut w zależności od łącza.

---

### Instalacja PyTorch z obsługą GPU (CUDA) — opcjonalnie

Jeśli masz kartę NVIDIA i chcesz trenować na GPU, zainstaluj PyTorch ze wsparciem CUDA. Sprawdź wersję CUDA swojej karty:

```bash
nvidia-smi
```

Następnie wejdź na **https://pytorch.org/get-started/locally/** i wybierz swoją konfigurację — strona wygeneruje odpowiednią komendę `pip install`. Przykład dla CUDA 12.1:

```bash
pip install torch torchvision --index-url https://download.pytorch.org/whl/cu121
```

TensorFlow automatycznie wykrywa GPU — nie wymaga osobnej komendy instalacyjnej.

---

## Krok 5 — Uruchomienie Jupyter Notebook

Upewnij się, że środowisko jest aktywne, a następnie:

```bash
jupyter notebook
```

W przeglądarce automatycznie otworzy się interfejs Jupyter pod adresem `http://localhost:8888`.

Kliknij na `neural_network.ipynb` lub `neural_network_pytorch.ipynb`, żeby otworzyć notatnik.

**Alternatywnie — JupyterLab (nowszy interfejs):**

```bash
pip install jupyterlab
jupyter lab
```

---

### Jak uruchamiać komórki notatnika?

| Akcja | Skrót klawiaturowy |
|-------|--------------------|
| Uruchom komórkę i przejdź do następnej | `Shift + Enter` |
| Uruchom komórkę bez przechodzenia | `Ctrl + Enter` |
| Uruchom wszystkie komórki od góry | Menu: `Kernel → Restart & Run All` |
| Zatrzymaj działającą komórkę | `I, I` (dwukrotne naciśnięcie `I`) lub przycisk ⏹ |

> Przy pierwszym uruchomieniu dane MNIST (~11 MB) są automatycznie pobierane. Kolejne uruchomienia korzystają z lokalnego cache.

---

## Weryfikacja instalacji

Uruchom poniższy kod w nowej komórce notatnika lub bezpośrednio w terminalu:

```python
import sys
import numpy
import matplotlib
import PIL

print(f"Python:      {sys.version}")
print(f"NumPy:       {numpy.__version__}")
print(f"Matplotlib:  {matplotlib.__version__}")
print(f"Pillow:      {PIL.__version__}")

# TensorFlow (jeśli zainstalowany)
try:
    import tensorflow as tf
    print(f"TensorFlow:  {tf.__version__}")
    gpus = tf.config.list_physical_devices('GPU')
    print(f"GPU (TF):    {[g.name for g in gpus] if gpus else 'brak — działa na CPU'}")
except ImportError:
    print("TensorFlow:  nie zainstalowany")

# PyTorch (jeśli zainstalowany)
try:
    import torch
    print(f"PyTorch:     {torch.__version__}")
    print(f"GPU (PT):    {torch.cuda.get_device_name(0) if torch.cuda.is_available() else 'brak — działa na CPU'}")
except ImportError:
    print("PyTorch:     nie zainstalowany")
```

Poprawny wynik wygląda mniej więcej tak:

```
Python:      3.12.3 (main, ...)
NumPy:       2.0.1
Matplotlib:  3.9.2
Pillow:      10.4.0
TensorFlow:  2.20.0
GPU (TF):    brak — działa na CPU
PyTorch:     2.4.0+cpu
GPU (PT):    brak — działa na CPU
```

---

## Rozwiązywanie problemów

### `python` nie jest rozpoznawany (Windows)

Pythona nie ma w PATH. Rozwiązania:
- Odinstaluj i zainstaluj Pythona ponownie, zaznaczając ✅ **"Add Python to PATH"**
- Lub użyj `py` zamiast `python`: `py --version`

---

### Błąd przy tworzeniu venv: `No module named venv`

```bash
# Ubuntu/Debian
sudo apt install python3-venv -y
```

---

### Błąd przy aktywacji w PowerShell: `cannot be loaded because running scripts is disabled`

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

---

### `pip install tensorflow` kończy się błędem na M1/M2 Mac (Apple Silicon)

Użyj wersji zoptymalizowanej pod Apple Silicon:

```bash
pip install tensorflow-macos tensorflow-metal
```

---

### Jupyter otwiera się, ale nie widzi pakietów z venv

Środowisko nie jest zarejestrowane jako kernel Jupytera. Napraw to:

```bash
pip install ipykernel
python -m ipykernel install --user --name=.venv --display-name "Python (MNIST)"
```

Następnie w Jupyterze wybierz: `Kernel → Change Kernel → Python (MNIST)`.

---

### Trening trwa bardzo długo (ponad 10 minut na epokę)

- Upewnij się, że środowisko jest aktywne i TensorFlow/PyTorch są zainstalowane poprawnie.
- Na CPU 10 epok to ok. 3–5 minut — jeśli trwa dłużej, możliwy jest konflikt z inną wersją biblioteki.
- Sprawdź zużycie procesora podczas treningu: powinno być wysokie (80–100%).

---

## Alternatywa — Google Colab (bez instalacji)

Jeśli nie chcesz nic instalować lokalnie, skorzystaj z **Google Colab** — działa w przeglądarce i oferuje darmowy dostęp do GPU.

1. Wejdź na **https://colab.research.google.com**
2. Zaloguj się kontem Google.
3. Kliknij **File → Upload notebook** i wgraj plik `neural_network.ipynb`.
4. Włącz GPU: **Runtime → Change runtime type → T4 GPU → Save**.
5. Uruchamiaj komórki normalnie — wszystkie biblioteki (TensorFlow, NumPy, Matplotlib) są preinstalowane.

> W Colabie dane MNIST pobierane są automatycznie. Trening 10 epok z GPU T4 trwa ok. 30–60 sekund.

---

## Polecane wideo — instalacja krok po kroku

Jeśli wolisz zobaczyć instalację w praktyce, poniższe wideo przeprowadza przez cały proces:

[![Instalacja Pythona, venv i Jupyter — poradnik wideo](https://img.youtube.com/vi/urmxXHukIGM/maxresdefault.jpg)](https://www.youtube.com/watch?v=urmxXHukIGM)

**[Obejrzyj na YouTube →](https://www.youtube.com/watch?v=urmxXHukIGM)**

[![Instalacja Pythona, venv i Jupyter — poradnik wideo 2](https://img.youtube.com/vi/9Xg0M1Lz020/maxresdefault.jpg)](https://www.youtube.com/watch?v=9Xg0M1Lz020)

**[Obejrzyj na YouTube →](https://www.youtube.com/watch?v=9Xg0M1Lz020)**
