# Progetto SQL – Analisi della rappresentanza di genere in diplomazia.

**File principale:** `Francesca_Carrera_SQL_GenDip.pdf` (presentazione completa del progetto).

**Obiettivo del progetto**  
Un’organizzazione no profit che promuove la parità di genere in ogni ambito della società ha fornito il dataset GenDip (Gender and Diplomatic Representation), che copre oltre 200 Paesi e 10 anni tra il 1968 e il 2021, con informazioni su Paese di invio, Paese di destinazione, tipo di titolo diplomatico e genere del diplomatico.  

Questo progetto sviluppa un’analisi articolata in tre focus:
1. **Genere e titoli dei diplomatici** – conteggi per anno, paesi con più diplomatici femminili, distribuzione dei titoli.
2. **Presenza femminile nei parlamenti e politiche estere femministe** – medie percentuali annuali, paesi aderenti a politiche estere femministe.
3. **Approfondimento sull’Italia** – confronto con gli altri paesi e analisi specifica della rappresentanza diplomatica e parlamentare.

**Strumenti SQL utilizzati**  
- SQL Server Express + SQL Server Management Studio (SSMS)
- Creazione database, tabelle, vincoli (PK, FK, UNIQUE), IDENTITY
- CTE (WITH) e funzioni finestra (LAG, DENSE_RANK, ROW_NUMBER)
- JOIN (INNER, LEFT), GROUP BY, HAVING, ORDER BY, WITH ROLLUP
- Sottoquery con INTERSECT, EXCEPT, EXISTS
- VIEW (vw_unifiedCountries)
- Stored procedure (usp_cnameSendID_UPDATE, usp_regionID_UPDATE)
- Funzioni di formattazione e conversione (FORMAT, ROUND, CAST, CONVERT)
- Gestione dei NULL con COALESCE, NULLIF
- Pattern matching con PATINDEX, REPLACE, ASCII, CHAR
- Excel per diagramma ER e grafici

**Struttura della repository**  

La repository `SQL_GenDip` contiene tre cartelle principali e un file nella root.

**[`Dataset_source`](https://github.com/Francesca-Carrera/SQL_GenDip/tree/main/Dataset_source)** – Dati originali e documentazione di supporto:
  - `CodeBook.pdf` – Documento ufficiale del progetto GenDip (Università di Goteborg). Descrive finalità, metodologia, significato di tutte le variabili (genere, titoli, regioni, politiche estere, ecc.).
  - `tab_stagingArea.csv` – Dataset originale importato nel database come tabella di staging (`stagingArea`).
  - `tab_updatedCountries.csv` – Tabella aggiornata dei paesi (codice alpha, nome, regione), scaricata da Wikipedia e usata per normalizzare e correggere i dati dei paesi non più esistenti o con nomi variati.

**[`ETL`](https://github.com/Francesca-Carrera/SQL_GenDip/tree/main/ETL)** – Script SQL per l'intero processo di estrazione, trasformazione e caricamento:
  - `1_tab_stagingArea_workingArea.sql` – Creazione del database, importazione del CSV, normalizzazione di tutte le variabili (tipi, NULL, caratteri non standard, ricodifica di titoli e regioni).
  - `2_tab_regions_countries_updatedCountries.sql` – Creazione delle tabelle `regions`, `countries` e `updatedCountries`. Popolamento delle chiavi esterne in `workingArea` tramite stored procedure con `INTERSECT`. Pulizia manuale dei dati e correzione di entità geografiche.
  - `3_other_tabs.sql` – Creazione delle tabelle dimensionali (`years`, `femaleLegislators`, `titles`) e delle tabelle dei fatti (`sendingCountries`, `receivingCountries`, `diplomats`). Assemblaggio finale nella tabella `targetArea` (fact table). Creazione di tutti i vincoli di integrità referenziale (PK, FK, UNIQUE).
  - `ER_diagram_SSMS.png` – Diagramma Entità-Relazioni del database normalizzato, che illustra la struttura a stella e le relazioni tra le tabelle.

**[`Graphs_and_Analysis`](https://github.com/Francesca-Carrera/SQL_GenDip/tree/main/Graphs_and_Analysis)** – Script SQL e file Excel per l'analisi dei dati:
  - `1_analysis_dipGenre_titles.sql` – Query del primo focus (genere e titoli dei diplomatici). Include la vista `vw_unifiedCountries` per unificare i nomi dei paesi, analisi di conteggio annuale e totale, crescita percentuale, rapporto 2021/1968, paesi con più diplomatici femminili (per anno e in assoluto), distribuzione dei titoli per categoria e per genere.
  - `2_analysis_femLegPct_femForeignPolicy.sql` – Query del secondo focus (parlamentari femminili e politica estera femminista). Analisi della media percentuale annuale di legislatrici nella Camera Bassa, crescita media annua, paesi con la percentuale più alta per anno, TOP 10 paesi in assoluto, media per regione geografica, e paesi aderenti a una politica estera femminista (FFP).
  - `3_analysis_Italy.sql` – Query del terzo focus (approfondimento sull'Italia). Analisi del conteggio diplomatici per genere (totale e annuale), crescita percentuale maggiore dei diplomatici maschili (63,81% nel 2014), media percentuale annuale di legislatrici (da 2,70% a 35,71%), crescita media annua (3,67%) e classifica dell'Italia su 203 paesi (57° posto con 21,60%).
  - `Graphs.xlsx` – File Excel con i dati aggregati e le tabelle per la creazione dei grafici, suddiviso nei tre focus dell'analisi.

**Nella root della repository si trova inoltre:**
- `Francesca_Carrera_SQL_GenDip.pdf` – Presentazione completa del progetto (72 pagine), che illustra l'intero processo ETL e le analisi con grafici.

**📌 Feedback del tutor (Start2Impact)**  

> *"Hai fatto davvero un ottimo lavoro. Si vede che ci hai messo un grande impegno e dedizione.*  
> *Il codice SQL è corretto e ben formattato. Hai una grande padronanza del linguaggio.*  
> *Hai fatto un grosso lavoro di data modeling, dimostrando una solida comprensione delle tecniche di modellazione dei dati. Il diagramma ER è completo e ben fatto.*  
> *L'analisi si apprezza per l'uso di tecniche avanzate come le specializzazioni e le viste.*  
> *Hai svolto un lavoro eccellente che dimostra la tua abilità nel data modeling e nell'analisi dei dati con SQL."*  

> — *Francesco Cursale, Lead Data Engineer in Accenture – Start2Impact*
