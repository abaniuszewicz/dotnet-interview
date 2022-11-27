---
layout: default
title: Behawioralne
parent: Wzorce projektowe
nav_order: 3
---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---


Opisują jak efektywnie komunikować oraz dzielić się obowiązkami między obiektami.

## Chain of responsibility

{: .def }
Pozwala przekazywać żadania wzdłuż łańcucha handlerów. Handlery mogą albo obsłużyć żądanie, albo przekazać je dalej.

- można ustalać kolejność obsługi żądania
- linked list: każdy node ma referencję do następnego elementu
  - SRP: tworzymy proste handlery które wiedzą jak obsłużyć żądanie
  - O/CP: można łatwo rozłączyć łańcuch i dodać/usunąć jakiś element bez naruszania innych
- dekorator: zawsze przekaże żądanie dalej, chain of responsibility: może zatrzymać procesowanie
- przykład: mouse click na UI
  - zdarzenie dostanie najpierw _najbardziej wewnętrzny_ element, jak nie obsłuży to bombelkujemy wyżej
  - klikamy np. na disabled button, jako że jest wyłączony - nie będzie obsługiwał zdarzenia
  - button znajduje się w jakimś gridzie, ale go też nie obchodzi akurat to kliknięcie (może w złym miejscu?)
  - grid znajduje się w oknie, a ono może np. wyświetlić tooltipa z wiadomością helpa

## Command

{: .def }
Hermetyzuje żądania w postaci obiektów, co pozwala na parametryzowanie obiektów różnymi żądaniami oraz obsługiwanie operacji, które można wycofać.

- pozwala na składowanie żądań w formie obiektu, dzięki czemu można je przekazywać, kolejkować, zapisywać, undo-ować
- separuje klasę wysyłającą żądanie od konkretnej implementacji
- `interface ICommand` z jedną metodą `.Execute()`, ewentualnie `IUndoableCommand : ICommand` z metodą `.Undo()`
  - w C\#: `ICommand` posiada `.Execute(object param)`, `.CanExecute(object param)` oraz `event CanExecuteChanged`
- memento: składuje _stan_ obiektu (potencjalnie dużo pamięci), command w/ undo: składuje _wiedzę_ jak cofnąć komendę
- przykład: przycisk w edytorze tekstu
  - zamiast wielu podklas `OkButton`, `CancelButton`, `SaveButton` które zawierają swoją logikę, tworzymy jedną klasę `Button` która przyjmuje `ICommand` (`OkCommand`, `CancelCommand`, `SaveCommand`)
  - poszczególne komendy przyjmują w swoim konstruktorze serwisy do których delegują swoje operacje: `SaveCommand.Execute()` → `FileManager.SaveFile(file)`
  - dzięki temu ten sam kod (`SaveCommand`) może być użyty też w innych sytuacjach (np. użycie skrótu <kbd>CTRL</kbd> + <kbd>S</kbd>)

## Interpreter

## Iterator

{: .def }
Zapewnia sekwencyjny dostęp do jakiegoś zbioru bez ujawniania jego wewnętrznej implementacji.

- Hermetyzacja zbioru: klient nie musi wiedzieć jaka jest kolekcja pod spodem, wystarczy mu możliwość iteracji
- SRP: zbiór nie jest odpowiedzialny za iterowanie się po sobie, zajmuje się tym jego iterator
- O/CP: można dodawać nowe iteratory (np. różne możliwości przejścia po drzewie binarnym) do tego samego zbioru
- można iterować tą samą kolekcję równolegle różnymi iteratorami (każdy przechowuje swój stan) 
- implementacja:
  - _ręcznie_: `bool MoveNext()` + `T GetCurrent()`
  - C#: `IEnumerator`/`IEnumerator<T>`
- przykład: książka kucharska
  - `Recipe` + iterator na `Ingredient`

## Mediator

{: .def }
Ogranicza bezpośrednią komunikację między obiektami i zmusza je do współpracy wyłącznie za pośrednictwem obiektu mediatora.

- obiekty nie wiedzą o sobie, wiedzą tylko o mediatorze
- upraszcza system, bo cała _logika sterująca_ znajduje się w jednym miejscu
- zmniejsza liczbę komunikatów przesyłanych w systemie (zamiast n:n, n:1:n)
- mediatory mają tendencje do rozrastania się (_god object_)
- przykład: dialog box
  - dialog box zawiera button <kbd>Save</kbd> (enabled/disabled) oraz listview
  - chcemy żeby przycisk był enabled kiedy _coś_ jest wybrane w liście
  - listview nie informuje przycisku bezpośrednio, zamiast tego informuje mediator, a on przekazuje do innych zainteresowanych
  - dzięki temu możemy podpiąć dodatkowe kontrolki, np. label który będzie wyświetlał co zostało zaznaczone
- implementacja:
  - klasycznie: mediator tworzy komponenty i przekazuje do nich siebie. Komponenty po aktualizacji wywołują metodę mediatora `mediator.Updated(this)`. Mediator sprawdza co zostało zaktualizowane, a następnie wykonuje odpowiednie akcje (`if (component == listview) button.Enabled = listview.HasSelection`).
  - observer: mediator subskrybuje się do komponentów (`component.Subscribe(this)`), a następnie po jego powiadomieniu przez komponent `subscribers.ForEach(s => s.Notify(this))`, wykonuje odpowiednie akcje (`button.Enabled = listview.HasSelection`).

## Memento

{: .def }
Pozwala zapisywać i przywracać wcześniejszy stan obiektu bez ujawniania szczegółów jego implementacji.

- zapewnia funkcję przywracania (`undo`)
- SRP/hermetyzacja głównego obiektu
  - poprzednie stany trzymamy w _memento_
  - zarządzaniem stanu zajmuje się _caretaker_  
- zapisywanie/przywracanie może być czasochłonne
- trzeba uważać na typy referencyjne: shallow/deep copy
- przykład: edytor tekstu
  - `Editor` - przechowuje zawartość, tytuł pliku, położenie kursora. Dostarcza metody `.GetState()`, `.RestoreState()`.
  - `EditorState` - przechowuje część/wszystkie parametry (np. samą zawartość)
  - `EditorHistory` - przechowuje listę (`Stack`) poprzednich stanów. Dostarcza metody `.Push(state)`, `.Pop()`.

## Observer

{: .def }
Definiuje pomiędzy obiektami relację jeden-do-wielu w taki sposób, że kiedy wybrany obiekt zmienia swój stan, to wszystkie jego obiekty zależne zostają o tym powiadomione i automatycznie zaktualizowane.

- używany kiedy inne obiekty muszą zostać powiadomione o zmianie stanu jakiegoś obiektu
- publisher posiada 3 metody: `.Subscribe(subscriber)`, `.Unsubscribe(subscriber)` oraz `.Notify()`, często są zamykane w klasie bazowej lub osobnym obiekcie
- push vs pull:
  - push: publisher wysyła dane których potrzebują subskrybenci (`subscriber.Notify(int)`)
  - pull: subskrybenci pobierają dane których potrzebują (`subscriber.Notify(this)`)
- podobne wzorce:
  - **chain of command**: przekazuje żądania sekwencyjnie do momentu aż ktoś obsłuży, **observer**: _losowo_ (w środku jest _jakaś_ struktura, ale nie wiemy jaka i co/kiedy się podpięło) do wszystkich
  - **command**: enkapsuluje akcje, **observer**: enkapsuluje mechanizm powiadamiania
  - **mediator**: komponenty rozmawiają z punktem centralnym, **observer**: kompenenty mogą być jednocześnie publisherami i subskrybentami i być powiązane ze sobą
    - można też stworzyć mediatora z użyciem obserwatora, gdzie mediator będzie subskrybentem i będzie obserwował komponenty

## State

{: .def }
Umożliwia obiektowi zmianę zachowania wraz ze zmianą jego wewnętrznego stanu. Po takiej zmianie funkcjonuje on jak inna klasa.

- :x: zazwyczaj implementowane za pomocą instrukcji warunkowych if/switch
  - nie skaluje (nowy stan = nowe zmiany)
  - dobre dla prostych i **skończonych** sytuacji
- :heavy_check_mark: możemy wykorzystać polimorfizm
  - poszczególne stany implementują ten sam interfejs
  - klasa główna deleguje wywołania do poszczególnych stanów
  - stan robi co musi, a następnie przełącza klasę główną w inny stan (referencję do klasy głównej dostaje przez konstruktor)
- podobne do strategii
  - strategia jest nieświadoma innych implementacji, state jest i może przestawiać aktualny tryb
  - obiekt może zawierać wiele strategii, state jest tylko jeden
- przykład: nawigacja
  - inaczej wyznaczamy drogę oraz ETA dla samochodu, roweru czy pójścia pieszo
  - możemy to rozwiązać ifami/switchem (`if (mode == TravelMode.Car)`) ale nie będzie to skalowało (nowy mode = nowy warunek)
  - lepiej wyodrębnić interfejs `ITravelMode` z metodami `.CalculateEta()` oraz `.GetDirection()`

## Strategy

{: .def }
Definiuje rodzinę algorytmów i wkłada je w osobne klasy które można wymieniać. Sprawia że algorytm klienta staje się niezależny od konkretnej implementacji.

- izolacja algorytmu od szczegółów implementacji kroków
- umożliwia wybór algorytmu w trakcie runtime (wstrzykiwanie, nawet ify)
- można zapewnić nowe działanie bez modyfikacji klienta
- przykład: `ImageProcessor`
  - `ICompressor`: `JpegCompressor : ICompressor`, `PngCompressor : ICompressor`
  - `IFilter`: `BlackAndWhiteFilter : IFilter`, `HighContrastFilter : IFilter`

## Template method

{: .def }
Definiuje szkielet algorytmu, przekazując realizację niektórych kroków klasom dziedziczącym. Pozwala im na realizację niektórych kroków, ale **nie pozwala** na zmianę struktury algorytmu.

- podobne do strategii (_has-a_ vs _is-a_)
- podobne do factory method, ale różni się **celem** (_void_ tj. zrób coś, zamiast _\<T\>_ tj. stwórz coś)
- klasy nie mogą zmieniać algorytmu głównego: _sealed_ jest defaultową opcją w C#, w Javie trzeba użyć _final_
- podkroki mogą być wymagane (`abstract`) albo opcjonalne (`virtual`: jakaś domyślna implementacja albo pusty _hook_)
- przykład: czytanie pliku
  - `Parser` z definicją `.Parse()` które otwiera plik, deleguje `.ProcessStream()` do klas dziedziczących oraz zamyka plik
  - klasy dziedziczące `CsvParser : Parser`, `HtmlParser : Parser`, `XmlParser : Parser`, które umieją czytać swoje formaty ze `Stream`

## Visitor

{: .def }
Pozwala na dodanie nowych zachowań do struktur obiektów bez ich modyfikacji.

- używamy kiedy mamy niezmieniającą się strukturę obiektów i chcemy mieć możliwość dodawania na nich nowych operacji
- kiedy chcemy dodać nową operację, dodajemy nową implementację visitora
- poszczególne klasy przyjmują visitora (`.Accept(visitor)`) i wywołują odpowiednią dla siebie metodę interfejsu visitora (przez polimorfizm `.Visit(anchor)` lub sprecyzowaną nazwę `VisitAnchor(anchor)`)
- wady: złe dla niestabilnych struktur, brak dostępu do prywatnych składowych
- przykład: operacje na nodach html
  - zbiór jest zamknięty i zdefiniowany w standardzie, oprócz anchor, heading itd. nie będzie nowych node'ów
  - visitory dla nich będą różne w zależności od use case, np. `Highlight`, `Format`, `ExtractPlainText`
  - mając `visitor = new HighlightVisitor()` i listę node'ów, wywołamy na niej `.Accept(visitor)`. Każdy visitor poliformicznie wywoła odpowiednią dla siebie metodę visitora, przekazując do niej siebie `.Visit(this)`

## Null object

{: .def }
Usuwa konieczność obsługi _NULL_ przez klienta.

- upraszcza kod - brak `if (obj != null)`
- powoduje problem w sytuacjach w których trzeba coś zwrócić z metody (bo skoro nie `throw null`, to co zwrócić?)