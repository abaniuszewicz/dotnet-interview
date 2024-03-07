---
layout: default
title: End to end tests
parent: Web
nav_order: 4
---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Charakterystyka
- testuje wszystko w systemie od początku do samego końca
  - przykład: jak 2 API współpracują ze sobą. Jedno wpisuje coś do bazy danych, czekamy 1 minutę i sprawdzamy czy drugie może to odczytać.
- zazwyczaj testowany jest prawdziwy system (ale można też staging environment)
- mogą być też _niebezpieczne_ do odpalenia równolegle, jako że zmieniają stan faktycznego systemu

## Rodzaje
- smoke - testują prawdziwe zdeployowane środowisko
- performance
  - load
  - spike
  - stress