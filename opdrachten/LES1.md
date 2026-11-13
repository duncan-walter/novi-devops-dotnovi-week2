# Les 1: DevOps Fundamentals

## Opdrachtbeschrijving

Gefeliciteerd, het is je eerste werkdag bij dotNOVI! Je moet gaan werken aan een notitie applicatie die prima werkt.
Nouja... op jouw computer dan. Klassiek gevalletje van *it works on my machine*. Hoe de applicatie ooit veilig en
betrouwbaar bij gebruikers komt? Daar heeft nog niemand over nagedacht. Klinkt als een mooie klus voor jou!

Deze week begin je klein. Je zorgt ervoor dat je de applicatie aan de praat krijgt en onderzoekt wat er nog ontbreekt.
Daarna lever je een eerste wijziging op via een feature branch en een Pull Request. Je schrijft een DevOps-plan voor
dotNOVI en je zorgt dat er geen wachtwoorden op GitHub belanden.

## Opdrachten

### Opdracht 1: dotNOVI starten en verkennen

1. Maak op GitHub onder je eigen account een nieuwe, lege repository aan.
2. Clone [hogeschoolnovi/DevOps-dotnovi-week1](https://github.com/hogeschoolnovi/DevOps-dotnovi-week1) en push de code
   naar je eigen repository:

   ```bash
   git clone git@github.com:hogeschoolnovi/DevOps-dotnovi-week1.git
   cd DevOps-dotnovi-week1
   git remote remove origin
   git remote add origin git@github.com:<jouw-gebruikersnaam>/<jouw-repository>.git
   git branch -M main
   git push -u origin main
   ```

3. Start de app:

   ```bash
   npm install
   npm start
   ```

4. Open deze adressen in je browser en bekijk wat je ziet:
    - http://localhost:3000/
    - http://localhost:3000/health
    - http://localhost:3000/api/notes

   Zie je foutmeldingen? Die zijn in deze les normaal: de applicatie draait, maar er is nog geen database gekoppeld. Dit
   gaan we in een latere opdracht oplossen.

5. Bekijk de bestanden in de repo. Noteer minstens drie dingen die ontbreken om de app automatisch te testen, te bouwen,
   uit te rollen of in de gaten te houden. Geef per punt in één zin aan waarom het nodig is. Dit lijstje gebruik je in
   opdracht 3.

### Opdracht 2: Een wijziging doorvoeren
In deze opdracht voeg je via een feature branch en een Pull Request een extra endpoint toe aan de applicatie:

1. Maak een feature branch aan:

   ```bash
   git checkout -b feature/add-about-endpoint
   ```
2. Plak deze route in `src/index.js`, direct boven de 404 handler:
   ```js
   app.get('/about', (req, res) => {
     res.json({ name: 'dotNOVI', version: '1.0.0' });
   });
   ```
3. Stop de applicatie, start hem opnieuw met `npm start` en ga naar http://localhost:3000/about.
4. Commit en push je wijziging:

   ```bash
   git add .
   git commit -m "feat: add /about endpoint"
   git push origin feature/add-about-endpoint
   ```

5. Open op GitHub in je eigen repository een Pull Request van je feature branch naar `main`.
6. Vul de PR-beschrijving in met deze opzet:

   ```markdown
   ## Wat heb je veranderd en waarom?
   
   ## Wat is het type wijziging?
   - [ ] Feature
   - [ ] Bug fix
   - [ ] Documentatie

   ## Hoe test je dit?

   ## Checklist
   - [ ] Code volgt de style guidelines
   - [ ] Conventionele commit message gebruikt
   - [ ] Geen nieuwe warnings
   ```

7. Merge de PR.
8. Haal de wijziging lokaal op:

   ```bash
   git checkout main
   git pull origin main
   ```

### Opdracht 3: DevOps-plan schetsen
In deze opdracht ga je een eerste DevOps-plan voor dotNOVI schetsen. Dit wordt de basis voor je eindopdracht (deelopdracht 1):

- Neem je lijstje uit opdracht 1 als startpunt.
- Kies per fase van de DevOps lifecycle een tool.
- Beschrijf per fase in 1 of 2 zinnen wat je daar gaat doen.
- Kies een branching-strategie en beschrijf waarom deze past bij dotNOVI.

### Opdracht 4: Security awareness

Tot slot controleer je dat `.gitignore` onze secrets tegenhoudt.

1. Open `.gitignore` en controleer of `node_modules/`, `.env` en `.DS_Store` erin staan.
2. Maak een `.env` met een nep wachtwoord. Heb je al een `.env`? Sla dan deze stap over, want dit commando overschrijft
   het bestand.

   ```bash
   echo "DATABASE_PASSWORD=geheim123" > .env
   ```

3. Probeer het bestand toe te voegen en bekijk de melding:

   ```bash
   git add .env
   git status
   ```
   Als je de voorgaande stappen goed hebt gedaan zou je een foutmelding moeten krijgen.
4. Zoek uit wat je moet doen als je per ongeluk een secret hebt gecommit en gepusht. Bedenk ook waarom het niet genoeg is om het bestand in een nieuwe commit te verwijderen.


## Naslag

### Branching Strategieën

#### GitHub Flow

De eenvoudigste strategie, perfect voor continuous deployment:

```
main ──●──────●──────●──────●──── (altijd deploybaar)
        \          /
         ●──●──●──  feature/add-about-endpoint
```

**Werkwijze:**
1. Branch van main
2. Maak je wijzigingen
3. Open een Pull Request
4. Review en merge naar main

```bash
# Creëer feature branch
git checkout -b feature/add-about-endpoint

# Maak wijzigingen en commit met conventionele commits
git add .
git commit -m "feat: add /about endpoint"
git push origin feature/add-about-endpoint

# Open PR op GitHub → review → merge
```

#### Git Flow

Gestructureerder, voor teams met geplande releases:

**Branches:**
- `main` - Production ready code
- `develop` - Integration branch
- `feature/*` - Feature development
- `release/*` - Release preparation
- `hotfix/*` - Production fixes

```bash
# Feature branch van develop
git checkout develop
git checkout -b feature/add-categories

# Na voltooiing: PR naar develop
# Later: develop → release branch → main
```

#### Trunk-Based Development

Voor teams met sterke CI/CD en test coverage:
- Iedereen werkt op `main`
- Zeer korte feature branches (max 1-2 dagen)
- Feature flags i.p.v. lange branches
- Vereist: goede tests en CI pipeline

### Conventionele Commits

```bash
# Format: <type>(<scope>): <description>

git commit -m "feat(notes): add priority field to notes table"
git commit -m "fix(api): handle null values in title"
git commit -m "docs(readme): add installation steps"
git commit -m "test(notes): add tests for priority filtering"
git commit -m "chore(deps): update eslint"
```

**Types:**
- `feat` — nieuwe feature
- `fix` — bugfix
- `docs` — documentatie
- `test` — tests toevoegen/wijzigen
- `chore` — onderhoud (dependencies, config)
- `refactor` — code verbeteren zonder gedragswijziging

### Branch Naming

```
feature/short-description     # Nieuwe feature
bugfix/description-of-fix     # Bug fix
docs/update-readme            # Documentatie
refactor/improve-performance  # Code improvements
test/add-integration-tests    # Tests
```

## Volgende les

In les 2 gaan we:

- Docker introduceren
- Containerisatie begrijpen
- Een Dockerfile schrijven voor dotNOVI

## Resources

- [GitHub Flow](https://guides.github.com/introduction/flow/)
- [Git Flow Cheatsheet](https://danielkummer.github.io/git-flow-cheatsheet/)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [State of DevOps Report](https://dora.dev/research/)
