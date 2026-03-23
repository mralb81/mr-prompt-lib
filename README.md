# mr-prompt-lib

Repository personale di **agenti**, **prompt** e **skill** per GitHub Copilot.

## Cosa contiene

Una libreria organizzata e riutilizzabile di istruzioni AI per automazioni quotidiane con Copilot, suddivisa per tipo e progetto.

## Struttura

```
mr-prompt-lib/
├── .github/
│   ├── agents/          # Agenti Copilot (.agent.md) — attivi nel workspace
│   └── prompts/         # Prompt Copilot (.prompt.md) — attivi nel workspace
├── prompts/             # Archivio prompt per progetto/categoria
│   ├── Jira/
│   ├── SqueroVecio/
│   └── TestPilot-video-demo/
└── .vscode/
    └── mcp.json         # Configurazione MCP server (Atlassian)
```

### Cartelle principali

| Cartella | Scopo |
|---|---|
| `.github/agents/` | Agenti attivi — GitHub Copilot li carica automaticamente nel workspace |
| `.github/prompts/` | Prompt slash attivi — richiamabili con `/nome-prompt` in Copilot |
| `prompts/` | Archivio tematico — prompt organizzati per progetto o dominio |

## Come usare

### Agenti (`.github/agents/*.agent.md`)

Gli agenti in `.github/agents/` vengono caricati automaticamente da GitHub Copilot nel workspace corrente. Invocali dal chat con `@nome-agente`.

**Esempio:**
```
@mrprompt crea un nuovo prompt per fare code review
```

### Prompt slash (`.github/prompts/*.prompt.md`)

I prompt in `.github/prompts/` sono richiamabili direttamente con il prefisso `/` nella chat di Copilot.

**Esempio:**
```
/markdown-revriter
/create-my-jira-task
```

### Prompt archivio (`prompts/`)

I file in `prompts/` sono prompt tematici da copiare/adattare. Organizzati per progetto in sottocartelle.

## Convenzioni di naming

| Tipo | Posizione canonica | Estensione |
|---|---|---|
| Agente Copilot | `.github/agents/` | `nome.agent.md` |
| Prompt slash Copilot | `.github/prompts/` | `nome.prompt.md` |
| Prompt archivio | `prompts/categoria/` | `nome.md` |

**Nomi cartelle**: usa `kebab-case` (es. `code-review/`, `jira-automation/`).

## Agenti disponibili

| Agente | Descrizione |
|---|---|
| `@mrprompt` | Aiuta a scrivere, organizzare e migliorare agenti, prompt e skill |

## Prompt disponibili

| Prompt | Descrizione |
|---|---|
| `/markdown-revriter` | Riscrive e formatta in Markdown il file aperto o il testo selezionato |
| `/create-my-jira-task` | Crea un ticket Jira nell'epic e sprint configurati |

## Contribuire

Vedi [CONTRIBUTING.md](CONTRIBUTING.md) per le linee guida su come aggiungere nuovi prompt o agenti.
