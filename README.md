# 🔁 API Regressietest met Postman & Newman

Dit project is een proof of concept (PoC) voor het automatisch draaien van API-regressietesten via Postman, Newman en CI/CD (GitHub Actions).

---

## 📦 Inhoud

- ✅ 3 API-testcases:
  - GET /posts/1 → Verwacht 200
  - POST /posts → Dummy JSON → Verwacht 201
  - GET /invalidendpoint → Verwacht 404 (negatieve test)
- 🌍 Variabelen via een Postman environment
- 🧪 Automatische regressietest via GitHub Actions
- 📄 Rapportage in HTML + JSON
- 📤 Artefact export via CI/CD

---

## 🔧 Installatie (lokaal)

### 1. Vereisten

- Node.js (v16 of hoger)
- Newman

### 2. Newman installeren

```bash
npm install -g newman
```

## ▶️ Test lokaal draaien
```bash
newman run collection.json \
  -e environment.json \
  --reporters cli,html,json \
  --reporter-html-export newman-report.html \
  --reporter-json-export newman-report.json
```

* newman-report.html → Mooi visueel rapport
* newman-report.json → Voor log/analyse
* cli output → Zie je in je terminal


## 🛠️ CI/CD Integratie (GitHub Actions)
Bij elke push of pull request op main:

* Newman draait automatisch alle API-tests
* De pipeline faalt als een test faalt (exit code ≠ 0)
* HTML-report wordt als artefact opgeslagen

### Configuratiebestand:
.github/workflows/api-tests.yml
```yaml
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
```
Output van de pipeline:

* ✅ Testresultaten in de GitHub Actions logs
* 📎 HTML-rapport downloadbaar via "Artifacts"
* ❌ Fout = pipeline failed (ideaal voor regressietest)

## Beperkingen van Postman (in CI/CD context)
* Minder flexibel dan pure code (bv. Python/Robot)
* Testlogica in JS is beperkt
* Moeilijk om dynamisch veel tests te genereren
* Niet ideaal voor versiebeheer / grote projecten


## 📁 Projectstructuur

```bash
project-root/
│
├── collection.json           ← Postman collectie
├── environment.json          ← Environment met variabelen
├── newman-report.html        ← HTML rapport (na run)
├── newman-report.json        ← JSON rapport (na run)
├── .github/workflows/
│   └── api-tests.yml         ← GitHub Actions config
└── README.md                 ← Deze handleiding
```



## 🧑‍💻 Auteur
Naam: Abdi Ali