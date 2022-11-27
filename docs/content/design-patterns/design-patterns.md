---
layout: default
title: Wzorce projektowe
nav_order: 4
has_children: true
permalink: /docs/content/design-patterns
---


{: .def }
Typowe rozwiązania problemów często napotykanych podczas projektowania oprogramowania. Stanowią plan, który po dostosowaniu pomaga poradzić sobie z konkretnym problemem. **Nie mylić** z algorytmem. Algorytm opisuje konkretne kroki, które po zastosowaniu zawsze prowadzą do znanego rezultatu; wzorce projektowe stanowią wysokopoziomowy opis rozwiązania.

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