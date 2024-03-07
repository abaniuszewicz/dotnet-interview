---
layout: default
title: Kreacyjne
parent: Wzorce projektowe
nav_order: 1
---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Opisują jak efektywnie tworzyć obiekty.

## Factory method

{: .def }
Przekazuje odpowiedzialność za tworzenie obiektów do klas podrzędnych.

- sygnatura metody wytwórczej powinna zwracać _abstrakcję_, poszczególne podklasy zwracają jej różne implementacje
- podobne do template method, ale różni się **celem** (_return \<T\>_ tj. stwórz coś, zamiast _void_ tj. zrób coś)
- przykład: `abstract class FileReader` który deleguje stworzenie `IParser` do podklas
  - szkielet algorytmu znajduje się w klasie `FileReader`
  - w jednym z kroków _prosi on_ podklasy o stworzenie mu `IParser`
  - poszczególne parsery (csv, html, xlsx) wiedzą jak czytać siebie
  - podklasy zwracają mu konkretne instancje `CsvParser : IParser`, a on `FileReader` bazuje na abstrakcji

## Abstract factory

{: .def }
Dostarcza interfejs do tworzenia całych rodzin spokrewnionych lub zależnych od siebie obiektów bez konieczności określania ich klas rzeczywistych.

- abstrakcyjne fabryki wytwarzają całe **spójne** rodziny produktów
- abstrakcyjna fabryka jest interfejsem, który jest implementowany przez konkretne fabryki
- często korzysta z factory method (to jedyne publiczne metody jakie znajdują się w abstrakcyjnej fabryce)
- factory method - dziedziczenie (**my** tworzymy _coś_), abstract factory - kompozycja (**mamy** fabrykę, która tworzy _coś_)
- przykład: GUI framework
  - `IUiElementFactory` pozwalające stworzyć abstrakcyjne `abstract class Button` oraz `abstract class Dialog`
    - `HtmlElementFactory` tworzące `HtmlButton` oraz `HtmlDialog`
    - `WindowsElementFactory` tworzące `WinButton` oraz `WinDialog`

## Builder

{: .def }
Oddziela konstrukcje złożonego obiektu od jego reprezentacji, dzięki czemu ten sam proces konstrukcji może prowadzić do różnych reprezentacji.

- hermetyzuje operacje niezbędne do stworzenia obiektu
- tworzenie w procedurze wielokrokowej (różnica względem factory)
- klient korzysta z interfejsu budowniczego, jego implementacja może być zmieniona bez jego wiedzy
- często wykorzystuje fluent interface (`return this`)
- jeżeli chcemy żeby jakieś kroki były wymagane, umieszczamy je w konstruktorze
- _sprząta_ konstruktor
  - nie musimy przekazywać 999 parametrów na raz
  - nadajemy znaczenie _magic parametrom_ (np. zamiast `new (1, 5, 8)` mamy `.WithRadius(1).SetX(5).SetY(8).Build()`)

## Prototype

{: .def }
Umożliwia kopiowanie istniejących obiektów bez tworzenia zależności do konkretnych klas.

- używamy kiedy musimy stworzyć obiekt poprzez skopiowanie istniejącego obiektu
- deleguje proces klonowania do samego obiektu, dzięki czemu mamy dostęp do pól prywatnych
  - serializacja + deserializacja **nie jest** implementacją wzorca, ale osiąga to samo
- przykład: powerpoint klonowanie figur
  - `abstract class Shape` ze zdefiniowaną metodą `abstract Shape Clone()`
  - każda podklasa np. `Circle : Shape` nadpisuje metodę i wie jak sklonować siebie, mając dostęp do wszystkiego
  - wiąże się to z tym że klient pracuje na `Shape` a nie na konkretnych klasach np. `Circle`

## Singleton

{: .def }
Pozwala zapewnić istnienie wyłącznie jednej instancji klasy oraz zapewnia globalny punkt dostępu do niej.

- prywatny konstruktor, aby nikt nie mógł go użyć
- globalny punkt dostępu `static T GetInstance()` który stworzy i zapisze się w polu statycznym (tylko za pierwszym razem; lazy loading), po czym zwróci istniejący obiekt
- problem przy wielu wątkach: użyj `Lazy<T>` lub double check lock `instance is null` >> `lock` >> `instance is null` (bo lockowanie jest kosztowne, a potrzebne tylko raz)
- zły bo
  - coupling
  - łamie SRP (robi _coś_ + tworzy siebie oraz kontroluje swój lifetime)
  - utrudnia testy (bo współdzielą stan singletona)
- przykład: klasa `ConfigManager` powinna mieć tylko jedną instancję

## Simple factory

{: .def }
Hermetyzuje tworzenie obiektu, zwłaszcza jeżeli proces tworzenia jest bardzo złożony.

- **nie** mylić z factory method/abstract factory
- ukrywa proces tworzenia (często skomplikowany)
- jeżeli obiekty były tworzone w różnych miejscach w kodzie, to mamy centralizacje i zmiany musimy wprowadzać tylko w jednym miejscu