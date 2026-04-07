---
type: concept
tags: [ki]
sources: ["[[raw/Thread by @karpathy]]"]
created: 2026-04-07
updated: 2026-04-07
---

# LLM Wiki Pattern

Architekturmuster für persönliche Wissensbasen: Ein LLM kompiliert aus Rohdaten ein strukturiertes, verlinktes Markdown-Wiki und pflegt es inkrementell. Der Mensch kuratiert Quellen und stellt Fragen — das LLM erledigt die gesamte Buchführung.

## Drei Schichten

| Schicht | Inhalt | Besitzer |
|---------|--------|----------|
| **Raw** | Quelldokumente (Artikel, Papers, Bilder) | Mensch (immutable) |
| **Wiki** | LLM-generierte Markdown-Seiten mit Querverweisen | LLM |
| **Schema** | Regeln, Konventionen, Workflows (z.B. CLAUDE.md) | Beide |

## Operationen

- **Ingest** — Quelle einlesen, zusammenfassen, in bestehendes Wiki integrieren, Querverweise aktualisieren
- **Query** — Fragen gegen das Wiki stellen, Antworten mit Zitationen, gute Antworten zurück ins Wiki filen
- **Lint** — Gesundheitscheck: Widersprüche, Orphans, fehlende Seiten, veraltete Claims

## Warum es funktioniert

> Der mühsame Teil einer Wissensbasis ist nicht das Lesen oder Denken — es ist die Buchführung.

LLMs werden nicht müde, vergessen keine Querverweise und können 15 Dateien in einem Durchgang aktualisieren. Die Pflege kostet praktisch nichts.

## Skalierung

[[Andrej-Karpathy]] berichtet, dass bei ~100 Artikeln / ~400K Wörtern ein LLM-gepflegter Index ohne Embedding-basiertes [[rag]] ausreicht. Bei größerem Umfang: lokale Suchmaschine (z.B. qmd) oder Finetuning.

## Herkunft

Konzeptuell verwandt mit Vannevar Bushs Memex (1945) — eine private, kuratierte Wissenssammlung mit assoziativen Pfaden. Der fehlende Baustein war die automatische Pflege. LLMs lösen dieses Problem.

## Siehe auch

- [[andrej-karpathy]]
- [[obsidian]]
- [[rag]]
