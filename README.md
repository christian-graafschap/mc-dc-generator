# MC/DC — Modified Condition/Decision Coverage

Educatief, single-page web-applicatie voor het uitleggen en genereren van
MC/DC-testcases volgens de **neutrale-waardenmethode** (TMap-stijl). Bedoeld
voor docent en MBO-4 SD-studenten in Nederland.

Alles in het Nederlands, in één `index.html`, geen build step, geen
frameworks, geen externe dependencies.

## Lokaal openen

Dubbelklik op `index.html`, of:

- **Windows:** `start index.html`
- **macOS:** `open index.html`
- **Linux:** `xdg-open index.html`

Werkt vanaf het bestandssysteem (`file://...`) — er is geen webserver nodig.

## Op GitHub Pages zetten

1. Maak een nieuwe repository aan op GitHub (bv. `mcdc-tool`).
2. Push de inhoud van deze map:
   ```
   git init
   git add index.html README.md
   git commit -m "Eerste versie MC/DC-tool"
   git branch -M main
   git remote add origin https://github.com/<jouw-gebruikersnaam>/mcdc-tool.git
   git push -u origin main
   ```
3. Ga in de repository naar **Settings → Pages**.
4. Bij **Build and deployment**: kies **Source: Deploy from a branch**,
   **Branch: `main`**, **Folder: `/ (root)`**, en klik **Save**.
5. Na ~1 minuut staat je app op
   `https://<jouw-gebruikersnaam>.github.io/mcdc-tool/`.

## Gebruiksaanwijzing

1. **Condities en expressie** (sectie 2):
   - Voeg de afzonderlijke voorwaarden toe. Elke conditie krijgt
     automatisch een letter (`A`, `B`, …). Minimaal 2, maximaal 6.
   - Schrijf de Booleaanse expressie met die letters. Onder het
     invoerveld verschijnt de expressie met de operator-neutrale waarden
     (`1` onder EN, `0` onder OF).
   - De opbouw-tabel toont per conditie twee rijen (WAAR-rij en
     NIET-WAAR-rij). Cellen zijn visueel onderscheiden:
     - normaal = de variabele die *aan de beurt* is
     - **vet** = pure neutrale waarde
     - ***vet cursief*** = aanpassing (sub-expressie als geheel non-neutraal)
     - ~~doorgestreept~~ = duplicaat van een eerdere rij-helft
2. **Resultaat** (sectie 3) — een beslistabel met de unieke testcases in
   kolommen en de condities in rijen, plus een Uitkomst-rij. De Uitkomst
   wordt automatisch afgeleid uit de expressie; je hoeft die niet zelf op
   te geven. N+1-controle: bij afwijking verschijnt een waarschuwing.

## Syntax

- Variabelen: losse letters `A`, `B`, `C`, … (hoofdletters niet vereist).
- Operatoren: `EN` / `AND` / `&&`, `OF` / `OR` / `||`, `NIET` / `NOT` / `!`.
- Haakjes: `(` … `)`.

**Verplichte haakjes bij gemengde operatoren.** `A EN B EN C` mag (alleen
EN). `A OF B OF C` mag (alleen OF). Maar `A EN B OF C` is een fout — schrijf
expliciet `(A EN B) OF C` óf `A EN (B OF C)`.

**Variabele maximaal één keer**, ongeacht polariteit. `A EN NIET A` is dus
niet toegestaan — de neutrale-waardenmethode werkt alleen als elke variabele
één keer in de expressie staat.

## Technische details

- **Geen `eval()`.** Handgeschreven recursive-descent parser; expressie wordt
  geïnterpreteerd op een AST.
- **N+1 testcases.** Het algoritme genereert per conditie twee rijen (WAAR
  en NIET-WAAR) volgens de TMap-conventie (rightmost variabele aanpassen,
  leftmost op operator-neutraal). Na deduplicatie blijven ongeveer N+1
  unieke testcases over.
- **Onbereikbare rijen.** Als geen waarde van de geteste variabele het
  rijdoel haalt, wordt `?` in de cel getoond plus een melding.
- **Minimum ondersteunde expressies** (door het algoritme geverifieerd, niet
  zichtbaar op de pagina):
  `A EN B`, `A OF B`, `A EN (B OF C)`, `A OF (B EN C)`,
  `(A EN B) OF C`, `(A OF B) EN C`, `(A EN B) OF C OF D`.
- **Statisch hostbaar** op elke webserver of vanaf bestand. Geen
  localStorage, geen netwerk, geen tracking.

## Out of scope

- MC/DC met short-circuit-evaluatie.
- Expressies met dubbele variabelen.
- Expressies met gemengde operatoren zonder haakjes.
- Andere talen dan Nederlands.

## Aanpassen

Alle code, stijl en uitlegtekst staat in `index.html`. Voorbeelden toevoegen,
kleuren wijzigen of de uitleg herschrijven kan rechtstreeks in dat bestand.
