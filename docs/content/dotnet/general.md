---
layout: default
title: Ogólne
parent: .NET/C#
nav_order: 1
---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## CLR

## Modyfikatory dostępu
- `public` - widoczne wszędzie
- `private` - widoczne tylko wewnątrz tej klasy
- `protected` - widoczne tylko wewnątrz tej klasy i klas dziedziczących
- `internal` - widoczne tylko wewnątrz danego assembly
- `protected internal` - widoczne w tym samym assembly assembly **lub** w klasach dziedziczących znajdujących się w innym assembly
- `private protected` - widoczne tylko wewnątrz klas dziedziczących znajdujących się w tym samym assembly

## Rodzaje kolekcji
- `T[]` - raczej nie używamy arrayów, chyba że wiemy że rozmiar kolekcji się nie zmieni albo dla konwencji (np. `byte[]`)
- `List<T>` - możliwość dodawania/usuwania elementów, pod spodem siedzi array który zmienia swój rozmiar. Dodawanie na koniec jest zazwyczaj szybkie, dodawanie na początku powoduje przepisanie całego arraya.
- `HashSet<T>` - brak kolejności, brak duplikatów
- `Queue<T>` - kolejka. Metody: `.Enqueue(t)`, `t .Dequeue()`, `t .Peek()`.
- `Stack<T>` - stos. Metody: `Push(t)`, `t Pop()`, `t .Peek()`.
- `LinkedList<T>` - metody: `.AddBefore(node, t)`, `.AddAfter(node, t)`
- `Dictionary<TKey, TValue>` - metody: `.Add(key, value)`, `.Contains(key)`, `bool TryGetValue(key, out value)`

## Sposoby iterowania
- `foreach` loop
- LINQ `collection.ForEach(el => {})`
- `for` loop
- `while`
- `do-while`
- używając biblioteki `Polly`

## Boxing/unboxing
- boxing - typ wartościowy na typ referencyjny; implicit (`int i = 123; object o = i;`)
- unboxing - typ referencyjny na wartościowy; explicit (`int i = 123; object o = i; int x = (int)o`)

## Dependency Injection

{: .def }
Dostarczanie obiektowi obiektów których potrzebuje zamiast _zmuszanie_ go do stworzenia ich samemu.

- dostarczane poprzez: konstruktor, settery, metody
- SRP: obiekt nie musi sam tworzyć swoich zależności
- DI/OCP: można podmienić zależności bez zmiany klas która ich używa (promuje _programowanie do interfejsów_)
- pozwala na mockowanie zależności

## Regex

{: .def }
Wzorzec opisujący łańcuch symboli

- symbole:
  - `.` - wszystko, `\d` - cyfra, `\D` - **nie**-cyfra, `\w` - `[a-zA-Z0-9]`, `\s` - whitespace
  - `|` - or, `()` - grupa
  - `[]` - zbiór znaków, `[a-z]` - zakres zbioru znaków, `[^a-z]` - żaden z zakresu
  - `^` - początek, `$` - koniec, `\b` - początek słowa
  - `*` - zero lub więcej, `+` - jeden lub więcej, `?` - zero lub jeden, `{m}` - _m_ razy, `{m,n}` - od _m_ do _n_ razy (włącznie), `{m,}` - _m_ lub więcej razy, `*?` - lazy (jak najmniej)
- przykład: `\b\d{1,2}/\d{1,2}/\d{4}\b` matchowanie dat, np. 11/12/1994

## Stos/sterta
- typy referencyjne: sterta
- typy wartościowe: to zależy (np. jeżeli jest częścią klasy, jest zboxowany, jest w arrayu itd. to na stercie)

## `Task`/`Thread`
- `Task` - _obietnica rezultatu uzyskanego w przyszłości_. Używa `Threadpool`.
  - może zwrócić rezultat
  - można je cancellować
- `Thread` - osobny wątek egzekucji, koncepcja niskopoziomowa (nie używaj). Wbudowane w system operacyjny, każdy ma swoją własną pamięć. Mogą wykonywać wiele tasków jednocześnie.
  - nie zwraca rezultatu
  - nie można ich cancellować

## Garbage collector
- wykonuje się kiedy:
  - systemowi brakuje pamięci, **lub**
  - obiekty zużywają dużo pamięci, **lub**
  - kiedy wywołamy `GC.Collect()` (don't)
- zbiera elementy do których nikt już nie używa referencji
- operuje na _generacjach_ (0, 1, 2):
  - w niższych generacjach żyją obiekty _krótkożyjące_, pamięć zbierana jest tam najczęściej
  - każdy obiekt który przeżyje swoją generację jest promowany do wyższej generacji
- dla obiektów _unmanaged_ (np. dostęp do plików, sieci) musimy wyczyścić pamięć sami:
  - dostarczamy implementację `IDisposable`
  - dostarczamy `~dekonstruktor` w razie gdyby klient zapomniał wywołać `.Dispose()`

## Sposoby realizowania współbieżności
- `async`/`await` - wykorzystuje obietnice w celu uniknięcia tworzenia niepotrzebnych wątków
- **Rx** (programowanie reaktywne) - opiera się na zdarzeniach asynchronicznych. Model `Observer`/`Observable` z subskrypcjami do wydarzeń.
- **Parallel** (programowanie równoległe) - obsługiwane przez statyczną klasę `Parallel` (np. `Parallel.ForEach(collection, element => {}))`
- **TPL Dataflow** - bloki przepływu danych które łączymy wejściami/wyjściami.
- **Thread** - just don't
- **BackgroundWorker** - just don't