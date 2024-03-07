---
layout: default
title: Frameworki
parent: .NET/C#
nav_order: 3
---

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Testowe

| NUnit                           | MSTest                      | xUnit                                | Komentarze                                 |
|---------------------------------|-----------------------------|--------------------------------------|--------------------------------------------|
| `[Test]`                        | `[TestMethod]`              | `[Fact]`                             | oznaczanie metod testowych                 |
| `[SetUp]`                       | `[TestInitialize]`          | Constructor                          | setup co pojedynczą metodę testową         |
| `[TearDown]`                    | `[TestCleanup]`             | `IDisposable.Dispose`                | tear down co pojedynczą metodę testową     |
| `[OneTimeSetUp/TearDown]`       | `[ClassInitialize/CleanUp]` | `IClassFixture<T>`                   | setup/teardown co pojedynczą klasę testową |
| `[Test]`<br />`[TestCase(1, 2]` | `[DataSource]`              | `[Theory]`<br />`[InLineData(1, 2)]` | Theory (data-driven test)                  |

- aby odpalić testy: 
  - wszystkie: `dotnet test`
  - wybrane: `dotnet test --filter FullyQualifiedName=~Tests.Integration`
- co sprawdzamy w przypadku API:
  - status code: `response.StatusCode().Should().Be(HttpStatusCode.NotFound)`
  - headery: `response.Headers.Location.Should().Be("users/id")` albo `response.Headers.GetValues("Location").Should().Contain("users/id")`
  - body: `var body = response.Content.ReadFromJsonAsync<T>()`
- w jakiej kolejności lecą testy (xUnit):
  - wewnątrz jednej klasy testy lecą po kolei
  - osobne klasy odpalają się jednocześnie