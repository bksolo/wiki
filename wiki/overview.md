---
type: synthesis
tags: [netzwerk, ki, workflow, disaster-recovery]
created: 2026-04-07
updated: 2026-04-08
---

# Überblick

Dieses Wiki ist Brunos persönliche Wissensbasis — ein "Second Brain", das von einem LLM gepflegt wird. Der Mensch kuratiert Quellen und stellt Fragen. Das LLM erledigt die gesamte Pflege: zusammenfassen, verknüpfen, einordnen, aktuell halten.

## Fachgebiete

- **[[netzwerk|Netzwerk-Infrastruktur]]** — Sicheres Heimnetz, Camper-Netzwerk, VLANs, Firewall
- **[[ki|KI & Claude Code]]** — LLM-Projekte, RAG, Prompt Engineering
- **[[workflow|Workflow-Automatisierung]]** — N8N, Shell-Skripte, Systemintegration
- **[[disaster-recovery|Disaster Recovery]]** — Eaton USVs, NUT-Server, Backup-Strategien

## Status

2 Quellen verarbeitet, 9 fachliche Wiki-Seiten erstellt (Stand: 2026-04-08).

### KI & Methodik

Dieses Wiki folgt dem [[llm-wiki-pattern]], beschrieben von [[andrej-karpathy]]. Kernidee: LLM kompiliert und pflegt ein Markdown-Wiki inkrementell — statt Wissen bei jeder Frage neu via [[rag]] zusammenzusetzen. [[obsidian]] dient als Frontend, [[marp]] als optionales Output-Format.

Mit [[source-gemma-4-ollama]] ist nun auch lokale Modell-Ausführung dokumentiert: [[ollama]] dient als Laufzeit, [[gemma-4]] als konkretes Modell für Reasoning-, Coding- und multimodale Workflows.

### Nächste Schritte

- Weitere Quellen zu Netzwerk, KI-Projekten oder Workflow-Automatisierung einpflegen
- Bei ~10 Quellen: ersten Lint-Durchlauf starten
