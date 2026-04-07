---
type: concept
tags: [ki]
sources: ["[[raw/Thread by @karpathy]]"]
created: 2026-04-07
updated: 2026-04-07
---

# RAG (Retrieval-Augmented Generation)

Standard-Ansatz für LLMs + Dokumente: Dokumente werden in Chunks aufgeteilt, als Embeddings gespeichert, und bei jeder Frage werden relevante Chunks abgerufen. Das LLM generiert dann eine Antwort auf Basis der gefundenen Chunks.

## Grenzen

- Wissen wird bei jeder Frage neu zusammengesetzt — keine Akkumulation
- Subtile Fragen, die mehrere Dokumente synthetisieren erfordern, scheitern oft
- Keine persistenten Querverweise, keine aufgebaute Synthese

## Alternative: LLM Wiki

Das [[llm-wiki-pattern]] kompiliert Wissen einmal und hält es aktuell, statt es bei jeder Query neu abzuleiten. [[Andrej-Karpathy]] berichtet, dass bei ~100 Quellen ein LLM-gepflegter Index ausreicht — kein Embedding-basiertes RAG nötig.

> [!question] Offene Frage
> Ab welcher Skalierung wird RAG oder eine lokale Suchmaschine (z.B. qmd) nötig? Karpathy nennt ~100 Artikel als funktionierend ohne RAG.

## Siehe auch

- [[llm-wiki-pattern]]
