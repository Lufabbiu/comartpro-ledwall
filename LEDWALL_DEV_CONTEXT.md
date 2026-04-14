# LEDWALL_DEV_CONTEXT.md

## Contesto prodotto e contenuti — Area dev

Questo documento serve al team development per capire il contesto del progetto, la logica prodotto, i ruoli, i moduli funzionali, i flussi, i vincoli e i contenuti necessari.

Non è un documento tecnico di stack o architettura definitiva.  
È un documento di **contesto applicativo** e di **requirements funzionali**.

---

## 1. Sintesi del progetto

ComArtPro APS ETS intende realizzare un progetto composto da due elementi integrati:

1. **Led Wall esterno**
2. **Portale riservato agli associati**

Il Led Wall è uno spazio digitale esterno, fisico, usato per mostrare contenuti in rotazione.  
Il portale è il sistema tramite cui i contenuti vengono caricati, approvati, pianificati e pubblicati.

Il modello scelto è **Modello B**, cioè:
- servizio principalmente dedicato agli associati
- accesso tramite slot/pacchetti regolati
- quota riservata a contenuti associativi
- quota riservata a comunicazioni istituzionali/pubbliche

---

## 2. Obiettivi prodotto

### Obiettivi principali
- dare visibilità agli associati
- permettere la gestione ordinata delle campagne
- creare un flusso semplice tra upload contenuti e pubblicazione
- garantire controllo e moderazione
- mantenere prevalente il beneficio per i soci
- includere una quota di comunicazione di interesse pubblico

### Obiettivi secondari
- rafforzare l’identità di ComArtPro
- creare storico e tracciabilità dei contenuti
- raccogliere dati di utilizzo
- predisporre il sistema a una futura espansione

---

## 3. Vincoli di prodotto

### Vincolo 1 — Priorità associati
Il sistema deve rendere evidente e applicabile che gli associati sono i beneficiari principali.

### Vincolo 2 — Quota pubblica limitata
Le comunicazioni istituzionali/pubbliche devono avere uno spazio definito, ma non devono occupare la maggioranza della programmazione.

### Vincolo 3 — Moderazione obbligatoria
Nessun contenuto deve andare online senza workflow di revisione, salvo eventuali utenti super-admin espressamente autorizzati.

### Vincolo 4 — Usabilità alta
Il portale deve poter essere usato anche da utenti poco tecnici.

### Vincolo 5 — Tracciabilità
Ogni contenuto e ogni campagna devono avere stato, autore, data creazione, data approvazione, periodo pubblicazione e log minimo.

### Vincolo 6 — Struttura scalabile
Il progetto deve poter partire in forma semplice, ma senza precludere:
- più schermi
- più location
- più playlist
- più ruoli
- più tipologie di contenuto

---

## 4. Logica di business

Il Led Wall non è un semplice player di file multimediali.  
È un sistema di gestione campagne con logica di priorità, categorie e slot.

### Macro-categorie di pubblicazione
1. **Associati**
2. **ComArtPro**
3. **Istituzionale / Pubblica utilità**

### Ripartizione iniziale consigliata
- 70% Associati
- 20% ComArtPro
- 10% Istituzionale / Pubblica utilità

Questa ripartizione non deve essere solo informativa: idealmente il sistema dovrebbe poterla rappresentare e farla rispettare almeno a livello di planning.

---

## 5. Tipologie di contenuto

### Contenuti associati
- promozioni
- offerte
- eventi
- aperture straordinarie
- campagne stagionali
- comunicazioni brand activity

### Contenuti ComArtPro
- campagne dell’associazione
- eventi associativi
- messaggi collettivi
- iniziative territoriali
- promozioni di rete

### Contenuti istituzionali/pubblici
- avvisi comunali
- campagne civiche
- eventi patrocinati
- utilità pubblica
- messaggi informativi territoriali

---

## 6. Ruoli utente

### 1. Associato
Può:
- accedere al portale
- gestire il proprio profilo attività
- creare contenuti
- creare campagne
- inviare richiesta di pubblicazione
- vedere stato e calendario
- consultare storico e regolamento

Non può:
- approvare contenuti
- modificare campagne di altri
- pubblicare direttamente

### 2. Admin ComArtPro
Può:
- vedere tutte le campagne
- approvare/rifiutare contenuti
- programmare pubblicazioni
- modificare priorità
- inserire campagne ComArtPro
- inserire campagne istituzionali
- sospendere o archiviare contenuti
- vedere report globali

### 3. Super Admin
Può:
- configurare impostazioni globali
- gestire ruoli
- gestire schermi/location
- gestire categorie e parametri di rotazione
- intervenire su contenuti e campagne in modo completo

### 4. Operatore istituzionale
Opzionale.  
Per la v1 è preferibile non esporre accesso diretto al Comune.  
I contenuti istituzionali possono essere caricati dagli admin ComArtPro.

---

## 7. Oggetti principali di dominio

Il team dev deve ragionare almeno su queste entità logiche.

### User
Campi logici:
- id
- nome
- cognome o referente
- email / login
- ruolo
- stato attivo
- associazione a socio/attività
- date di creazione e aggiornamento

### Member / Associato
Campi logici:
- id
- ragione sociale / nome attività
- categoria merceologica
- indirizzo
- contatti
- logo
- stato quota associativa
- eventuale visibilità pubblica
- referente principale

### Media Asset
Campi logici:
- id
- owner
- tipo file
- url/file path
- thumbnail
- titolo interno
- descrizione interna
- dimensioni/formato
- stato validazione tecnica
- date

### Campaign
Campi logici:
- id
- owner type (associato / ComArtPro / istituzionale)
- owner id
- titolo
- descrizione
- categoria
- periodo validità
- priorità
- stato workflow
- note admin
- created by
- approved by
- timestamps

### Campaign Asset Link
Per associare uno o più asset a una campagna.

### Schedule / Playlist Entry
Campi logici:
- id
- screen id
- campaign id
- data inizio
- data fine
- fascia oraria
- ordine o peso di rotazione
- stato attivo/inattivo

### Screen
Per futura scalabilità:
- id
- nome schermo
- location
- stato online/offline
- configurazione base

### Audit Log
Minimo log eventi:
- chi ha fatto cosa
- su quale entità
- quando
- azione
- note

---

## 8. Workflow contenuti

### Workflow principale
1. associato accede
2. carica media
3. crea campagna
4. imposta periodo e dati
5. invia per revisione
6. admin verifica
7. admin approva o rifiuta
8. se approvata, viene schedulata
9. il player la mostra nel Led Wall
10. la campagna scade o viene archiviata

### Stati minimi consigliati
Per i media:
- draft
- uploaded
- rejected
- approved

Per le campagne:
- draft
- submitted
- under_review
- approved
- scheduled
- live
- rejected
- expired
- archived

---

## 9. Sezioni funzionali del portale

### A. Dashboard associato
Contenuti:
- riepilogo campagne attive
- contenuti in approvazione
- prossime scadenze
- slot disponibili / utilizzo
- stato del profilo

### B. Profilo attività
Contenuti:
- dati attività
- logo
- contatti
- categoria
- breve descrizione
- stato associativo

### C. Libreria contenuti
Contenuti:
- immagini
- video
- file caricati
- stato approvazione tecnica
- riuso asset esistenti

### D. Gestione campagne
Contenuti:
- lista campagne
- crea nuova campagna
- modifica bozza
- visualizza esiti approvazione
- storico campagne

### E. Calendario pubblicazioni
Contenuti:
- visione per giorno/settimana/mese
- periodi occupati
- campagne attive
- stato programmazione

### F. Regolamento e specifiche
Contenuti:
- contenuti ammessi
- contenuti vietati
- limiti tecnici file
- linee guida grafiche
- principi di assegnazione slot

### G. Area admin
Contenuti:
- coda approvazioni
- gestione campagne globali
- inserimento contenuti istituzionali
- gestione playlist
- report
- gestione utenti

---

## 10. Contenuti da prevedere lato CMS o backend

Il team dev deve considerare che non ci sono solo file media.  
Ci sono anche contenuti gestionali e informativi.

### Contenuti editoriali/statici
- testo landing progetto
- regolamento utilizzo
- FAQ
- specifiche file
- testo consenso e responsabilità
- eventuali messaggi di sistema

### Contenuti dinamici
- campagne
- asset
- notifiche
- esiti approvazione
- calendario
- playlist

---

## 11. Regole funzionali importanti

### Regola 1 — Niente pubblicazione diretta associato
L’associato non va live senza approvazione.

### Regola 2 — Separazione contenuti vs campagne
Un asset non coincide necessariamente con una campagna.  
Una campagna può usare uno o più asset.

### Regola 3 — Periodo di validità obbligatorio
Ogni campagna deve avere una finestra temporale chiara.

### Regola 4 — Categoria obbligatoria
Ogni campagna deve appartenere a una categoria o macro-tipo.

### Regola 5 — Storico non distruttivo
Meglio archiviare che cancellare.  
Serve memoria delle campagne e delle decisioni.

### Regola 6 — Possibile logica futura di quote/crediti
Anche se la v1 può essere semplice, il dominio va pensato in modo che in futuro si possa introdurre:
- slot base inclusi
- pacchetti premium
- crediti di pubblicazione
- campagne speciali

---

## 12. Notifiche utili

Non obbligatorie in v1, ma molto utili.

### Notifiche associato
- contenuto caricato correttamente
- campagna inviata
- campagna approvata
- campagna rifiutata
- campagna in scadenza
- richiesta integrazione/modifica

### Notifiche admin
- nuova campagna da revisionare
- contenuto con problemi
- campagna in partenza
- schermo offline o non sincronizzato

---

## 13. Vista player / pubblicazione

Il team dev deve distinguere chiaramente:
- **backoffice gestionale**
- **engine di programmazione**
- **vista player**

### La vista player deve essere:
- minimale
- robusta
- full-screen
- orientata alla sola riproduzione
- capace di ricevere playlist/configurazione

Per la v1 non serve un sistema complesso di advertising engine.  
Serve una pipeline affidabile:
contenuti approvati -> programmazione -> playlist -> player

---

## 14. KPI e report utili

### Per admin
- numero campagne per periodo
- numero campagne per categoria
- utilizzo per associato
- occupazione slot
- campagne attive/scadute
- contenuti rifiutati
- tempo medio approvazione

### Per associato
- campagne inviate
- campagne approvate
- campagne live
- storico campagne
- utilizzo del servizio

---

## 15. Vincoli UX

### UX associato
- pochi passaggi
- interfaccia leggibile
- pulsanti chiari
- stato visibile sempre
- evitare linguaggio tecnico inutile

### UX admin
- coda revisioni molto chiara
- filtri forti
- editing rapido
- stato campagne immediatamente leggibile
- calendario utile davvero, non solo decorativo

---

## 16. Contenuti e copy da prevedere nel sistema

Il team dev deve sapere che il sistema dovrà ospitare copy come:
- descrizioni campagna
- motivazioni di rifiuto
- note admin
- messaggi regolamentari
- microcopy di stato

### Esempi di stati user-facing
- In bozza
- In revisione
- Approvata
- Programmata
- In pubblicazione
- Respinta
- Scaduta
- Archiviata

### Esempi di motivi di rifiuto
- formato non conforme
- contenuto incompleto
- qualità grafica insufficiente
- contenuto non ammesso da regolamento
- date non corrette

---

## 17. Priorità V1

Se bisogna partire in modo pragmatico, la V1 deve coprire bene queste parti:

1. autenticazione utenti
2. profilo associato
3. upload asset
4. creazione campagna
5. workflow approvazione
6. calendario / scheduling base
7. area admin
8. playlist/player base
9. storico campagne
10. regolamento e contenuti statici

---

## 18. Funzioni da rimandare a fasi successive

Meglio posticipare:
- billing avanzato
- automazioni complesse di allocazione slot
- accesso diretto ente pubblico
- multi-ledwall completo
- analytics sofisticate
- editing grafico interno
- template generator avanzato
- workflow multilivello con più approvatori

---

## 19. Domande di prodotto che il team dovrà tenere aperte

Questioni non bloccanti, ma da tenere presenti:
- gli slot sono basati su tempo, passaggi o peso percentuale?
- gli associati vedono disponibilità reale o fanno solo richiesta?
- esiste un tetto massimo per associato?
- il calendario è vincolante o indicativo?
- le comunicazioni ComArtPro hanno priorità assoluta in certe finestre?
- gli slot pubblici sono programmati manualmente o ricorrenti?
- i video saranno ammessi fin da subito o solo immagini statiche?

---

## 20. Sintesi finale per il team dev

Il prodotto da costruire non è solo:
“un pannello per caricare immagini da mandare su uno schermo”.

È piuttosto:
- un sistema di gestione campagne
- con ruoli
- con governance
- con moderazione
- con pianificazione
- con una distinzione forte tra contenuti associati, contenuti associativi e contenuti istituzionali

Il cuore del progetto è l’equilibrio tra:
- semplicità d’uso
- controllo centrale
- beneficio per gli associati
- funzione pubblica limitata ma reale

Questa logica deve riflettersi sia nel dominio dati sia nella UX del portale.
