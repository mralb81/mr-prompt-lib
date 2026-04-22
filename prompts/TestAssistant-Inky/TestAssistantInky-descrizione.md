# Inky – Test Assistant

## Panoramica

**Inky** è una piattaforma AI enterprise che automatizza la generazione di casi di test funzionali a partire dalla documentazione di progetto esistente. Fa parte del portafoglio **GenAI for IT** e viene adottata come strumento principale per la produzione di test case strutturati da consegnare ai team di QA.

L'obiettivo finale è produrre un **playbook di test** completo e strutturato, pronto per essere eseguito da un utente umano o da un agente AI. Il playbook è esportabile in formato compatibile con **HP ALM** (Application Lifecycle Management), il tool standard per la gestione del ciclo di vita dei test, e può essere importato direttamente nelle istanze ALM già in uso.

> Inky non produce script di automazione, ma **test case strutturati** che compongono un playbook eseguibile manualmente o tramite agente AI, e importabile in HP ALM.

---

## Fonti documentali supportate

Inky è in grado di elaborare le seguenti tipologie di documenti:

- Analisi Funzionali (AFU)
- Specifiche di prodotto
- User Story
- Reference API
- Manuali e documenti di processo

**Formati file accettati:** `.pdf`, `.doc`, `.docx`

---

## Funzionalità principali

### 1. Gestione Progetti

Un **Project** è il contenitore che raccoglie tutti i documenti su cui basare la generazione dei test. È consigliabile raggruppare nello stesso progetto documenti dello stesso macro-ambito funzionale.

Dalla home page è possibile:
- Creare un nuovo progetto
- Accedere a progetti esistenti
- Interagire con il **chatbot integrato** per creare progetti, generare requisiti o test tramite linguaggio naturale

### 2. Upload dei documenti

I documenti vengono caricati all'interno di un progetto tramite il pulsante **"+"**. L'elaborazione avviene in background come *job attivo*, monitorabile dal pannello **Active Jobs** (indicato dall'icona ⌛). Al termine, il documento è disponibile nella lista file del progetto.

### 3. Generazione di Requisiti e Test

La logica di Inky si articola su due livelli:

```
Documenti → Requirements (requisiti funzionali) → Tests (casi di test)
```

#### Master Document e Knowledge Base

Prima di generare i requisiti è necessario selezionare un **Master Document**: il documento principale da cui estrarre i requisiti (es. l'AFU della feature in scope). La selezione avviene tra i file già caricati nel progetto; un indicatore visivo (rosso/verde) segnala se la scelta è stata effettuata.

Al Master Document possono essere affiancati tutti gli altri documenti caricati nel progetto, che fungono da **knowledge base contestuale**: l'AI li utilizza per arricchire la comprensione del dominio e produrre requisiti e test più accurati e completi. Più ricca è la base documentale (specifiche tecniche, manuali, user story, reference API), migliore sarà la qualità del playbook generato.

#### Modalità di generazione dei Requirements

| Modalità | Come si usa |
|----------|-------------|
| **AI** ✨ | Il pulsante ✨ avvia l'estrazione automatica dei requisiti dal Master Document |
| **Chatbot** | Si descrive al chatbot quali requisiti si vogliono ottenere |
| **Manuale** | Inserimento diretto requisito per requisito tramite il pulsante "+" |

I requisiti sono sempre modificabili (✏️) ed eliminabili (🗑️).

#### Modalità di generazione dei Tests

Dalla tab **Requirements**, selezionando uno o più requisiti e cliccando l'icona **flask** (provetta), l'AI genera automaticamente i test case corrispondenti. In alternativa è possibile richiedere i test tramite chatbot o crearli manualmente.

Ogni test è identificato da due numeri:
- Il primo: ID del requisito di origine
- Il secondo: indice progressivo del test (univoco nell'ambito del requisito)

Anche i test sono modificabili ed eliminabili.

### 4. Esportazione del Playbook verso ALM

Una volta completata la revisione, l'intero set di test costituisce il **playbook di test** del progetto. Il playbook può essere:

- **Eseguito direttamente** da un tester umano, seguendo i passi e i criteri di accettazione generati da Inky
- **Passato a un agente AI** che esegue i test in modo automatico, utilizzando il playbook come istruzioni operative

Il playbook è esportabile in **formato Excel compatibile con [HP ALM](https://www.guru99.com/it/hp-alm-introduction.html)** (Application Lifecycle Management / Quality Center) tramite il pulsante di download nella tab Test. Il file è strutturato per essere importato direttamente nelle istanze ALM già in uso, mantenendo la tracciabilità tra requisiti e test case.

---

## Accesso e abilitazioni

L'accesso a Inky è soggetto a un processo di abilitazione. Per richiedere l'accesso:

1. Aprire un **Task Jira** sulla board `GenAI4IT Support – Kanban`
2. Indicare obbligatoriamente:
   - Nome e username/mail `@GEN` del richiedente
   - Area IT di appartenenza del progetto
   - Etichette Jira: `inky` + `support_genai4it`

> Le richieste prive dei campi obbligatori non vengono prese in carico. Al completamento dell'abilitazione, il ticket viene spostato in stato **"Certificazione"**.

**Contatto team:** it-generative-ai-team@generali.onmicrosoft.com

---

## Contesto di utilizzo

Inky è attivo come pilota (**Test Assistant Inky**) con referenti **Eleonora Notari** e **Alberto Tofanelli** (GenAI4IT). Viene utilizzato per:

- Generare casi di test da AFU nell'ambito del progetto **Maestro/Vita**
- Confrontare i test generati con quelli già presenti in ALM
- Supportare i **Test Generator Agents** nel progetto Flagship GENAI4HCF

È incluso nel catalogo **GenAI for IT** (voce: *Generates test cases from AFU*), insieme ad altri strumenti come Cobol Analyzer e Ticket Solver.
