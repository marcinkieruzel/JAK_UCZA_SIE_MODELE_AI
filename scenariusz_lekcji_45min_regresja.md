# Scenariusz lekcji — 45 minut

---

## Podstawowe informacje

**Temat zajęć:** Jak komputer uczy się dopasować linię? Gradient descent na przykładzie regresji liniowej

**Etap edukacyjny i typ edukacji:** Szkoła ponadpodstawowa — IV etap edukacyjny (liceum / technikum)

**Przedmiot:** Informatyka (możliwa realizacja interdyscyplinarna z matematyką)

**Czas trwania zajęć:** 45 minut

---

## Szczegóły scenariusza

### Uzasadnienie wyboru tematu

Gradient descent to **fundament** współczesnego uczenia maszynowego — algorytm, na którym oparte są wszystkie sieci neuronowe, od dużych modeli językowych po systemy rozpoznawania obrazów. Uczniowie często postrzegają AI jako czarną skrzynkę pełną zaawansowanej matematyki; tymczasem główna idea — *zgadnij, sprawdź jak bardzo się pomyliłeś, popraw się* — jest prosta i intuicyjna, dostępna już na poziomie liceum. Regresja liniowa z jedną cechą to najprostszy możliwy poligon: model ma tylko **dwa parametry** ($a$ i $b$), funkcję kosztu można narysować jako miskę, a wynik gradient descent można porównać z rozwiązaniem analitycznym, aby zobaczyć, że algorytm rzeczywiście trafia w prawdziwe optimum.

Lekcja odpowiada na konkretne, bliskie uczniom pytanie: **„Czy z liczby godzin nauki można przewidzieć wynik egzaminu?"** — i przy okazji odsłania mechanikę uczenia maszynowego, którą uczniowie spotkają w każdym kolejnym kontakcie z AI.

### Cele zajęć

**Uczeń po zajęciach:**
- wyjaśnia, na czym polega uczenie modelu poprzez iteracyjne poprawianie parametrów (analogia: historia Jasia z notatnika)
- interpretuje parametry $a$ (nachylenie) i $b$ (przesunięcie) regresji liniowej geometrycznie i dziedzinowo
- oblicza wartość funkcji kosztu MSE krok po kroku na małym zbiorze danych (3 punkty)
- używa interaktywnych suwaków, by ręcznie znaleźć dobre dopasowanie prostej do chmury punktów
- wyjaśnia, dlaczego zbyt duże tempo uczenia (learning rate) powoduje rozbieżność algorytmu, a zbyt małe — bardzo powolną zbieżność

### Metody i formy pracy

| Metoda | Opis |
|--------|------|
| Wykład interaktywny | Nauczyciel prowadzi przez kluczowe pojęcia, zadaje pytania naprowadzające, uczniowie uzupełniają intuicję |
| Demonstracja na żywo | Nauczyciel uruchamia `simple_example.ipynb` na rzutniku, komórka po komórce |
| Praca indywidualna / w parach | Uczniowie pracują z suwakami `a`, `b`, `learning_rate` na własnych komputerach |
| Ćwiczenie eksploracyjne | „Wyścig" — kto ręcznie znajdzie najniższy MSE, zanim odpalimy gradient descent? |
| Dyskusja podsumowująca | Co się dzieje, gdy `lr` jest za duży/za mały? Co łączy Jasia z algorytmem uczącym sieci neuronowe? |

**Forma:** lekcja z elementami ćwiczeń praktycznych przy komputerze (pracownia komputerowa lub laptopy uczniów)

### Wymagania w zakresie technologii

- Komputer z zainstalowanym Pythonem 3.10+ oraz bibliotekami: NumPy, pandas, Matplotlib, ipywidgets, ipykernel
- Jupyter Notebook lub VSCode z rozszerzeniem Jupyter
- Przeglądarka internetowa — jako alternatywa: Google Colab (działa od ręki, bez instalacji)
- Rzutnik / ekran dla demonstracji nauczyciela
- Plik danych `student_exam_scores.csv` (200 wierszy) — załączony do notatnika

> **Wariant minimalny:** lekcja może być prowadzona w trybie wyłącznie demonstracji na rzutniku, jeśli pracownia nie jest dostępna. Interakcja sprowadza się wówczas do tego, że nauczyciel zmienia suwaki, a uczniowie prognozują (głośno), co się stanie.

---

## Przebieg zajęć

### Faza wstępna — historia Jasia (5 min)

Nauczyciel czyta lub opowiada historię z pierwszej komórki notatnika: Jaś, zapytany przez nauczyciela o wartość współczynnika $a$, strzela kolejno **50 → 20 → 4 → 2**, korygując się na podstawie wskazówek („za dużo", „lepiej, ale wciąż daleko").

**Kluczowe pytanie:** *„Czy Jaś znał wzór? Co go w takim razie poprowadziło do dobrego wyniku?"*

Dyskusja prowadzi do wniosku: **kierunek i wielkość pomyłki** — to wszystko, czego potrzebował. Nauczyciel zapowiada, że dzisiaj uczniowie zobaczą, jak komputer robi to samo, tylko automatycznie i znacznie szybciej — algorytm nazywa się **gradient descent**.

### Faza realizacji — część 1: dane i model (10 min)

Nauczyciel uruchamia komórki sekcji 1 notatnika (wczytanie CSV, scatter plot 200 studentów).

- **Kluczowe pytanie:** *„Patrząc na wykres — czy widzicie zależność? Jaką?"*
- Wniosek: dane układają się liniowo → spróbujmy dopasować prostą $\hat{y} = a x + b$.
- Nauczyciel pokazuje wizualizację z sekcji 2 („Co robi zmiana $a$, a co zmiana $b$") — dwa panele obok siebie:
  - lewy: różne nachylenia, ten sam punkt przecięcia z osią $Y$,
  - prawy: ten sam kąt, różne przesunięcia w pionie.
- **Pytanie sprawdzające:** *„Jaką wartość ma $b$, jeśli prosta przechodzi przez (0, 25)? A ile wynosi $a$, jeśli przy $x=10$ jest $y=45$?"*

### Faza realizacji — część 2: funkcja kosztu i ręczne strzelanie (10 min)

Nauczyciel uruchamia komórkę z **toy-przykładem na 3 punktach** (sekcja 2: „Policzmy MSE krok po kroku"). Pokazuje tabelkę z błędami i kwadratami błędów (MSE = 10.0).

- **Pytanie:** *„Dlaczego podnosimy błędy do kwadratu, a nie sumujemy ich z minusem?"* → bo plus i minus by się znosiły.
- Komórka z wykresem reszt (szare odcinki pionowe) — uczniowie widzą wizualnie, **co dokładnie MSE mierzy**.

**Ćwiczenie „wyścig":** uczniowie otwierają interaktywną komórkę z sekcji 3 („Teraz Twoja kolej — pobaw się suwakami"). Cel: kto ręcznie znajdzie najniższy MSE w ciągu 3 minut? Wskazówka z notatnika: optymalne MSE to ok. **18**. Najlepsi uczniowie zwykle dochodzą do 19–20.

### Faza realizacji — część 3: gradient descent (10 min)

**Pytanie wprowadzające:** *„Skoro ręcznie da się dojść do MSE około 20, czy komputer poradzi sobie lepiej? Jak miałby się o tym dowiedzieć?"*

Nauczyciel pokazuje **powierzchnię funkcji kosztu** (kontury + 3D z sekcji 4) i mówi: *„Komputer nie widzi całej tej miski. Widzi tylko, w którą stronę spada teren pod nogami — i robi krok w tę stronę."*

Krótkie omówienie idei gradient descent **bez wyprowadzania pochodnych** (te są w sekcji 5 notatnika — do pracy własnej):
1. Zacznij od dowolnego $(a, b)$.
2. Policz **gradient** — wektor wskazujący „pod górę".
3. Zrób mały krok **w przeciwną stronę**.
4. Powtarzaj, aż MSE przestanie spadać.

Nauczyciel uruchamia komórki sekcji 6 (implementacja) i sekcji 7 (trajektoria) — uczniowie widzą czerwoną ścieżkę staczającą się do dna miski oraz krzywą uczenia (loss → ~18 w 200 iteracjach).

### Faza ćwiczeń — interaktywna piaskownica (7 min)

Uczniowie otwierają sekcję 8 notatnika (interaktywna piaskownica z czterema suwakami). Testują przypadki:
1. **Dobre tempo uczenia** (`lr = 0.05`) → płynna zbieżność.
2. **Za duże tempo** (`lr = 0.5` lub więcej) → trajektoria oscyluje, MSE wybucha.
3. **Za małe tempo** (`lr = 0.001`) → algorytm pełza, nie dojdzie do dna w 50 iteracjach.
4. **Inny punkt startowy** → czy zawsze trafia w to samo minimum? (Tak — bo funkcja jest wypukła.)

Nauczyciel zachęca: *„Spróbuj zepsuć algorytm. Jaki najmniejszy `lr` daje rozbieżność?"*

### Faza podsumowania (3 min)

Rundka: każdy uczeń kończy zdanie *„Gradient descent działa, bo..."* lub *„Zaskoczyło mnie, że..."*

Nauczyciel podaje źródła do dalszej eksploracji (3Blue1Brown — *Gradient descent, how neural networks learn*) i zapowiada, że **ten sam algorytm**, tylko z milionami parametrów zamiast dwóch, napędza wszystkie współczesne sieci neuronowe.

---

## Ewaluacja / dodatkowe informacje

### Forma prowadzenia zajęć

Lekcja stacjonarna w pracowni komputerowej z elementami demonstracji nauczyciela i samodzielnej pracy uczniów z interaktywnym notatnikiem.

### Sposób ewaluacji zajęć

- Obserwacja aktywności podczas dyskusji i ćwiczenia „wyścigu" — kto trafi w najmniejszy MSE?
- Sprawdzenie zrozumienia przez krótki quiz (3 pytania zamknięte): co oznacza $a$, co oznacza $b$, co się dzieje gdy `lr` jest za duży?
- Opcjonalna karta wyjścia (exit ticket): *„Jednym zdaniem opisz, czym gradient descent różni się od strzelania Jasia."*

### Wskazówki dla innych nauczycieli korzystających z tego scenariusza

- **Uruchom notatnik dzień wcześniej** i upewnij się, że ipywidgets działają — w VSCode Jupyter wymaga wskazania właściwego kernela (`.venv`). Bez działających suwaków lekcja traci najważniejszy element interaktywności.
- **Historia Jasia jest kluczowa** — nie pomijaj jej. To jedyny moment, w którym uczniowie zbudują intuicję, do której będą wracać przy każdym kolejnym pojęciu.
- **Nie wyprowadzaj pochodnych na lekcji** — to zabija tempo. Wyprowadzenie jest w sekcji 5 notatnika; dla zainteresowanych uczniów może być częścią pracy własnej lub punktem wyjścia do kolejnej lekcji.
- **Ćwiczenie „wyścigu"** świetnie angażuje. Można zaproponować małą nagrodę. Najlepszy uczeń dochodzi zwykle do MSE około 19, czyli niewiele więcej niż optimum (18.19) — co stanowi naturalne wprowadzenie do gradient descent.
- **Pytanie kontrolne na koniec:** *„Co by się stało, gdyby nasza funkcja kosztu miała kilka miejscowych dolin, jak w sieciach neuronowych?"* — świetne wprowadzenie do następnej lekcji o sieciach.
- **Jeśli zostanie czas:** pokaż uczniom sekcję „Forma zamknięta (least squares)" — uczniowie widzą, że gradient descent dochodzi dokładnie do tego samego wyniku co wzór matematyczny, co buduje zaufanie do algorytmu.

### Materiały pomocnicze

- Notatnik: `simple_example.ipynb`
- Dane: `student_exam_scores.csv` (200 rekordów: godziny nauki, sen, frekwencja, poprzednie wyniki, wynik egzaminu)
- Wideo wprowadzające: [3Blue1Brown — Gradient descent, how neural networks learn](https://www.youtube.com/watch?v=IHZwWFHWa-w) (polecane jako praca domowa przed lekcją lub po niej)
- Plansza pomocnicza na tablicy: trzy panele *model — funkcja kosztu — algorytm uczenia* — uczniowie mają cały czas przed oczami strukturę problemu

---

## Licencja

CC BY-NC 4.0 — Uznanie autorstwa — Użycie niekomercyjne — Na tych samych warunkach (Międzynarodowe 4.0)
