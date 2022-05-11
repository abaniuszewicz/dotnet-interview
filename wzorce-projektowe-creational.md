# Creational
Opisują jak efektywnie tworzyć obiekty.

## Factory method
Przekazuje odpowiedzialność za tworzenie obiektów do klas podrzędnych.
- metoda wytwórcza powinna zwracać _abstrakcję_, poszczególne podklasy mogą zwracać jej różne implementacje
- podobne do template method, ale różni się **celem** (_return \<T\>_ tj. stwórz coś, zamiast _void_ tj. zrób coś)

## Abstract factory
Dostarcza interfejs do tworzenia całych rodzin spokrewnionych lub zależnych od siebie obiektów bez konieczności określania ich klas rzeczywistych.
- abstrakcyjne fabryki wytwarzają całe **spójne** rodziny produktów - np. `IUiElementFactory` pozwalające stworzyć abstrakcyjne `Button` oraz `Dialog`, który jest implementowany przez `HtmlElementFactory` (`HtmlButton`, `HtmlDialog`) oraz `WindowsElementFactory` (`WinButton`, `WinDialog`)
- często korzysta z factory method (to jedyne publiczne metody jakie znajdują się w abstrakcyjnej fabryce)
- factory method - dziedziczenie (**my** tworzymy _coś_), abstract factory - kompozycja (**mamy** fabrykę, która tworzy _coś_)

## Builder
Oddziela konstrukcje złożonego obiektu od jego reprezentacji, dzięki czemu ten sam proces konstrukcji może prowadzić do różnych reprezentacji.
- hermetyzuje operacje niezbędne do stworzenia obiektu
- tworzenie w procedurze wielokrokowej (różnica względem factory)
- klient korzysta z interfejsu budowniczego, jego implementacja może być zmieniona bez jego wiedzy

## Prototype

## Singleton

## Simple factory
Hermetyzuje tworzenie obiektu, zwłaszcza jeżeli proces tworzenia jest bardzo złożony.
- **nie** mylić z factory method/abstract factory
- ukrywa proces tworzenia (często skomplikowany)
- jeżeli obiekty były tworzone w różnych miejscach w kodzie, to mamy centralizacje i zmiany musimy wprowadzać tylko w jednym miejscu