# Blazor Circuit

Un __Circuit__ è un'astrazione sopra a _SignalR_ tra il client ed il server per gestire lo _stato_ e lo _scopo_.

## Utilizzo
Ogni _Circuit_ viene creato all'atto della connessione di un nuovo client e determina una referenza all'interno dello spazio di memoria del server.\
A differenza delle applicazioni web _classiche_ un'applicazione Blazor Server mantiene lo stato di tutte le connessioni,  attraverso i Circuit.

Qualora un client cambi pagina o effettui una chiusura del browser il Circuit è rilasciato correttamente.

Se il client subisce una disconnessione forzata (problemi di rete) il Circuit tenterà di ricollegarsi per un determianto periodo di tempo (retry), fino al fallimento.

## Scoped Circuit
Esempio:

User 1 <== SignalR Connection ==> Server _Circuit A_

User 2 <== SignalR Connection ==> Server _Circuit B_

Se il server deve fornire informazioni per 2 componenti presenti in una pagina, utilizzerà la medesima connessione _Circuit A o B_ per lo User richiedente.

In questo modo i _Circuit_ sfruttano la loro connessione permanente per abbattere l'overhead necessario delle riconnessioni HTTP classiche.



