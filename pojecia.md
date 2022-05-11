# Pojęcia
## Założenia programowania obiektowego
### Hermetyzacja
### Dziedziczenie
### Polimorfizm
### Abstrakcja
## SOLID
### Single Responsibility Principle
> Klasa powinna mieć tylko jedną odpowiedzialność (nigdy nie powinien istnieć więcej niż jeden powód do modyfikacji klasy).
- 
### Open-Closed Principle
> Klasy (encje) powinny być otwarte na rozszerzenia i zamknięte na modyfikacje.
### Liskov Substitution Principle
> Funkcje które używają wskaźników lub referencji do klas bazowych, muszą być w stanie używać również obiektów klas dziedziczących po klasach bazowych, bez dokładnej znajomości tych obiektów.
### Interface Segregation Principle
> Wiele dedykowanych interfejsów jest lepsze niż jeden ogólny.
### Dependency Inversion Principle
> Wysokopoziomowe moduły nie powinny zależeć od modułów niskopoziomowych - zależności między nimi powinny wynikać z abstrakcji.
## DRY - Don't Repeat Yourself
## KISS - Keep It Simple Stupid
## YAGNI - You Aren't Gonna Need It
## LOD - Law of Demeter
## SLAP - Single Level of Abstraction Principle
## API
- zbiór definicji i protokołów służących budowaniu oraz integrowaniu aplikacji
- kontrakt pomiędzy dostawcą _informacji_, a end-userem
- _mediator_ pomiędzy userami, a resource'ami których potrzebują