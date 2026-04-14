# Prompt: GitHub Copilot Usage Dashboard

## Ruolo

Agisci come un esperto programmatore Python senior con esperienza in data engineering, dashboard web e containerizzazione Docker. Segui le best practice Python (PEP 8, type hints, logging strutturato) e scrivi codice production-ready.

---

## Obiettivo

Crea una dashboard web in Python per visualizzare le metriche di utilizzo di GitHub Copilot per un tenant aziendale. La dashboard deve essere un server HTTP containerizzato, responsivo e moderno.

---

## Fonte dati

Hai accesso a un'istanza **Amazon Aurora PostgreSQL** con le seguenti coordinate:

| Parametro | Valore |
|---|---|
| Istanza / identificativo | `DB_PROD_COPILOT` |
| Database | `SXMLAIPRODP1` |
| Schema | `copilot` |

I dati provengono dalle API GitHub Copilot (documentazione: https://docs.github.com/en/enterprise-cloud@latest/rest/copilot?apiVersion=2026-03-10).

**Fase 1 — Esplora il database:**

Prima di scrivere qualsiasi codice applicativo, connettiti al database ed esegui le seguenti query di discovery. Mostra i risultati di ciascuna e commentali prima di procedere.

```sql
-- 1. Elenco tabelle nello schema copilot
SELECT table_name
FROM information_schema.tables
WHERE table_schema = 'copilot'
  AND table_type = 'BASE TABLE'
ORDER BY table_name;

-- 2. Struttura di ogni tabella (colonne, tipi, nullable, default)
SELECT table_name, column_name, data_type, character_maximum_length,
       is_nullable, column_default
FROM information_schema.columns
WHERE table_schema = 'copilot'
ORDER BY table_name, ordinal_position;

-- 3. Chiavi primarie ed esterne
SELECT
    tc.table_name, kcu.column_name,
    tc.constraint_type,
    ccu.table_name  AS foreign_table,
    ccu.column_name AS foreign_column
FROM information_schema.table_constraints tc
JOIN information_schema.key_column_usage kcu
     ON tc.constraint_name = kcu.constraint_name AND tc.table_schema = kcu.table_schema
LEFT JOIN information_schema.constraint_column_usage ccu
     ON ccu.constraint_name = tc.constraint_name AND ccu.table_schema = tc.table_schema
WHERE tc.table_schema = 'copilot'
ORDER BY tc.table_name;

-- 4. Campione dati (ripeti per ogni tabella trovata al passo 1)
-- SELECT * FROM copilot.<table_name> LIMIT 5;
```

Sulla base dei risultati:
1. Elenca tutte le tabelle trovate con una breve descrizione del loro contenuto.
2. Identifica quali colonne contengono timestamp / date di attività utente.
3. Identifica le colonne legate alle premium request.
4. Prima di procedere con il codice, presenta la mappa delle tabelle e chiedi conferma se qualcosa è ambiguo.

---

## Metriche da implementare

Sulla base delle tabelle trovate, identifica e implementa tutte le metriche applicabili tra quelle elencate di seguito. Se una metrica non è calcolabile dai dati disponibili, segnalalo con un commento nel codice.

### Sezione 1 — KPI Generali

- Totale utenti attivi nel periodo selezionato
- Totale suggestion generate / accepted / dismissed
- Acceptance rate (accepted / shown) — trend temporale
- Linguaggi più usati (top 10) per suggestion count e acceptance rate
- Editor / IDE più usati
- Utilizzo giornaliero / settimanale / mensile (serie temporale)
- Utenti più attivi (top 20 per suggestions accepted)
- **Tabella "Ultimo accesso per utente"**: per ogni utente mostra data e ora dell'ultima attività registrata nel DB; evidenzia gli utenti inattivi da più di 30 giorni (soglia configurabile via env var `INACTIVITY_DAYS_WARN`, default `30`)
- Distribuzione utilizzo per team / org (se disponibile)
- Chat turns totali e per utente (se disponibile)
- Completions vs chat usage ratio

### Sezione 2 — Premium Users

Gli utenti "premium" sono quelli che hanno un'estensione del limite standard di **300 premium request mensili** incluse nell'account standard.

Implementa:

- Lista di tutti gli utenti con il totale di premium request nel mese corrente e nel mese precedente
- Evidenzia (colore / badge) gli utenti che hanno **già superato** le 300 request
- Evidenzia gli utenti che sono **vicini alla soglia** (configura la soglia tramite env var, default: 80%)
- Grafico a barre orizzontale: top utenti per premium request
- Gauge o progress bar per ogni premium user: consumo vs soglia 300
- Trend mensile delle premium request per utente (sparkline o mini chart)
- Alert testuale: "X utenti hanno superato la soglia questo mese"

---

## Stack tecnologico

- **Framework web**: [Dash by Plotly](https://dash.plotly.com/) — usa `dash`, `plotly`, `dash-bootstrap-components` per un layout responsivo e moderno
- **DB driver**: `psycopg2-binary` per la connessione a Aurora PostgreSQL
- **Connection pooling**: `psycopg2` con `ThreadedConnectionPool` oppure `SQLAlchemy` con pool configurabile
- **ORM / query layer**: query SQL raw parametrizzate con `%s` placeholder (stile psycopg2, no ORM), con connection pooling
- **Containerizzazione**: Docker + Docker Compose
- **Configurazione**: tutte le variabili esterne gestite via environment variables (mai hardcoded)

---

## Architettura del progetto

```
copilot-dashboard/
├── app/
│   ├── __init__.py
│   ├── main.py              # entry point Dash
│   ├── layout.py            # layout della dashboard
│   ├── callbacks.py         # callback Dash per refresh e interattività
│   ├── db.py                # connessione e query al database
│   ├── metrics.py           # calcolo delle metriche / trasformazioni
│   └── config.py            # lettura env vars con validazione
├── assets/
│   └── custom.css           # stili aggiuntivi (se necessario)
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
└── .env.example             # template variabili d'ambiente (senza valori reali)
```

---

## Variabili d'ambiente

Tutte le configurazioni devono essere lette da env var. Crea `config.py` che le legge con `os.environ` e fallisce in modo esplicito se una variabile obbligatoria manca.

| Variabile | Obbligatoria | Default | Descrizione |
|---|---|---|---|
| `DB_HOST` | Sì | — | Endpoint Aurora PostgreSQL dell'istanza `DB_PROD_COPILOT` |
| `DB_PORT` | No | `5432` | Porta PostgreSQL |
| `DB_NAME` | Sì | `SXMLAIPRODP1` | Nome del database |
| `DB_USER` | Sì | — | Username PostgreSQL |
| `DB_PASSWORD` | Sì | — | Password PostgreSQL |
| `DB_SCHEMA` | No | `copilot` | Schema PostgreSQL da utilizzare |
| `DB_SSL_MODE` | No | `require` | Modalità SSL (`require`, `verify-full`, `disable`) — Aurora richiede SSL |
| `DB_POOL_MIN` | No | `1` | Dimensione minima del connection pool |
| `DB_POOL_MAX` | No | `10` | Dimensione massima del connection pool |
| `INACTIVITY_DAYS_WARN` | No | `30` | Giorni di inattività oltre i quali un utente viene evidenziato nella tabella "Ultimo accesso" |
| `PREMIUM_THRESHOLD` | No | `300` | Soglia premium request standard |
| `PREMIUM_WARNING_PCT` | No | `80` | % soglia per warning (es. 80 = warning a 240/300) |
| `REFRESH_INTERVAL_SEC` | No | `300` | Intervallo auto-refresh in secondi |
| `LOG_LEVEL` | No | `INFO` | Livello di log (`DEBUG`, `INFO`, `WARNING`, `ERROR`) |
| `DASHBOARD_PORT` | No | `8050` | Porta HTTP esposta dal server |
| `DASHBOARD_DEBUG` | No | `false` | Modalità debug Dash |

---

## Dockerfile

- Immagine base: `python:3.12-slim`
- Installa le dipendenze di sistema necessarie per `psycopg2`: `libpq-dev`, `gcc` (solo in build stage); nell'immagine finale bastano le librerie runtime `libpq5`
- Usa un **multi-stage build** per tenere l'immagine finale leggera (build stage con gcc, runtime stage senza)
- Copia solo i file necessari (usa `.dockerignore`)
- Esegue l'app come utente non-root
- `EXPOSE` la porta configurata (default 8050)
- Usa `CMD ["python", "-m", "app.main"]`

---

## docker-compose.yml

- Servizio `dashboard` che builda l'immagine locale
- Mappa la porta `8050:8050`
- Carica le variabili d'ambiente da un file `.env` (non committato)
- Aggiungi un `healthcheck` sull'endpoint `/` o `/_dash-ping`
- Nota: Aurora PostgreSQL è un servizio gestito AWS; dal container locale la connessione avviene via rete. Aggiungi un commento nel `docker-compose.yml` su come gestire l'accesso (VPN / SSH tunnel / AWS SSM port forwarding) se il cluster non è pubblicamente raggiungibile

---

## Requisiti UI / UX

- Layout responsivo basato su Bootstrap grid (`dash-bootstrap-components`, tema `DARKLY` o `FLATLY` a tua scelta)
- Header con titolo, data ultimo aggiornamento e pulsante **"Refresh"** che forza il ricalcolo
- Auto-refresh automatico ogni `REFRESH_INTERVAL_SEC` secondi (usa `dcc.Interval`)
- Sidebar o tab navigation per separare: **Overview**, **Utilizzo**, **Premium Users**
- Grafici Plotly interattivi (hover, zoom, download PNG)
- Tabelle con sorting e filtering (usa `dash_table.DataTable`)
- Colori semantici: verde = OK, arancione = warning, rosso = superata soglia
- Loading spinner durante il caricamento dati

---

## Qualità del codice

- Type hints su tutte le funzioni
- Docstring sulle funzioni principali
- Logging strutturato con `logging` (mai `print`)
- Gestione errori: se il DB non è raggiungibile, mostra un messaggio di errore nella dashboard anziché crashare
- Query SQL parametrizzate (mai f-string con input utente)
- Nessun segreto hardcoded nel codice

---

## Output atteso

Genera in sequenza:

1. Il contenuto di ogni file del progetto (uno alla volta, con path relativo come titolo)
2. Istruzioni per il primo avvio locale con Docker Compose
3. Eventuali note su connettività Aurora (SSL, VPC, security group) e permessi DB necessari
