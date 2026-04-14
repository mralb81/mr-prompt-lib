# GenAI Ticket Solver – Descrizione dello strumento

## Cos'è

Il GenAI Ticket Solver è uno strumento basato su intelligenza artificiale generativa progettato per supportare gli operatori nella gestione di ticket di supporto di secondo livello (L2). Nasce per affrontare uno dei problemi più comuni nei team di supporto: ticket spesso lunghi, incompleti, con allegati eterogenei, che richiedono tempo e competenza per essere diagnosticati e risolti in modo coerente.

L'obiettivo non è sostituire l'operatore, ma affiancarlo: ridurre i tempi di diagnosi, garantire coerenza nelle risposte e valorizzare la conoscenza già disponibile nell'organizzazione.

---

## Come funziona – Flusso end-to-end

### 1. Analisi del ticket

Quando un operatore seleziona un ticket, il Ticket Solver avvia un'analisi automatica. Il sistema interpreta:

- la **descrizione testuale** del problema
- i **campi strutturati** (categoria, priorità, sistema coinvolto, ecc.)
- gli **allegati** eventualmente presenti

Parallelamente esegue una serie di **controlli statici** per valutare la completezza e la qualità del ticket, stimando quanto il caso sia risolvibile in modo automatico o semi-automatico.

### 2. Ricerca per similarità

Il sistema effettua una ricerca nei ticket storici chiusi per trovare casi analoghi. Il processo avviene in due passaggi:

- **Ricerca vettoriale per similarità semantica**: individua i ticket passati più simili per contenuto e contesto.
- **Filtro generativo**: un modello generativo rilegge i candidati e seleziona quelli realmente pertinenti, scartando i falsi positivi.

Il risultato è un pannello con 3–5 ticket simili, ciascuno con le proprie soluzioni adottate, che l'operatore può consultare come riferimento.

### 3. Integrazione con Advisor (Knowledge Base)

Il Ticket Solver si integra con **Advisor**, la piattaforma interna di knowledge base. L'integrazione non è una semplice ricerca testuale: il sistema genera in modo automatico **domande sintetiche** a partire dal contenuto del ticket, e le usa per interrogare Advisor in modo mirato.

Questo permette di verificare se nella documentazione esistente sia già presente una risposta alla problematica segnalata — articoli tecnici, procedure, best practice, FAQ.

Quando Advisor restituisce risultati rilevanti, l'operatore ha a disposizione:
- estratti degli articoli pertinenti
- riferimenti diretti alle fonti
- un'indicazione su quanto la documentazione copra il problema

### 4. Agenti investigativi (opzionale)

Se il caso richiede informazioni non disponibili nei ticket storici né nella knowledge base, il Ticket Solver può attivare **agenti investigativi** che eseguono verifiche live sui sistemi: interrogano API, controllano log, recuperano dati di contesto in tempo reale per arricchire l'analisi.

### 5. Proposta automatica di risoluzione

Al termine del processo, il sistema genera una **proposta di risoluzione strutturata**, che include:

- una spiegazione delle possibili cause
- i passaggi operativi suggeriti
- le verifiche raccomandate
- i riferimenti alle fonti utilizzate (ticket simili, articoli Advisor, evidenze)

La proposta non è una risposta definitiva, ma un punto di partenza qualificato che l'operatore può validare, modificare e inviare.

---

## Il valore aggiunto: Advisor come leva di miglioramento

Uno degli aspetti più strategici del Ticket Solver è il collegamento con Advisor in chiave proattiva. Quando il sistema trova una risposta nella knowledge base, l'operatore può valutare se il problema sia in realtà già documentato e la soluzione sia già disponibile agli utenti finali.

In questo caso, l'azione più efficace non è necessariamente intervenire sul sistema, ma **aggiornare o arricchire la documentazione**: un articolo più chiaro, una procedura più dettagliata, una FAQ aggiornata. Il beneficio si propaga nel tempo: i ticket futuri sullo stesso problema troveranno risposta direttamente da Advisor, riducendo ulteriormente il carico sul team L2.

---

## Feedback e miglioramento continuo

Ogni proposta di risoluzione può essere valutata dall'operatore tramite un meccanismo di feedback:

- **Utile / Non utile**
- **Commento libero** per spiegare cosa ha funzionato o cosa manca

Questi segnali vengono usati dal sistema per **aggiornare i pesi delle soluzioni**: le risposte utili vengono valorizzate nelle proposte future, quelle deboli vengono penalizzate. Nel tempo, la qualità delle proposte migliora in modo progressivo e contestuale all'organizzazione.

---

## Clustering automatico e KPI operativi

Il Ticket Solver produce anche una vista aggregata sui ticket gestiti, attraverso:

- **Clustering automatico**: raggruppa i ticket per tipologia di problema, evidenziando pattern ricorrenti e aree critiche.
- **Indicatori KPI**, tra cui:
  - qualità media dei ticket in ingresso (completezza, chiarezza)
  - copertura della knowledge base (quante problematiche trovano risposta in Advisor)
  - aggiornamento della documentazione (frequenza e recency degli articoli usati)

Questi indicatori aiutano i responsabili a prendere decisioni su dove intervenire: migliorare i template di ticket, formare gli utenti, o investire nell'aggiornamento di Advisor.

---

## In sintesi

| Funzionalità | Descrizione |
|---|---|
| Analisi ticket | Interpreta testo, campi strutturati e allegati |
| Ricerca per similarità | Trova ticket storici simili con filtro generativo |
| Integrazione Advisor | Interroga la knowledge base con domande sintetiche generate dal ticket |
| Agenti investigativi | Verifiche live sui sistemi per casi complessi |
| Proposta di risoluzione | Output strutturato con cause, passi operativi e fonti |
| Feedback loop | Apprendimento continuo dalle valutazioni degli operatori |
| Clustering & KPI | Vista aggregata su problematiche ricorrenti e qualità operativa |
