# Linee guida per contribuire

## Aggiungere un nuovo prompt

### 1. Scegli la posizione giusta

- **Vuoi richiamarlo con `/nome` in Copilot?** → mettilo in `.github/prompts/nome.prompt.md`
- **È un prompt tematico/archivio?** → mettilo in `prompts/categoria/nome.md`

### 2. Usa il frontmatter corretto

Per i file in `.github/prompts/`:

```markdown
---
name: nome-del-prompt
description: Breve descrizione di cosa fa e quando usarlo (1-2 righe).
---

Contenuto del prompt...
```

Per i file in `prompts/` (archivio):

```markdown
# Titolo del prompt

## Scopo

Breve descrizione.

## Istruzioni

...
```

### 3. Naming

- **File**: `kebab-case` (es. `create-jira-task.prompt.md`)
- **Cartelle**: `kebab-case` (es. `code-review/`, `jira-automation/`)
- **Estensioni**: `.agent.md` per agenti, `.prompt.md` per prompt slash, `.md` per archivio

### 4. Aggiorna il README

Aggiungi una riga alla tabella "Agenti disponibili" o "Prompt disponibili" in `README.md`.

---

## Aggiungere un nuovo agente

Gli agenti vanno in `.github/agents/nome.agent.md`.

Struttura minima:

```markdown
---
name: nome-agente
description: Cosa fa questo agente e quando invocarlo.
argument-hint: Descrizione degli input attesi (es. "il tipo di prompt da creare").
---

Sei un agente specializzato in...

Il tuo compito è...

Quando ricevi una richiesta:
1. ...
2. ...
```

---

## Evitare duplicati

- I file in `.github/prompts/` sono la versione **canonica** dei prompt slash.
- Non duplicare lo stesso prompt in `prompts/` a meno che non sia una versione significativamente diversa.
- Prima di aggiungere un nuovo file, cerca se esiste già qualcosa di simile.

---

## Dati sensibili

- Non includere nei file ID hardcoded di sprint, board o epic a meno che non siano intenzionalmente specifici per uso personale.
- Non includere indirizzi email o dati personali di terzi.
