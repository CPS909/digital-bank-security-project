# Gap assessment DORA – Digital Bank (bozza)

Stato: bozza iniziale, da verificare e correggere. Gli stati sono ipotesi basate sul design del progetto (Zero Trust, threat model STRIDE, motore AI di scoring, lab AWS con dati finti).

Legenda stato: Presente / Parziale / Mancante

| Pilastro DORA | Cosa richiede (in sintesi) | Cosa c'è già nel progetto | Stato | Cosa manca |
|---|---|---|---|---|
| 1. Gestione del rischio ICT | Framework di gestione del rischio ICT, governance, ruoli, inventario degli asset, controlli di sicurezza | Architettura Zero Trust, threat model STRIDE | Parziale | Framework formale, ruoli e responsabilità, inventario asset, registro dei rischi ICT |
| 2. Gestione e segnalazione degli incidenti | Processo di gestione, classificazione e segnalazione degli incidenti gravi | Nulla di formalizzato | Mancante | Criteri di classificazione, albero decisionale, flusso di segnalazione |
| 3. Test di resilienza operativa digitale | Programma di test periodici (vulnerability assessment, scenari, per i soggetti rilevanti anche TLPT) | Scansione Prowler fatta una volta (evidenza iniziale) | Parziale | Programma di test, frequenza, responsabilità |
| 4. Rischio dei fornitori terzi ICT | Registro delle informazioni, clausole contrattuali, exit strategy, concentrazione | AWS usato nel lab, non analizzato come terza parte | Mancante | Registro dei fornitori ICT, valutazione di criticità di AWS, exit strategy |
| 5. Condivisione delle informazioni | Condivisione volontaria di informazioni sulle minacce | Nulla | Mancante | Posizione della banca sulla partecipazione a scambi di threat intelligence |

## Prossimo passo
Controlla la colonna "Stato" e correggi dove la tua banca fittizia è messa diversamente da come ho ipotizzato.
