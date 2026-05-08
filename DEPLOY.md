# KlinikAI – Deployguide

## Filer
- `index.html` (omdøb klinikai.html → index.html)
- `manifest.json`
- `sw.js`
- `icon-192.png` + `icon-512.png` (lav selv eller brug generator)

## Deploy til klinikai.helgason.io

### 1. GitHub repo
```bash
git init klinikai
cd klinikai
cp klinikai.html index.html
cp manifest.json sw.js .
git add .
git commit -m "KlinikAI v1"
git remote add origin https://github.com/sonny987/klinikai
git push -u origin main
```

### 2. GitHub Pages
- Gå til repo Settings → Pages
- Source: main branch / root
- Custom domain: klinikai.helgason.io

### 3. DNS (Simply.com)
Tilføj CNAME record:
```
klinikai → sonny987.github.io
```

### 4. HTTPS
GitHub Pages håndterer automatisk SSL via Let's Encrypt.

## Integration i Clinote / Patienttjek
Kald KlinikAI's prompt-engine som et modul:

```javascript
// I Clinote – importer prompt-logikken
import { buildSystemPrompt } from './klinikai-core.js';

const prompt = buildSystemPrompt({
  spec: 'neurologi_stroke',
  faggruppe: 'sosu',
  doctype: 'plejeplan',
  niveau: 'faglig'
});

// Send til Claude API med prompt + brugerens råtekst
```

## API-nøgle
- Bruges direkte fra browser via Anthropic's CORS-header
- Gemmes i localStorage (aldrig sendt til server)
- Til produktion: brug en backend-proxy (Firebase Functions / Cloudflare Workers)
  for at skjule API-nøglen

## Prissætning (forslag)
| Plan | Pris | Indhold |
|------|------|---------|
| Gratis | 0 kr. | Eget API-nøgle, alle specialer |
| Pro | 99 kr./md | Hosted nøgle, ingen opsætning |
| Klinik | 499 kr./md | Team-adgang, branding, support |
| Afdeling | 2.500 kr./md | OUH/hospital-plan, SLA |
