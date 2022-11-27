---
layout: default
title: Założenia OOP
parent: Pojęcia
nav_order: 1
---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

# Założenia OOP
## Hermetyzacja

{: .def }
Ukrywanie składowych (pól, metod) tak, aby były one dostępne tylko wewnątrz danej klasy

- wyodrębnia interfejs (możemy zmieniać prywatną implementację bez złamania kontraktu)
- uodparnia model na błędy (chronimy wewnętrzny stan sprawdzeniem poprawności argumentów)
- lepiej odzwierciedla rzeczywistość
  - :x: `decimal Account.Balance += -10`
  - :heavy_check_mark: `Account.Deposit(decimal amount)` + `Account.Withdraw(decimal amount)`

## Dziedziczenie

{: .def }
Mechanizm pozwalający na współdzielenie części zachowań poprzez definiowanie zależności derived/base

- redukuje duplikacje w kodzie
- pojedynczy punkt zmian/poprawek
- relacja: **is-a** (nie mylić z **has-a**)

## Polimorfizm

{: .def }
Zdolność (obiektu - dziedziczenie, metody - przeładowania) do przybierania wielu form

- ten sam interfejs prowadzi do różnych zachowań
  - :x: `void DrawCircle(Circle circle)` + `void DrawSquare(Square square)`
  - :heavy_check_mark: `void Draw(Shape shape)` + `Draw(new Circle())`, `Draw(new Square())`

## Abstrakcja

{: .def }
Obiekty mogą być reprezentowane jako modele stanowiące uproszczenie problemu. Wykonują one zadania bez zdradzania jak zostały one zrealizowane.

- wyodrębnienie (generalizacja) funkcjonalności, które mogą być zastosowane w innych kontekstach
  - :heavy_check_mark: `Sms : Message`, `Email : Message`, wspólna funkcjonalność: `Message.Send()`

# Różnice
## hermetyzacja vs abstrakcja
- hermetyzacja: ukrycie informacji, abstrakcja: ukrycie implementacji
- hermetyzacja: gettery/settery, abstrakcja: abstract classes, interfaces