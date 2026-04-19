# Projekt-setup — dansk prompt

Du skal sætte en standardiseret plan- og dokumentationsstruktur op i dette repo, samt tre custom slash commands. Alt indhold skal være på dansk.

## Overordnede regler

1. **Brug built-in subagents — ikke dig selv — til tunge opslag.** Claude Code har disse indbyggede agents (se `/agents`): `Explore`, `Plan`, `claude-code-guide`, `general-purpose`, `statusline-setup`. Brug dem aktivt:
   - Brug **`Explore`**-agenten til at surveye repoet (struktur, sprog, manifest-filer, README) før du udfylder indhold. Den har sit eget kontekstvindue og belaster ikke hovedsessionen.
   - Brug **`claude-code-guide`**-agenten til at verificere det korrekte format for `.claude/commands/*.md` (YAML frontmatter, `allowed-tools`, placering) og for `CLAUDE.md` `@`-imports, hvis du er i tvivl. Spørg agenten — gæt ikke.
   - Brug **`Plan`**-agenten til at producere en rigtig eksekveringsplan før du begynder at skrive filer. Vis mig planen kort, og fortsæt så.
2. **Lav IKKE skills.** Skills (`.claude/skills/`) er overkill til dette. Brug almindelige slash commands som markdown-filer i `.claude/commands/` (fx `genstart.md`, `gem.md`, `afslut.md`). Intet andet.
3. **Spørg før overskrivning.** Hvis en af filerne nedenfor allerede findes med indhold, så vis mig indholdet og spørg om jeg vil bevare, flette eller erstatte. Overskriv aldrig uden bekræftelse.
4. **Overskriv aldrig `plan/CLAUDE.md`** efter den er oprettet første gang. Den er en stabil "spillebog". `/gem` og `/afslut` må aldrig røre den.
5. **Alt tekstindhold på dansk.**
6. Når alt er oprettet: kør `/genstart` til sidst som verifikation.

---

## Filer der skal oprettes

### 1. `CLAUDE.md` (i repo-roden)

**Hvis `CLAUDE.md` allerede findes:** overskriv den IKKE. I stedet:

1. Læs den eksisterende `CLAUDE.md`.
2. Tilføj øverst (eller under den første overskrift) linjen:
   ```
   Se @plan/CLAUDE.md for hvordan plan-mappen bruges og den aktuelle projekt-status.
   ```
3. Gennemgå det eksisterende indhold og flyt det til den rigtige plan-fil:
   - 1-afsnits overordnet beskrivelse af hvad projektet er → `plan/projekt.md`
   - Vision/formål/målgruppe → `plan/vision.md`
   - Feature-beskrivelser → `plan/features.md`
   - Tech stack / dependencies / versioner / build-kommandoer → `plan/teknik.md`
   - Ideer/ønsker/roadmap → `plan/fremtid.md`
   - Aktuel status / "i gang med X" → `plan/status.md`
   - Kodekonventioner, stilregler, "svar på dansk" → bliver i rod-`CLAUDE.md`
4. Vis mig en diff før du gemmer.

**Hvis `CLAUDE.md` ikke findes:** opret den med følgende minimale skabelon. Den importerer kun `plan/CLAUDE.md` — resten loades ikke automatisk.

```markdown
# Projekt-instruktioner

Se @plan/CLAUDE.md for hvordan plan-mappen bruges og den aktuelle projekt-status.

## Repo-specifikke regler

- Svar på dansk medmindre andet angives.
- Følg eksisterende kodestil i repoet.
```

### 2. `plan/CLAUDE.md` (immutable spillebog — må ikke ændres automatisk)

Forklarer strukturen i `plan/` og importerer den aktuelle status. Alle andre filer i `plan/` er on-demand referencer.

Brug præcis denne skabelon (tilpas kun hvis jeg beder om det):

```markdown
# Plan-mappen — sådan bruges den

> Denne fil er immutable. `/gem` og `/afslut` må ikke ændre den.
> Kun mennesker redigerer `plan/CLAUDE.md`.

Den aktuelle status er altid i kontekst via importet nedenfor:

@status.md

## On-demand filer

Følgende filer loades **ikke** automatisk. Læs dem når opgaven kræver det:

- `plan/projekt.md` — **læses altid først** når en ny opgave starter, eller når konteksten skal etableres. Kort overordnet beskrivelse af hvad projektet er. Alle andre plan-filer uddyber denne.
- `plan/vision.md` — læses når brugeren spørger om produktets formål, målgruppe eller problem.
- `plan/features.md` — læses når brugeren spørger hvad produktet kan, eller planlægger nye features.
- `plan/teknik.md` — læses når brugeren spørger om tech stack, dependencies, versioner, build-, test- eller run-kommandoer, eller før du tilføjer en ny dependency.
- `plan/fremtid.md` — læses når brugeren spørger om backlog, ideer eller næste skridt ud over nuværende arbejde.
- `plan/historik/` — læses når brugeren spørger hvad der tidligere er gjort, eller når kontekst fra en tidligere session mangler. Start med de 3 seneste filer.

## Hvem opdaterer hvad

| Fil                      | Opdateres af                                | Hvornår                                           |
| ------------------------ | ------------------------------------------- | ------------------------------------------------- |
| `plan/CLAUDE.md`         | **Kun mennesker**                           | Sjældent, ved strukturelle ændringer              |
| `plan/projekt.md`        | Mennesker                                   | Når projektets kerne ændres (sjældent)            |
| `plan/status.md`         | `/gem`, `/afslut`, eller efter større skridt | Løbende                                           |
| `plan/vision.md`         | Mennesker                                   | Ved produktbeslutninger                           |
| `plan/features.md`       | Mennesker + Claude efter aftale             | Når features tilføjes/fjernes                     |
| `plan/teknik.md`         | Mennesker + Claude efter aftale             | Når dependencies/versioner/build-kommandoer ændres |
| `plan/fremtid.md`        | `/afslut` eller mennesker                   | Når nye ideer opstår                              |
| `plan/historik/*.md`     | `/gem`, `/afslut`                           | En fil pr. afsluttet ting                         |

## Historik-konvention

Filnavn: `YYYY-MM-DD-kort-titel.md` (fx `2026-04-19-tilfoej-login.md`).
Indhold: kontekst, beslutninger, ændrede filer, næste skridt.
```

### 3. `plan/status.md`

Aktuel tilstand. Fyld ind baseret på hvad du faktisk ser i repoet.

```markdown
# Status

**Sidst opdateret:** <dagens dato på formen YYYY-MM-DD>

## Hvad virker

- <udfyld ud fra hvad du kan se i repoet>

## Hvad er i gang

- <udfyld, eller skriv "Intet registreret endnu.">

## Kendte problemer

- <udfyld, eller skriv "Ingen registreret.">

## Næste skridt

- <udfyld, eller skriv "Defineres i næste session.">
```

### 4. `plan/projekt.md`

Overordnet projektbeskrivelse i ét kort afsnit. Denne fil er sandhedskilden for "hvad er dette projekt" og refereres af alle andre plan-filer og slash commands. Skal kunne læses på under 30 sekunder.

```markdown
# Projektbeskrivelse

<1 afsnit (3-6 sætninger) der beskriver hvad projektet er, hvad det gør,
og i hvilket teknisk miljø det kører. Skrives i nutid. Ingen historik,
ingen roadmap — kun hvad projektet ER lige nu.>

**Navn:** <repo-navn>
**Type:** <fx CLI-værktøj, webapp, bibliotek, service>
**Primær bruger:** <kort — uddybes i vision.md>

Se også:
- `plan/vision.md` — hvorfor projektet findes
- `plan/features.md` — hvad det konkret kan
- `plan/teknik.md` — stack og dependencies
- `plan/status.md` — aktuel tilstand
```

### 5. `plan/vision.md`

```markdown
# Vision

**Hvad vil vi med dette projekt?**
<1-3 sætninger — udfyld ud fra README/kode, eller skriv "Defineres." hvis intet er klart.>

**Hvem er brugeren?**
<målgruppe>

**Hvilket problem løses?**
<problemet i 1-2 sætninger>
```

### 6. `plan/features.md`

```markdown
# Features (højt niveau)

Liste over hvad produktet kan — ikke implementation.

- <feature 1>
- <feature 2>

## Planlagte

- <feature der er besluttet men ikke bygget>
```

### 7. `plan/teknik.md`

Tech stack og dependencies med **faktiske versioner** — hentet fra manifest-filerne i repoet (`package.json`, `pnpm-lock.yaml`, `pyproject.toml`, `poetry.lock`, `Cargo.toml`, `go.mod`, `Gemfile.lock` osv.). Ingen omtrentlige værdier, ingen "^"/"~"-ranges i selve dokumentet — skriv den ophørte version der faktisk bruges. Hvis der er en lockfile, tager den forrang over manifestet.

```markdown
# Teknik

**Sidst opdateret:** <dagens dato på formen YYYY-MM-DD>
**Kilde:** <hvilke filer blev læst, fx `package.json` + `pnpm-lock.yaml`>

## Stack

- Sprog: <fx TypeScript 5.6.3, Node 22.11.0>
- Runtime / framework: <fx Next.js 15.0.3, Vite 5.4.10>
- Database: <fx PostgreSQL 16>
- Test: <fx Vitest 2.1.4>

## Dependencies (produktion)

| Pakke | Version | Formål |
| ----- | ------- | ------ |
| <navn> | <nøjagtig version fra lockfile> | <1 linje> |

## Dev dependencies (udvalg)

| Pakke | Version | Formål |
| ----- | ------- | ------ |
| <navn> | <version> | <formål> |

## Kommandoer

- Installer: `<fx pnpm install>`
- Byg: `<fx pnpm build>`
- Test: `<fx pnpm test>`
- Kor lokalt: `<fx pnpm dev>`
```

### 8. `plan/fremtid.md`

```markdown
# Fremtid

Ønsker, ideer og backlog på højt niveau. Intet her er en forpligtelse.

## Ideer

- <idé>

## Nice-to-have

- <ting>
```

### 9. `plan/historik/CLAUDE.md`

Bruger filnavnet `CLAUDE.md` (ikke `README.md`) så Claude Code auto-loader den når der arbejdes med filer i `plan/historik/`. Den ligger ikke i hovedsessionens start-kontekst — kun når historik-filer faktisk læses.

```markdown
# Historik — mappe-instruktioner

Én markdown-fil pr. afsluttet ting (feature, beslutning, session).

## Filnavn-konvention

`YYYY-MM-DD-kort-titel.md` — fx `2026-04-19-opsaet-docs-struktur.md`.

## Indhold pr. fil

- **Dato** og kort titel
- **Kontekst** — hvorfor blev dette gjort
- **Beslutninger** — hvad blev valgt og hvorfor
- **Ændrede filer** — liste
- **Næste skridt** — hvis nogen
```

### 10. `.claude/commands/genstart.md`

```markdown
---
description: Genstart session — læs plan + kode og resumér hvor vi er
allowed-tools: Read, Glob, Grep, Bash
---

Genstart sessionen ved at opbygge fuld kontekst:

1. Læs `CLAUDE.md` og `plan/CLAUDE.md`.
2. Læs `plan/projekt.md` først (overordnet beskrivelse), derefter: `plan/vision.md`, `plan/features.md`, `plan/teknik.md`, `plan/status.md`, `plan/fremtid.md`.
3. Læs `plan/historik/CLAUDE.md` og de 3 senest ændrede filer i `plan/historik/` (sorter efter filnavnets dato).
4. Scan kodebasen overfladisk: list top-level mapper, læs `README.md` hvis den findes, og tjek `package.json` / `pyproject.toml` / tilsvarende manifest.
5. Resumér på dansk:
   - Hvad er projektet (1-2 sætninger)?
   - Hvad er den aktuelle status?
   - Hvad er næste skridt?
   - Er der uoverensstemmelser mellem `plan/status.md` og hvad du ser i koden?

Skriv ingen filer i dette skridt.
```

### 11. `.claude/commands/gem.md`

```markdown
---
description: Gem midt-session snapshot — opdater status og skriv historik-fil
allowed-tools: Read, Write, Edit, Bash
---

Gem et snapshot af den aktuelle session uden at committe.

1. **Rør ALDRIG `plan/CLAUDE.md`.**
2. Opdater `plan/status.md`:
   - Sæt "Sidst opdateret" til dagens dato (YYYY-MM-DD).
   - Opdater "Hvad virker", "Hvad er i gang", "Kendte problemer", "Næste skridt" ud fra hvad der er lavet i denne session.
3. Opret en ny fil i `plan/historik/` med navn `YYYY-MM-DD-<kort-titel>.md` (find en kort, sigende titel selv):
   - Dato og titel
   - Kontekst (hvad var udgangspunktet)
   - Beslutninger taget i sessionen
   - Ændrede filer (faktisk liste)
   - Næste skridt
4. Commit ikke. Bare vis mig hvad du har skrevet.
```

### 12. `.claude/commands/afslut.md`

```markdown
---
description: Wrap-up ved sessionens slutning — status, fremtid, historik, commit-forslag
allowed-tools: Read, Write, Edit, Bash
---

Afslut sessionen grundigt.

1. **Rør ALDRIG `plan/CLAUDE.md`.**
2. Opdater `plan/status.md` (dato + alle sektioner).
3. Hvis der er opstået nye ideer eller ønsker i sessionen: tilføj dem i `plan/fremtid.md` under "Ideer" eller "Nice-to-have".
4. Opret en afsluttende fil i `plan/historik/YYYY-MM-DD-<kort-titel>.md` med:
   - Kontekst, beslutninger, ændrede filer, næste skridt (samme format som `/gem`).
5. Foreslå en commit-besked i formatet `type: kort beskrivelse` (fx `docs: opdater status efter session`). **Committer ikke selv** — vis beskeden og lad mig køre `git commit`.
```

---

## Udførelsesrækkefølge

1. **Delegér til `Explore`-agenten**: bed den surveye repoet — top-level mapper, sprog/stack, manifest-filer, README-indhold, eksisterende `.claude/`- eller `plan/`-mappe. Få et kort resumé tilbage.
2. **Delegér til `claude-code-guide`-agenten** hvis du er usikker på: `.claude/commands/*.md`-frontmatter-felter, tilladte værdier for `allowed-tools`, eller `@`-import-syntaks i `CLAUDE.md`. Brug svaret til at validere skabelonerne ovenfor før du skriver.
3. **Delegér til `Plan`-agenten**: bed den lave en konkret eksekveringsplan (hvilke filer oprettes i hvilken rækkefølge, hvilke eksisterer allerede, hvor skal der spørges). Vis mig planen.
4. Tjek om nogen af filerne ovenfor allerede findes — hvis ja, spørg før du rører dem.
5. Opret mapperne `plan/`, `plan/historik/`, `.claude/commands/`.
6. Opret/opdatér alle 12 filer i den angivne rækkefølge med det viste indhold. Udfyld placeholders realistisk ud fra `Explore`-surveyet — især `plan/teknik.md` skal have nøjagtige versioner fra lockfile/manifest. Hvis rod-`CLAUDE.md` allerede fandtes, flyt dens indhold ind i de relevante plan-filer som beskrevet i afsnit 1.
7. Vis mig en kort liste over hvad der blev oprettet.
8. Kør `/genstart` som verifikation.
