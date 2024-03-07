---
layout: default
title: REST
parent: Web
nav_order: 2
---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---


{: .def }
REST - REpresentational State Transfer. Zbiór architektonicznych wymagań. **Nie jest** protokołem ani żadnym standardem; można zbudować RESTful API na wiele sposobów.

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