<details><summary><h1>Pojęcia</h1></summary>

<details><summary><h2>Założenia programowania obiektowego</h2></summary>

### Hermetyzacja
> Ukrywanie składowych (pól, metod) tak, aby były one dostępne tylko wewnątrz danej klasy

- wyodrębnia interfejs (możemy zmieniać prywatną implementację bez złamania kontraktu)
- uodparnia model na błędy (chronimy wewnętrzny stan sprawdzeniem poprawności argumentów)
- lepiej odzwierciedla rzeczywistość
  - :x: `decimal Account.Balance += -10`
  - :heavy_check_mark: `Account.Deposit(decimal amount)` + `Account.Withdraw(decimal amount)`

### Dziedziczenie
> Mechanizm pozwalający na współdzielenie części zachowań poprzez definiowanie zależności derived/base

- redukuje duplikacje w kodzie
- pojedynczy punkt zmian/poprawek
- relacja: **is-a** (nie mylić z **has-a**)

### Polimorfizm
> Zdolność (obiektu - dziedziczenie, metody - przeładowania) do przybierania wielu form

- ten sam interfejs prowadzi do różnych zachowań
  - :x: `void DrawCircle(Circle circle)` + `void DrawSquare(Square square)`
  - :heavy_check_mark: `void Draw(Shape shape)` + `Draw(new Circle())`, `Draw(new Square())`

### Abstrakcja
> Obiekty mogą być reprezentowane jako modele stanowiące uproszczenie problemu. Wykonują one zadania bez zdradzania jak zostały one zrealizowane.

- wyodrębnienie (generalizacja) funkcjonalności, które mogą być zastosowane w innych kontekstach
  - :heavy_check_mark: `Sms : Message`, `Email : Message`, wspólna funkcjonalność: `Message.Send()`

### Różnice: hermetyzacja vs abstrakcja
- hermetyzacja: ukrycie informacji, abstrakcja: ukrycie implementacji
- hermetyzacja: gettery/settery, abstrakcja: abstract classes, interfaces

</details>

<details><summary><h2>SOLID</h2></summary>

### Single Responsibility Principle
> Klasa powinna mieć tylko jedną odpowiedzialność (nigdy nie powinien istnieć więcej niż jeden powód do modyfikacji klasy).

### Open-Closed Principle
> Klasy (encje) powinny być otwarte na rozszerzenia i zamknięte na modyfikacje.

### Liskov Substitution Principle
> Funkcje które używają wskaźników lub referencji do klas bazowych, muszą być w stanie używać również obiektów klas dziedziczących po klasach bazowych, bez dokładnej znajomości tych obiektów.
- przykłady:
  - kwadrat dziedziczący po prostokącie: kwadrat **jest** prostokątem, jednak przy implementacjach z metodami `.SetWidth()` i `.SetHeight()`, które mają sens dla `Rectangle`, klasa dziedzicząca `Square : Rectangle` musiałaby je nadpisać żeby wywołanie `.SetWidth()` wywoływało jednocześnie `.SetHeight()` do tej samej wartości. To z kolei mogłoby popsuć np. unit test, który przyjmuje `Rectangle` (a więc potencjalnie też i `Square`), a następnie liczy jego pole poprzez ustawienie boków odpowiednio do długości 2 i 3. Jeżeli policzylibyśmy `rectangle.GetWidth() * rectangle.GetHeight()`, dla instancji _prawdziwego_ `Rectangle` dostalibyśmy 6, a dla `Square` 9.
  - struś dziedziczący po ptaku: struś **jest** ptakiem, jednak niektóre metody nie mają w jego kontekście sensu, jak np. `.Fly()`. Jeżeli do metody która spodziewa się dostać `Bird` i chce wywołać jego metodę latania, przekażemy instancję `Ostrich` aplikacja może znaleźć się w złym stanie przez to że struś nie potrafi latać.
### Interface Segregation Principle
> Wiele dedykowanych interfejsów jest lepsze niż jeden ogólny.

### Dependency Inversion Principle
> Wysokopoziomowe moduły nie powinny zależeć od modułów niskopoziomowych - zależności między nimi powinny wynikać z abstrakcji.

</details>

<details><summary><h2>Akronimy/buzzwordy</h2></summary>

## DRY - Don't Repeat Yourself
## KISS - Keep It Simple Stupid
## YAGNI - You Aren't Gonna Need It
## LOD - Law of Demeter
## SLAP - Single Level of Abstraction Principle
## CQRS - Command/Query Responsibility Separation
## API
- zbiór definicji i protokołów służących budowaniu oraz integrowaniu aplikacji
- kontrakt pomiędzy dostawcą _informacji_, a end-userem
- _mediator_ pomiędzy userami, a resource'ami których potrzebują

## Coupling/Cohesion

## Ko-/Kontra-/In-wariancja


</details>

</details>

<details><summary><h1>Wzorce projektowe</h1></summary>

Typowe rozwiązania problemów często napotykanych podczas projektowania oprogramowania. Stanowią plan, który po dostosowaniu pomaga poradzić sobie z konkretnym problemem.

**Nie mylić** z algorytmem. Algorytm opisuje konkretne kroki, które po zastosowaniu zawsze prowadzą do znanego rezultatu; wzorce projektowe stanowią wysokopoziomowy opis rozwiązania.

**Zalety**
- nie trzeba ich _znać_, ale prędzej czy później _wynajdzie_ się je samemu
- opisują wypróbowane rozwiązania
- opisują jak nie dopuścić do częstego problemu
- stanowią _wspólny język_ z innymi programistami ("_użyj singletona_")
- stanowią _core_ wielu frameworków, znając je musimy poznać tylko syntax bo zasady pozostają te same

**Wady**
- nadużywanie: _jeżeli masz do dyspozycji tylko młotek, to wszystko wygląda jak gwóźdź_
- bezmyślna copy-paste: wzorce **trzeba** dostosować do danego rozwiązania
- zbyt abstrakcyjny kod w przypadku prostych projektów

<details><summary><h2>Creational</h2></summary>

Opisują jak efektywnie tworzyć obiekty.

### Factory method
> Przekazuje odpowiedzialność za tworzenie obiektów do klas podrzędnych.

- metoda wytwórcza powinna zwracać _abstrakcję_, poszczególne podklasy mogą zwracać jej różne implementacje
- podobne do template method, ale różni się **celem** (_return \<T\>_ tj. stwórz coś, zamiast _void_ tj. zrób coś)

### Abstract factory
> Dostarcza interfejs do tworzenia całych rodzin spokrewnionych lub zależnych od siebie obiektów bez konieczności określania ich klas rzeczywistych.

- abstrakcyjne fabryki wytwarzają całe **spójne** rodziny produktów - np. `IUiElementFactory` pozwalające stworzyć abstrakcyjne `Button` oraz `Dialog`, który jest implementowany przez `HtmlElementFactory` (`HtmlButton`, `HtmlDialog`) oraz `WindowsElementFactory` (`WinButton`, `WinDialog`)
- często korzysta z factory method (to jedyne publiczne metody jakie znajdują się w abstrakcyjnej fabryce)
- factory method - dziedziczenie (**my** tworzymy _coś_), abstract factory - kompozycja (**mamy** fabrykę, która tworzy _coś_)

### Builder
> Oddziela konstrukcje złożonego obiektu od jego reprezentacji, dzięki czemu ten sam proces konstrukcji może prowadzić do różnych reprezentacji.

- hermetyzuje operacje niezbędne do stworzenia obiektu
- tworzenie w procedurze wielokrokowej (różnica względem factory)
- klient korzysta z interfejsu budowniczego, jego implementacja może być zmieniona bez jego wiedzy

### Prototype

### Singleton

### Simple factory
> Hermetyzuje tworzenie obiektu, zwłaszcza jeżeli proces tworzenia jest bardzo złożony.

- **nie** mylić z factory method/abstract factory
- ukrywa proces tworzenia (często skomplikowany)
- jeżeli obiekty były tworzone w różnych miejscach w kodzie, to mamy centralizacje i zmiany musimy wprowadzać tylko w jednym miejscu

</details>

<details><summary><h2>Structural</h2></summary>

Opisują jak efektywnie składać obiekty w większe struktury.

### Adapter
> Przekształca interfejs danej klasy do postaci innego interfejsu.

- pozwala na współpracę klas które ze względu na niekompatybilne interfejsy nie mogły ze sobą współpracować
- adapter **implementuje** (_is-a_) interfejs docelowy oraz **posiada** (_has-a_) instancję obiektu adaptowanego, do którego deleguje żądania 
  
### Bridge
### Composite
### Decorator
> Pozwala na dynamiczne przydzielanie obiektowi nowych zachowań. Zapewnia alternatywny sposób rozszerzenia funkcjonalności.

- wykorzystuje kompozycje (_has-a_), a nie dziedziczenie (_is-a_)
- klasy dekorujące są tego samego typu co dekorowane (interfejs)
- można je _chainować_, konsumer nie wie czy pracuje z dekoratorem czy z "_prawdziwym_" obiektem (i nie musi tego wiedzieć)

### Proxy
> Zapewnia obiekt pośredniczący który kontroluje dostęp do innego obiektu.

- implementacja podobna do decoratora, ale różni się **celem** (kontrola dostępu vs nowe funkcjonalności)
- proxy może sam stworzyć obiekt, dekorator dostaje go z zewnątrz
- typy proxy:
  - **remote** - pośredniczy z obiektem _zdalnym_, tj. na innym serwerze, w innej libce
  - **virtual** - lazy loading; kiedy stworzenie obiektu jest kosztowne
  - **protection** - kontrola dostępu do obiektu ("_mogę to wywołać czy nie mam uprawnień?_") 
  
### Façade
> Zapewnia jeden interfejs dla wielu interfejsów podsystemu. Tworzy interfejs wysokiego poziomu, który upraszcza korzystanie z systemu.

- odseparowuje klienta od złożonego interfejsu
- fasada **posiada** (_has-a_) elementy podsystemu i to do nich deleguje żądania

### Flyweight

</details>

<details><summary><h2>Behavioural</h2></summary>

Opisują jak efektywnie komunikować oraz dzielić się obowiązkami między obiektami.

### Chain of responsibility
> Pozwala przekazywać żadania wzdłuż łańcucha handlerów. Handlery mogą albo obsłużyć żądanie, albo przekazać je dalej.

- można ustalać kolejność obsługi żądania
- linked list: każdy node ma referencję do następnego elementu
  - SRP: tworzymy proste handlery które wiedzą jak obsłużyć żądanie
  - O/CP: można łatwo rozłączyć łańcuch i dodać/usunąć jakiś element bez naruszania innych
- dekorator: zawsze przekaże żądanie dalej, chain of responsibility: może zatrzymać procesowanie
- przykład: mouse click na UI
  - zdarzenie dostanie najpierw _najbardziej wewnętrzny_ element, jak nie obsłuży to bombelkujemy wyżej
  - klikamy np. na disabled button, jako że jest wyłączony - nie będzie obsługiwał zdarzenia
  - button znajduje się w jakimś gridzie, ale go też nie obchodzi akurat to kliknięcie (może w złym miejscu?)
  - grid znajduje się w oknie, a ono może np. wyświetlić tooltipa z wiadomością helpa

### Command
> Hermetyzuje żądania w postaci obiektów, co pozwala na parametryzowanie obiektów różnymi żądaniami oraz obsługiwanie operacji, które można wycofać.

- pozwala na składowanie żądań w formie obiektu, dzięki czemu można je przekazywać, kolejkować, zapisywać, undo-ować
- separuje klasę wysyłającą żądanie od konkretnej implementacji
- `interface ICommand` z jedną metodą `.Execute()`, ewentualnie `IUndoableCommand : ICommand` z metodą `.Undo()`
  - w C\#: `ICommand` posiada `.Execute(object param)`, `.CanExecute(object param)` oraz `event CanExecuteChanged`
- memento: składuje _stan_ obiektu (potencjalnie dużo pamięci), command w/ undo: składuje _wiedzę_ jak cofnąć komendę
- przykład: przycisk w edytorze tekstu
  - zamiast wielu podklas `OkButton`, `CancelButton`, `SaveButton` które zawierają swoją logikę, tworzymy jedną klasę `Button` która przyjmuje `ICommand` (`OkCommand`, `CancelCommand`, `SaveCommand`)
  - poszczególne komendy przyjmują w swoim konstruktorze serwisy do których delegują swoje operacje: `SaveCommand.Execute()` → `FileManager.SaveFile(file)`
  - dzięki temu ten sam kod (`SaveCommand`) może być użyty też w innych sytuacjach (np. użycie skrótu <kbd>CTRL</kbd> + <kbd>S</kbd>)

### Interpreter

### Iterator
> Zapewnia sekwencyjny dostęp do jakiegoś zbioru bez ujawniania jego wewnętrznej implementacji.

- Hermetyzacja zbioru: klient nie musi wiedzieć jaka jest kolekcja pod spodem, wystarczy mu możliwość iteracji
- SRP: zbiór nie jest odpowiedzialny za iterowanie się po sobie, zajmuje się tym jego iterator
- O/CP: można dodawać nowe iteratory (np. różne możliwości przejścia po drzewie binarnym) do tego samego zbioru
- można iterować tą samą kolekcję równolegle różnymi iteratorami (każdy przechowuje swój stan) 
- implementacja:
  - _ręcznie_: `bool MoveNext()` + `T GetCurrent()`
  - C#: `IEnumerator`/`IEnumerator<T>`
- przykład: książka kucharska
  - `Recipe` + iterator na `Ingredient`

### Mediator
> Ogranicza bezpośrednią komunikację między obiektami i zmusza je do współpracy wyłącznie za pośrednictwem obiektu mediatora.

- obiekty nie wiedzą o sobie, wiedzą tylko o mediatorze
- upraszcza system, bo cała _logika sterująca_ znajduje się w jednym miejscu
- zmniejsza liczbę komunikatów przesyłanych w systemie (zamiast n:n, n:1:n)
- mediatory mają tendencje do rozrastania się (_god object_)
- przykład: dialog box
  - dialog box zawiera button <kbd>Save</kbd> (enabled/disabled) oraz listview
  - chcemy żeby przycisk był enabled kiedy _coś_ jest wybrane w liście
  - listview nie informuje przycisku bezpośrednio, zamiast tego informuje mediator, a on przekazuje do innych zainteresowanych
  - dzięki temu możemy podpiąć dodatkowe kontrolki, np. label który będzie wyświetlał co zostało zaznaczone
- implementacja:
  - klasycznie: mediator tworzy komponenty i przekazuje do nich siebie. Komponenty po aktualizacji wywołują metodę mediatora `mediator.Updated(this)`. Mediator sprawdza co zostało zaktualizowane, a następnie wykonuje odpowiednie akcje (`if (component == listview) button.Enabled = listview.HasSelection`).
  - observer: mediator subskrybuje się do komponentów (`component.Subscribe(this)`), a następnie po jego powiadomieniu przez komponent `subscribers.ForEach(s => s.Notify(this))`, wykonuje odpowiednie akcje (`button.Enabled = listview.HasSelection`).

### Memento
> Pozwala zapisywać i przywracać wcześniejszy stan obiektu bez ujawniania szczegółów jego implementacji.

- zapewnia funkcję przywracania (`undo`)
- SRP/hermetyzacja głównego obiektu
  - poprzednie stany trzymamy w _memento_
  - zarządzaniem stanu zajmuje się _caretaker_  
- zapisywanie/przywracanie może być czasochłonne
- trzeba uważać na typy referencyjne: shallow/deep copy
- przykład: edytor tekstu
  - `Editor` - przechowuje zawartość, tytuł pliku, położenie kursora. Dostarcza metody `.GetState()`, `.RestoreState()`.
  - `EditorState` - przechowuje część/wszystkie parametry (np. samą zawartość)
  - `EditorHistory` - przechowuje listę (`Stack`) poprzednich stanów. Dostarcza metody `.Push(state)`, `.Pop()`.

### Observer
> Definiuje pomiędzy obiektami relację jeden-do-wielu w taki sposób, że kiedy wybrany obiekt zmienia swój stan, to wszystkie jego obiekty zależne zostają o tym powiadomione i automatycznie zaktualizowane.

- używany kiedy inne obiekty muszą zostać powiadomione o zmianie stanu jakiegoś obiektu
- publisher posiada 3 metody: `.Subscribe(subscriber)`, `.Unsubscribe(subscriber)` oraz `.Notify()`, często są zamykane w klasie bazowej lub osobnym obiekcie
- push vs pull:
  - push: publisher wysyła dane których potrzebują subskrybenci (`subscriber.Notify(int)`)
  - pull: subskrybenci pobierają dane których potrzebują (`subscriber.Notify(this)`)
- podobne wzorce:
  - **chain of command**: przekazuje żądania sekwencyjnie do momentu aż ktoś obsłuży, **observer**: _losowo_ (w środku jest _jakaś_ struktura, ale nie wiemy jaka i co/kiedy się podpięło) do wszystkich
  - **command**: enkapsuluje akcje, **observer**: enkapsuluje mechanizm powiadamiania
  - **mediator**: komponenty rozmawiają z punktem centralnym, **observer**: kompenenty mogą być jednocześnie publisherami i subskrybentami i być powiązane ze sobą
    - można też stworzyć mediatora z użyciem obserwatora, gdzie mediator będzie subskrybentem i będzie obserwował komponenty

### State
> Umożliwia obiektowi zmianę zachowania wraz ze zmianą jego wewnętrznego stanu. Po takiej zmianie funkcjonuje on jak inna klasa.

- :x: zazwyczaj implementowane za pomocą instrukcji warunkowych if/switch
  - nie skaluje (nowy stan = nowe zmiany)
  - dobre dla prostych i **skończonych** sytuacji
- :heavy_check_mark: możemy wykorzystać polimorfizm
  - poszczególne stany implementują ten sam interfejs
  - klasa główna deleguje wywołania do poszczególnych stanów
  - stan robi co musi, a następnie przełącza klasę główną w inny stan (referencję do klasy głównej dostaje przez konstruktor)
- podobne do strategii
  - strategia jest nieświadoma innych implementacji, state jest i może przestawiać aktualny tryb
  - obiekt może zawierać wiele strategii, state jest tylko jeden
- przykład: nawigacja
  - inaczej wyznaczamy drogę oraz ETA dla samochodu, roweru czy pójścia pieszo
  - możemy to rozwiązać ifami/switchem (`if (mode == TravelMode.Car)`) ale nie będzie to skalowało (nowy mode = nowy warunek)
  - lepiej wyodrębnić interfejs `ITravelMode` z metodami `.CalculateEta()` oraz `.GetDirection()`

### Strategy
> Definiuje rodzinę algorytmów i wkłada je w osobne klasy które można wymieniać. Sprawia że algorytm klienta staje się niezależny od konkretnej implementacji.

- izolacja algorytmu od szczegółów implementacji kroków
- umożliwia wybór algorytmu w trakcie runtime (wstrzykiwanie, nawet ify)
- można zapewnić nowe działanie bez modyfikacji klienta
- przykład: `ImageProcessor`
  - `ICompressor`: `JpegCompressor : ICompressor`, `PngCompressor : ICompressor`
  - `IFilter`: `BlackAndWhiteFilter : IFilter`, `HighContrastFilter : IFilter`

### Template method
> Definiuje szkielet algorytmu, przekazując realizację niektórych kroków klasom dziedziczącym. Pozwala im na realizację niektórych kroków, ale **nie pozwala** na zmianę struktury algorytmu.

- podobne do strategii (_has-a_ vs _is-a_)
- podobne do factory method, ale różni się **celem** (_void_ tj. zrób coś, zamiast _\<T\>_ tj. stwórz coś)
- klasy nie mogą zmieniać algorytmu głównego: _sealed_ jest defaultową opcją w C#, w Javie trzeba użyć _final_
- podkroki mogą być wymagane (`abstract`) albo opcjonalne (`virtual`: jakaś domyślna implementacja albo pusty _hook_)
- przykład: czytanie pliku
  - `Parser` z definicją `.Parse()` które otwiera plik, deleguje `.ProcessStream()` do klas dziedziczących oraz zamyka plik
  - klasy dziedziczące `CsvParser : Parser`, `HtmlParser : Parser`, `XmlParser : Parser`, które umieją czytać swoje formaty ze `Stream`

### Visitor
> Pozwala na dodanie nowych zachowań do struktur obiektów bez ich modyfikacji.

- używamy kiedy mamy niezmieniającą się strukturę obiektów i chcemy mieć możliwość dodawania na nich nowych operacji
- kiedy chcemy dodać nową operację, dodajemy nową implementację visitora
- poszczególne klasy przyjmują visitora (`.Accept(visitor)`) i wywołują odpowiednią dla siebie metodę interfejsu visitora (przez polimorfizm `.Visit(anchor)` lub sprecyzowaną nazwę `VisitAnchor(anchor)`)
- wady: złe dla niestabilnych struktur, brak dostępu do prywatnych składowych
- przykład: operacje na nodach html
  - zbiór jest zamknięty i zdefiniowany w standardzie, oprócz anchor, heading itd. nie będzie nowych node'ów
  - visitory dla nich będą różne w zależności od use case, np. `Highlight`, `Format`, `ExtractPlainText`
  - mając `visitor = new HighlightVisitor()` i listę node'ów, wywołamy na niej `.Accept(visitor)`. Każdy visitor z kolei wywoła odpowiednią dla siebie metodę visitora, przekazując do niej siebie `.Visit(this)`.

### Null object
> Usuwa konieczność obsługi _NULL_ przez klienta.
- upraszcza kod - brak `if (obj != null)`
- powoduje problem w sytuacjach w których trzeba coś zwrócić z metody (bo skoro nie `throw null`, to co zwrócić?)

</details>

</details>

<details><summary><h1>Web</h1></summary>

<details><summary><h2>HTTP</h2></summary>

### Methods
- `GET`
  - pobiera wskazany zasób
- `POST` 
  - tworzy nowy zasób
  - pobiera dane w jeżeli musimy dostarczyć wielu parametrów (lub nie chcemy ich _pokazać_ w url)
  - robi wszystko inne co nie pasuje do innych metod 
- `PUT`
  - zastępuje wskazany zasób tym co przekazujemy
  - _(czasami)_ tworzy nowy zasób, jeżeli możemy mu nadać _id_ z góry
- `PATCH`
  - aktualizuje część wskazanego zasobu
- `DELETE`
  - kasuje wskazany zasób
- `HEAD`
  - zwraca to co `GET`, ale bez body
- `CONNECT`
  - używane do _https_; `GET` = _http_, `CONNECT` = _https_
  - służy do nawiązania komunikacji z serwerem (tunnel)
- `OPTIONS`
  - sprawdzenie możliwości serwera żeby dowiedzieć się _jak z nim gadać_
  - zwraca informacje o np. wersji HTTP,  
- `TRACE`
  - diagnostyka; jeżeli włączona serwer będzie zwracał dokładnie to co mu przesłaliśmy (_echo_)
  - kiedyś wykorzystywane do hackowania, bo niekiedy serwery zwracały również swoje wewnętrzne authentication headery
  
### Responses
- 1xx - Informational responses
  - 100 - continue
- 2xx - Sucessfull responses
  - 200 - ok
  - 201 - created (nagłówek `Location` powinien zawierać łącze do nowego zasobu)
  - 204 - no content (np. przy `DETELE` i `PUT`)
- 3xx - Redirection messages
  - 301 - moved permamenty
  - 304 - not modified (caching; zasób się nie zmienił więc możemy go nadal używać)
- 4xx - Client error responses
  - 400 - bad request (brakujące lub niepoprawne dane przesłane do servera)
  - 401 - unauthorized (użytkownik nie uwierzytelniony)
  - 403 - forbidden (użytkownik nie ma uprawnień)
  - 404 - not found
  - 409 - conflict (próba wykonania błędnej operacji, np. stworzenie czegoś o zduplikowanym _id_)
- 5xx - Server error responses
  - 500 - internal server error
  - 502 - bad gateway
  - 503 - service unavailable

</details>

<details><summary><h2>REST (REpresentational State Transfer)</h2></summary>

Zbiór architektonicznych wymagań. **Nie** jest protokołem ani żadnym standardem, można zbudować RESTful API na wiele sposobów.

- client-server
  - separation of concerns; nie musimy się _martwić_ o UI
- statelessness
  - klient musi przekazać wszystko czego potrzebuje serwer aby poprawnie zinterpretować żądanie
  - server nie utrzymuje sesji
  - dobre do high-performance, bo serwer nie musi trzymać śmieci
- cacheability
  - aby lepiej skalowało i było szybsze
  - używamy headerów aby określić czy coś może być scacheowane lub też nie
- layered system
  - ani klient ani serwer nie powinny zależeć od informacji czy komunikują się z aplikacją końcową czy _gadają_ przez proxy/inną warstwę
- uniform interface
  - zasoby są identyfikowane w requeście (np. w URI)
  - zasób powinien zawierać wszystko czego potrzebuje klient żeby go zmodyfikować/skasować
  - response'y zawierają opis jak pracować z wiadomością
  - HATEOAS (Hypermedia As The Engine Of Aplication State): mając główny link do API, klient powinien być w stanie _odkryć_ wszystkie resource'y których potrzebuje 
- code on demand (optional)
  - server może zwracać _kod wykonywalny_, np. skompilowane komponenty (Java applets)

</details>

<details><summary><h2>SOAP (Simple Object Access Protocol)</h2></summary>

</details>

<details><summary><h2>Identity</h2></summary>

### OAuth
### JWT

</details>