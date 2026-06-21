# PROCEDIMENTO STAMPA CARTELLINI

## 1) Writer → Strumenti > Etichette

Qui definisci il formato fisico del singolo cartellino e il layout sul foglio A4 (quante colonne/righe, margini, spaziatura tra etichette). Cose da sapere:

- Se le dimensioni del tuo cartellino non corrispondono a un formato predefinito in elenco (es. Avery), scegli **"Formato definito dall'utente"** e inserisci manualmente larghezza/altezza/margini — qui tornano utili le stesse misure in mm che userai/hai usato nel prototipo HTML, così i due approcci restano coerenti.
- Quando premi "Nuovo documento" da quella finestra, Writer crea un documento con una **tabella** che replica la griglia di etichette: ogni cella è un'etichetta. Solo la **prima cella** va popolata con i campi unione + layout grafico — Writer poi la propaga automaticamente alle altre durante la stampa unione (questo è un comportamento un po' "magico" di LibreOffice, non preoccuparti se le altre celle sembrano vuote nel template).
- Lo sfondo del cartellino vuoto: lo inserisci come immagine nella prima cella, ancorata "come carattere" o con posizione fissa — qui LibreOffice è più indulgente del browser, non c'è il problema delle impostazioni di stampa che possono escludere gli sfondi.

## 2) Calc → file ODS sorgente dati

Una riga per cartellino, una colonna per campo. Per il tuo caso, qualcosa come:

| nome_originale | nome_corrente | commestibilita |
|---|---|---|
| Boletus luridus Schaeff. 1774 | Suillellus luridus (Schaeff.) Murrill 1909 | NON COMMESTIBILE |

Suggerimento: se in futuro vuoi automatizzare l'export dal DB della PWA, fai in modo che le intestazioni colonna del CSV esportato combacino già con questi nomi campo, così risparmi un passaggio manuale ogni volta.

## 3) Stampa unione da Writer

- **File > Stampa unione...** (in versioni più recenti potrebbe essere sotto **Strumenti > Stampa guidata in serie** o un wizard simile, dipende dalla versione di LibreOffice).
- Devi prima **registrare la sorgente dati**: Writer non legge l'ODS al volo, va registrato come "Database" in **Strumenti > Sorgenti dati** (o LibreOffice te lo chiede automaticamente al primo inserimento campo). Un foglio Calc registrato come sorgente dati appare come una "tabella" navigabile.
- Inserisci i **campi unione** (Inserisci > Campo > Altri campi > scheda Database) nella cella-modello, posizionandoli sopra/vicino allo sfondo come hai fatto nell'HTML.
- A questo punto la stampa unione genera un documento con tante pagine quante servono, replicando la griglia di etichette e popolandola riga per riga dal foglio Calc.

## Un paio di attenzioni pratiche

- **Punto critico**: se cambi il file ODS *dopo* aver registrato la sorgente dati, a volte Writer non rilegge automaticamente le modifiche — conviene chiudere/riaprire la sorgente dati (o il documento) prima di una nuova stampa unione, altrimenti rischi di stampare dati vecchi.
- **Caratteri speciali**: i nomi scientifici hanno spesso caratteri come `×` (ibridi) o apici d'autore con caratteri accentati — verifica che l'encoding del CSV/ODS non li corrompa, specie se in futuro lo generi via script Node.js (UTF-8 esplicito).
- **Corsivo selettivo nel nome scientifico**: un limite della stampa unione "base" è che il campo è tutto un blocco di testo con uno stile uniforme — se nel nome hai parti che vuoi *non* corsive (es. l'autore "Schaeff. 1774" spesso si scrive non in corsivo, solo il binomio lo è), la stampa unione semplice non lo gestisce bene a meno di spezzare in due campi separati (es. `nome_binomio` corsivo + `autore_anno` non corsivo, come due campi/colonne distinti nel Calc).
