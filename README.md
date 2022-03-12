# .NET interview topics

## Acronyms
#### SOLID
##### **S**ingle Responsibility Principle
##### **O**pen-Closed Principle
##### **L**iskov Substitution Principle
##### **I**nterface Segregation Principle
##### **D**ependency Inversion Principle
#### DRY - Don't Repeat Yourself
#### KISS - Keep It Simple Stupid
#### YAGNI - You Aren't Gonna Need It
### 

## Design Patterns
Typical, well-tested solution to a commonly occurring problems in software design. 

**Don't** mistake them for algorithm. Algorithm define exact steps that always achieve concrete goal, design pattern is more of high-level description.

Why bother?
- you don't have to _know_ them, but eventually you'll _discover_ them
- known and well tested solution/approach to a problem
- guide how to solve given problem, or how to not let the problem occurr at all
- common language with other devs: "_hey bro, just use singleton_" 

Caveats?
- overuse: _if all you have is a hammer, everything looks like a nail_
- dumb copy-paste: design patterns **has** to be adopted
- too abstract: might lead to hard-to-read code, especially in simple domains

### Creational
Describe how to effectively create objects.

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
Describe how to effectively compose objects into larger structures.

#### Adapter
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
Describe how to effectively communicate and share responsibilities between objects.

#### Chain of responsibility
#### Command
#### Interpreter
#### Iterator
#### Mediator
#### Memento
#### Observer
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

