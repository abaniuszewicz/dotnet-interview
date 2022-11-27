---
layout: default
title: Słowa kluczowe
parent: .NET/C#
nav_order: 2
---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---


## `interface`/`abstract`

| `interface`               | `abstract`                                |
|---------------------------|-------------------------------------------|
| _I can do something_      | _I'm something_                           |
| można implementować wiele | tylko jedna klasa bazowa                  |
| może mieć prywatne metody | może mieć tylko publiczne/internal metody |
| może mieć konstruktor     | nie może mieć konstruktora                |

## `class/struct`

| class                         | struct                        |
|-------------------------------|-------------------------------|
| typ referencyjny              | typ wartościowy               |
| może być nullem               | nie może być nullem           |
| można dziedziczyć             | nie można dziedziczyć         |
| mogą mieć metody              | mogą mieć metody              |
| mogą implementować interfejsy | mogą implementować interfejsy |

## `async`/`await`
- `async` - dodawane do deklaracji metody, umożliwia zastosowanie w niej `await`
  - zostały wprowadzone jako para żeby zachować kompatybilność wsteczną
  - metoda `async` musi zwracać `Task`, `Task<T>` lub `void` (tylko dla eventów)
- `await` - wykonuje asynchroniczne oczekiwanie na argument
  - najpierw sprawdza czy operacja się zakończyła:
    - jeżeli tak: kontynuuje wykonywanie (synchronicznie) 
    - jeżeli nie: wstrzymuje metodę `async` i zwraca niekompletne zadanie
  - gdy operacja się zakończyła jakiś czas później, metoda `async` wznawia działanie
- metoda `async` to kilka synchronicznych porcji rozdzielonych przez `await`
- _synchronization context_ jest przechwytywany w momencie kiedy `await` decytuje o wstrzymaniu metody
  - domyślnie metoda wraca na _kontekst_ na którym wystąpił `await` (np. interfejs użytkownika)
  - `.ConfigureAwait(false)` oznacza że kod zostanie wznowiony w wątku puli wątków
- `async`/`await` jest transformowany do maszyny stanów: w pętli oczekuje na zakończenie/anulowanie/wyjątek

## `break`/`continue`
- `break` - wyjdzie z pętli
- `continue` - przeskoczy aktualną iterację

## `using`/`Dispose`
- `using` - import typów w namespace/alias dla namespaceów
- `using` - automatyczne zwolnienie zasobów poprzez wygenerowanie kodu `try`-`finally`, gdzie `.Dispose()` jest umieszczone w `finally`
- `Dispose()` - pochodzi z interfejsu `IDisposable`; służy do zwolnienia _unmanaged_ zasobów (lub wywołania metod `.Dispose()` swoich podklas)

## `try`-`catch`-`finally`
- `try`
  - w bloku `try` umieszczamy operacje które _mogą_ się nie powieść (np. problemy z dostępem do pliku)
  - w przypadku wystąpienia wyjątku, kod przejdzie do bloku `catch` (o ile istnieje), a następnie `finally` (o ile istnieje)
  - blok `try` nie może występować samodzielnie; zawsze z `catch` i/lub `finally`
- `catch`
  - może być wiele bloków `catch` (dla różnych wyjątków) - zostanie wykonany pierwszy który spełni warunek wejścia
    - po typie: `catch (CustomException e)`
    - po typie + predykacie: `catch (CustomException e) when (e.Code == 123)`
- `finally`
  - najczęściej używane do zwolnienia zasobów: `.Dispose()`
  - wykona się zawsze, niezależnie czy w `try` poleci wyjątek czy nie
  - jeżeli w `try` umieścimy `return` statement, `finally` i tak się wykona

## `out`/`ref`
- oba to modyfikatory parametrów w sygnaturze metody
- `out` - zmienna **nie zostaje** przekazana z zewnątrz i musi zostać stworzona wewnątrz metody przed jej opuszczeniem (np. `int.TryParse(s, out result)`)
- `ref` - zmienna zostaje przekazana z zewnątrz
  - _podwójny wskaźnik_ - jeżeli parametr zmieni się wewnątrz funkcji, to zmieni się też w funkcji która go przekazała przez `ref`
  - używane też żeby metoda _mogła zwracać wiele argumentów_ (np. `DoSomething(ref int x, ref int y)`; to złe użycie, metoda powinna zamiast tego zwracać jakiś agregat/robić mniej)

## `!`/`?`/`?:`/`??`
- `?` - nullable type
  - używane przy definicji zmiennej: `int?`/`string?`
  - w przypadku typu wartościowego oznacza że może mieć on również wartość `null` (poprzez owrapowanie go w `Nullable<int>`)
  - w przypadku typu referencyjnego jest to wskazówka dla kompilatora że wartość może być `null`
- `?:` - ternary conditional operator
  - syntax na `if` - `warunek ? jeżeli_prawda : jeżeli_fałsz`
  - np. `var weather = temperature < 15 ? "cold" : "hot";`
- `?.` lub `?[]` - null conditional
  - wywoła operację po prawej stronie znaku tylko jeżeli obiekt po lewej nie jest nullem
  - jeżeli obiekt jest nullem, operacja zwróci wartość `null`
  - np. przypisanie wartości do zmiennej: `var inner = circle?.InnerCircle`
- `??` - null-coalescing operator
  - zwraca wartość po lewej stronie jeżeli nie jest ona nullem, w przeciwnym razie wykona operację po prawej i zwróci jej wartość
  - np. `var inner = circle.InnerCircle ?? new Circle()`
- `!` - null forviving/suppression operator
  - nie ma znaczenia w runtime, jest używany tylko żeby wyłączyć warningi kompilatora kiedy wiemy że coś _nie jest_ nullem

## `sealed`
- na klasach używane aby zapobiec powstawaniu klas dziedziczących
  - klasy związane z bezpieczeństwem
  - ogólnie _wszystkie_ klasy których nie przewidujemy dziedziczenia
- na klasach dodatkowo przyspiesza kod, bo CLR nie musi szukać czy istnieje jakaś implementacja _pod spodem_
- na metodach używane kiedy chcemy nadpisać zablokować możliwość dalszego override'owania metody jej nadpisywania w kolejnych klasach dziedziczących
  - może być tylko użyte razem ze słówkiem `override`
  - _normalne_ metody są `sealed` by default

## `virtual`/`abstract`

| `abstract`                                                           | `virtual`                                                                    |
|----------------------------------------------------------------------|------------------------------------------------------------------------------|
| tylko w klasach abstrakcyjnych                                       | w klasach abstrakcyjnych i _normalnych_                                      |
| nie posiada implementacji, klasa dziedzicząca **musi** ją dostarczyć | posiada implementację która **może** zostać nadpisana w klasie dziedziczącej |