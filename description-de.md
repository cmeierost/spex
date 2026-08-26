# Was soll SPEX werden?

SPEX soll eine ausführbare Spezifikationssprache für Businesslogik und verhaltensbezogene UI-Logik werden.

Die zentrale Idee ist, dass eine von den fachlich Verantwortlichen gelesene und freigegebene Spezifikation zur **verbindlichen Quelle des fachlichen Verhaltens** wird. Sie soll dieses Verhalten so präzise beschreiben, dass ein Compiler es deterministisch in den produktiven Business-Kern übersetzen kann. Gleichzeitig soll ihre kontrollierte Syntax nahe genug am Englischen bleiben, damit Fachexperten sie diskutieren und überprüfen können, ohne eine herkömmliche Programmiersprache lernen zu müssen.

SPEX ist deshalb weder gewöhnliche natürliche Sprache noch lediglich ein besserer Prompt. Es ist eine eingeschränkte Sprache, in der jedes gültige Konstrukt eine definierte Semantik haben muss.

## Die Grundidee

In klassischer Softwareentwicklung interpretieren Menschen eine Spezifikation und übersetzen sie in Code. Heutiges AI-orientiertes Spec-Driven Development beschleunigt im Wesentlichen dieselbe Abfolge:

```text
Spezifikation
     ↓
Interpretation
     ↓
Plan / Aufgaben
     ↓
Code
     ↓
Tests / Reviews
```

Bei jedem Schritt können Informationen verloren gehen, Annahmen ergänzt werden oder Anforderungen eine Bedeutung erhalten, die niemand ausdrücklich freigegeben hat. Tests, Reviews und Guardrails können einige falsche Ergebnisse erkennen. Sie schaffen aber weitere Darstellungen, für die ebenfalls gezeigt werden müsste, dass sie die Bedeutung der Spezifikation erhalten.

Damit bleiben zwei Fragen:

> Was genau soll die Software tun?

und

> Woher wissen wir, dass die Software tatsächlich genau das tut?

Das zugrunde liegende Problem hängt davon ab, wie wichtig und vollständig die Spezifikation ist:

- **Das Verhalten ist uns nicht wichtig.** Ein Team kann es bewusst einem Menschen oder einer AI zur freien Umsetzung überlassen.
- **Die Spezifikation ist unvollständig.** Die implementierende Person oder AI muss raten; es lässt sich nicht nachweisen, dass Software einem beabsichtigten Verhalten entspricht, das nie definiert wurde.
- **Die Spezifikation ist präzise.** Es sollte nichts mehr zu interpretieren geben, sodass eine erneute probabilistische Interpretation ein vermeidbares semantisches Risiko erzeugt.

Selbst die intelligenteste AI kann nicht exakt das umsetzen, was beabsichtigt war, wenn diese Absicht nie exakt ausgedrückt wurde.

SPEX konzentriert sich auf den dritten Fall und hilft Menschen gleichzeitig dabei, den zweiten sichtbar zu machen und zu klären:

```text
Menschliche Absicht
     ↓
LLM und formale Werkzeuge
     ↓
Präzise SPEX-Spezifikation
     ↓
Menschliche Freigabe
     ↓
Deterministischer Compiler
     ↓
Produktiver Business-Kern
```

Ein LLM kann Fachexperten befragen, Unklarheiten aufzeigen und SPEX-Formulierungen vorschlagen. Der Compiler und formale Analysewerkzeuge prüfen, was sich aus der Sprachsemantik ableiten lässt, beispielsweise Syntax, Typen, Referenzen, strukturelle Vollständigkeit, Widersprüche, unerreichbare Fälle oder Gegenbeispiele.

Diese Werkzeuge können nicht entscheiden, was fachlich beabsichtigt war. Das LLM darf die Spezifikation vorschlagen, entscheidet aber weder über das erforderliche Verhalten noch über die Sprachsemantik. Der verantwortliche Mensch liest das präzise Ergebnis und entscheidet, ob es die beabsichtigte Bedeutung ausdrückt. Nach der Freigabe wird diese Bedeutung von AI **nicht mehr interpretiert**, sondern durch deterministische Kompilierung erhalten.

## Präzise und trotzdem lesbar

SPEX ist formal, seine Konstrukte sollen sich aber wie kontrolliertes Englisch lesen lassen:

```
An Account has a Balance of Dollars.

When a Withdrawal Request occurs, the result is a Withdrawal:
  - provided that its Requested Amount does not exceed
    its Customer's Account's Balance:
    - the resulting Outcome is successful; and
    - the resulting Balance is its Customer's Account's Balance
      minus its Requested Amount;
  - otherwise:
    - the resulting Outcome is declined; and
    - the resulting Balance is its Customer's Account's Balance.
```

Präzision bedeutet hier nicht lediglich gut formuliertes Englisch. Eine Spezifikation braucht Definitionen, keine Eindrücke. Konstrukte wie Typen, Beziehungen, Bedingungen, Alternativen und Zustandsübergänge müssen ihre Bedeutung durch die Sprache erhalten und nicht durch Interpretation aus dem Kontext. Sobald eine Sprache formal definierte Semantik besitzt, kann ein Compiler statt eines LLMs diese bei der Übersetzung erhalten.

So sollen unklare Regeln, fehlende Fälle und konkurrierende Interpretationen sichtbar werden, bevor sie in einer Implementation verschwinden.

## Menschliche Freigabe und Nachvollziehbarkeit

Alles, was ein LLM oder ein Entwickler vorschlägt, bleibt ein Vorschlag, bis ein verantwortlicher Mensch es akzeptiert. Teile einer Spezifikation können deshalb explizite Zustände wie vorgeschlagen, durch Entwickler geprüft oder fachlich freigegeben erhalten.

Ändert sich freigegebenes Verhalten, verliert es seine Freigabe und muss erneut geprüft werden. Verantwortung und Nachvollziehbarkeit bleiben dadurch direkt mit dem Verhalten verbunden statt mit Kommentaren, Sitzungsnotizen oder Tests rundherum.

Formale Gültigkeit und fachliche Freigabe beantworten unterschiedliche Fragen:

- Der Compiler stellt fest, ob eine Aussage innerhalb der definierten SPEX-Semantik gültig ist.
- Ein Mensch stellt fest, ob diese gültige Aussage die beabsichtigte fachliche Bedeutung ausdrückt.

## Beispiele überprüfen die Spezifikation

Konkrete, typisierte Beispiele ergänzen die Regeln und können direkt mit Fachexperten diskutiert werden. Ihre zentrale Frage ist nicht:

> Hat der Entwickler die Spezifikation richtig implementiert?

sondern:

> Haben wir das gewünschte Verhalten richtig spezifiziert?

Beispiele werden mit derselben SPEX-Semantik wie die Regeln ausgewertet. Sie dürfen fehlendes Verhalten nicht stillschweigend ergänzen und nicht zu einer zweiten, versteckten Spezifikation werden. Dieselben freigegebenen Beispiele können später gegen die integrierte Anwendung ausgeführt werden, um zu prüfen, ob Persistenz, Kommunikation, Darstellung und andere Infrastruktur das spezifizierte Verhalten erhalten.

Tests bleiben wichtig, müssen aber nicht länger eine probabilistische Übersetzung bereits definierten fachlichen Verhaltens kompensieren. Andernfalls verschiebt sich das ursprüngliche Vertrauensproblem lediglich zu einer weiteren Frage: Wer überprüft, ob die Tests selbst tatsächlich ausdrücken, was die Spezifikation sagt?

## Businesslogik und verhaltensbezogene UI-Logik

SPEX soll Verhalten beschreiben, dessen Ergebnis als richtig oder falsch beurteilt werden kann. Dazu gehören:

- typisierte Fachbegriffe und Werte;
- fachliche Entscheidungen und Validierungen;
- Berechtigungen und Benutzerabsichten;
- Ereignisse, Zustand und Zustandsübergänge;
- beobachtbarer UI-Zustand und erlaubte Interaktionen; sowie
- externe Autoritäten und deklarierte Boundary Ports.

Die konkrete Darstellung ist bewusst ausgeschlossen. SPEX kann definieren, **wann eine Aktion erlaubt ist** und wie sich der Anwendungszustand dadurch ändert, aber nicht, wie ein Button aussieht oder welches UI-Framework ihn darstellt.

## Funktionales Verhalten wird kompiliert, Infrastruktur bleibt flexibel

SPEX definiert, was das System garantieren muss, ohne jeden technischen Mechanismus vorzuschreiben, der diese Garantie umsetzt.

SPEX kann beispielsweise festlegen, dass eine akzeptierte Auszahlung den Kontostand reduziert. Dieser Zustandsübergang gehört zum spezifizierten Verhalten. Ob der neue Kontostand in PostgreSQL, einem Event Store oder einem entfernten Banksystem gespeichert wird, bleibt eine Infrastrukturentscheidung. Muss der Kontostand einen Neustart überleben, ist Dauerhaftigkeit als messbare Anforderung zu deklarieren und von der Infrastruktur zu erfüllen.

Dieselbe Unterscheidung gilt in anderen Bereichen. Die Regel, dass nur eine bestimmte Rolle eine Zahlung freigeben darf, ist fachliches Verhalten. Der Identity Provider, signierte Claims oder der Zugriffskontrollmechanismus, mit dem die Regel durchgesetzt wird, gehören zur technischen Umsetzung.

Damit entsteht eine bewusste Grenze:

> *welches Verhalten das System garantieren muss*

und

> *wie dieses Verhalten implementiert wird.*

Erforderliches Verhalten wird semantisch spezifiziert und kompiliert. Offene Umsetzungsentscheidungen bleiben Menschen und AI überlassen, eingeschränkt durch deklarierte Schnittstellen und messbare Anforderungen.

Diese Anforderungen können Performance, Verfügbarkeit, Dauerhaftigkeit, technische Sicherheitseigenschaften, Deployment und Kosten betreffen. AI bleibt rund um diese Grenze nützlich: vor der Freigabe, wo Interpretation beim Formulieren der Spezifikation hilft, und ausserhalb des kompilierten Kerns, wo sie gute Wege finden kann, bereits definierte Garantien umzusetzen.

## Änderungen beginnen bei SPEX

Ändert sich freigegebenes fachliches Verhalten, wird die SPEX-Quelle geändert, erneut freigegeben und neu kompiliert:

```text
Fachliche Änderung
      ↓
SPEX-Änderung
      ↓
Erneute Freigabe
      ↓
Compiler
      ↓
Aktualisierter Business-Kern
```

Es gibt keine separat gepflegte Businessimplementation, die mit der Spezifikation synchronisiert werden muss. Für das von SPEX abgedeckte Verhalten soll die freigegebene Spezifikation die Quellsprache des produktiven Business-Kerns sein – und nicht ein Modell, gegen das eine andere Implementation unabhängig geschrieben wird.

Damit verschwinden nicht alle Verifikationsprobleme. Compiler und Runtime müssen ihrerseits nachweislich die definierte SPEX-Semantik erhalten, und die vollständige Anwendung benötigt weiterhin Integrationsprüfungen und technische Validierung. Wiederholte, anwendungsspezifische probabilistische Interpretation soll durch eine wiederverwendbare Vertrauensgrenze aus Sprache und Werkzeugkette ersetzt werden.

## Aktueller Stand

SPEX ist eine Forschungsrichtung mit einem explorativen Compilerprototyp, keine fertige Sprache oder produktionsreife Werkzeugkette. Der Prototyp implementiert Teile des vorgesehenen Typsystems und kompiliert eine begrenzte Menge von Spezifikationen in TypeScript, das als Business-Kern der Anwendung dienen soll.

Es gibt noch keine stabile Grammatik, vollständige formale Semantik, keinen allgemeinen Solver, keine Konformitätstestsuite und kein fertiges Ausführungsziel. Die Herausforderung besteht nicht darin, ausführbare Spezifikationen zu erfinden; sie existieren seit Jahrzehnten. Sie besteht darin, sie für die alltägliche Softwareentwicklung praktisch genug zu machen: präzise genug zum Kompilieren, natürlich genug zum Lesen und mit AI-Unterstützung einfach genug zum Schreiben.

Die zentralen Forschungsfragen sind, ob sich realistisches fachliches Verhalten ohne versteckte Implementationsannahmen ausdrücken lässt, ob Fachexperten es effizient überprüfen können und ob der Ansatz Mehrdeutigkeit, Entwicklungsaufwand und LLM-Inferenz in der Praxis reduziert.

## Der Unterschied in einem Satz

**Heutiges AI-orientiertes SDD beschleunigt die traditionelle Interpretation von Spezifikationen in Software. SPEX untersucht, ob diese Interpretation enden kann, sobald funktionales Verhalten präzise definiert und freigegeben ist.**

Das Prinzip ist einfach:

> **AI interpretiert, was noch offen ist.**
>
> **Was bereits entschieden ist, wird kompiliert.**

Die Spezifikation würde dann nicht nur beschreiben, was die Software tun soll.

**Sie wäre die ausführbare, von Menschen freigegebene Definition dessen, was die Software tut.**

## Weiterlesen

- [Spec-Driven Development Gets Your Spec Wrong — Part 1](./articles/spec-driven-development-gets-your-spec-wrong.md)
- [SPEX: Executable Specifications for AI-Assisted Development](./articles/spex-executable-specifications-for-ai-assisted-development.md)
- [Das SPEX-Manifest](./manifest.md)
