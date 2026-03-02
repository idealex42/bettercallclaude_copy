# Arbeitsrecht-Fallbeispiel: Missbräuchliche Kündigung

Dieses Tutorial führt Schritt für Schritt durch eine vollständige Rechtsangelegenheit mit BetterCallClaude — von der Fallaufnahme bis zur Enddokumentation. Es zeigt, wie die Commands, Agents, Skills und MCP Server des Frameworks bei einem realistischen Schweizer Arbeitsrechtsfall zusammenwirken.

**Zielgruppe**: Schweizer Anwältinnen und Anwälte, die das Tool evaluieren, Claude Code-Nutzer, die das Plugin-Framework erkunden, oder alle, die verstehen möchten, wie BetterCallClaude eine reale Rechtsangelegenheit von Anfang bis Ende bearbeitet.

**Lesezeit**: 20–30 Minuten

**Voraussetzungen**: BetterCallClaude installiert und funktionsfähig. Siehe [docs/INSTALL.md](../INSTALL.md) für die Einrichtungsanleitung.

---

## Der Fall

> **Maria Keller** (34, Software-Ingenieurin) gegen **AlphaFin AG** (Fintech-Startup, Zürich)
>
> Maria war bei der AlphaFin AG 2,5 Jahre als Lead Backend Engineer angestellt. Während einer Systemmigration stellte sie fest, dass das Payment-Processing-Modul die von der FINMA vorgeschriebene Transaktionsüberwachung nicht implementiert hatte. Am 3. März 2026 meldete sie diese Compliance-Lücke per E-Mail an ihren direkten Vorgesetzten und den CTO.
>
> Am 10. März verlängerte die Geschäftsleitung ihre Probezeit rückwirkend — ungültig gemäss Art. 335b OR, da die ursprüngliche Probezeit längst abgelaufen war. Am 24. März erhielt sie die Kündigung per 30. April unter Berufung auf eine einmonatige Kündigungsfrist (als ob sie sich noch in der Probezeit befände).
>
> Maria ist der Auffassung, dass die Kündigung eine Vergeltungsmassnahme darstellt (missbräuchliche Kündigung, Art. 336 Abs. 1 lit. d OR) und macht eine Entschädigung von bis zu 6 Monatslöhnen geltend (Art. 336a OR). Ihr monatliches Bruttogehalt beträgt CHF 12'500.

**Rechtliche Streitpunkte**:
1. Gültigkeit der rückwirkenden Probezeitverlängerung (Art. 335b OR)
2. Vergeltungskündigung nach Compliance-Meldung (Art. 336 Abs. 1 lit. d OR)
3. Kündigungsfrist — ordentlich vs. Probezeit (Art. 335a/335c OR)
4. Anwendbarkeit der Sperrfrist (Art. 336c OR)
5. Berechnung und Obergrenze der Entschädigung (Art. 336a Abs. 2 OR — max. 6 Monate)
6. Zuständigkeit und Verfahren (Arbeitsgericht Zürich, ZPO)

---

## Schritt 1: Fallaufnahme mit Briefing-Sitzung

Beginnen Sie mit einer umfassenden Briefing-Sitzung, damit BetterCallClaude den Sachverhalt versteht, bevor die Agents ihre Arbeit aufnehmen.

```
/bettercallclaude:briefing --depth comprehensive

Maria Keller, Software-Ingenieurin, wurde von der AlphaFin AG (Zürich) gekündigt,
nachdem sie FINMA-Compliance-Lücken gemeldet hatte. Die Probezeit wurde rückwirkend
verlängert und die Kündigung mit einmonatiger Frist ausgesprochen. Sie macht eine
missbräuchliche Kündigung nach Art. 336 OR geltend.
Monatslohn CHF 12'500, Anstellungsdauer 2,5 Jahre.
```

**Was geschieht**: Der Briefing-Command aktiviert den **swiss-legal-briefing-coordinator** Agent. Dieser klassifiziert die Anfrage als hochkomplex (Arbeitsrecht + regulatorische Compliance + Vergeltungskündigung), stellt ein Spezialistengremium zusammen und stellt strukturierte Aufnahmefragen.

**Aktivierte Agents**: briefing-coordinator, der ein Gremium aus folgenden Spezialisten zusammenstellt: researcher, strategist, procedure-specialist, risk-analyst, compliance-officer

Der Briefing-Koordinator stellt gezielte Fragen — jeweils mit Angabe, welcher Spezialist die Antwort benötigt:

> **Aufnahmefragen**
>
> 1. Verfügt Maria über eine schriftliche Dokumentation ihrer Compliance-Meldung (E-Mail, internes Ticket)? *(Benötigt von: Researcher — Beweiseinschätzung; Advocate — Urkundenbeweis)*
>
> 2. Gab es dokumentierte Leistungsprobleme vor der Compliance-Meldung? *(Benötigt von: Adversary — Analyse der Arbeitgeberverteidigung; Risk — Risikoberechnung)*
>
> 3. Hat die AlphaFin AG bei der Kündigung ihre internen HR-Verfahren eingehalten? *(Benötigt von: Procedure — formelle Gültigkeit; Strategist — verfahrensrechtliche Angriffspunkte)*
>
> 4. Hat Maria bereits einen schriftlichen Einspruch gegen die Kündigung erhoben? *(Benötigt von: Procedure — Fristenüberwachung; Strategist — Anspruchswahrung)*
>
> 5. Ist die AlphaFin AG ein FINMA-reguliertes Institut oder operiert sie unter einer Fintech-Lizenz? *(Benötigt von: Compliance — regulatorischer Kontext; Researcher — anwendbare FINMA-Vorschriften)*

Nach Beantwortung dieser Fragen erstellt das System einen Ausführungsplan:

> **Ausführungsplan**
>
> | Schritt | Agent | Aufgabe | Kontrollpunkt |
> |---------|-------|---------|---------------|
> | 1 | Researcher | Suche nach Präzedenzfällen zu Art. 335b/336/336a OR, FINMA-Meldekontext | Ergebnisse prüfen |
> | 2 | Advocate + Adversary + Judicial Analyst | Dreigliedrige kontradiktorische Analyse | Beurteilung prüfen |
> | 3 | Strategist + Risk Analyst + Procedure Specialist | Prozessstrategie mit Kosten-Nutzen-Analyse | Strategie prüfen |
> | 4 | Drafter | Klageschrift für das Arbeitsgericht Zürich | Entwurf prüfen |
> | 5 | Summarizer | Konsolidierung aller Ergebnisse | Endkontrolle |
>
> **Geschätzte Pipeline-Dauer**: 15–25 Minuten mit Kontrollpunkten

**Kernfunktion**: BetterCallClaude erkennt den Gerichtsstand (Zürich) automatisch aus dem Sachverhalt und leitet an das eidgenössische Arbeitsrecht (OR) sowie das zürcherische Verfahrensrecht weiter. Eine manuelle Angabe ist nicht erforderlich.

---

## Schritt 2: Juristische Recherche — Präzedenzfälle finden

Nach Abschluss des Briefings folgt die gezielte juristische Recherche zu den Kernfragen.

```
/bettercallclaude:research "missbräuchliche Kündigung nach interner Compliance-Meldung, OR 336 lit. d, Sperrfrist OR 336c, rückwirkende Verlängerung der Probezeit OR 335b"
```

**Aktivierte Agents**: swiss-legal-researcher

**Verwendete MCP Server**: bge-search (Bundesgerichtsentscheide), entscheidsuche (gerichtsübergreifende Suche), fedlex-sparql (Gesetzestexte)

Der Researcher führt einen strukturierten sechsstufigen Workflow aus: Frage analysieren, BGE durchsuchen, kantonale Entscheide durchsuchen, Quellen bewerten, Lehrmeinungen identifizieren und ein Memorandum zusammenstellen.

> **Recherche-Ergebnisse — Missbräuchliche Kündigung (Art. 336 OR)**
>
> **Gesetzlicher Rahmen**
> - Art. 336 Abs. 1 lit. d OR: Die Kündigung ist missbräuchlich, wenn sie ausgesprochen wird, weil der Arbeitnehmer nach Treu und Glauben Ansprüche aus dem Arbeitsverhältnis geltend macht.
> - Art. 336a Abs. 2 OR: Entschädigung bei missbräuchlicher Kündigung — max. 6 Monatslöhne.
> - Art. 335b OR: Die Probezeit muss bei Beginn des Arbeitsverhältnisses vereinbart werden; Verlängerungen bedürfen einer schriftlichen Vereinbarung vor Ablauf der ursprünglichen Frist.
>
> **Leitentscheide**
>
> 1. **BGE 136 III 513** — Elemente der Vergeltungskündigung
>    - Leitsatz: Die zeitliche Nähe zwischen geschützter Tätigkeit und Kündigung begründet eine Vermutung des Vergeltungsmotivs. Die Beweislast verlagert sich auf den Arbeitgeber, einen legitimen, unabhängigen Kündigungsgrund nachzuweisen.
>    - Relevanz: Der Zeitabstand von 21 Tagen zwischen Marias Meldung und der Kündigung stützt den Vergeltungsvorwurf stark.
>
> 2. **BGE 132 III 115** — Methodik der Entschädigungsberechnung
>    - Leitsatz: Das Gericht verfügt über Ermessen innerhalb des Rahmens von 0–6 Monaten. Kriterien: Schwere des Fehlverhaltens, Alter und Dienstalter der Arbeitnehmerin, wirtschaftliche Auswirkungen, Verhalten des Arbeitgebers.
>    - Relevanz: 2,5 Jahre Dienstalter, offensichtliches Vergeltungsmuster → voraussichtlich 3–4 Monate.
>
> 3. **BGE 134 III 108** — Beweislast bei missbräuchlicher Kündigung
>    - Leitsatz: Die Arbeitnehmerin muss den Kausalzusammenhang zwischen geschützter Tätigkeit und Kündigung beweisen. Indizienbeweise (zeitlicher Zusammenhang, Fehlen anderer Gründe) genügen.
>    - Relevanz: E-Mail-Dokumentation der Compliance-Meldung + keine früheren Leistungsprobleme = starke Indizienbeweislage.
>
> 4. **BGE 125 III 70** — Regelung der Probezeit
>    - Leitsatz: Probezeitverlängerungen müssen vor Ablauf der ursprünglichen Frist vereinbart werden. Rückwirkende Verlängerungen sind nichtig.
>    - Relevanz: Macht die rückwirkende Probezeitverlängerung der AlphaFin AG unmittelbar unwirksam.
>
> *Quellen verifiziert über die MCP Server bge-search und legal-citations.*

Einzelne Zitate lassen sich mit dem Cite-Command überprüfen:

```
/bettercallclaude:cite BGE 136 III 513
```

> **Zitatverifikation**
>
> **BGE 136 III 513** ✓ Verifiziert
> - DE: BGE 136 III 513
> - FR: ATF 136 III 513
> - IT: DTF 136 III 513
> - Gegenstand: Missbräuchliche Kündigung / Licenciement abusif
> - Datum: 2010
> - Kernpassage: E. 2.5 (Analyse der zeitlichen Nähe)

**Kernfunktion**: Dieselbe Recherche funktioniert sprachübergreifend. Eine französische Anfrage (`/bettercallclaude:research "licenciement abusif après dénonciation de non-conformité FINMA, art. 336 let. d CO"`) liefert Ergebnisse mit ATF-Zitaten und französischer Rechtsterminologie.

---

## Schritt 3: Kontradiktorische Analyse — Den Fall auf die Probe stellen

Nun wird die Rechtsposition einem Stresstest unterzogen. Der Adversarial-Command führt drei unabhängige Agents nacheinander aus — einen Advocate, einen Adversary und einen Judicial Analyst — um eine ausgewogene Beurteilung zu erstellen.

```
/bettercallclaude:adversarial Maria Keller wurde von der AlphaFin AG missbräuchlich gekündigt, nachdem sie FINMA-Compliance-Lücken gemeldet hatte. 2,5 Jahre angestellt, Meldung fehlender Transaktionsüberwachung am 3. März, rückwirkende Probezeitverlängerung am 10. März, Kündigung am 24. März mit einmonatiger Frist. Anspruch auf Entschädigung nach Art. 336 Abs. 1 lit. d OR, bis 6 Monatslöhne.
```

**Aktivierte Agents**: swiss-legal-advocate, swiss-legal-adversary, swiss-judicial-analyst

Die drei Agents erstellen unabhängige Analysen. Nachfolgend eine zusammengefasste Version jeder Analyse:

### Position des Advocate

> **Beurteilung des Advocate — Fall Maria Keller**
>
> **Stärkste Argumente**:
> 1. *Zeitliche Nähe*: Nur 21 Tage zwischen der Compliance-Meldung (3. März) und der Kündigung (24. März). Gemäss BGE 136 III 513 begründet dies eine starke Vermutung des Vergeltungsmotivs.
> 2. *Schein-Probezeitverlängerung*: Die rückwirkende Verlängerung vom 10. März ist nach BGE 125 III 70 nichtig. Sie dient als zusätzlicher Beleg für den bösen Glauben des Arbeitgebers — er benötigte einen Vorwand zur Verkürzung der Kündigungsfrist.
> 3. *Urkundliche Beweise*: Die E-Mail mit der FINMA-Meldung trägt einen Zeitstempel und dokumentiert die geschützte Tätigkeit eindeutig.
> 4. *Keine früheren Leistungsprobleme*: Das Fehlen dokumentierter Leistungsbeanstandungen vor dem 3. März untergräbt jede alternative Erklärung für die Kündigung.
> 5. *FINMA-Meldekontext*: Die Compliance-Lücke war ein reales regulatorisches Risiko (fehlende Transaktionsüberwachung). Es handelte sich nicht um eine querulatorische Beschwerde, sondern um eine berechtigte Ausübung von Arbeitnehmerrechten nach Treu und Glauben.
>
> **Empfohlene Forderung**: 4–6 Monatslöhne (CHF 50'000–75'000) gestützt auf Art. 336a OR, zuzüglich Differenz aus korrekter Kündigungsfrist.

### Position des Adversary

> **Beurteilung des Adversary — Verteidigungsmöglichkeiten der AlphaFin AG**
>
> **Mögliche Arbeitgeberargumente**:
> 1. *Betriebliche Umstrukturierung*: Die AlphaFin AG könnte geltend machen, die Kündigung sei Teil einer geplanten Reorganisation im Rahmen der Systemmigration gewesen, unabhängig von der Compliance-Meldung.
> 2. *Leistungsprobleme*: Auch ohne formelle Dokumentation könnte der Arbeitgeber Zeugenaussagen über vorbestehende Unzufriedenheit mit Marias Arbeit vorbringen.
> 3. *Compliance-Meldung nicht «nach Treu und Glauben»*: Der Arbeitgeber könnte argumentieren, Marias Meldung sei durch persönliche Konflikte motiviert gewesen und nicht durch echte Compliance-Bedenken — was den Schutz nach Art. 336 Abs. 1 lit. d OR abschwächen würde.
> 4. *Probezeitverlängerung als Verwaltungsfehler*: Die AlphaFin AG könnte die rückwirkende Verlängerung als HR-Versehen darstellen statt als bewusste Vergeltung, um deren Beweiswert zu minimieren.
>
> **Schwächen der Arbeitgeberposition**:
> - Der zeitliche Ablauf von 21 Tagen ist äusserst schwer zu erklären.
> - Die nichtige Probezeitverlängerung schadet der Glaubwürdigkeit des Arbeitgebers aktiv.
> - Das Fehlen dokumentierter Leistungsprobleme vor dem 3. März ist eine erhebliche Lücke.

### Synthese des Judicial Analyst

> **Richterliche Beurteilung — Ausgewogene Analyse**
>
> **Erwägungen**
>
> *E. 1 — Missbräuchliche Kündigung (Art. 336 Abs. 1 lit. d OR)*
> Die zeitliche Nähe (21 Tage) zwischen Compliance-Meldung und Kündigung, verbunden mit dem Fehlen dokumentierter Leistungsprobleme, begründet einen starken Prima-facie-Beweis für eine Vergeltungskündigung. Der Arbeitgeber trägt die Beweislast für einen legitimen, unabhängigen Kündigungsgrund (BGE 134 III 108). Die Schein-Probezeitverlängerung untergräbt die Glaubwürdigkeit des Arbeitgebers zusätzlich.
> **Wahrscheinlichkeit der Feststellung einer missbräuchlichen Kündigung: 70–80 %**
>
> *E. 2 — Gültigkeit der Probezeitverlängerung (Art. 335b OR)*
> Die rückwirkende Verlängerung ist von Gesetzes wegen nichtig (BGE 125 III 70). Die ursprüngliche Probezeit war rund zwei Jahre vor dem Verlängerungsversuch abgelaufen. Dies ist keine Grenzfrage.
> **Wahrscheinlichkeit: >95 %, dass die Verlängerung nichtig ist**
>
> *E. 3 — Kündigungsfrist*
> Bei Nichtigkeit der Probezeitverlängerung gilt die ordentliche Kündigungsfrist von zwei Monaten (Art. 335c Abs. 1 OR, zweites Dienstjahr). Die einmonatige Frist ist ungenügend. Das Arbeitsverhältnis besteht bis zum nächsten gültigen Kündigungstermin fort.
> **Wahrscheinlichkeit: >90 %, dass die Kündigungsfrist falsch war**
>
> *E. 4 — Entschädigung (Art. 336a OR)*
> Angesichts der Stärke der Vergeltungsbeweise und des treuwidrigen Verhaltens des Arbeitgebers (Schein-Probezeitverlängerung) dürfte das Gericht voraussichtlich 3–4 Monatslöhne zusprechen. Das Maximum von 6 Monaten bleibt den gravierendsten Fällen vorbehalten.
> **Erwarteter Entschädigungsrahmen: CHF 37'500–50'000 (3–4 Monate)**
>
> *Gesamtbeurteilung*: **Günstiger Ausgang wahrscheinlich (65–75 %)**. Vergleich im Bereich von CHF 40'000–55'000 empfohlen.

**Kernfunktion**: Der Judicial Analyst verwendet die schweizerische Erwägungsstruktur — dasselbe Format, das Schweizer Gerichte in ihrer Urteilsbegründung verwenden. Jede Rechtsfrage erhält eine nummerierte Erwägung mit Wahrscheinlichkeitseinschätzung.

---

## Schritt 4: Strategieentwicklung

Nach Abschluss der Recherche und der kontradiktorischen Analyse wird die Prozessstrategie erarbeitet.

```
/bettercallclaude:strategy Klage wegen missbräuchlicher Kündigung, Maria Keller gegen AlphaFin AG, Zürich. Art. 336 Abs. 1 lit. d OR, Vergeltung nach FINMA-Compliance-Meldung. Nichtige Probezeitverlängerung (Art. 335b OR). Entschädigungsforderung bis 6 Monatslöhne. Monatslohn CHF 12'500.
```

**Aktivierte Agents**: swiss-case-strategist, risk-analyst, procedure-specialist

> **Prozessstrategie — Keller gegen AlphaFin AG**
>
> **1. Verfahrensablauf**
>
> | Phase | Forum | Frist | Gesch. Dauer |
> |-------|-------|-------|--------------|
> | Schriftlicher Einspruch (Einsprache) | An Arbeitgeberin | Vor Ablauf der Kündigungsfrist | Sofort |
> | Schlichtungsverfahren | Friedensrichteramt Zürich | Innerhalb von 180 Tagen nach Beendigung | 1–3 Monate |
> | Bei Scheitern → Klage | Arbeitsgericht Zürich | 30 Tage nach Klagebewilligung | 6–12 Monate |
>
> Hinweis: Arbeitsrechtliche Streitigkeiten bis CHF 30'000 sind gerichtskostenfrei (Art. 114 lit. c ZPO). Marias Forderung (bis CHF 75'000) übersteigt diese Schwelle, weshalb die üblichen Gerichtskosten anfallen.
>
> **2. Kostenschätzung**
>
> | Position | Schätzung |
> |----------|-----------|
> | Gerichtskosten | CHF 3'000–6'000 |
> | Rechtsvertretung | CHF 8'000–15'000 |
> | Gutachterkosten (falls erforderlich) | CHF 2'000–5'000 |
> | **Gesamte Prozesskosten** | **CHF 13'000–26'000** |
>
> **3. Vergleichsanalyse**
>
> - **Bestfall** (volle Entschädigung): CHF 75'000 (6 Monate) + Schadenersatz für Kündigungsfristdifferenz
> - **Erwartetes Ergebnis**: CHF 37'500–50'000 (3–4 Monate) + Korrektur der Kündigungsfrist
> - **Vergleichsziel**: CHF 45'000–55'000 (einschliesslich Kündigungsfristdifferenz)
> - **Untere Schmerzgrenze**: CHF 30'000
>
> Das Kosten-Nutzen-Verhältnis spricht für einen Vergleich: Erwartete Prozesskosten von ~CHF 20'000 bei einer erwarteten Entschädigung von ~CHF 45'000 ergeben einen Nettogewinn von ~CHF 25'000. Ein vorprozessualer Vergleich bei CHF 45'000 sichert dasselbe Nettoergebnis ohne Prozessrisiko.
>
> **4. Risikomatrix**
>
> | Risiko | Wahrscheinlichkeit | Auswirkung | Gegenmassnahme |
> |--------|-------------------|------------|----------------|
> | Arbeitgeberin legt vorbestehende Leistungsprobleme dar | 30 % | Hoch | Frühzeitige Herausgabe der vollständigen HR-Akte verlangen |
> | Gericht spricht weniger als 3 Monate zu | 25 % | Mittel | Schein-Probezeitverlängerung als erschwerenden Umstand hervorheben |
> | AlphaFin AG erhebt Widerklage (z.B. NDA-Verletzung) | 10 % | Mittel | Arbeitsvertrag auf NDA-Umfang prüfen |
> | Langwieriges Verfahren (>18 Monate) | 20 % | Mittel-gering | Auf frühzeitigen Vergleich im Schlichtungsverfahren drängen |
>
> **5. Empfohlenes Vorgehen**
>
> Sofortiger schriftlicher Einspruch gegen die Kündigung. Einreichung des Schlichtungsgesuchs beim Friedensrichteramt Zürich mit einer Eröffnungsforderung von CHF 65'000 (hoher Anker). Vergleichsziel CHF 45'000–55'000. Klage beim Arbeitsgericht nur bei Scheitern der Schlichtung und wenn die Mandantin das Kosten-/Zeitrisiko akzeptiert.

**Kernfunktion**: BetterCallClaude berechnet die finanzielle Exposition anhand der tatsächlichen Zürcher Gerichtskostenrahmen und bildet den vollständigen Verfahrensweg spezifisch für den Kanton ab.

---

## Schritt 5: Dokumententwurf

Erstellung einer professionellen Klageschrift für das Arbeitsgericht Zürich.

```
/bettercallclaude:draft Klageschrift wegen missbräuchlicher Kündigung, Maria Keller gegen AlphaFin AG, Arbeitsgericht Zürich. Begehren: (1) Feststellung der Missbräuchlichkeit der Kündigung nach Art. 336 Abs. 1 lit. d OR, (2) Entschädigung von CHF 75'000 gestützt auf Art. 336a OR, (3) Korrektur der Kündigungsfrist. Gestützt auf Vergeltung nach FINMA-Compliance-Meldung, nichtige Probezeitverlängerung.
```

**Aktivierte Agents**: swiss-legal-drafter

Der Drafter erstellt eine formelle Klageschrift gemäss den Anforderungen der ZPO. Nachfolgend ein Auszug — das Rechtsbegehren und der Beginn der rechtlichen Begründung:

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

**Kernfunktion**: Der Drafter strukturiert das Dokument gemäss den ZPO-Anforderungen — Rechtsbegehren, Sachverhalt, Rechtliche Begründung, Beweismittel — mit korrektem Zitierformat und formellem Register. Platzhalterfelder (Adressen, Daten) sind für die anwaltliche Ergänzung gekennzeichnet.

---

## Schritt 6: Abschliessende Konsolidierung

Nach Abschluss aller Pipeline-Schritte werden die Ergebnisse in einer Zusammenfassung konsolidiert.

```
/bettercallclaude:summarize --medium
```

**Aktivierte Agents**: swiss-legal-summarizer

> **Fallzusammenfassung — Keller gegen AlphaFin AG**
>
> **Gegenstand**: Missbräuchliche Kündigung (Art. 336 Abs. 1 lit. d OR)
> **Gerichtsstand**: Arbeitsgericht Zürich
> **Beurteilung**: Günstiger Ausgang wahrscheinlich (65–75 %)
>
> **Kernbefunde**:
> - Der zeitliche Abstand von 21 Tagen zwischen Compliance-Meldung und Kündigung begründet eine starke Vermutung der Vergeltung (BGE 136 III 513).
> - Die rückwirkende Probezeitverlängerung ist von Gesetzes wegen nichtig (BGE 125 III 70).
> - Erwartete Entschädigung: 3–4 Monatslöhne (CHF 37'500–50'000).
> - Vergleichsziel: CHF 45'000–55'000.
>
> **Empfohlene Massnahmen**:
> 1. Sofortiger schriftlicher Einspruch (Einsprache) an die AlphaFin AG.
> 2. Einreichung des Schlichtungsgesuchs beim Friedensrichteramt Zürich.
> 3. Aufnahme von Vergleichsverhandlungen mit Ankerforderung von CHF 65'000.
> 4. Vorbereitung der Klageschrift für das Arbeitsgericht Zürich als Rückfalloption.
>
> **Erstellte Dokumente**:
> - Recherche-Memorandum mit 4 BGE-Leitentscheiden
> - Kontradiktorische Analyse mit Wahrscheinlichkeitseinschätzung
> - Prozessstrategie mit Kosten-Nutzen-Analyse
> - Klageschrift-Entwurf (Arbeitsgericht Zürich)
>
> *Professioneller Haftungsausschluss: Diese Analyse wurde von einem KI-gestützten juristischen Recherchetool erstellt. Sämtliche Ergebnisse bedürfen der Überprüfung und Validierung durch eine qualifizierte Schweizer Anwältin oder einen qualifizierten Schweizer Anwalt, bevor sie verwendet werden. Dies stellt keine Rechtsberatung dar.*

**Kernfunktion**: Der Summarizer dedupliziert Haftungsausschlüsse, Begriffsdefinitionen und Zitate, die in mehreren Agent-Ausgaben vorkommen, und erstellt eine bereinigte, konsolidierte Zusammenfassung.

---

## Tipps und nächste Schritte

### Den Workflow anpassen

Nicht jeder Schritt muss durchlaufen werden. Einige Varianten:

- **Nur Recherche**: Führen Sie nur `/bettercallclaude:research` aus, um Präzedenzfälle ohne die gesamte Pipeline zu erhalten.
- **Direkt zur Strategie**: Wenn die Rechtsfragen bereits bekannt sind, gehen Sie direkt zu `/bettercallclaude:strategy`.
- **Schnelle kontradiktorische Analyse**: Verwenden Sie `/bettercallclaude:adversarial --short` für eine verkürzte Beurteilung.
- **Automatisierte Pipeline**: `/bettercallclaude:workflow litigation-prep` führt die gesamte Pipeline (Researcher → Strategist → Adversarial → Drafter) mit automatischen Übergaben aus.

### Kantonale Unterschiede

Derselbe Fall in Genf würde andere Verfahrensregeln umfassen:

```
/bettercallclaude:strategy Licenciement abusif, Maria Keller c. AlphaFin SA, Genève.
Art. 336 let. d CO, dénonciation de non-conformité FINMA.
```

BetterCallClaude wechselt automatisch zur französischen Rechtsterminologie (CO statt OR, ATF statt BGE) und leitet an das Genfer Kantonsverfahren (Tribunal des prud'hommes) weiter.

### Mehrsprachige Analyse

Jeder Command kann auf Deutsch, Französisch oder Italienisch ausgeführt werden:

```
# Deutsch (Standard für Zürich)
/bettercallclaude:research "Missbräuchliche Kündigung OR 336"

# Französisch
/bettercallclaude:research "Licenciement abusif art. 336 CO"

# Italienisch
/bettercallclaude:research "Disdetta abusiva art. 336 CO"
```

### Weiterführende Dokumentation

- [Command-Referenz](../command-reference.md) — Vollständige Dokumentation aller 18 Commands
- [Workflow juristische Recherche](../workflows/research-precedents.md) — Vertiefte Darstellung der BGE-Recherchemethodik
- [Workflow Fallstrategie](../workflows/case-strategy.md) — Ausführliche Anleitung zur Strategieentwicklung
- [Installationsanleitung](../INSTALL.md) — Einrichtung, MCP Server-Konfiguration, Fehlerbehebung

---

## In diesem Tutorial demonstrierte BCC-Funktionen

| Funktion | Schritt | Verwendete Komponenten |
|----------|---------|------------------------|
| Briefing-Sitzung | Schritt 1 | briefing-coordinator Agent, Skill zur Gerichtsstandserkennung |
| Juristische Recherche | Schritt 2 | researcher Agent, bge-search MCP, entscheidsuche MCP, fedlex-sparql MCP |
| Zitatverifikation | Schritt 2 | citation-specialist Agent, legal-citations MCP |
| Kontradiktorische Analyse | Schritt 3 | advocate + adversary + judicial-analyst Agents, adversarial-analysis Skill |
| Prozessstrategie | Schritt 4 | case-strategist + risk-analyst + procedure-specialist Agents, strategy Skill |
| Dokumententwurf | Schritt 5 | drafter Agent, swiss-legal-drafting Skill |
| Ergebniskonsolidierung | Schritt 6 | summarizer Agent |
| Mehrsprachige Unterstützung | Durchgehend | Spracherkennung, DE/FR/IT/EN Rechtstermini |
| Gerichtsstandsweiterleitung | Durchgehend | swiss-jurisdictions Skill, Kantonserkennung |

**Total**: 10 Agents, 5 Commands, 4 Skills, 4 MCP Server in einem einzigen Fall demonstriert.
