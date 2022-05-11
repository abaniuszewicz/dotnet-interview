# Structural
Opisują jak efektywnie składać obiekty w większe struktury.

## Adapter
Przekształca interfejs danej klasy do postaci innego interfejsu.
- pozwala na współpracę klas które ze względu na niekompatybilne interfejsy nie mogły ze sobą współpracować
- adapter **implementuje** (_is-a_) interfejs docelowy oraz **posiada** (_has-a_) instancję obiektu adaptowanego, do którego deleguje żądania 
  
## Bridge
## Composite
## Decorator
Pozwala na dynamiczne przydzielanie obiektowi nowych zachowań. Zapewnia alternatywny sposób rozszerzenia funkcjonalności.
- wykorzystuje kompozycje (_has-a_), a nie dziedziczenie (_is-a_)
- klasy dekorujące są tego samego typu co dekorowane (interfejs)
- można je _chainować_, konsumer nie wie czy pracuje z dekoratorem czy z "_prawdziwym_" obiektem (i nie musi tego wiedzieć)

## Proxy
Zapewnia obiekt pośredniczący który kontroluje dostęp do innego obiektu.
- implementacja podobna do decoratora, ale różni się **celem** (kontrola dostępu vs nowe funkcjonalności)
- proxy może sam stworzyć obiekt, dekorator dostaje go z zewnątrz
- typy proxy:
  - **remote** - pośredniczy z obiektem _zdalnym_, tj. na innym serwerze, w innej libce
  - **virtual** - lazy loading; kiedy stworzenie obiektu jest kosztowne
  - **protection** - kontrola dostępu do obiektu ("_mogę to wywołać czy nie mam uprawnień?_") 
  
## Façade
Zapewnia jeden interfejs dla wielu interfejsów podsystemu. Tworzy interfejs wysokiego poziomu, który upraszcza korzystanie z systemu.
- odseparowuje klienta od złożonego interfejsu
- fasada **posiada** (_has-a_) elementy podsystemu i to do nich deleguje żądania

## Flyweight