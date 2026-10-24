# Registro dei rischi ICT – Digital Bank (bozza v2)

Fonti:
- Threat model OWASP Threat Dragon "Digital Bank Credit Scoring" (rilasciato il 23/10/2026): 8 minacce, 0 mitigate, 8 aperte.
- Assessment di configurazione AWS S3 del Blocco 3 (Prowler 5.45.0, regione eu-north-1, 18 controlli, 8 PASS, 10 FAIL). Ambiente di laboratorio con dati fittizi.

Tutti i rischi ricadono nel pilastro 1 di DORA (gestione del rischio ICT). L'area indica la funzione del controllo.

## A. Rischi di progetto (dal threat model)

| ID | Rischio | Componente | STRIDE | Gravità | Stato | Mitigazioni previste | Area DORA | Collegato a |
|---|---|---|---|---|---|---|---|---|
| TD-1 | Data poisoning: dati manipolati falsano il punteggio e portano a concedere o negare credito in modo errato | Motore AI Scoring | Tampering | Alta | Aperto | Validazione degli input, controlli di integrità sui dataset | Protezione | – |
| TD-2 | Model extraction tramite query ripetute per ricostruire o aggirare i criteri del modello | Motore AI Scoring | Information Disclosure | Media | Aperto | Rate limiting sulle inferenze, monitoraggio di query anomale, autenticazione obbligatoria | Protezione | – |
| TD-3 | Accesso non autorizzato ai dati dei clienti (riservatezza, rischio GDPR) | Database clienti | Information Disclosure | Alta | Aperto | Cifratura a riposo, privilegi minimi, audit log di ogni lettura, monitoraggio accessi anomali | Protezione | R1, R2, R5 |
| TD-4 | Alterazione dei dati di un cliente per falsare il punteggio e ottenere credito non dovuto | Database clienti | Tampering | Alta | Aperto | Permessi di scrittura minimi, log immutabili, controllo incrociato con fonti esterne, doppia approvazione | Protezione | R3, R4, R9 |
| TD-5 | Furto d'identità del cliente (credenziali rubate, phishing, MFA fatigue) | Auth Service | Spoofing | Alta | Aperto | MFA resistente al phishing, limite ai tentativi, blocco account, rilevamento accessi anomali | Protezione | – |
| TD-6 | Abuso dei privilegi dell'applicazione compromessa su database, motore AI e notifiche | Applicazione | Elevation of Privilege | Alta | Aperto | Permessi minimi, credenziali separate per servizio, isolamento di rete | Protezione | – |
| TD-8 | Impersonificazione di un operatore interno per modificare i punteggi | Operatore interno | Spoofing | Alta | Aperto | MFA resistente al phishing, accesso solo da rete aziendale, monitoraggio accessi anomali | Protezione | – |

Nota: nel report di Threat Dragon le minacce 6 e 7 sono identiche (stesso titolo, descrizione e mitigazioni). Qui sono contate come un solo rischio (TD-6). Conviene eliminare il duplicato nel modello.

## B. Rischi di configurazione cloud (dal Blocco 3)

| ID | Rischio | Controllo Prowler | Gravità | STRIDE | Area DORA | Trattamento |
|---|---|---|---|---|---|---|
| R1 | Un altro bucket potrebbe essere esposto pubblicamente per errore di configurazione (Block Public Access non attivo a livello di account) | s3_account_level_public_access_blocks | Alta | Information Disclosure | Protezione | Mitigare |
| R2 | Dati esposti in transito se le richieste usano un canale non cifrato | s3_bucket_secure_transport_policy | Media | Information Disclosure, Tampering | Protezione | Mitigare |
| R3 | Perdita o alterazione dei dati senza possibilità di recupero (versioning disattivato, MFA Delete assente) | s3_bucket_object_versioning, s3_bucket_no_mfa_delete | Media | Tampering | Continuità e ripristino | Mitigare |
| R4 | Difficoltà a ricostruire eventi e attribuire azioni (access logging assente) | s3_bucket_server_access_logging_enabled | Media | Repudiation | Rilevazione | Valutare |
| R5 | Controllo sulle chiavi di cifratura insufficiente per requisiti bancari (AES-256 senza SSE-KMS) | s3_bucket_kms_encryption | Media | Information Disclosure | Protezione | Valutare |
| R6 | Ciclo di vita degli oggetti non gestito | s3_bucket_lifecycle_enabled | Bassa | Non indicato | Continuità e ripristino | Accettare (solo laboratorio) |
| R7 | Nessuna replica tra regioni | s3_bucket_cross_region_replication | Bassa | Non indicato | Continuità e ripristino | Accettare (solo laboratorio) |
| R8 | Nessuna notifica sugli eventi del bucket | s3_bucket_event_notifications_enabled | Bassa | Non indicato | Rilevazione | Accettare (solo laboratorio) |
| R9 | Oggetti modificabili o cancellabili senza protezione (Object Lock assente) | s3_bucket_object_lock | Bassa | Non indicato | Continuità e ripristino | Accettare (solo laboratorio) |

## Note
- Il collegamento tra sezione A e B mostra dove un controllo previsto nel progetto (ad esempio "cifratura a riposo" o "log immutabili") è stato verificato nella configurazione reale.
- Trattamento "Accettare" vale solo per il laboratorio. In un ambiente bancario reale R6-R9 andrebbero rivalutati su continuità operativa, conservazione e integrità delle evidenze.
- Stato: nessuna remediation eseguita e nessun confronto prima/dopo. I trattamenti "Mitigare" sono azioni pianificate, non completate. Le mitigazioni del threat model sono tutte ancora da implementare.
- Il threat model copre solo alcune categorie STRIDE (Spoofing, Tampering, Information Disclosure, Elevation of Privilege) e non ha minacce su API Gateway, Sistema notifica, Data Warehouse e flussi di dati. Possibile estensione futura.
- Prossime colonne da aggiungere: proprietario del rischio, probabilità e impatto economico stimato.
