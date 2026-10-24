# Quantificazione economica del rischio – Digital Bank (bozza v1)

IMPORTANTE: tutti i numeri sono ipotesi didattiche della banca fittizia, non dati di mercato. Il valore del lavoro sta nel metodo. In un contesto reale i numeri andrebbero sostituiti con dati interni e benchmark di settore.

## Metodo
Per ogni rischio: Perdita annua attesa = probabilità annua dell'evento × impatto stimato per evento.
La perdita è calcolata prima delle mitigazioni (rischio inerente).

## Ipotesi di base
- Banca digitale fittizia con 50.000 clienti e 20.000 richieste di credito all'anno.
- Probabilità annua: Bassa = 5%, Media = 15%, Alta = 30%.
- Perdita media per un credito concesso in modo errato: da 5.000 a 8.000 euro, secondo il rischio.
- Costo complessivo di un data breach (gestione dell'incidente, comunicazioni, sanzioni e reputazione): 30 euro per cliente coinvolto.

## Stima per rischio

| ID | Rischio | Gravità nel threat model | Probabilità annua | Impatto per evento | Perdita annua attesa | Base dell'impatto |
|---|---|---|---|---|---|---|
| TD-3 | Accesso non autorizzato ai dati dei clienti | Alta | Media (15%) | 1.500.000 € | 225.000 € | 50.000 clienti × 30 € |
| TD-5 | Furto d'identità del cliente | Alta | Alta (30%) | 300.000 € | 90.000 € | 200 clienti colpiti × 1.500 € di frode |
| TD-6 | Abuso dei privilegi dell'applicazione | Alta | Bassa (5%) | 1.000.000 € | 50.000 € | Compromissione ampia di dati e servizi |
| TD-1 | Data poisoning sul motore AI | Alta | Bassa (5%) | 500.000 € | 25.000 € | 100 crediti errati × 5.000 € |
| TD-8 | Impersonificazione di un operatore | Alta | Bassa (5%) | 400.000 € | 20.000 € | 50 punteggi alterati × 8.000 € |
| TD-4 | Alterazione dei dati di un cliente | Alta | Bassa (5%) | 240.000 € | 12.000 € | 30 crediti non dovuti × 8.000 € |
| TD-2 | Model extraction | Media | Bassa (5%) | 150.000 € | 7.500 € | Perdita di vantaggio competitivo e frodi facilitate |

Totale perdita annua attesa: 429.500 €.

## Cosa mostra l'esercizio
- La gravità "Alta" non basta per decidere le priorità: nel threat model sei rischi su sette sono Alta, ma in termini economici il furto di dati dei clienti (TD-3) pesa più della metà del totale e il data poisoning (TD-1) è solo quarto.
- Un'unica misura, l'MFA resistente al phishing, copre sia TD-5 sia TD-8: insieme valgono 110.000 € di perdita annua attesa, circa un quarto del totale.
- Con questi numeri la priorità di investimento sarebbe: protezione dei dati dei clienti (TD-3), poi identità (TD-5, TD-8), poi isolamento dei privilegi (TD-6).

## Prossimo passo possibile
Stimare il costo annuo di ogni mitigazione e confrontarlo con la perdita evitata, per arrivare a una lista di priorità da presentare al board.
