/skill-creator

Voglio creare una skill per Claude Code che usa Playwright MCP (tool browser_*) per aprire un browser e compilare una richiesta su Generali UserCare (BMC DWP).
URL fisso della pagina del catalog item:
https://generaliusercare-dwp.onbmc.com/dwp/app/#/srm/profile/SRGAA5V0GKEK9AOC5WJD8M2R32FA52/srmti

Obiettivo:
- Aprire l’URL
- Il sito prevede LOGIN.
- La skill deve poter fare login usando un file locale nella cartella della skill chiamato:
  .claude/skills/<skill-name>/creds.json
  che conterrà le credenziali (username/password o campi equivalenti).
- Dopo login: arrivare al form “Nuova Identità Dipendente / Personale Esterno”
- Compilare i campi del form con i dati che fornisco io
- Verificare che non ci siano errori di validazione
- Produrre un riepilogo dei dati inseriti + snapshot/screenshot
- Fermarsi SEMPRE prima di cliccare “Inoltra richiesta” (non inviare mai)

Campi da gestire (nomi come a UI):
- Richiesta (dropdown/select se presente)
- Cognome (textbox)
- Nome (textbox)
- CF (textbox)
- Contatto Email (textbox)
- Referente Interno (textbox o autocomplete)
- Società Collaboratore (textbox o select)
- Data inizio Collaborazione (date)
- Fine Validità (date)
- Account di Rete (checkbox/radio)
- Account Applicativi (LDAP) (SI/NO — default SI)
- Sedi di Lavoro (UE / Extra UE)

Requisiti di design della skill:
1) Naming: nome skill in kebab-case, es. “generali-usercare-new-identity”
2) Description: deve far capire quando triggerare (creazione nuova identità/nuovo utente/utente esterno) e includere esplicitamente “stop before submit”
3) Guardrail critici per sicurezza:
   - NON stampare mai a schermo il contenuto di creds.json
   - NON includere credenziali nel testo di output, log, screenshot o riepiloghi
   - creds.json deve essere SOLO locale e MAI committato su git
   - genera/aggiorna un .gitignore nella cartella skill (o istruzioni) per escludere creds.json
4) Login:
   - La skill deve leggere creds.json e compilare la schermata di login tramite Playwright MCP
   - Dopo login riuscito, proseguire col form
   - Se login fallisce: fermarsi e mostrare messaggio di errore senza esporre segreti
5) Procedura robusta:
   - usare browser_snapshot per individuare controlli tramite ruoli/nome accessibile
   - poi browser_click/browser_type/browser_select_option
   - usare browser_wait_for in caso di timing
6) Output:
   - stato “compilata, pronta per inoltro”
   - riepilogo campi inseriti (senza credenziali)
   - evidenza (snapshot + screenshot)
   - lista problemi se presenti

Deliverable richiesti:
- Genera la struttura cartelle in .claude/skills/<skill-name>/
- Crea SKILL.md completo (frontmatter + istruzioni operative + esempi)
- Crea references/field-map.md con mappa campi e note (formati date, eventuali dropdown)
- Crea references/examples.md con 3 esempi di utilizzo:
  a) compilazione completa e stop
  b) compilazione con campi mancanti (la skill deve chiedere i dati)
  c) scenario con errori di validazione (la skill deve correggere o segnalare)
- Aggiungi un template di creds.json (senza valori reali) come creds.example.json
  e istruzioni per copiare in creds.json

Vincolo finale NON negoziabile:
- La skill deve fermarsi prima dell’invio e non deve mai cliccare “Inoltra richiesta”.
``