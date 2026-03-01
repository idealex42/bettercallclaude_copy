# Guida Introduttiva a BetterCallClaude

Benvenuto su BetterCallClaude - il tuo assistente legale intelligente per il diritto svizzero.

Questa guida ti aiuterà a iniziare in pochi minuti, sia che tu stia usando Claude Desktop o Claude Code CLI.

---

## Cos'è BetterCallClaude?

BetterCallClaude è un framework di intelligenza legale progettato specificamente per gli avvocati svizzeri. Offre:

- **Ricerca Legale Intelligente** - Cerca nelle decisioni del Tribunale federale (DTF) e nelle decisioni cantonali
- **Supporto Multilingue** - Funziona in tedesco, francese, italiano e inglese
- **Tutti i 26 Cantoni Svizzeri** - Accedi alle decisioni dei tribunali cantonali di ogni cantone
- **Analisi delle Citazioni** - Comprende e formatta automaticamente le citazioni legali svizzere
- **Agenti Legali Specializzati** - Assistenti IA esperti in ricerca, redazione, conformità e altro

---

## Installazione (2 minuti)

### Passo 1: Esegui l'installatore

Apri il tuo terminale e incolla questo comando:

```bash
curl -fsSL https://raw.githubusercontent.com/fedec65/BetterCallClaude/main/install.sh | bash
```

Ecco fatto! L'installatore:
- Scaricherà tutti i componenti necessari
- Configurerà i server MCP
- Imposterà automaticamente l'integrazione con Claude Desktop

### Passo 2: Riavvia Claude Desktop

Dopo l'installazione, **riavvia completamente Claude Desktop** (chiudi e riapri) per caricare i nuovi server MCP.

---

## Usare BetterCallClaude

### Opzione A: Con Claude Desktop (Consigliato per principianti)

1. **Apri Claude Desktop**
2. **Inizia una nuova conversazione**
3. **Prova la tua prima richiesta legale:**

   Chiedi semplicemente in linguaggio naturale:
   ```
   Trova le decisioni DTF recenti sulla risoluzione del contratto
   ```

   Oppure usa un comando specifico:
   ```
   /legal-research diritto del lavoro protezione dal licenziamento
   ```

### Opzione B: Con Claude Code CLI

1. **Apri il tuo terminale**
2. **Avvia Claude Code:**
   ```bash
   claude
   ```
3. **Usa i comandi legali:**
   ```
   /legal-help
   ```

---

## I Tuoi Primi Comandi

Ecco alcuni comandi da provare subito:

### Ottenere Aiuto
```
/legal-help
```
Mostra tutti i comandi legali disponibili e le loro descrizioni.

### Ricerca Legale
```
/legal-research [il tuo argomento]
```
Esempio: `/legal-research diritto locativo disdetta`

### Cercare Decisioni del Tribunale Federale
```
/legal-research [citazione o argomento]
```
Esempio: `/legal-research DTF 147 III 226`

### Ricerca Cantonale
```
/legal-cantonal [cantone] [argomento]
```
Esempio: `/legal-cantonal TI controversie locative`

### Strategia del Caso
```
/legal-strategy [descrivi il tuo caso]
```
Ottieni analisi strategiche e raccomandazioni per il tuo caso.

### Redigere Documenti Legali
```
/legal-draft [tipo di documento] [dettagli]
```
Esempio: `/legal-draft lettera di disdetta contratto di lavoro`

---

## Usare gli Agenti Legali

BetterCallClaude include agenti IA specializzati per diverse attività. Chiamali con `@`:

| Agente | Scopo |
|--------|-------|
| `@researcher` | Ricerca legale approfondita e analisi |
| `@strategist` | Strategia del caso e raccomandazioni |
| `@drafter` | Redazione di documenti legali |
| `@compliance` | Verifiche di conformità normativa |
| `@risk` | Valutazione e mitigazione dei rischi |
| `@translator` | Traduzione legale (DE/FR/IT/EN) |

**Esempio:**
```
@researcher Trova tutte le decisioni DTF del 2023 sulla protezione dei dati
```

---

## Cantoni Supportati

BetterCallClaude supporta tutti i 26 cantoni svizzeri:

**Germanofoni:** ZH, BE, LU, UR, SZ, OW, NW, GL, ZG, SO, BS, BL, SH, AR, AI, SG, GR, AG, TG

**Francofoni:** VD, GE, NE, JU

**Italofono:** TI

**Bilingui:** FR (DE/FR), VS (DE/FR)

---

## Modalità Linguistiche

BetterCallClaude si adatta automaticamente alla tua lingua preferita. Scrivi semplicemente le tue richieste in tedesco, francese, italiano o inglese.

Usa `/legal-translate` per le traduzioni giuridiche tra le lingue ufficiali.

---

## Risoluzione dei Problemi

### I server MCP non si caricano?

1. Assicurati di aver riavviato Claude Desktop dopo l'installazione
2. Verifica se i server sono configurati nelle impostazioni di Claude Desktop
3. Esegui nuovamente l'installatore: `curl -fsSL https://raw.githubusercontent.com/fedec65/BetterCallClaude/main/install.sh | bash`

### Comandi non riconosciuti?

1. Prova `/legal-help` per vedere i comandi disponibili
2. Assicurati di usare la sintassi corretta (nota il trattino dopo `legal`)

### Hai bisogno di ulteriore aiuto?

- Consulta la [documentazione completa](https://github.com/fedec65/BetterCallClaude)
- Apri una issue su [GitHub](https://github.com/fedec65/BetterCallClaude/issues)

---

## Scheda di Riferimento Rapido

| Attività | Comando |
|----------|---------|
| Ottenere aiuto | `/legal-help` |
| Ricerca | `/legal-research [argomento]` |
| Ricerca cantonale | `/legal-cantonal [cantone] [argomento]` |
| Strategia del caso | `/legal-strategy [dettagli del caso]` |
| Redigere documento | `/legal-draft [tipo] [dettagli]` |
| Tradurre | `/legal-translate [testo]` |

---

**Pronto per iniziare?** Apri Claude e prova: `/legal-help`

---

*BetterCallClaude v3.1.0 - Framework di Intelligenza Legale per Avvocati Svizzeri*
