# Scenariusz projektu edukacyjnego — ~10 godzin

---

## Podstawowe informacje

**Temat projektu:** Od strzelania na ślepo do precyzyjnego uczenia — własna implementacja gradient descent

**Etap edukacyjny i typ edukacji:** Szkoła ponadpodstawowa — IV etap edukacyjny (liceum / technikum, klasa z rozszerzeniem informatyki lub matematyki)

**Przedmiot:** Informatyka (możliwa realizacja interdyscyplinarna z matematyką — analiza i statystyka)

**Czas trwania projektu:** ok. 10 godzin (5 spotkań × 2h lub 4 spotkania × 2h + praca własna ~2h)

---

## Szczegóły scenariusza

### Uzasadnienie wyboru tematu

W odróżnieniu od projektów opartych na gotowych modelach (np. CNN do rozpoznawania cyfr), gradient descent na regresji liniowej pozwala uczniom **zbudować algorytm od zera, w ok. 30 liniach kodu**, i bezpośrednio porównać go z rozwiązaniem analitycznym (metoda najmniejszych kwadratów). To rzadka okazja, by zobaczyć w pełni kompletny kawałek matematyki uczenia maszynowego — bez „magii" bibliotek typu Keras czy PyTorch, w których wszystko zamknięte jest w wywołaniach `model.fit()`.

Projekt prowadzi uczniów przez **cykl eksperymentu naukowego**: stawiają hipotezy o wpływie hiperparametrów (learning rate, punkt startowy, standaryzacja, liczba iteracji), uruchamiają serię treningów, mierzą rezultaty, wyciągają wnioski. Końcowym osiągnięciem jest **rozszerzenie modelu z 1 do 2+ cech** (regresja wielowymiarowa) — i odkrycie, że ten sam algorytm działa praktycznie bez zmian. To moment, w którym uczniowie czują, że dostali do ręki **uniwersalne narzędzie**, a nie jednorazową sztuczkę.

### Cele projektu

**Uczeń po projekcie:**
- samodzielnie wyprowadza wzory na gradienty funkcji kosztu MSE i implementuje gradient descent od zera w czystym NumPy
- projektuje i przeprowadza eksperyment porównujący co najmniej 4 warianty hiperparametrów algorytmu z udokumentowanymi wynikami
- interpretuje krzywe uczenia (MSE w funkcji iteracji) i rozpoznaje sygnały rozbieżności, stagnacji lub poprawnej zbieżności
- porównuje wyniki gradient descent z rozwiązaniem analitycznym i wyjaśnia, dlaczego oba dają to samo (wypukłość MSE w regresji liniowej)
- rozszerza model regresji z 1 do 2+ cech, modyfikując własną implementację, i interpretuje współczynniki dziedzinowo
- przedstawia wyniki w formie raportu eksperymentalnego lub prezentacji z wizualizacjami

### Metody i formy pracy

| Metoda | Opis |
|--------|------|
| Projekt grupowy (2–3 osoby) | Uczniowie pracują w zespołach z podziałem ról: badacz / programista / dokumentalista |
| Eksperyment naukowy | Każdy zespół formułuje hipotezę i przeprowadza co najmniej 4 warianty treningu |
| Praca z notatnikiem Jupyter | Modyfikacja i rozszerzanie kodu z `simple_example.ipynb` |
| Prezentacja wyników | Krótkie (5–7 min) wystąpienie z wnioskami i wizualizacjami |
| Peer review | Wzajemna ocena raportów zespołowych według wcześniej ustalonej rubryki |

**Forma:** zajęcia stacjonarne w pracowni komputerowej + praca własna uczniów w domu (Google Colab albo lokalna instalacja)

### Wymagania w zakresie technologii

- Python 3.10+ z bibliotekami: NumPy, pandas, Matplotlib, ipywidgets, ipykernel, nbformat
- Jupyter Notebook / JupyterLab lub Google Colab (opcja bez instalacji)
- Plik danych `student_exam_scores.csv` (200 rekordów, załączony do notatnika)
- Telefon lub aparat — opcjonalnie, jeśli zespoły chcą zebrać własne dane (ankieta wśród klasy)

> **Bez GPU:** cały projekt działa na CPU — pojedynczy eksperyment trwa kilka sekund, więc nawet seria 20 wariantów to mniej niż 2 minuty czasu obliczeniowego. To pozwala uczniom na **bardzo intensywną eksplorację** bez czekania.

---

## Przebieg projektu

### Etap 1 — Kick-off i model bazowy (2h, spotkanie 1)

**Godzina 1 — Wprowadzenie i intuicja**

- Nauczyciel przedstawia cel projektu i kryteria oceniania.
- Uczniowie tworzą zespoły (2–3 osoby) i otwierają `simple_example.ipynb`.
- Wspólne przejście przez sekcje 1–3 notatnika: dane, model, ręczne strzelanie. Każdy zespół zapisuje, jaki najmniejszy MSE udało im się znaleźć **ręcznie** z suwakami — będzie to punkt odniesienia.
- Nauczyciel pokazuje wyprowadzenie gradientów (sekcja 5) i wspólnie z klasą weryfikuje pochodne na tablicy.

**Godzina 2 — Pierwszy własny gradient descent**

- Każdy zespół uruchamia komórkę z implementacją (sekcja 6) i potwierdza, że wynik zgadza się z rozwiązaniem analitycznym ($a^* \approx 1.6341$, $b^* \approx 23.6184$, MSE ≈ 18.19).
- Uczniowie zapisują **wyniki modelu bazowego** jako punkt odniesienia: $a^*$, $b^*$, końcowy MSE, liczba iteracji do zbieżności.
- Każdy zespół wybiera **pytanie badawcze** do realizacji w kolejnych etapach:

> Przykłady pytań badawczych:
> - *„Jak wartość learning rate wpływa na liczbę iteracji potrzebnych do zbieżności i ryzyko rozbieżności?"*
> - *„Czy standaryzacja cech naprawdę przyspiesza uczenie? O ile dokładnie i dlaczego?"*
> - *„Czy punkt startowy ma znaczenie, jeśli funkcja kosztu jest wypukła? A jeśli wystartujemy 100 jednostek od optimum?"*
> - *„Jak zachowa się gradient descent po dodaniu drugiej cechy (`sleep_hours`)? Czy ta cecha rzeczywiście pomaga przewidzieć wynik?"*
> - *„Jak zmiana liczby iteracji wpływa na końcowy MSE? Czy zawsze więcej znaczy lepiej?"*

**Praca własna po etapie 1:** Każdy uczeń przegląda sekcje 4–5 notatnika (powierzchnia kosztu, wyprowadzenie gradientów) i przygotowuje 3 pytania na następne spotkanie.

---

### Etap 2 — Eksperymenty z hiperparametrami (4h, spotkania 2–3)

**Spotkanie 2 (2h) — Pierwsza seria eksperymentów**

Każdy zespół przeprowadza co najmniej 4 warianty treningu wokół wybranego pytania badawczego. Wyniki zapisuje w tabeli:

| Wariant | Zmiana parametru | Końcowe MSE | Iteracji do zbieżności | Stabilność (czy się rozbiegało?) | Wnioski |
|---------|------------------|-------------|------------------------|----------------------------------|---------|
| Baseline | lr=0.05, n=200, start (0,0) | 18.19 | ~150 | stabilny | — |
| Wariant A | … | … | … | … | … |
| Wariant B | … | … | … | … | … |
| Wariant C | … | … | … | … | … |

Uczniowie traktują notatnik Jupyter jako **dziennik badań** — każda komórka opisana w markdownie, każdy wykres podpisany. Nauczyciel wymusza zasadę: *„Każdy eksperyment kończysz zrzutem do tabeli — inaczej nie istniał."*

**Spotkanie 3 (2h) — Porównanie z formą zamkniętą i analiza wyników**

- Nauczyciel pokazuje sekcję notatnika „Forma zamknięta (least squares)" i tłumaczy, dlaczego dla regresji liniowej z MSE wzór analityczny istnieje (zerowanie gradientu funkcji wypukłej).
- Każdy zespół potwierdza, że ich najlepszy wariant gradient descent dochodzi do **identycznego** $(a^*, b^*)$ co wzór.
- Pytanie kontrolne dla całej klasy: *„Skoro mamy wzór, po co GD?"* — wspólne wypracowanie odpowiedzi (sieci neuronowe, duże zbiory danych, ogólność, nieliniowości).
- Każdy zespół generuje **wykres porównawczy**: krzywe uczenia różnych wariantów obok siebie, zbiegające do tej samej wartości (lub rozbieżne).

**Praca własna po etapie 2:** Każdy zespół pisze 1 stronę raportu z opisem przeprowadzonych eksperymentów i wstępnymi wnioskami.

---

### Etap 3 — Rozszerzenie: regresja wielowymiarowa (2h, spotkanie 4)

**Godzina 1 — Druga cecha**

Każdy zespół modyfikuje implementację, by przyjmowała **dwie cechy** (np. `hours_studied` i `sleep_hours`):
$$\hat{y} = a_1 x_1 + a_2 x_2 + b$$

Wyprowadzenie nowych gradientów (dla $a_1$, $a_2$, $b$) — wspólnie na tablicy, jeśli zespoły mają trudność. To zwykle 5 minut.

Kluczowe pytania:
- *„Czy dodanie drugiej cechy poprawiło dopasowanie? O ile spadł MSE w porównaniu do modelu jednowymiarowego?"*
- *„Który współczynnik jest większy — $a_1$ czy $a_2$? Co to znaczy dziedzinowo (która cecha „bardziej" przewiduje wynik)?"*
- *„Spróbujcie dodać 3. cechę (`previous_scores`). Czy MSE spadł znacząco?"*

**Godzina 2 — Własne dane (opcjonalnie, dla zaawansowanych)**

Zespoły zbierają mini-ankietę w klasie (5–10 osób): ile godzin nauki, ile snu, jaki wynik z ostatniego sprawdzianu. Trenują swój model na tych danych i porównują z modelem z 200 studentów.

- *„Czy nasz model 'na klasie' przewiduje sensownie wyniki? Na ile?"*
- *„Co jeśli zbiór jest tak mały, że MSE wynosi prawie 0? Czy to znaczy, że model jest świetny?"* → wprowadzenie pojęcia **przeuczenia** (overfitting).

---

### Etap 4 — Raport i prezentacje (2h, spotkanie 5)

**Przygotowanie (30 min)** — każdy zespół finalizuje slajdy lub żywy notatnik zawierający:
1. Pytanie badawcze i hipotezę
2. Tabelę wyników eksperymentów (co najmniej 4 warianty)
3. Co najmniej dwa wykresy: krzywa uczenia + scatter z dopasowaną prostą
4. Porównanie z rozwiązaniem analitycznym
5. Wyniki rozszerzenia na 2+ cechy
6. Wnioski + co zrobiliby inaczej, gdyby zaczynali od nowa

**Prezentacje zespołów (ok. 7 min × liczba zespołów)**

- Każdy zespół prezentuje (5–7 min) + 2 min na pytania od klasy i nauczyciela.
- Inne zespoły wypełniają kartę peer review (skala 1–5: jasność prezentacji, solidność eksperymentu, trafność wniosków).

**Podsumowanie nauczyciela (15 min)**

- Zestawienie najlepszych wariantów ze wszystkich grup.
- Co najczęściej zaskakiwało? Które hipotezy zostały obalone?
- Perspektywy: gradient descent w sieciach neuronowych — co się zmienia (autograd, miliony wag, minima lokalne, mini-batche), a co zostaje (ta sama pętla update). Naturalne wprowadzenie do następnego projektu.

---

## Ewaluacja / dodatkowe informacje

### Forma prowadzenia zajęć

Projekt realizowany w pracowni komputerowej (spotkania stacjonarne) z częścią pracy własnej w domu. Możliwa realizacja w całości zdalnie z wykorzystaniem Google Colab.

### Sposób ewaluacji zajęć

**Ocena projektu (100 pkt):**

| Kryterium | Punkty |
|-----------|--------|
| Poprawne sformułowanie i uzasadnienie hipotezy badawczej | 10 |
| Wykonanie co najmniej 4 wariantów eksperymentu z pełną dokumentacją (tabela + wykresy) | 25 |
| Poprawna analiza wyników — interpretacja krzywych uczenia, porównanie z formą zamkniętą | 20 |
| Rozszerzenie modelu na 2+ cech z analizą dziedzinową współczynników | 15 |
| Jakość kodu (czytelność, komentarze, brak duplikacji) | 10 |
| Jakość prezentacji / raportu | 10 |
| Peer review od innych zespołów | 10 |

**Próg zaliczenia:** 60 pkt

### Wskazówki dla innych nauczycieli korzystających z tego scenariusza

- **Wyprowadzenie pochodnych na tablicy** w etapie 1 jest kluczowe — wielu uczniów do tej pory nie widziało, do czego analiza matematyczna im się przyda. To moment, w którym pochodne nabierają konkretnego, praktycznego sensu.
- **Pytania badawcze warto zatwierdzać** przed etapem 2 — uczniowie czasem wybierają pytania zbyt trywialne („czy lr=0 to złe?") albo niemierzalne. Dobry zakres: pytanie z nieoczywistą odpowiedzią, mierzalne w 3–4 wariantach.
- **Standaryzacja cech** to klasyczna pułapka — bez niej trzeba `lr` rzędu $10^{-4}$, a uczniowie często myślą, że ich kod ma błąd. Warto omówić to wcześnie i wymusić standaryzację jako część baseline.
- **Dla mocniejszych zespołów:** poproś o implementację **stochastic gradient descent** (z mini-batchami zamiast całego zbioru) — to naturalny krok ku sieciom neuronowym i pokazuje, dlaczego SGD jest standardem w deep learningu.
- **Częsty błąd:** zespoły zapominają zapisywać wyniki każdego wariantu przed uruchomieniem kolejnego. Wymuszaj prowadzenie tabeli wyników od pierwszego eksperymentu — najlepiej w formacie markdown w samym notatniku.
- **Etap 3 z własnymi danymi** może być przeniesiony do pracy własnej, jeśli brakuje czasu. Najważniejsze jest rozszerzenie na 2 cechy z dostarczonego zbioru.
- **Dla klas mniej zaawansowanych:** ogranicz eksperymenty do jednego parametru (np. tylko learning rate). Pomiń etap regresji wielowymiarowej.

### Materiały pomocnicze

- Notatnik bazowy: `simple_example.ipynb`
- Dane: `student_exam_scores.csv` (200 wierszy, kolumny: `hours_studied`, `sleep_hours`, `attendance_percent`, `previous_scores`, `exam_score`)
- Wideo wprowadzające: [3Blue1Brown — Gradient descent, how neural networks learn](https://www.youtube.com/watch?v=IHZwWFHWa-w)
- Dokumentacja NumPy: [numpy.org/doc](https://numpy.org/doc/)
- Karta peer review (do przygotowania przez nauczyciela na podstawie kryteriów ewaluacji)
- Opcjonalnie: notatnik wykorzystujący PyTorch z `autograd` — jako podgląd, jak ten sam algorytm wygląda w nowoczesnym frameworku (dla zainteresowanych)

---

## Licencja

CC BY-NC 4.0 — Uznanie autorstwa — Użycie niekomercyjne — Na tych samych warunkach (Międzynarodowe 4.0)
