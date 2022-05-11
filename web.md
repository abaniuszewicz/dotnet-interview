# Web
## HTTP
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
  
## REST (REpresentational State Transfer)
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
  
## SOAP (Simple Object Access Protocol)
  
## Identity

### OAuth
### JWT