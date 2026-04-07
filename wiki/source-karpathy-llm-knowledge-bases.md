---
type: source
tags: [ki]
sources: ["[[raw/Thread by @karpathy]]"]
created: 2026-04-07
updated: 2026-04-07
---

# Karpathy: LLM Knowledge Bases

**Quelle:** Thread von [[andrej-karpathy]] auf X, 2026-04-02

## Zusammenfassung

Andrej Karpathy beschreibt sein Workflow-Pattern für persönliche Wissensbasen mit LLMs — das Grundkonzept hinter diesem Wiki. Statt [[rag]] nutzt er LLMs, um aus Rohdaten ein strukturiertes, verlinktes Markdown-Wiki zu kompilieren und inkrementell zu pflegen.

## Kernaussagen

1. **Daten-Ingest**: Quellen in `raw/` sammeln, LLM kompiliert daraus ein Wiki mit Zusammenfassungen, Backlinks, Konzeptseiten und Querverweisen
2. **IDE**: [[obsidian]] als Frontend zum Browsen — das LLM schreibt und pflegt alle Wiki-Seiten, der Mensch editiert selten direkt
3. **Q&A ohne RAG**: Bei ~100 Artikeln / ~400K Wörtern reicht ein LLM-gepflegter Index statt Embedding-basiertem [[rag]]. Das LLM liest relevante Seiten on demand.
4. **Outputs zurückfilen**: Antworten als Markdown, [[marp]]-Slides oder matplotlib-Charts rendern und zurück ins Wiki legen — Wissen kumuliert
5. **Linting**: LLM-Health-Checks über das Wiki: Widersprüche finden, fehlende Daten mit Web-Suche ergänzen, neue Verbindungen vorschlagen
6. **Zusatztools**: Kleine Tools vibe-coden (z.B. Suchmaschine über das Wiki), nutzbar via CLI als LLM-Tool
7. **Ausblick**: Bei wachsendem Repo — synthetische Datengenerierung + Finetuning, damit das LLM das Wissen "in den Gewichten" hat statt nur im Kontext

## Zitat

> "You rarely ever write or edit the wiki manually, it's the domain of the LLM."

## Relevanz

Dieses Wiki folgt exakt dem von Karpathy beschriebenen Pattern. Sein Thread ist die konzeptuelle Grundlage für die gesamte Architektur.
