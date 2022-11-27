---
layout: default
title: Strukturalne
parent: Wzorce projektowe
nav_order: 2
---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

Opisują jak efektywnie składać obiekty w większe struktury.

## Adapter

{: .def }
Przekształca interfejs danej klasy do postaci innego interfejsu.

- pozwala na współpracę klas które ze względu na niekompatybilne interfejsy nie mogły ze sobą współpracować
- używamy kiedy nie mamy dostępu klasy i nie możemy do niej dodać interfejsu którego potrzebujemy
- adapter **implementuje** (_is-a_) interfejs docelowy oraz **posiada** (_has-a_) instancję obiektu adaptowanego, do którego deleguje żądania 
  - można też użyć dziedziczenia `Adapter : Source, IDestination`, ale tylko dla interfejsów (brak wielokrotnego dziedziczenia)
  
## Bridge

{: .def }
Służy do rozdzielenia skomplikowanej hierarchii na dwie osobne, które mogą rosnąć niezależnie od siebie.

- problem powstaje podczas pracy ze strukturami które mogą rosnąć w różnych kierunkach np. kolorowa (czerwona, niebieska, różowa) figura (okrąg, kwadrat, prostokąt)
  - zazwyczaj rozwiązane dziedziczeniem: `RedCircle`, `BlueCircle`, `PinkCircle`, `RedSquare`, ...
  - dodanie któregokolwiek nowego wymiaru pociąga za sobą dodanie wielu nowych klas np. nowy kolor: `PurpleCircle`, `PurpleSquare`, `PurpleRectangle`
- rozwiązaniem jest przestawienie się z dziedziczenia na kompozycję, tj. figura **ma** kolor
- GoF używa terminologii abstrakcja (figura) + szczególna abstrakcja (kwadrat, ...) oraz implementacja (kolor) + konkretne implementacje (czerwony, ...), ale nie chodzi o pojęcia z programowania; to tylko nazwy
- przykład: UI + API
  - mamy dwie hierarchie: 
    - UI: dla normalnego użytkownika, dla administratora, dla moderatora
    - API: Windows, Linux, Mac
  - :x: nie rozwiązujemy tego przez ify (`if (IsOnMac()) DoSomethingMacLike()`) - straszne spaghetti
  - :x: nie rozwiązujemy tego przez dziedziczenie (`UserWindowsLogic`, `AdminWindowsLogic`, `UserMacLogic,` itd.) - eskplozja klas
  - :heavy_check_mark: rozwiązujemy to przez kompozycję (`class UserLogic` który zawiera `DeviceApi`)
    - _lewa_ (`class UserLogic`) i _prawa strona_ (`interface IDeviceApi`) mostu powiązane są abstrakcją
    - poszczególne implementacje prawej strony (`WindowsApi`, `LinuxApi`) mogą być modyfikowane bez wpłynięcia na - opierają się na abstrakcji
    - _lewą stronę_ mostu też możemy rozwijać poprzez `AdminLogic : UserLogic` + dodatkowe metody (ale to nie jest wymagane)

## Composite

{: .def }
Pozwala łączyć obiekty w drzewa, a następnie jednolicie obsługiwać pojedyncze obiekty i ich zbiory.

- wykorzystuje rekurencję
- trzeba wyodrębnić wspólny interfejs dla liści i kompozytów
- **dekorator**: ma tylko jeden element podrzędny oraz dodaje nowe obowiązki, **kompozyt**: może mieć wiele elementów podrzędnych oraz nie dodaje nowych funkcjonalności
- przykład: PowerPoint grupowanie figur
  - interfejs `IShape` z metodą `.Move()`
  - okrąg `Circle : IShape`:
    - metoda `.Move()` przemieszcza po prostu siebie
  - zbiór okręgów/innych figur `Group : IShape`:
    - posiada metodę `.Add(IShape)`, dzięki czemu możemy mieć w środku zarówno pojedyncze figury jak i inne grupy
    - metoda `.Move()` iteruje po wszystkich `IShape` (figura/grupa figur) w sobie, a następnie wywołuje ich metodę move

## Decorator

{: .def }
Pozwala na dynamiczne przydzielanie obiektowi nowych zachowań. Zapewnia alternatywny sposób rozszerzenia funkcjonalności.

- pozwala na uniknięcie eksplozji subklas
- wykorzystuje kompozycje (_has-a_), a nie dziedziczenie (_is-a_)
- klasy dekorujące są tego samego typu co dekorowane (interfejs)
- można je _chainować_, konsumer nie wie czy pracuje z dekoratorem czy z "_prawdziwym_" obiektem (i nie musi tego wiedzieć)
- nowe zachowania możemy wywołać przed lub po zachowaniu klasy dekorowanej, w zależności od tego co chcemy osiągnąć
- wada: niektóre frameworki DI mają problem żeby zarejestrować dekoratory ze względu na _circular dependency_
- przykład: stream `IStream`
  - zamiast: `CloudStream : IStream`, `CompressedCloudStream : CloudStream`, `EncryptedCloudStream : CloudStream`, `CompressedAndEncryptedCloudStream : CloudStream`
  - mamy: `CloudStream : IStream` + dwa dekoratory `CompressedStream : IStream`, `EncryptedStream : IStream` które przyjmują w konstruktorze `IStream` 

## Proxy

{: .def }
Zapewnia obiekt pośredniczący który kontroluje dostęp do innego obiektu.

- implementacja podobna do decoratora, ale różni się **celem** (kontrola dostępu vs nowe funkcjonalności)
- proxy może sam stworzyć obiekt, dekorator dostaje go z zewnątrz
- typy proxy:
  - **remote** - pośredniczy z obiektem _zdalnym_, tj. na innym serwerze, w innej libce
  - **virtual** - lazy loading; kiedy stworzenie obiektu jest kosztowne
  - **protection** - kontrola dostępu do obiektu ("_mogę to wywołać czy nie mam uprawnień?_")
- przykład: logger który nie spamuje
  - pewna klasa (nie mamy do niej dostępu) potrzebuje loggera, ale loguje za dużo powtarzających się wiadomości przez co zapycha nam bazę danych
  - zamiast `Logger`, możemy jej dać `NonSpammingLoggerProxy` który w każdy wywołaniu metody `.Log(message)` będzie sprawdzał czy wiadomość była już zalogowana (cache)
    - jeżeli była - nic nie zrobi
    - jeżeli nie była - przekaże request do faktycznego loggera
  
## Façade

{: .def }
Zapewnia prosty interfejs dla złożonego systemu.

- odseparowuje klienta od złożonego interfejsu
- ogranicza coupling: zamiast pracować z **n** klasami, pracujemy tylko z fasadą
- fasada **posiada** (_has-a_) elementy systemu i to do nich deleguje żądania
- przykład: chat
  - zamiast manualnego tworzenia użytkownika, nawiązania połączenia, uwierzytelniania, enkrypcji, kompresji, wysłania wiadomości i rozłączenia
  - łatwiej owinąć to w fasadę i wystawić tylko interfejs `.Send(from, to, message)`

## Flyweight

{: .def }
Obniża ilość pamięci którą zużywa aplikacja.

- używamy kiedy mamy wiele obiektów i zajmują one dużo pamięci
- pyłkiem (_flyweight_) nazywamy konkretnie obiekt który jest współdzielony (przechowujący stan wewnętrzny_, tj. niezmienne dane)
- jako że pyłek jest współdzielony, najczęściej powinien być tylko read-only
- przykład: renderowanie ikon na mapie
  - budujemy a'la google maps, chcemy móc oznaczyć rzeczy (kawiarnie, restauracje itp.) na mapie
  - typowa implementacja miałaby `class Point` który miałby `float X`, `float Y`, `enum PointType { Cafe, Restaurant }` oraz `byte[] Icon`
    - mając tysiące kawiarni, dla każdej powtarzalibyśmy taki sam byte array ikony, który zajmuje dużo miejsca - out of RAM
  - lepiej zagregować `PointType` i `Icon` w osobnej klasie, np. `PointIcon`
    - wystawiamy fabrykę tworzącą `PointIcon`, która ma jakiś wewnętrzny cache - jak została już stworzona taka ikonka to ją zwróci, jak nie to stworzy, zapisze i zwróci
    - `PointIcon` jest naszym pyłkiem