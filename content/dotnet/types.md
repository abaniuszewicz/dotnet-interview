---
layout: default
title: Typy
parent: .NET/C#
nav_order: 5
---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## `WebApplicationFactory<T>`
- sposób aby odpalić API które testujemy w pamięci
- daje możliwość stworzenia `HttpClient` który może wywoływać to API działające w pamięci
- zazwyczaj umieszczamy w `IClassFixture<WebApplicationFactory<T>>` żeby cała klasa testowa dzieliła to samo API

## `HttpClient`/`WebClient`