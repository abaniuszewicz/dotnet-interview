# .NET interview topics

## Akronimy
#### SOLID
##### Single Responsibility Principle
##### Open-Closed Principle
##### Liskov Substitution Principle
##### Interface Segregation Principle
##### Dependency Inversion Principle
#### DRY - Don't Repeat Yourself
#### KISS - Keep It Simple Stupid
#### YAGNI - You Aren't Gonna Need It
### 

## Design Patterns
Typowe rozwiązania problemów często napotykanych podczas projektowania oprogramowania. Stanowią plan, który po dostosowaniu pomaga poradzić sobie z konkretnym problemem.

**Nie mylić** z algorytmem. Algorytm opisuje konkretne kroki, które po zastosowaniu zawsze prowadzą do znanego rezultatu; wzorce projektowe stanowią wysokopoziomowy opis rozwiązania.

**Zalety**
- nie trzeba ich _znać_, ale prędzej czy później _wynajdzie_ się je samemu
- opisują wypróbowane rozwiązania
- opisują jak nie dopuścić do częstego problemu
- stanowią _wspólny język_ z innymi programistami ("_użyj singletona_")

**Wady**
- nadużywanie: _jeżeli masz do dyspozycji tylko młotek, to wszystko wygląda jak gwóźdź_
- bezmyślna copy-paste: wzorce **trzeba** dostosować do danego rozwiązania
- zbyt abstrakcyjny kod w przypadku prostych projektów

### Creational
Opisują jak efektywnie tworzyć obiekty.

#### Factory method
#### Abstract factory
#### Builder
Oddziela konstrukcje złożonego obiektu od jego reprezentacji, dzięki czemu ten sam proces konstrukcji może prowadzić do różnych reprezentacji.
- hermetyzuje operacje niezbędne do stworzenia obiektu
- tworzenie w procedurze wielokrokowej (różnica względem factory)
- klient korzysta z interfejsu budowniczego, jego implementacja może być zmieniona bez jego wiedzy
#### Prototype
#### Singleton
#### Simple factory (not cannonical)
Hermetyzuje tworzenie obiektu, zwłaszcza jeżeli proces tworzenia jest bardzo złożony.
- **nie** mylić z factory method/abstract factory
- ukrywa proces tworzenia (często skomplikowany)
- jeżeli obiekty były tworzone w różnych miejscach w kodzie, to mamy centralizacje i zmiany musimy wprowadzać tylko w jednym miejscu

### Structural
Opisują jak efektywnie składać obiekty w większe struktury.

#### Adapter
Przekształca interfejs danej klasy do postaci innego interfejsu.
- pozwala na współpracę klas które ze względu na niekompatybilne interfejsy nie mogły ze sobą współpracować
- adapter **implementuje** (_is-a_) interfejs docelowy oraz **posiada** (_has-a_) intancje obiektu adaptowanego, do którego deleguje żądania 
#### Bridge
#### Composite
#### Decorator
Pozwala na dynamiczne przydzielanie obiektowi nowych zachowań. Zapewnia alternatywny sposób rozszerzenia funkcjonalości.
- wykorzystuje kompozycje (_has-a_), a nie dziedziczenie (_is-a_)
- klasy dekorujące są tego samego typu co dekorowane (interfejs)
- można je _chainować_, konsumer nie wie czy pracuje z dekoratorem czy z "_prawdziwym_" obiektem (i nie musi wiedzieć)
#### Proxy
Zapewnia obiekt pośredniczący który kontroluje dostęp do innego obiektu.
- implementacja podobna do decoratora, ale różni się **celem** (kontrola dostępu vs nowe funkcjonalności)
- proxy może sam stworzyć obiekt, dekorator dostaje go z zewnątrz
- typy proxy:
  - **remote** - pośredniczy z obiektem _zdalnym_, tj. na innym serwerze, w innej libce
  - **virtual** - lazy loading; kiedy stworzenie obiektu jest kosztowne
  - **protection** - kontrola dostępu do obiektu ("_mogę to wywołać czy nie mam uprawnień?_") 
#### Facade
Zapewnia jeden interfejs dla wielu interfejsów podsystemu. Tworzy interfejs wysokiego poziomu, który upraszcza korzystanie z systemu.
- odseparowuje klienta od złożonego interfejsu
- fasada **posiada** (_has-a_) elementy podsystemu i to do nich deleguje żądania
#### Flyweight

### Behavioral
Opisują jak efektywnie kommunikowac oraz dzielić się obowiązkami między obiektami.

#### Chain of responsibility
#### Command
#### Interpreter
#### Iterator
#### Mediator
#### Memento
#### Observer
Definiuje pomiędzy obiektami relację jeden-do-wielu w taki sposób, że kiedy wybrany obiekt zmienia swój stan, to wszystkie jego obiekty zależne zostają o tym powiadomione i automatycznie zaktualizowane.
#### State
#### Strategy
Definiuje rodzinę algorytmów i wkłada je w osobne klasy które można wymieniać. Sprawia że algorytm klienta staje się niezależny od konkretnej implementacji.
- izolacja algorytmu od szczegółów implementacji kroków
- umożliwia wybór algorytmu w trakcie runtime (wstrzykiwanie, nawet ify)
- można zapewnić nowe działanie bez modyfikacji klienta
#### Template method
Definiuje szkielet algorytmu, przekazując realizację niektórych kroków klasom dziedziczącym. Pozwala im na realizację niektórych kroków, ale **nie pozwala** na zmianę struktury algorytmu.
- podobne do strategii (_has-a_ vs _is-a_)
- podobne do factory method, ale różni się **celem** (_void_ tj. zrób coś, zamiast _<T>_ tj. stwórz coś)
- klasy nie moga zmieniać algorytmu głównego: _sealed_ jest defaultową opcją w C#, w Javie trzeba użyć _final_
#### Visitor
#### Null object (not cannonical)
Usuwa konieczność obsługi _NULL_ przez klienta.
- upraszcza kod - brak `if (obj != null)`
- powoduje problem w sytuacjach w których trzeba coś zwrócić z metody (bo skoro nie `throw null`, to co zwrócić?)

## Architecture

## Web
### HTTP
### REST

## OS

