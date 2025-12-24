# KB-CREATOR Agent for Kiro CLI

A specialized Kiro CLI agent that orchestrates knowledge base creation by integrating web crawling via Firecrawl MCP and semantic indexing.

## Features

- **Multi-domain Knowledge Aggregation** - Crawl and index content from multiple sources
- **Intelligent Deduplication** - Strict duplicate detection with update-check hooks
- **Semantic Indexing** - Uses all-minilm-l6-v2 for natural language queries
- **Technical Content Filtering** - Relevance scoring and strict filtering criteria
- **Rich Metadata Storage** - Timestamps, content hashes, relevance scores, extraction methods
- **Flexible Crawling** - Shallow crawl by default; 3-level deep crawl when specified
- **Silent Mode Reporting** - Concise summaries with Relevance Reports

## Quick Start

1. Install Firecrawl MCP:
   ```bash
   npx -y firecrawl-mcp
   ```

2. Enable knowledge feature:
   ```bash
   kiro-cli settings chat.enableKnowledge true
   kiro-cli settings knowledge.indexType Best
   ```

3. Launch KB-CREATOR agent:
   ```bash
   kiro-cli chat --agent kb-creator
   ```

## Architecture

- **Agent Config**: `~/.kiro/agents/kb-creator.json`
- **Steering Docs**: `~/.kiro/steering/kb-standards.md`, `~/.kiro/steering/AGENTS.md`
- **Workflow**: Fetch → Evaluate → Query → Persist pipeline

## Implementation Plan

See `steering-agent-plan.md` for detailed task breakdown and progress tracking.

## Deliverables

- Agent configuration (kb-creator.json)
- Technical relevance criteria (kb-standards.md)
- Operational logic (AGENTS.md)
- Workflow documentation
- Example knowledge entries
