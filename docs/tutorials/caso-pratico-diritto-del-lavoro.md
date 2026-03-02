# Caso pratico di diritto del lavoro: disdetta abusiva

Il presente tutorial guida passo dopo passo attraverso un caso giuridico completo con BetterCallClaude -- dalla presa in carico iniziale del mandato fino alla consegna dei documenti finali. Illustra come i commands, gli agents, gli skills e i MCP servers del framework collaborano su un caso realistico di diritto svizzero del lavoro.

**Destinatari**: avvocate e avvocati svizzeri che intendono valutare lo strumento, utenti di Claude Code che esplorano il framework di plugin, o chiunque desideri comprendere come BetterCallClaude gestisce un caso giuridico reale dall'inizio alla fine.

**Tempo di lettura**: 20-30 minuti

**Prerequisiti**: BetterCallClaude installato e funzionante. Consultare [docs/INSTALL.md](../INSTALL.md) per le istruzioni di installazione.

---

## Il caso

> **Maria Keller** (34 anni, ingegnera informatica) contro **AlphaFin SA** (startup fintech, Lugano)
>
> Maria era impiegata presso AlphaFin SA da 2,5 anni in qualità di lead backend engineer. Durante una migrazione di sistema, ha scoperto che il modulo di elaborazione dei pagamenti non disponeva del monitoraggio delle transazioni prescritto dalla FINMA. Il 3 marzo 2026, ha segnalato la lacuna di conformità per e-mail al suo superiore diretto e al CTO.
>
> Il 10 marzo, la direzione ha prorogato retroattivamente il suo periodo di prova -- provvedimento nullo in virtù dell'art. 335b CO, poiché il periodo di prova iniziale era scaduto da tempo. Il 24 marzo, ha ricevuto la disdetta con effetto al 30 aprile, con un termine di disdetta di un mese (come se fosse ancora in periodo di prova).
>
> Maria ritiene che la disdetta sia abusiva (disdetta abusiva, art. 336 lett. d CO) e intende reclamare un'indennità fino a 6 mesi di salario (art. 336a CO). Il suo salario mensile lordo ammonta a CHF 12'500.

**Questioni giuridiche in causa**:
1. Validità della proroga retroattiva del periodo di prova (art. 335b CO)
2. Disdetta abusiva dopo segnalazione di conformità (art. 336 lett. d CO)
3. Termine di disdetta -- ordinario vs. periodo di prova (art. 335a/335c CO)
4. Periodo di protezione (termine di protezione) (art. 336c CO)
5. Calcolo dell'indennità e massimale (art. 336a cpv. 2 CO -- max 6 mesi)
6. Competenza e procedura (Pretura di Lugano, CPC)

---

## Tappa 1: presa in carico del mandato con sessione di briefing

Si inizia con una sessione di briefing completa affinché BetterCallClaude comprenda il caso prima che gli agents entrino in azione.

```
/bettercallclaude:briefing --depth comprehensive

Maria Keller, ingegnera informatica, è stata licenziata da AlphaFin SA (Lugano)
dopo aver segnalato lacune di conformità FINMA. Il periodo di prova è stato prorogato
retroattivamente e la disdetta è stata data con un termine di un mese. Invoca una
disdetta abusiva ai sensi dell'art. 336 CO.
Salario mensile CHF 12'500, durata del rapporto di lavoro 2,5 anni.
```

**Cosa succede**: il command di briefing attiva l'agent **swiss-legal-briefing-coordinator**. Quest'ultimo classifica la richiesta come altamente complessa (diritto del lavoro + conformità regolamentare + disdetta ritorsiva), compone un panel di specialisti e pone domande strutturate di intake.

**Agents attivati**: briefing-coordinator, che costituisce un panel tra: researcher, strategist, procedure-specialist, risk-analyst, compliance-officer

Il coordinatore di briefing pone domande mirate -- ciascuna indicando quale specialista necessita della risposta:

> **Domande di intake**
>
> 1. Maria dispone di una documentazione scritta della sua segnalazione di conformità (e-mail, ticket interno)? *(Richiesto da: Researcher -- valutazione delle prove; Advocate -- prova documentale)*
>
> 2. Esistevano problemi di rendimento documentati prima della segnalazione di conformità? *(Richiesto da: Adversary -- analisi della difesa del datore di lavoro; Risk -- calcolo dell'esposizione)*
>
> 3. AlphaFin SA ha rispettato le proprie procedure HR interne per la disdetta? *(Richiesto da: Procedure -- validità formale; Strategist -- mezzi di impugnazione procedurali)*
>
> 4. Maria ha già inviato un'opposizione scritta alla disdetta? *(Richiesto da: Procedure -- monitoraggio dei termini; Strategist -- salvaguardia delle pretese)*
>
> 5. AlphaFin SA è un istituto assoggettato alla FINMA oppure opera sotto licenza fintech? *(Richiesto da: Compliance -- contesto regolamentare; Researcher -- norme FINMA applicabili)*

Dopo aver risposto a queste domande, il sistema elabora un piano di esecuzione:

> **Piano di esecuzione**
>
> | Tappa | Agent | Compito | Punto di controllo |
> |-------|-------|---------|-------------------|
> | 1 | Researcher | Ricerca di precedenti su art. 335b/336/336a CO, contesto segnalazione FINMA | Esame dei risultati |
> | 2 | Advocate + Adversary + Judicial Analyst | Analisi contraddittoria tripartita | Esame della valutazione |
> | 3 | Strategist + Risk Analyst + Procedure Specialist | Strategia processuale con analisi costi-benefici | Esame della strategia |
> | 4 | Drafter | Petizione per la Pretura di Lugano | Esame del progetto |
> | 5 | Summarizer | Consolidamento dell'insieme dei risultati | Controllo finale |
>
> **Durata stimata del pipeline**: 15-25 minuti con punti di controllo

**Funzionalità chiave**: BetterCallClaude rileva automaticamente la giurisdizione (Lugano/Ticino) dai fatti e indirizza verso il diritto federale del lavoro (CO) nonché la procedura cantonale ticinese. Non è necessaria alcuna specificazione manuale.

---

## Tappa 2: ricerca giuridica -- trovare i precedenti

Completato il briefing, si lancia la ricerca giuridica mirata sulle questioni centrali.

```
/bettercallclaude:research "disdetta abusiva dopo segnalazione interna di non conformità, art. 336 lett. d CO, termine di protezione art. 336c CO, proroga retroattiva del periodo di prova art. 335b CO"
```

**Agents attivati**: swiss-legal-researcher

**MCP servers utilizzati**: bge-search (sentenze del Tribunale federale), entscheidsuche (ricerca intergiurisdizionale), fedlex-sparql (testi legislativi)

Il researcher esegue un workflow strutturato in sei tappe: analizzare la domanda, cercare nelle DTF, cercare la giurisprudenza cantonale, valutare le fonti, identificare la dottrina e compilare un memorandum.

> **Risultati della ricerca -- Disdetta abusiva (art. 336 CO)**
>
> **Quadro normativo**
> - Art. 336 lett. d CO: la disdetta è abusiva se data perché il lavoratore fa valere in buona fede pretese derivanti dal rapporto di lavoro.
> - Art. 336a cpv. 2 CO: indennità in caso di disdetta abusiva -- max 6 mesi di salario.
> - Art. 335b CO: il periodo di prova dev'essere convenuto all'inizio del rapporto di lavoro; una proroga richiede un accordo scritto prima della scadenza del periodo iniziale.
>
> **Precedenti di principio**
>
> 1. **DTF 136 III 513** -- Elementi della disdetta ritorsiva
>    - Massima: la prossimità temporale tra l'attività protetta e la disdetta fonda una presunzione di motivo ritorsivo. L'onere della prova si inverte: spetta al datore di lavoro dimostrare un motivo legittimo e indipendente.
>    - Pertinenza: l'intervallo di 21 giorni tra la segnalazione di Maria e la disdetta avvalora fortemente l'allegazione di ritorsione.
>
> 2. **DTF 132 III 115** -- Metodologia di calcolo dell'indennità
>    - Massima: il giudice dispone di un potere di apprezzamento nella forchetta da 0 a 6 mesi. Criteri: gravità della colpa, età e anzianità della lavoratrice, impatto economico, comportamento del datore di lavoro.
>    - Pertinenza: 2,5 anni di anzianità, schema ritorsivo manifesto -> verosimilmente 3-4 mesi.
>
> 3. **DTF 134 III 108** -- Onere della prova in caso di disdetta abusiva
>    - Massima: la lavoratrice deve provare il nesso causale tra l'attività protetta e la disdetta. La prova per indizi (prossimità temporale, assenza di altri motivi) è sufficiente.
>    - Pertinenza: documentazione per e-mail della segnalazione di conformità + assenza di problemi di rendimento anteriori = solido fascio di indizi.
>
> 4. **DTF 125 III 70** -- Regime del periodo di prova
>    - Massima: le proroghe del periodo di prova devono essere convenute prima della scadenza del periodo iniziale. Le proroghe retroattive sono nulle.
>    - Pertinenza: invalida direttamente la proroga retroattiva del periodo di prova da parte di AlphaFin SA.
>
> *Fonti verificate tramite i MCP servers bge-search e legal-citations.*

Ogni citazione può essere verificata singolarmente con il command cite:

```
/bettercallclaude:cite DTF 136 III 513
```

> **Verifica della citazione**
>
> **DTF 136 III 513** -- Verificata
> - IT: DTF 136 III 513
> - DE: BGE 136 III 513
> - FR: ATF 136 III 513
> - Oggetto: Disdetta abusiva / Missbräuchliche Kündigung
> - Data: 2010
> - Passaggio chiave: consid. 2.5 (analisi della prossimità temporale)

**Funzionalità chiave**: la medesima ricerca funziona in tutte le lingue. Una ricerca in tedesco (`/bettercallclaude:research "missbräuchliche Kündigung nach interner Compliance-Meldung, OR 336 lit. d"`) restituisce risultati con riferimenti BGE e terminologia giuridica tedesca.

---

## Tappa 3: analisi contraddittoria -- mettere il caso alla prova

Si tratta ora di sottoporre la posizione giuridica a un test di resistenza. Il command adversarial esegue tre agents indipendenti in sequenza -- un advocate, un adversary e un judicial analyst -- al fine di produrre una valutazione equilibrata.

```
/bettercallclaude:adversarial Maria Keller è stata licenziata in modo abusivo da AlphaFin SA dopo aver segnalato lacune di conformità FINMA. 2,5 anni di impiego, segnalazione dell'assenza di monitoraggio delle transazioni il 3 marzo, proroga retroattiva del periodo di prova il 10 marzo, disdetta il 24 marzo con termine di un mese. Pretese fondate sull'art. 336 lett. d CO, indennità fino a 6 mesi di salario.
```

**Agents attivati**: swiss-legal-advocate, swiss-legal-adversary, swiss-judicial-analyst

I tre agents producono analisi indipendenti. Di seguito una versione condensata di ciascuna:

### Posizione dell'Advocate

> **Valutazione dell'Advocate -- Caso Maria Keller**
>
> **Argomenti più solidi**:
> 1. *Prossimità temporale*: solo 21 giorni tra la segnalazione di conformità (3 marzo) e la disdetta (24 marzo). Secondo la DTF 136 III 513, ciò fonda una forte presunzione di motivo ritorsivo.
> 2. *Proroga fittizia del periodo di prova*: la proroga retroattiva del 10 marzo è nulla secondo la DTF 125 III 70. Costituisce un ulteriore indizio della malafede del datore di lavoro -- questi aveva bisogno di un pretesto per abbreviare il termine di disdetta.
> 3. *Prove documentali*: la e-mail che segnala le lacune FINMA reca data e ora certe e costituisce una traccia inequivocabile dell'attività protetta.
> 4. *Assenza di problemi di rendimento anteriori*: l'assenza di critiche documentate sulle prestazioni prima del 3 marzo compromette qualsiasi spiegazione alternativa della disdetta.
> 5. *Contesto della segnalazione FINMA*: la lacuna di conformità rappresentava un rischio regolamentare reale (assenza di monitoraggio delle transazioni). Non si trattava di una denuncia temeraria, bensì dell'esercizio legittimo di diritti derivanti dal rapporto di lavoro, esercitato in buona fede.
>
> **Pretesa raccomandata**: 4-6 mesi di salario (CHF 50'000-75'000) fondati sull'art. 336a CO, più il differenziale risultante dal termine di disdetta corretto.

### Posizione dell'Adversary

> **Valutazione dell'Adversary -- Mezzi di difesa di AlphaFin SA**
>
> **Argomenti potenziali del datore di lavoro**:
> 1. *Ristrutturazione operativa*: AlphaFin SA potrebbe sostenere che la disdetta si iscriveva in una riorganizzazione pianificata nell'ambito della migrazione di sistema, senza legame con la segnalazione di conformità.
> 2. *Problemi di rendimento*: anche in assenza di documentazione formale, il datore di lavoro potrebbe produrre testimonianze su un'insoddisfazione preesistente nei confronti del lavoro di Maria.
> 3. *Segnalazione di conformità non effettuata "in buona fede"*: il datore di lavoro potrebbe argomentare che la segnalazione di Maria era motivata da un conflitto personale piuttosto che da una genuina preoccupazione di conformità -- indebolendo la protezione dell'art. 336 lett. d CO.
> 4. *Proroga del periodo di prova come errore amministrativo*: AlphaFin SA potrebbe presentare la proroga retroattiva come un errore delle risorse umane piuttosto che come un atto deliberato di ritorsione, al fine di ridurne il valore probatorio.
>
> **Punti deboli della posizione del datore di lavoro**:
> - La cronologia di 21 giorni è estremamente difficile da giustificare.
> - La proroga nulla del periodo di prova pregiudica attivamente la credibilità del datore di lavoro.
> - L'assenza di problemi di rendimento documentati prima del 3 marzo costituisce una lacuna significativa.

### Sintesi del Judicial Analyst

> **Valutazione giudiziaria -- Analisi equilibrata**
>
> **Considerando**
>
> *Consid. 1 -- Disdetta abusiva (art. 336 lett. d CO)*
> La prossimità temporale (21 giorni) tra la segnalazione di conformità e la disdetta, combinata con l'assenza di problemi di rendimento documentati, fonda un solido caso prima facie di disdetta ritorsiva. Spetta al datore di lavoro provare un motivo legittimo e indipendente (DTF 134 III 108). La proroga fittizia del periodo di prova compromette ulteriormente la credibilità del datore di lavoro.
> **Probabilità di constatazione di disdetta abusiva: 70-80%**
>
> *Consid. 2 -- Validità della proroga del periodo di prova (art. 335b CO)*
> La proroga retroattiva è nulla di pieno diritto (DTF 125 III 70). Il periodo di prova iniziale era scaduto da circa due anni prima del tentativo di proroga. La questione non si presta a discussione.
> **Probabilità: >95% che la proroga sia nulla**
>
> *Consid. 3 -- Termine di disdetta*
> Essendo la proroga del periodo di prova nulla, il termine di disdetta applicabile è di due mesi (art. 335c cpv. 1 CO, secondo anno di servizio). Il termine di un mese è insufficiente. Il rapporto di lavoro prosegue fino alla prossima data di scioglimento valida.
> **Probabilità: >90% che il termine di disdetta fosse errato**
>
> *Consid. 4 -- Indennità (art. 336a CO)*
> Tenuto conto della solidità delle prove di ritorsione e del comportamento contrario alla buona fede del datore di lavoro (proroga fittizia del periodo di prova), il giudice accorderebbe verosimilmente 3-4 mesi di salario. Il massimale di 6 mesi è riservato ai casi più gravi.
> **Forchetta d'indennità attesa: CHF 37'500-50'000 (3-4 mesi)**
>
> *Valutazione complessiva*: **esito favorevole probabile (65-75%)**. Transazione raccomandata nella forchetta di CHF 40'000-55'000.

**Funzionalità chiave**: il Judicial Analyst impiega la struttura svizzera dei considerando -- lo stesso formato utilizzato dai tribunali svizzeri nelle loro motivazioni. Ogni questione giuridica è oggetto di un considerando numerato corredato di una stima di probabilità.

---

## Tappa 4: elaborazione della strategia

Concluse la ricerca e l'analisi contraddittoria, si elabora la strategia processuale.

```
/bettercallclaude:strategy Azione per disdetta abusiva, Maria Keller c. AlphaFin SA, Lugano. Art. 336 lett. d CO, ritorsione dopo segnalazione di non conformità FINMA. Proroga nulla del periodo di prova (art. 335b CO). Indennità reclamata fino a 6 mesi di salario. Salario mensile CHF 12'500.
```

**Agents attivati**: swiss-case-strategist, risk-analyst, procedure-specialist

> **Strategia processuale -- Keller c. AlphaFin SA**
>
> **1. Tabella di marcia procedurale**
>
> | Fase | Foro | Termine | Durata stimata |
> |------|------|---------|----------------|
> | Opposizione scritta | Al datore di lavoro | Prima della scadenza del termine di disdetta | Immediato |
> | Conciliazione | Autorità di conciliazione, Lugano | Entro 180 giorni dalla fine del rapporto di lavoro | 1-3 mesi |
> | In caso di insuccesso -> petizione | Pretura di Lugano | 30 giorni dopo il rilascio dell'autorizzazione ad agire | 6-12 mesi |
>
> Nota: le controversie di diritto del lavoro fino a CHF 30'000 sono esenti da spese giudiziarie (art. 114 lett. c CPC). La pretesa di Maria (fino a CHF 75'000) supera questa soglia; si applicano pertanto le spese giudiziarie ordinarie.
>
> **2. Stima dei costi**
>
> | Voce | Stima |
> |------|-------|
> | Spese giudiziarie | CHF 3'000-6'000 |
> | Rappresentanza legale | CHF 8'000-15'000 |
> | Spese peritali (se necessarie) | CHF 2'000-5'000 |
> | **Totale delle spese processuali** | **CHF 13'000-26'000** |
>
> **3. Analisi transattiva**
>
> - **Miglior scenario** (indennità integrale): CHF 75'000 (6 mesi) + risarcimento per il differenziale del termine di disdetta
> - **Esito atteso**: CHF 37'500-50'000 (3-4 mesi) + correzione del termine di disdetta
> - **Obiettivo transattivo**: CHF 45'000-55'000 (incluso il differenziale del termine di disdetta)
> - **Soglia minima accettabile**: CHF 30'000
>
> Il rapporto costi-benefici depone a favore di una transazione: spese processuali stimate a ~CHF 20'000 a fronte di un ricavo atteso di ~CHF 45'000, con un utile netto di ~CHF 25'000. Una transazione stragiudiziale a CHF 45'000 preserva il medesimo risultato netto senza il rischio del processo.
>
> **4. Matrice dei rischi**
>
> | Rischio | Probabilità | Impatto | Misura di attenuazione |
> |---------|-------------|---------|------------------------|
> | Il datore di lavoro fa valere problemi di rendimento preesistenti | 30% | Elevato | Esigere la produzione del fascicolo HR completo in fase istruttoria |
> | Il giudice accorda meno di 3 mesi | 25% | Medio | Sottolineare la proroga fittizia come fattore aggravante |
> | AlphaFin SA deposita una domanda riconvenzionale (p. es. violazione NDA) | 10% | Medio | Esaminare il contratto di lavoro quanto alla portata della clausola NDA |
> | Procedura prolungata (>18 mesi) | 20% | Medio-basso | Insistere su un accordo in fase di conciliazione |
>
> **5. Approccio raccomandato**
>
> Inviare immediatamente un'opposizione scritta formale alla disdetta. Depositare un'istanza di conciliazione presso l'Autorità di conciliazione di Lugano con una domanda iniziale di CHF 65'000 (ancoraggio alto). Obiettivo transattivo di CHF 45'000-55'000. Procedere dinanzi alla Pretura solo se la conciliazione fallisce e la mandante accetta il rischio legato ai costi e ai tempi.

**Funzionalità chiave**: BetterCallClaude calcola l'esposizione finanziaria sulla base delle tariffe giudiziarie effettive del Ticino e traccia l'intera via procedurale specifica del cantone.

---

## Tappa 5: redazione dell'atto giudiziario

Generazione di una petizione professionale per la Pretura di Lugano.

```
/bettercallclaude:draft Petizione per disdetta abusiva, Maria Keller c. AlphaFin SA, Pretura di Lugano. Conclusioni: (1) accertamento del carattere abusivo della disdetta ai sensi dell'art. 336 lett. d CO, (2) indennità di CHF 75'000 fondata sull'art. 336a CO, (3) correzione del termine di disdetta. Fondata su ritorsione dopo segnalazione di non conformità FINMA, proroga nulla del periodo di prova.
```

**Agents attivati**: swiss-legal-drafter

Il drafter produce un atto giudiziario formale conforme alle esigenze del CPC. Di seguito un estratto -- le conclusioni e l'inizio dell'esposizione dei fatti:

> **Petizione**
> **alla Pretura di Lugano**
>
> ---
>
> **Attrice**: Maria Keller, [indirizzo], rappresentata dall'avv. [Nome Cognome]
>
> **Convenuta**: AlphaFin SA, [indirizzo], [numero RC]
>
> ---
>
> ## I. Conclusioni
>
> Piaccia alla Pretura:
>
> 1. Accertare che la disdetta intimata dalla convenuta il 24 marzo 2026 è abusiva ai sensi dell'art. 336 cpv. 1 lett. d CO.
>
> 2. Condannare la convenuta a versare all'attrice un'indennità di CHF 75'000.00 (sei mensilità di salario) in applicazione dell'art. 336a cpv. 2 CO, oltre interessi al 5% dal 1. maggio 2026.
>
> 3. Accertare che la proroga retroattiva del periodo di prova del 10 marzo 2026 è nulla e che si applica il termine di disdetta ordinario di due mesi previsto dall'art. 335c cpv. 1 CO.
>
> 4. Il tutto con protesta di spese e ripetibili a carico della convenuta.
>
> ---
>
> ## II. Fatti
>
> 1. L'attrice era impiegata dal [data] in qualità di lead backend engineer presso la convenuta a Lugano. Il suo salario mensile lordo ammontava a CHF 12'500.00.
>
> 2. Il 3 marzo 2026, l'attrice ha segnalato per e-mail al suo superiore diretto e al CTO della convenuta che il modulo di elaborazione dei pagamenti non implementava il monitoraggio delle transazioni prescritto dalla FINMA.
>
> 3. Il 10 marzo 2026 -- sette giorni dopo la segnalazione di conformità -- la convenuta ha prorogato retroattivamente il periodo di prova dell'attrice. Tale proroga è nulla (DTF 125 III 70), poiché il periodo di prova iniziale era scaduto da oltre due anni.
>
> 4. Il 24 marzo 2026 -- 21 giorni dopo la segnalazione di conformità -- la convenuta ha disdetto il rapporto di lavoro per il 30 aprile 2026, con un termine di un mese.
>
> [...]
>
> ---
>
> *Il presente documento è stato redatto con il supporto di BetterCallClaude. L'intero contenuto deve essere rivisto e approvato dall'avvocata o dall'avvocato incaricato del caso.*

**Funzionalità chiave**: il drafter struttura il documento conformemente alle esigenze del CPC -- conclusioni, fatti, motivazione giuridica, mezzi di prova -- con un formato di citazione corretto e un registro formale. I campi da completare (indirizzi, date) sono contrassegnati per l'avvocata o l'avvocato.

---

## Tappa 6: consolidamento finale

Completate tutte le tappe del pipeline, i risultati vengono consolidati in un riassunto esecutivo.

```
/bettercallclaude:summarize --medium
```

**Agents attivati**: swiss-legal-summarizer

> **Riassunto del caso -- Keller c. AlphaFin SA**
>
> **Oggetto**: disdetta abusiva (art. 336 lett. d CO)
> **Giurisdizione**: Pretura di Lugano
> **Valutazione**: esito favorevole probabile (65-75%)
>
> **Constatazioni principali**:
> - L'intervallo di 21 giorni tra la segnalazione di conformità e la disdetta fonda una forte presunzione di ritorsione (DTF 136 III 513).
> - La proroga retroattiva del periodo di prova è nulla di pieno diritto (DTF 125 III 70).
> - Indennità attesa: 3-4 mesi di salario (CHF 37'500-50'000).
> - Obiettivo transattivo: CHF 45'000-55'000.
>
> **Azioni raccomandate**:
> 1. Inviare immediatamente un'opposizione scritta ad AlphaFin SA.
> 2. Depositare un'istanza di conciliazione presso l'Autorità di conciliazione di Lugano.
> 3. Avviare trattative transattive con una domanda di ancoraggio di CHF 65'000.
> 4. Preparare una petizione per la Pretura di Lugano quale soluzione sussidiaria.
>
> **Documenti prodotti**:
> - Memorandum di ricerca con 4 sentenze di principio DTF
> - Analisi contraddittoria con valutazione delle probabilità
> - Strategia processuale con analisi costi-benefici
> - Progetto di petizione (Pretura di Lugano)
>
> *Avvertenza professionale: la presente analisi è stata prodotta da uno strumento di ricerca giuridica fondato sull'intelligenza artificiale. L'insieme dei risultati deve essere rivisto e convalidato da un'avvocata o un avvocato qualificato/a iscritto/a a un albo svizzero prima di qualsiasi utilizzo. Il presente documento non costituisce un parere giuridico.*

**Funzionalità chiave**: il summarizer elimina i duplicati fra avvertenze, definizioni terminologiche e citazioni che compaiono nei risultati di più agents, producendo un riassunto consolidato e ordinato.

---

## Consigli e prossimi passi

### Personalizzare il workflow

Non è necessario eseguire ogni tappa. Alcune varianti:

- **Solo ricerca**: eseguire unicamente `/bettercallclaude:research` per ottenere i precedenti senza l'intero pipeline.
- **Passare direttamente alla strategia**: se le questioni giuridiche sono già note, passare direttamente a `/bettercallclaude:strategy`.
- **Analisi contraddittoria rapida**: utilizzare `/bettercallclaude:adversarial --short` per una valutazione condensata.
- **Pipeline automatizzato**: `/bettercallclaude:workflow litigation-prep` esegue l'intero pipeline (Researcher -> Strategist -> Adversarial -> Drafter) con trasferimenti automatici.

### Variazioni cantonali

Il medesimo caso a Zurigo implicherebbe regole procedurali diverse:

```
/bettercallclaude:strategy Missbräuchliche Kündigung, Maria Keller gegen AlphaFin AG, Zürich.
Art. 336 Abs. 1 lit. d OR, Vergeltung nach FINMA-Compliance-Meldung.
```

BetterCallClaude passa automaticamente alla terminologia giuridica tedesca (OR anziché CO, BGE anziché DTF) e indirizza verso la procedura cantonale zurighese (Arbeitsgericht) in modo automatico.

### Analisi multilingue

Ogni command può essere eseguito in italiano, in tedesco o in francese:

```
# Italiano (standard per il Ticino)
/bettercallclaude:research "Disdetta abusiva art. 336 CO"

# Tedesco
/bettercallclaude:research "Missbräuchliche Kündigung OR 336"

# Francese
/bettercallclaude:research "Licenciement abusif art. 336 CO"
```

### Documentazione complementare

- [Riferimento dei commands](../command-reference.md) -- Documentazione completa dei 18 commands
- [Workflow di ricerca giuridica](../workflows/research-precedents.md) -- Approfondimento della metodologia di ricerca DTF
- [Workflow di strategia del caso](../workflows/case-strategy.md) -- Guida dettagliata all'elaborazione della strategia
- [Guida all'installazione](../INSTALL.md) -- Installazione, configurazione dei MCP servers, risoluzione dei problemi

---

## Funzionalità di BCC dimostrate in questo tutorial

| Funzionalità | Tappa | Componenti utilizzati |
|--------------|-------|----------------------|
| Sessione di briefing | Tappa 1 | agent briefing-coordinator, skill di rilevamento della giurisdizione |
| Ricerca giuridica | Tappa 2 | agent researcher, MCP bge-search, MCP entscheidsuche, MCP fedlex-sparql |
| Verifica della citazione | Tappa 2 | agent citation-specialist, MCP legal-citations |
| Analisi contraddittoria | Tappa 3 | agents advocate + adversary + judicial-analyst, skill adversarial-analysis |
| Strategia processuale | Tappa 4 | agents case-strategist + risk-analyst + procedure-specialist, skill strategy |
| Redazione dell'atto giudiziario | Tappa 5 | agent drafter, skill swiss-legal-drafting |
| Consolidamento dei risultati | Tappa 6 | agent summarizer |
| Supporto multilingue | Tutto il percorso | rilevamento della lingua, terminologia giuridica DE/FR/IT/EN |
| Instradamento giurisdizionale | Tutto il percorso | skill swiss-jurisdictions, rilevamento cantonale |

**Totale**: 10 agents, 5 commands, 4 skills, 4 MCP servers dimostrati in un unico caso.
