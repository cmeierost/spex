# Was soll SPEX werden?

SPEX ist eine ausführbare Spezifikationssprache für Business- und UI-Logik.

Die Spezifikation ist dabei die **Single Source of Truth**: Sie beschreibt das gewünschte Verhalten präzise genug, dass ein Compiler daraus deterministisch ausführbare Logik erzeugen kann. Gleichzeitig bleibt die Syntax korrektes Englisch und soll deshalb auch für Business-Experten ohne Programmierkenntnisse lesbar und überprüfbar sein.

## Die Grundidee

In klassischer Softwareentwicklung wird eine Spezifikation von Menschen interpretiert und in Code übersetzt. Agentic Spec-Driven Development mit AI automatisiert im Wesentlichen denselben Prozess:

```text
Specification
     ↓
Interpretation
     ↓
Plan / Tasks
     ↓
Code
     ↓
Tests / Reviews
```

AI kann diesen traditionellen Entwicklungsprozess massiv beschleunigen. Das grundlegende Risiko bleibt jedoch bestehen: Bei jeder Übersetzung können Informationen verloren gehen, Annahmen ergänzt oder Anforderungen anders interpretiert werden als ursprünglich gemeint.

Agentic SDD beseitigt dieses Problem also nicht. Es macht die Übersetzung schneller – und damit können sich auch Missverständnisse und Fehler schneller in Code, Tests und weitere Artefakte fortpflanzen.

SPEX setzt deshalb früher an:

```text
Natural Language
      ↓
LLM assists
      ↓
Precise SPEX Specification
      ↓
Compiler
      ↓
Executable Behaviour
```

Das LLM hilft dabei, eine zunächst unvollständige oder schwammige Beschreibung in präzise SPEX-Syntax zu überführen. Anschliessend prüft der Compiler die Spezifikation auf fehlende Fälle, Widersprüche, Typfehler und andere Unvollständigkeiten.

Das funktionale Verhalten wird danach **nicht mehr interpretiert**, sondern deterministisch kompiliert.

## Präzise und trotzdem lesbar

SPEX ist formal, soll aber weiterhin wie korrektes Englisch gelesen werden können:

```text
An Account has a Balance of Dollars.

When a Withdrawal Request occurs:

- Provided that its Requested Amount does not exceed
  its Customer's Account's Balance:

- The resulting Outcome is successful.

- Otherwise:

- The resulting Outcome is declined.
```

Damit kann dieselbe Spezifikation sowohl vom Compiler als auch vom Business-Experten gelesen werden.

Die Präzision soll die Kommunikation nicht erschweren, sondern verbessern: Unklare Regeln, fehlende Fälle und unterschiedliche Interpretationen werden sichtbar, bevor sie in der Implementation verschwinden.

## Beispiele überprüfen die Spezifikation

Konkrete Beispiele ergänzen die Regeln und können gemeinsam mit Business-Experten diskutiert werden.

Ihre zentrale Frage ist nicht:

> Hat der Entwickler die Spezifikation richtig implementiert?

sondern:

> Haben wir das gewünschte Verhalten richtig spezifiziert?

Damit helfen Beispiele, Missverständnisse in der Spezifikation selbst zu erkennen.

## Business- und UI-Logik

SPEX beschreibt sowohl klassische Businesslogik als auch UI-Logik, zum Beispiel:

- Zustände
- Benutzeraktionen
- Validierungen
- Zustandsübergänge
- fachliche Abhängigkeiten

Die konkrete Darstellung gehört bewusst nicht dazu.

SPEX kann festlegen, **wann eine Aktion erlaubt ist**, aber nicht, wie ein Button aussieht oder mit welchem UI-Framework er dargestellt wird.

## Funktionales Verhalten wird kompiliert, Infrastruktur bleibt frei

SPEX konzentriert sich auf Verhalten, das eindeutig richtig oder falsch sein kann.

Nichtfunktionale Anforderungen wie Performance, Skalierbarkeit oder Verfügbarkeit werden dagegen als messbare Ziele behandelt.

Dadurch entsteht eine klare Trennung:

**Funktionale Anforderungen werden spezifiziert und kompiliert.**

**Nichtfunktionale Anforderungen werden gemessen und durch die technische Implementation erfüllt.**

Die Infrastruktur rund um den Business-Kernel kann deshalb weiterhin konventionell oder mit AI-Unterstützung entwickelt und optimiert werden.

## Änderungen beginnen bei SPEX

Ändert sich eine Businessregel, wird die Spezifikation angepasst und neu kompiliert:

```text
Business Change
      ↓
SPEX Change
      ↓
Compiler
      ↓
Updated Behaviour
```

Es gibt keine separate Businessimplementation, die anschliessend mit der Spezifikation synchronisiert werden muss.

Die Spezifikation bleibt dauerhaft die verbindliche Beschreibung des tatsächlich ausgeführten Verhaltens.

## Der Unterschied in einem Satz

**Agentic SDD mit AI beschleunigt die traditionelle Übersetzung von Spezifikation in Software. SPEX eliminiert diese Übersetzung für das funktionale Verhalten.**

Die Philosophie dahinter ist:

> **Interpretation dort, wo Freiheit sinnvoll ist. Determinismus dort, wo Korrektheit erforderlich ist.**

Die Spezifikation beschreibt damit nicht nur, was die Software tun soll.

**Sie ist die ausführbare Definition dessen, was die Software tut.**
