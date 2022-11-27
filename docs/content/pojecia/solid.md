---
layout: default
title: SOLID
parent: Pojęcia
nav_order: 2
---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Single Responsibility Principle

{: .def }
Klasa powinna mieć tylko jedną odpowiedzialność (nigdy nie powinien istnieć więcej niż jeden powód do modyfikacji klasy).

## Open-Closed Principle

{: .def }
Klasy (encje) powinny być otwarte na rozszerzenia i zamknięte na modyfikacje.

## Liskov Substitution Principle

{: .def }
Funkcje które używają wskaźników lub referencji do klas bazowych, muszą być w stanie używać również obiektów klas dziedziczących po klasach bazowych, bez dokładnej znajomości tych obiektów.

- przykłady:
  - kwadrat dziedziczący po prostokącie: kwadrat **jest** prostokątem, jednak przy implementacjach z metodami `.SetWidth()` i `.SetHeight()`, które mają sens dla `Rectangle`, klasa dziedzicząca `Square : Rectangle` musiałaby je nadpisać żeby wywołanie `.SetWidth()` wywoływało jednocześnie `.SetHeight()` do tej samej wartości. To z kolei mogłoby popsuć np. unit test, który przyjmuje `Rectangle` (a więc potencjalnie też i `Square`), a następnie liczy jego pole poprzez ustawienie boków odpowiednio do długości 2 i 3. Jeżeli policzylibyśmy `rectangle.GetWidth() * rectangle.GetHeight()`, dla instancji _prawdziwego_ `Rectangle` dostalibyśmy 6, a dla `Square` 9.
  - struś dziedziczący po ptaku: struś **jest** ptakiem, jednak niektóre metody nie mają w jego kontekście sensu, jak np. `.Fly()`. Jeżeli do metody która spodziewa się dostać `Bird` i chce wywołać jego metodę latania, przekażemy instancję `Ostrich` aplikacja może znaleźć się w złym stanie przez to że struś nie potrafi latać.

## Interface Segregation Principle

{: .def }
Wiele dedykowanych interfejsów jest lepsze niż jeden ogólny.

## Dependency Inversion Principle

{: .def }
Wysokopoziomowe moduły nie powinny zależeć od modułów niskopoziomowych - zależności między nimi powinny wynikać z abstrakcji.