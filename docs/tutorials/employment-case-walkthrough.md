# Employment Case Walkthrough: Wrongful Termination (Missbräuchliche Kündigung)

This tutorial walks through a complete legal matter using BetterCallClaude — from initial case intake to final document delivery. You will see how the framework's commands, agents, skills, and MCP servers work together on a realistic Swiss employment law case.

**Who this is for**: Swiss lawyers evaluating the tool, Claude Code users exploring the plugin framework, or anyone who wants to understand how BetterCallClaude handles a real-world legal matter end to end.

**Time to read**: 20-30 minutes

**Prerequisites**: BetterCallClaude installed and working. See [docs/INSTALL.md](../INSTALL.md) for setup instructions.

---

## The Case

> **Maria Keller** (34, software engineer) vs. **AlphaFin AG** (fintech startup, Zürich)
>
> Maria worked at AlphaFin AG for 2.5 years as lead backend engineer. During a system migration, she discovered the payment processing module lacked required FINMA transaction monitoring. She reported the compliance gap to her direct manager and the CTO via email on March 3, 2026.
>
> On March 10, management extended her probation period retroactively — invalid under OR 335b, since the original probation ended long ago. On March 24, she received termination notice effective April 30, citing a one-month notice period (as if she were still in probation).
>
> Maria believes the termination is retaliatory (missbräuchliche Kündigung, OR 336 lit. d) and wants to claim indemnity (Entschädigung) of up to 6 months' salary (OR 336a). Her monthly salary is CHF 12,500.

**Legal issues at stake**:
1. Validity of retroactive probation extension (OR 335b)
2. Retaliatory dismissal after compliance report (OR 336 lit. d)
3. Notice period — regular vs. probation (OR 335a/335c)
4. Protection period (Sperrfrist) applicability (OR 336c)
5. Indemnity calculation and ceiling (OR 336a II — max 6 months)
6. Jurisdiction and procedure (Zürich Arbeitsgericht, ZPO)

---

## Step 1: Case Intake with Briefing Session

Start by running a full briefing session to let BetterCallClaude understand the matter before any agents begin working.

```
/bettercallclaude:briefing --depth comprehensive

Maria Keller, software engineer, was terminated by AlphaFin AG (Zürich) after
reporting FINMA compliance gaps. They retroactively extended her probation period
and gave her one month notice. She wants to claim wrongful termination under OR 336.
Monthly salary CHF 12,500, employed 2.5 years.
```

**What happens**: The briefing command activates the **swiss-legal-briefing-coordinator** agent. It classifies the query as high-complexity (employment law + regulatory compliance + retaliatory dismissal), selects a specialist panel, and asks structured intake questions.

**Agents activated**: briefing-coordinator, which spawns a panel from: researcher, strategist, procedure-specialist, risk-analyst, compliance-officer

The briefing coordinator asks targeted questions — each labeled with which specialist needs the answer:

> **Intake Questions**
>
> 1. Does Maria have written documentation of her compliance report (email, internal ticket)? *(Needed by: Researcher — evidence assessment; Advocate — documentary proof)*
>
> 2. Were there any documented performance issues before the compliance report? *(Needed by: Adversary — employer defense analysis; Risk — exposure calculation)*
>
> 3. Did AlphaFin AG follow its internal HR procedures for the termination? *(Needed by: Procedure — formal validity; Strategist — procedural attack vectors)*
>
> 4. Has Maria already sent a written objection (Einsprache) to the termination? *(Needed by: Procedure — deadline tracking; Strategist — preservation of claims)*
>
> 5. Is AlphaFin AG a FINMA-regulated entity, or does it operate under a fintech license? *(Needed by: Compliance — regulatory context; Researcher — applicable FINMA rules)*

After you answer these questions, the system builds an execution plan:

> **Execution Plan**
>
> | Step | Agent | Task | Checkpoint |
> |------|-------|------|------------|
> | 1 | Researcher | Search OR 335b/336/336a precedents, FINMA reporting context | Review findings |
> | 2 | Advocate + Adversary + Judicial Analyst | Three-agent adversarial analysis | Review assessment |
> | 3 | Strategist + Risk Analyst + Procedure Specialist | Litigation strategy with cost-benefit | Review strategy |
> | 4 | Drafter | Klageschrift for Arbeitsgericht Zürich | Review draft |
> | 5 | Summarizer | Consolidate all output | Final review |
>
> **Estimated pipeline time**: 15-25 minutes with checkpoints

**Key feature**: BetterCallClaude auto-detects the jurisdiction (Zürich) from the facts and routes to federal employment law (OR) plus Zürich cantonal procedure. You don't need to specify this manually.

---

## Step 2: Legal Research — Finding Precedents

With the briefing complete, run targeted legal research on the core issues.

```
/bettercallclaude:research "missbräuchliche Kündigung nach interner Compliance-Meldung, OR 336 lit. d, Sperrfrist OR 336c, rückwirkende Verlängerung der Probezeit OR 335b"
```

**Agents activated**: swiss-legal-researcher

**MCP servers used**: bge-search (Federal Supreme Court decisions), entscheidsuche (cross-court search), fedlex-sparql (statutory text)

The researcher executes a structured six-step workflow: parse the question, search BGE, search cantonal courts, evaluate sources, identify doctrine, and compile a memorandum.

> **Research Results — Missbräuchliche Kündigung (OR 336)**
>
> **Statutory Framework**
> - Art. 336 lit. d OR: Dismissal is wrongful when it is because the employee exercises a right arising from the employment relationship in good faith (*weil der Arbeitnehmer nach Treu und Glauben Ansprüche aus dem Arbeitsverhältnis geltend macht*).
> - Art. 336a II OR: Indemnity for wrongful dismissal — max. 6 months' salary.
> - Art. 335b OR: Probation period must be agreed at the start of employment; extensions require written agreement before the original period expires.
>
> **Key Precedents**
>
> 1. **BGE 136 III 513** — Retaliatory dismissal elements
>    - Held: Temporal proximity between protected activity and termination creates a presumption of retaliatory motive. The burden shifts to the employer to prove a legitimate, independent reason.
>    - Relevance: 21-day gap between Maria's report and termination strongly supports retaliation claim.
>
> 2. **BGE 132 III 115** — Indemnity calculation methodology
>    - Held: The court has discretion within the 0-6 month range. Factors: severity of misconduct, employee's age and seniority, economic impact, employer's behavior.
>    - Relevance: 2.5 years seniority, blatant retaliation pattern → likely 3-4 months.
>
> 3. **BGE 134 III 108** — Burden of proof in wrongful termination
>    - Held: Employee must prove the causal link between protected activity and dismissal. Circumstantial evidence (timing, absence of other reasons) is sufficient.
>    - Relevance: Email documentation of compliance report + no prior performance issues = strong circumstantial case.
>
> 4. **BGE 125 III 70** — Probation period rules
>    - Held: Probation extensions must be agreed before the original period expires. Retroactive extensions are void.
>    - Relevance: Directly invalidates AlphaFin's retroactive probation extension.
>
> *Sources verified via bge-search and legal-citations MCP servers.*

You can verify any specific citation with the cite command:

```
/bettercallclaude:cite BGE 136 III 513
```

> **Citation Verification**
>
> **BGE 136 III 513** ✓ Verified
> - DE: BGE 136 III 513
> - FR: ATF 136 III 513
> - IT: DTF 136 III 513
> - Subject: Missbräuchliche Kündigung / Licenciement abusif
> - Date: 2010
> - Key passage: E. 2.5 (temporal proximity analysis)

**Key feature**: The same research works across languages. Running the query in French (`/bettercallclaude:research "licenciement abusif après dénonciation de non-conformité FINMA, art. 336 let. d CO"`) returns results with ATF citations and French legal terminology.

---

## Step 3: Adversarial Analysis — Testing the Case

Now stress-test the legal position. The adversarial command runs three independent agents in sequence — an advocate, an adversary, and a judicial analyst — to produce a balanced assessment.

```
/bettercallclaude:adversarial Maria Keller was wrongfully terminated by AlphaFin AG after reporting FINMA compliance gaps. She worked 2.5 years, reported missing transaction monitoring on March 3, probation retroactively extended March 10, terminated March 24 with 1-month notice. Claims OR 336 lit. d retaliation and up to 6 months indemnity.
```

**Agents activated**: swiss-legal-advocate, swiss-legal-adversary, swiss-judicial-analyst

The three agents produce independent analyses. Here is a condensed version of each:

### Advocate Position

> **Advocate Assessment — Maria Keller's Case**
>
> **Strongest arguments**:
> 1. *Temporal proximity*: Only 21 days between the compliance report (March 3) and termination (March 24). Per BGE 136 III 513, this creates a strong presumption of retaliatory motive.
> 2. *Sham probation extension*: The retroactive probation extension on March 10 is void under BGE 125 III 70. It serves as additional evidence of employer bad faith — they needed a pretext to shorten the notice period.
> 3. *Documentary evidence*: The email reporting FINMA gaps is timestamped, creating an unambiguous record of the protected activity.
> 4. *No prior performance issues*: Absence of documented performance concerns before March 3 undermines any alternative explanation for termination.
> 5. *FINMA reporting context*: The compliance gap was a genuine regulatory risk (missing transaction monitoring). This was not a frivolous complaint — it was a legitimate exercise of employee rights under Treu und Glauben.
>
> **Recommended claim**: 4-6 months' salary (CHF 50,000-75,000) under OR 336a, plus correct notice period differential.

### Adversary Position

> **Adversary Assessment — AlphaFin AG's Defenses**
>
> **Potential employer arguments**:
> 1. *Business restructuring*: AlphaFin may argue the termination was part of a planned reorganization during the system migration, unrelated to the compliance report.
> 2. *Performance concerns*: Even without formal documentation, the employer may present witness testimony about pre-existing dissatisfaction with Maria's work.
> 3. *Compliance report was not "in good faith"*: The employer could argue Maria's report was motivated by personal conflict rather than genuine compliance concern — weakening the OR 336 lit. d protection.
> 4. *Probation extension as administrative error*: AlphaFin may frame the retroactive extension as an HR mistake rather than deliberate retaliation, limiting its evidentiary weight.
>
> **Weaknesses in employer's position**:
> - The 21-day timeline is very difficult to explain away.
> - The void probation extension actively damages employer credibility.
> - Lack of documented performance issues before March 3 is a significant gap.

### Judicial Analyst Synthesis

> **Judicial Assessment — Balanced Analysis**
>
> **Erwägung (Consideration)**
>
> *E. 1 — Wrongful Termination (OR 336 lit. d)*
> The temporal proximity (21 days) between the compliance report and termination, combined with the absence of documented performance issues, establishes a strong prima facie case of retaliatory dismissal. The employer bears the burden of proving a legitimate independent reason (BGE 134 III 108). The sham probation extension further undermines employer credibility.
> **Probability of finding wrongful termination: 70-80%**
>
> *E. 2 — Probation Extension Validity (OR 335b)*
> The retroactive extension is void as a matter of law (BGE 125 III 70). The original probation expired approximately 2 years before the extension attempt. This is not a close question.
> **Probability: >95% that extension is void**
>
> *E. 3 — Notice Period*
> With the probation extension void, the applicable notice period is 2 months (OR 335c I, 2nd year of service). The 1-month notice is insufficient. Maria's employment continues until the next valid termination date.
> **Probability: >90% that notice period was incorrect**
>
> *E. 4 — Indemnity (OR 336a)*
> Given the strength of the retaliation evidence and employer's bad-faith conduct (sham probation extension), the court would likely award 3-4 months' salary. A 6-month maximum is reserved for the most egregious cases.
> **Expected indemnity range: CHF 37,500-50,000 (3-4 months)**
>
> *Overall assessment*: **Favorable outcome likely (65-75%)**. Settlement recommended in the range of CHF 40,000-55,000.

**Key feature**: The judicial analyst uses Swiss *Erwägung* (consideration) structure — the same format Swiss courts use in their reasoning. Each legal issue gets a numbered consideration with a probability estimate.

---

## Step 4: Strategy Development

With research and adversarial analysis complete, develop the litigation strategy.

```
/bettercallclaude:strategy Wrongful termination claim for Maria Keller vs AlphaFin AG, Zürich. OR 336 lit. d retaliation after FINMA compliance report. Void probation extension (OR 335b). Claiming up to 6 months indemnity. Monthly salary CHF 12,500.
```

**Agents activated**: swiss-case-strategist, risk-analyst, procedure-specialist

> **Litigation Strategy — Keller v. AlphaFin AG**
>
> **1. Procedural Roadmap**
>
> | Phase | Forum | Deadline | Est. Duration |
> |-------|-------|----------|---------------|
> | Written objection (Einsprache) | To employer | Before end of notice period | Immediate |
> | Conciliation (Schlichtungsverfahren) | Friedensrichteramt Zürich | Within 180 days of termination | 1-3 months |
> | If no settlement → Lawsuit | Arbeitsgericht Zürich | 30 days after authorization to sue | 6-12 months |
>
> Note: Employment disputes up to CHF 30,000 are free of court fees (Art. 114 lit. c ZPO). Maria's claim (up to CHF 75,000) exceeds this threshold, so standard court fees apply.
>
> **2. Cost Estimate**
>
> | Item | Estimate |
> |------|----------|
> | Court fees (Gerichtskosten) | CHF 3,000-6,000 |
> | Legal representation | CHF 8,000-15,000 |
> | Expert fees (if needed) | CHF 2,000-5,000 |
> | **Total litigation cost** | **CHF 13,000-26,000** |
>
> **3. Settlement Analysis**
>
> - **Best case** (full indemnity): CHF 75,000 (6 months) + notice period damages
> - **Expected outcome**: CHF 37,500-50,000 (3-4 months) + notice period correction
> - **Settlement target**: CHF 45,000-55,000 (including notice period differential)
> - **Walk-away minimum**: CHF 30,000
>
> The cost-benefit ratio favors settlement: expected litigation cost of ~CHF 20,000 against expected recovery of ~CHF 45,000 yields a net gain of ~CHF 25,000. A pre-trial settlement at CHF 45,000 preserves the same net outcome without litigation risk.
>
> **4. Risk Matrix**
>
> | Risk | Probability | Impact | Mitigation |
> |------|------------|--------|------------|
> | Employer presents pre-existing performance issues | 30% | High | Request full HR file early in discovery |
> | Court awards less than 3 months | 25% | Medium | Emphasize sham probation as aggravating factor |
> | AlphaFin files counterclaim (e.g., NDA breach) | 10% | Medium | Review employment contract for NDA scope |
> | Protracted litigation (>18 months) | 20% | Low-Medium | Push for early Schlichtung settlement |
>
> **5. Recommended Approach**
>
> Send a formal written objection immediately. File for Schlichtungsverfahren at Friedensrichteramt Zürich with an opening demand of CHF 65,000 (anchor high). Target settlement at CHF 45,000-55,000. Only proceed to Arbeitsgericht if conciliation fails and the client accepts the litigation cost/timeline risk.

**Key feature**: BetterCallClaude calculates financial exposure using actual Zürich court fee schedules and maps the full procedural pathway specific to the canton.

---

## Step 5: Document Drafting

Generate a professional court submission (Klageschrift) for the Arbeitsgericht Zürich.

```
/bettercallclaude:draft Klageschrift for wrongful termination, Maria Keller vs AlphaFin AG, Arbeitsgericht Zürich. Claims: (1) declaration that termination is wrongful under OR 336 lit. d, (2) indemnity of CHF 75,000 under OR 336a, (3) notice period correction. Based on retaliation after FINMA compliance report, void probation extension.
```

**Agents activated**: swiss-legal-drafter

The drafter produces a formal court submission following ZPO requirements. Here is an excerpt — the Rechtsbegehren (prayer for relief) and the opening of the legal analysis:

> **Klageschrift**
> **an das Arbeitsgericht Zürich**
>
> ---
>
> **Klägerin**: Maria Keller, [Adresse], vertreten durch [Rechtsanwalt/Rechtsanwältin]
>
> **Beklagte**: AlphaFin AG, [Adresse], [Handelsregisternummer]
>
> ---
>
> ## I. Rechtsbegehren
>
> 1. Es sei festzustellen, dass die Kündigung der Beklagten vom 24. März 2026 missbräuchlich im Sinne von Art. 336 Abs. 1 lit. d OR ist.
>
> 2. Die Beklagte sei zu verpflichten, der Klägerin eine Entschädigung von CHF 75,000.00 (sechs Monatslöhne) gemäss Art. 336a Abs. 2 OR nebst Zins zu 5% seit dem 1. Mai 2026 zu bezahlen.
>
> 3. Es sei festzustellen, dass die rückwirkende Verlängerung der Probezeit vom 10. März 2026 nichtig ist und die ordentliche Kündigungsfrist von zwei Monaten gemäss Art. 335c Abs. 1 OR gilt.
>
> 4. Unter Kosten- und Entschädigungsfolgen zu Lasten der Beklagten.
>
> ---
>
> ## II. Sachverhalt
>
> 1. Die Klägerin war seit [Datum] als Lead Backend Engineer bei der Beklagten in Zürich angestellt. Das monatliche Bruttogehalt betrug CHF 12,500.00.
>
> 2. Am 3. März 2026 meldete die Klägerin per E-Mail an ihren direkten Vorgesetzten und den CTO der Beklagten, dass das Payment-Processing-Modul die von der FINMA vorgeschriebene Transaktionsüberwachung nicht implementiert hatte.
>
> 3. Am 10. März 2026 — sieben Tage nach der Compliance-Meldung — verlängerte die Beklagte die Probezeit der Klägerin rückwirkend. Diese Verlängerung ist nichtig (BGE 125 III 70), da die ursprüngliche Probezeit bereits seit über zwei Jahren abgelaufen war.
>
> 4. Am 24. März 2026 — 21 Tage nach der Compliance-Meldung — kündigte die Beklagte das Arbeitsverhältnis per 30. April 2026 mit einmonatiger Frist.
>
> [...]
>
> ---
>
> *Dieses Dokument wurde mit Unterstützung von BetterCallClaude erstellt. Alle Inhalte bedürfen der Überprüfung und Freigabe durch die zuständige Rechtsanwältin / den zuständigen Rechtsanwalt.*

**Key feature**: The drafter structures the document per ZPO requirements — Rechtsbegehren, Sachverhalt, Rechtliche Begründung, Beweismittel — with proper citation format and formal register. Placeholder fields (addresses, dates) are marked for the lawyer to fill in.

---

## Step 6: Final Consolidation

After all pipeline steps, consolidate the output into a single executive summary.

```
/bettercallclaude:summarize --medium
```

**Agents activated**: swiss-legal-summarizer

> **Case Summary — Keller v. AlphaFin AG**
>
> **Matter**: Wrongful termination (missbräuchliche Kündigung, OR 336 lit. d)
> **Jurisdiction**: Arbeitsgericht Zürich
> **Assessment**: Favorable outcome likely (65-75%)
>
> **Key Findings**:
> - The 21-day gap between compliance report and termination creates a strong presumption of retaliation (BGE 136 III 513).
> - The retroactive probation extension is void as a matter of law (BGE 125 III 70).
> - Expected indemnity: 3-4 months' salary (CHF 37,500-50,000).
> - Settlement target: CHF 45,000-55,000.
>
> **Recommended Actions**:
> 1. Send written objection (Einsprache) to AlphaFin AG immediately.
> 2. File for Schlichtungsverfahren at Friedensrichteramt Zürich.
> 3. Open settlement negotiations with anchor demand of CHF 65,000.
> 4. Prepare Klageschrift for Arbeitsgericht Zürich as fallback.
>
> **Documents Produced**:
> - Research memorandum with 4 BGE precedents
> - Adversarial analysis with probability assessment
> - Litigation strategy with cost-benefit analysis
> - Draft Klageschrift (Arbeitsgericht Zürich)
>
> *Professional disclaimer: This analysis is produced by an AI legal research tool. All outputs require review and validation by a qualified Swiss lawyer before use. This does not constitute legal advice.*

**Key feature**: The summarizer deduplicates disclaimers, terminology definitions, and citations that appear across multiple agent outputs, producing a clean consolidated summary.

---

## Tips and Next Steps

### Customize the Workflow

You don't have to run every step. Some variations:

- **Research only**: Just run `/bettercallclaude:research` to get precedents without the full pipeline.
- **Skip to strategy**: If you already know the legal issues, go straight to `/bettercallclaude:strategy`.
- **Quick adversarial**: Use `/bettercallclaude:adversarial --short` for a condensed assessment.
- **Automated pipeline**: `/bettercallclaude:workflow litigation-prep` runs the full pipeline (Researcher -> Strategist -> Adversarial -> Drafter) with automatic handoffs.

### Cantonal Variations

The same case in Geneva would involve different procedural rules:

```
/bettercallclaude:strategy Licenciement abusif, Maria Keller c. AlphaFin SA, Genève.
Art. 336 let. d CO, dénonciation de non-conformité FINMA.
```

BetterCallClaude switches to French legal terminology (CO instead of OR, ATF instead of BGE) and routes to Geneva cantonal procedure (Tribunal des prud'hommes) automatically.

### Multi-Lingual Analysis

Run any command in German, French, or Italian:

```
# German (default for Zürich)
/bettercallclaude:research "Missbräuchliche Kündigung OR 336"

# French
/bettercallclaude:research "Licenciement abusif art. 336 CO"

# Italian
/bettercallclaude:research "Disdetta abusiva art. 336 CO"
```

### Further Reading

- [Command Reference](../command-reference.md) — Full documentation of all 18 commands
- [Legal Research Workflow](../workflows/research-precedents.md) — Deep dive into BGE research methodology
- [Case Strategy Workflow](../workflows/case-strategy.md) — Detailed strategy development guide
- [Installation Guide](../INSTALL.md) — Setup, MCP server configuration, troubleshooting

---

## BCC Features Demonstrated in This Tutorial

| Feature | Step | Components Used |
|---------|------|-----------------|
| Briefing session | Step 1 | briefing-coordinator agent, jurisdiction detection skill |
| Legal research | Step 2 | researcher agent, bge-search MCP, entscheidsuche MCP, fedlex-sparql MCP |
| Citation verification | Step 2 | citation-specialist agent, legal-citations MCP |
| Adversarial analysis | Step 3 | advocate + adversary + judicial-analyst agents, adversarial-analysis skill |
| Litigation strategy | Step 4 | case-strategist + risk-analyst + procedure-specialist agents, strategy skill |
| Document drafting | Step 5 | drafter agent, swiss-legal-drafting skill |
| Output consolidation | Step 6 | summarizer agent |
| Multi-lingual support | Throughout | language detection, DE/FR/IT/EN legal terms |
| Jurisdiction routing | Throughout | swiss-jurisdictions skill, cantonal detection |

**Total**: 10 agents, 5 commands, 4 skills, 4 MCP servers demonstrated in a single case.
