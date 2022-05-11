# Behavioural
Opisują jak efektywnie komunikować oraz dzielić się obowiązkami między obiektami.

## Chain of responsibility

## Command

## Interpreter

## Iterator

## Mediator

## Memento

## Observer
Definiuje pomiędzy obiektami relację jeden-do-wielu w taki sposób, że kiedy wybrany obiekt zmienia swój stan, to wszystkie jego obiekty zależne zostają o tym powiadomione i automatycznie zaktualizowane.

## State

## Strategy
Definiuje rodzinę algorytmów i wkłada je w osobne klasy które można wymieniać. Sprawia że algorytm klienta staje się niezależny od konkretnej implementacji.
- izolacja algorytmu od szczegółów implementacji kroków
- umożliwia wybór algorytmu w trakcie runtime (wstrzykiwanie, nawet ify)
- można zapewnić nowe działanie bez modyfikacji klienta

## Template method
Definiuje szkielet algorytmu, przekazując realizację niektórych kroków klasom dziedziczącym. Pozwala im na realizację niektórych kroków, ale **nie pozwala** na zmianę struktury algorytmu.
- podobne do strategii (_has-a_ vs _is-a_)
- podobne do factory method, ale różni się **celem** (_void_ tj. zrób coś, zamiast _\<T\>_ tj. stwórz coś)
- klasy nie mogą zmieniać algorytmu głównego: _sealed_ jest defaultową opcją w C#, w Javie trzeba użyć _final_

## Visitor

## Null object
Usuwa konieczność obsługi _NULL_ przez klienta.
- upraszcza kod - brak `if (obj != null)`
- powoduje problem w sytuacjach w których trzeba coś zwrócić z metody (bo skoro nie `throw null`, to co zwrócić?)