Aiutami a progettare un'automazione efficiente per 11 tapparelle automatiche.

Piattaforma e ambiente:
- iPhone con iOS 26.4.2
- App nativa Comandi Rapidi
- Domotica BTicino Living Now già configurata
- App Casa già funzionante

Vincolo tecnico:
- Le tapparelle sono alimentate da un UPS da 800 W.
- Per evitare sovraccarichi, non posso azionarle tutte insieme.
- Per sicurezza, devono muoversi una alla volta.

Obiettivo:
- Creare due automazioni: "Apri tapparelle" e "Chiudi tapparelle".
- Le tapparelle devono essere gestite in sequenza: la successiva parte solo quando la precedente ha terminato.

Requisiti funzionali:
- Voglio evitare copia-incolla ripetitivo dei comandi per ogni tapparella.
- Preferisco una soluzione basata su lista (o gruppo) + ciclo, così da cambiare i nomi in un solo punto.
- Il comando deve essere inviato solo se la tapparella non è già nello stato richiesto (aperta o chiusa), per risparmiare tempo e comandi inutili.

Output richiesto:
- Proponimi una struttura chiara, modulare e riutilizzabile dei comandi.
- Includi un esempio pratico completo (apertura e chiusura).
- Evidenzia come gestire il controllo dello stato prima dell'invio del comando.