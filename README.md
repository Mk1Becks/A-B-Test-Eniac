# Eniac A/B-Test: Welcher Button bringt die meisten Klicks?

Analyse eines A/B-Tests für die Startseite des (fiktiven) Online-Händlers Eniac, erstellt im Rahmen der Data-Science-Weiterbildung an der WBS Coding School.

## Fragestellung

Der weiße „JETZT SHOPPEN“-Button im Hauptbanner erreicht nur eine Klickrate (CTR) von etwa 2 %. Wäre eine andere Farbe oder ein anderer Text besser? Getestet wurden vier Versionen:

| Version | Farbe | Text |
|---|---|---|
| A (Original) | weiß | JETZT SHOPPEN |
| B | rot | JETZT SHOPPEN |
| C | weiß | ANGEBOTE ANSEHEN |
| D | rot | ANGEBOTE ANSEHEN |

## Experiment-Design

- **Hauptmetrik:** Klickrate = Klicks auf den Button / Besuche der Startseite
- **Zusatzmetriken:** Abbruchrate der verlinkten Seite, Rückkehrrate zur Startseite
- **Nullhypothese:** Alle Versionen haben die gleiche Klickrate.
- **Alternativhypothese:** Mindestens eine Version unterscheidet sich.
- **Signifikanzniveau:** α = 0,05, Power 80 %, minimal nachweisbarer Effekt (MDE) 20 %
- **Testdauer:** 14 Tage (2.–16. November 2021), ca. 25.000 Besuche pro Version

## Vorgehen

1. CSV-Daten der vier Versionen laden (pandas)
2. Klickrate pro Version berechnen und visualisieren
3. Chi-Quadrat-Test über alle Versionen (`scipy.stats.chi2_contingency`)
4. Post-hoc-Tests für alle 6 Paare mit Bonferroni-Korrektur (α = 0,05 / 6 ≈ 0,0083)
5. Zusatzkennzahlen in die Entscheidung einbeziehen

## Ergebnisse

| Version | Klicks | Besuche | CTR |
|---|---|---|---|
| A | 512 | 25.326 | 2,02 % |
| B | 281 | 24.747 | 1,14 % |
| C | 527 | 24.876 | 2,12 % |
| D | 193 | 25.233 | 0,77 % |

![CTR pro Version](https://raw.githubusercontent.com/Mk1Becks/A-B-Test-Eniac/main/images/ctr_per_version.png)

- Der Chi-Quadrat-Test über alle Versionen ist signifikant (p ≈ 2,7e-48): Die Klicks hängen von der Version ab.
- **Rot ist deutlich schlechter als Weiß.** Alle Paarvergleiche mit B oder D sind auch nach Bonferroni-Korrektur signifikant.
- **A gegen C ist nicht signifikant** (p ≈ 0,46). Der Text hat keinen nachweisbaren Einfluss.
- Bei den Zusatzkennzahlen (für B nicht verfügbar) hat A die niedrigste Abbruchrate (ca. 62 % gegen 71 % bei C).

## Empfehlung

Version A (Original) beibehalten. Die roten Buttons sollten nicht eingesetzt werden. Für einen belegten Wechsel zu C wäre ein weiteres Experiment nur mit A und C nötig.

## Einschränkungen

- Der Unterschied zwischen A und C (ca. 5 %) liegt weit unter dem geplanten MDE von 20 % und kann mit diesem Test nicht zuverlässig erkannt werden.
- Die Daten von Version A haben ein anderes Erstellungsdatum (14.09.2021) als die von B, C und D (27.10.2021), was den Vergleich verfälschen könnte.
- Die Werte für Abbruch- und Rückkehrrate sind ungefähr aus Diagrammen der Kursunterlagen abgelesen. Für Version B fehlen sie wegen eines Fehlers in der Datenerfassung.

## Projektstruktur

```
eniac-ab-test/
├── README.md
├── eniac_ab_test_analysis.ipynb   # komplette Analyse
├── data/                          # CSV-Daten der vier Versionen
└── images/                        # Diagramme
```

## Technologien

Python, pandas, SciPy, Google Colab

## Datenquelle

Die Daten stammen aus den Kursunterlagen der WBS Coding School (Fallstudie „Eniacs A/B-Test“).
