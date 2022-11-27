---
layout: default
title: Integration tests
parent: Tests
nav_order: 3
---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Charakterystyka
- Testują współdziałanie klas: IO, sieć, file system
- Testują np. pojedynczy endpoint, pojedynczy element UI oraz ich bezpośrednie zależności
- Potrzebują działającej infrastruktury żeby testować
- Również używają mocków
  - przykład: kiedy nasze API integruje się z innymi API, np. GithubAPI
  - i tak nie mamy nad tym kontroli, więc "mockujemy integracyjnie" - to nie jest taki mock jak w przypadku unit testów, ale działające fake api które zachwuje się tak samo
  - możemy przetestować różne scenariusze, np. co się stanie kiedy GithubAPI padnie? normalnie nie moglibyśmy przecież go ubić sami na potrzeby testów
- Niektórzy olewają inne rodzaje testów, bo te wprowadzają największą wartość do projektu

## Rodzaje
- top>down
- down>top
- sandwitch
- big bang
- risky/hottest

## Kroki
- Setup: postawić dockera, postawić bazę danych, zaseedować bazę danych itp.
- Mockowanie zależności: mockowanie zewnętrznych API (**nie** w sensie unit testowym, zamiast tego stawiamy fake api)
- Act + Assert
- Cleanup: wyczyszczenie dodanych rzeczy itp.