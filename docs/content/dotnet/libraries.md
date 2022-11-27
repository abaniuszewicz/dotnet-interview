---
layout: default
title: Biblioteki
parent: Pojęcia
nav_order: 4
---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## `FluentAssertions`
- służy do pisania asercji
- oferuje łatwiejszy, fluent syntax: `Assert.AreEqual("result", value)` vs `value.Should().Be("result")`
- porównanie obiektów:
  - `result.Should().Be(other)` - użyje implementacji `.Equals()`
  - `result.Should().BeSameAs(other)` - sprawdzi czy to jest ta sama referencja
  - `result.Should().BeEquivalentTo(other)` - sprawdzi czy oba obiekty (mogą być różnych typów, np. `Order` i `OrderDto`) mają takie same wartości w publicznych membersach. Porównywane są wartości w propertiesach o takich samych nazwach. Jeżeli nie będzie jakiegoś membersa w drugim obiekcie to poleci błąd.
- porównanie kolekcji:
  - `.NotBeEmpty()`, `.HaveCount(c => c > 3)`, `.HaveCountGreaterThan(3)`
  - `col.ShouldBeEquivalentTo(otherCol)`, `col.ShouldBeEquivalentTo(otherCol, opts => opts.WithStrictOrdering()`), 
- alternatywa: asercje dostępne we frameworku (statyczne `Assert.Cośtam()`)

## `Moq`
- służy do mockowania zależności
- operujemy na typie `Mock<T>`
- typowe operacje:
  - `mock.Setup(circle => circle.Radius).Returns(3)`
  - `mock.Verify(circle => circle.SetRadius(It.IsAny<int>(), Times.Once()))`
- alternatywa: `NSubstitute`

## `Bogus`
- służy do generowania fake'owych, realistycznych danych
- operujemy na typie `Faker<T>`
- typowe operacje:
  - `var personGenerator = new Faker<Person>().RuleFor(p => p.Name, f => f.Person.Name).RuleFor(p => p.Age, f => f.Person.Age)`
  - `var person = personGenerator.Generate()`

## `Testcontainers`
- służy do stawiania kontenerów docker
- każda metoda/klasa/zestaw klas może korzystać z niezależnego środowiska (np. bazy danych)
- typowe operacje:
  - `var dbContainer = new TestContainersBuilder<PostgresSqlTestcontainer>().WithDatabase(new PostgresSqlTestcontainerConfiguration()).Build())`
  - trzeba podmienić connection stringi używane przez apkę na nasze _testowe_: wywalić z `IServiceCollection` `IDbConnectionFactory` i zarejestrować nasze własne, używające `dbContainer.ConnectionString`

## `Wiremock`
- służy do stawiania fake api aby sprawdzić zachowania w przypadkach których nie możemy przetestować (np. kiedy padnie 3rd party API GitHuba)
- przechwytuje żądania HTTP i zwraca to co chcemy żeby zwróciło
- typowe operacje:
  - `_wireMockServer.Given(Request.Create().WithPath("/users").UsingGet()).RespondWith(Response.Create().WithBody(user))`