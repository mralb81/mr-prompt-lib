# Analisi Log GitHub Copilot — Premium Users

## Contesto

Gestisco un tenant GitHub Copilot aziendale. Ho creato un gruppo dedicato chiamato **PremiumUser** per gli utenti autorizzati a superare il limite standard di 300 premium request al mese. Ho bisogno di analizzare periodicamente i log per capire chi ha superato la soglia e aggiornare il gruppo di conseguenza.

## Dati disponibili

Nella cartella `CopilotPremiumInfo` trovi i log di utilizzo GitHub Copilot esportati dal tenant. Analizza tutti i file presenti.

## Cosa fare

1. Leggi tutti i log nella cartella `CopilotPremiumInfo`
2. Per ogni utente, calcola il totale delle premium request nel periodo coperto dai log
3. Genera un report in formato Markdown con le seguenti sezioni:

### Struttura del report

**Riepilogo esecutivo**
- Periodo analizzato
- Numero totale di utenti trovati nei log
- Numero di utenti che hanno superato le 300 premium request
- Numero di utenti già presenti nel gruppo PremiumUser

**Utenti che hanno superato il limite (> 300 premium request)**
- Tabella con: username, totale request, data primo superamento, stato attuale nel gruppo PremiumUser
- Evidenzia chi è già nel gruppo PremiumUser e chi invece deve ancora essere aggiunto

**Utenti nel gruppo PremiumUser che NON hanno superato il limite**
- Tabella con: username, totale request
- Questi utenti potrebbero essere candidati alla rimozione dal gruppo

**Lista azioni consigliate**
- Utenti da aggiungere al gruppo PremiumUser
- Utenti da valutare per la rimozione dal gruppo PremiumUser

## Note

- Usa il simbolo ⚠️ per gli utenti che hanno superato il limite ma non sono ancora nel gruppo PremiumUser
- Usa il simbolo ✅ per gli utenti già correttamente nel gruppo PremiumUser
- Usa il simbolo 🔵 per gli utenti nel gruppo PremiumUser che non hanno ancora superato il limite
