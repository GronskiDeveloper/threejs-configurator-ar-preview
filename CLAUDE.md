# Praca AI-first: notatki dla tego repo

Trzymam ten plik w repozytorium, bo buduję z Claude Code (Anthropic) i chcę, żeby podział „człowiek/AI" był widoczny z drzewa plików, a nie deklarowany w README.

## Podział pracy człowiek vs AI

| Warstwa | Kto zrobił | Dlaczego tak |
|---|---|---|
| Pomysł i teza repo | **Człowiek**. Klucz: produkt z konfiguratora i produkt w AR to ten sam obiekt, nie osobny eksport. | To jest realna wartość AR w sprzedaży: klient stawia u siebie dokładnie to, co złożył, nie generyczny model poglądowy. |
| Pętla WebXR hit-test (reticle, `requestHitTestSource`, tap to place) | **Draft AI, weryfikacja człowieka** | To boilerplate z przykładów Three.js, Claude składa go poprawnie. Moje było dopiąć cykl życia sesji: chowanie podłogi-łapacza cienia i pozycjonowanie produktu dopiero po tapnięciu. |
| Geometria produktu (otomana) | **Człowiek**, celowo prosta | Repo jest o AR, nie o meblu. Trzymałem geometrię minimalną, żeby uwaga szła na plumbing, a nie na model. |
| Obsługa braku wsparcia (desktop, iOS) | **Design człowieka, kod AR** | Zdecydowałem, że ma działać wszędzie: bez WebXR pokazuje podgląd 3D i wyłącza przycisk, nic nie rzuca. Ścieżkę iOS (USDZ + Quick Look) opisałem uczciwie w README jako poza zakresem minimalnego startera. |
| README + linki do grodev.pl | **Człowiek** | |

## Co zweryfikowałem przed wypchnięciem

- Otworzyłem `index.html` na desktopie: podgląd 3D renderuje się, przycisk AR jest poprawnie wyłączony (brak WebXR), zero błędów w konsoli, zero wyjątków z gałęzi AR.
- Puściłem detektor anty-wzorców na `index.html`. Czysto.
- Sprawdziłem, że gałąź AR nie odpala się bez `navigator.xr` i że `sessionend` przywraca scenę do stanu desktopowego.

## Znane pułapki dla następnej iteracji AI

- **WebXR wymaga HTTPS.** `http://localhost` nie uruchomi sesji AR. Lokalnie potrzebny tunel albo https. GitHub Pages daje HTTPS, więc live demo działa na telefonie.
- **iOS nie ma WebXR AR.** Nie udawać, że ARButton zadziała na iPhonie. Prawdziwa ścieżka to USDZ + Quick Look, osobny mechanizm. Jeśli następny przebieg obieca AR na iOS przez WebXR, to nieprawda.
- **Podłoga-łapacz cienia musi znikać w AR** (`floor.visible = false`), inaczej zasłania prawdziwy obraz z kamery.
- Produkt ustawiać z macierzy reticle (`setFromMatrixPosition`), nie zgadywać pozycji z raycastu kamery.

## Kiedy sięgać po Claude, a kiedy pisać samodzielnie

- **Sięgnąć po Claude:** inny produkt do podglądu, obracanie i skalowanie postawionego obiektu gestami, generowanie USDZ pod iOS.
- **Zrobić samodzielnie:** cykl życia sesji AR i to, co widać przy braku wsparcia. Tu łatwo zostawić użytkownika z czarnym ekranem, a to psuje pierwsze wrażenie bardziej niż brak funkcji.
