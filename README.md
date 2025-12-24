# KB-CREATOR Agent for Kiro CLI

A specialized Kiro CLI agent that orchestrates knowledge base creation using local browser automation and semantic indexing. Built with a local-first architecture leveraging Playwright MCP and Fetch tools for zero-cost, self-hosted web content extraction.

## Features

- **Local Browser Automation** - Uses Playwright MCP for DOM extraction and accessibility tree parsing
- **Zero External Dependencies** - No paid SaaS or API keys required
- **Intelligent Content Pruning** - AI-driven DOM filtering to remove navigation and non-technical elements
- **Semantic Indexing** - Uses all-minilm-l6-v2 for natural language queries
- **Technical Content Filtering** - Relevance scoring with strict technical depth criteria
- **Rich Metadata Storage** - Content hashes, DOM node counts, extraction methods, timestamps
- **Accessibility-First Extraction** - Prioritizes inner text and a11y tree over visual elements
- **Docker-Gateway Integration** - Scalable orchestration via containerized browser instances

## Quick Start

1. Enable knowledge feature:
   ```bash
   kiro-cli settings chat.enableKnowledge true
   kiro-cli settings knowledge.indexType Best
   ```

2. Launch KB-CREATOR agent:
   ```bash
   kiro-cli chat --agent kb-creator
   ```

## Local-First Architecture

- **Browser Engine**: Playwright MCP for DOM manipulation and content extraction
- **Content Processing**: Fetch MCP for fallback HTTP requests
- **Orchestration**: Docker-Gateway with Strands SDK integration
- **Storage**: Local knowledge base with semantic indexing
- **Workflow**: Extract → Prune → Evaluate → Index → Persist

## Knowledge Base Separation

KB-CREATOR is designed to populate knowledge bases for **other custom Kiro CLI agents**. Following best practices:

- **Agent Logic**: Stored in this repository (public)
- **Knowledge Bases**: Stored in separate directories/repositories (typically private)
- **Example**: [kiro-cli-custom-agent-screenpal-video-transcription](../kiro-cli-custom-agent-screenpal-video-transcription)

### Directory Structure
```
~/Code/
├── GHeL-Gemini/                                    # KB-CREATOR agent (this repo)
│   ├── ~/.kiro/agents/kb-creator.json             # Agent configuration
│   └── ~/.kiro/steering/                          # Operational logic
└── kiro-cli-custom-agent-screenpal-*/             # Target knowledge bases
    └── knowledge/                                  # Populated by KB-CREATOR
```

### Privacy Considerations
- **Public Example**: Screenpal transcription agent (demonstration)
- **Production Use**: Private repositories for proprietary knowledge bases
- **Configurable Paths**: Easy to redirect to private directories

## Implementation Plan

See `steering-agent-plan.md` for detailed task breakdown and progress tracking.

## Deliverables

- Agent configuration (kb-creator.json)
- Technical relevance criteria (kb-standards.md)
- Operational logic (AGENTS.md)
- Workflow documentation
- Example knowledge entries
