# KB-CREATOR Agent Implementation Plan

**Status**: In Progress  
**Last Updated**: 2025-12-24

## Problem Statement

Design and instantiate a specialized Kiro CLI agent named KB-CREATOR that orchestrates knowledge base creation by integrating the experimental knowledge feature, MCP-based web crawling via Firecrawl, and behavioral steering to transform URLs into structured, persistent knowledge entries across multiple domains.

## Requirements

- Multi-domain knowledge aggregation with strict deduplication
- Shallow crawl by default; 3-level deep crawl when related/sub-links specified
- Semantic indexing (Best/all-minilm-l6-v2) for natural language queries
- Strict technical-content filtering with relevance scoring
- Rich metadata storage (timestamp, content hash, relevance score, extraction method)
- Silent mode reporting (only skipped duplicates + final summary)
- Update-check hooks for duplicate detection
- Agentic RAG pattern for code extraction

## Task Breakdown

### Task 1: Enable Knowledge Feature & Configure Global Settings
- [ ] Activate experimental knowledge feature
- [ ] Set semantic indexing as default (Best)
- [ ] Verify settings persist across sessions
- [ ] Test: `/knowledge show` displays empty knowledge base

### Task 2: Create Steering Documents
- [ ] Create `~/.kiro/steering/kb-standards.md`
  - Technical depth criteria
  - Exclusion rules
  - Code extraction instructions
  - Digestion format
- [ ] Create `~/.kiro/steering/AGENTS.md`
  - Deduplication mandate
  - Media handling
  - Reporting guidelines

### Task 3: Create KB-CREATOR Agent Configuration
- [ ] Create `~/.kiro/agents/kb-creator.json`
  - Firecrawl MCP integration
  - Knowledge tool access
  - File read/write capabilities
  - Bash execution
- [ ] Verify Firecrawl MCP installation
- [ ] Test agent loads: `kiro-cli chat --agent kb-creator`

### Task 4: Implement Workflow Logic
- [ ] Fetch: Use @firecrawl-mcp/scrape for target URL
- [ ] Evaluate: Apply kb-standards.md relevance filter
- [ ] Query: Check existing entries with knowledge search
- [ ] Persist: Invoke knowledge add with structured metadata
- [ ] Implement shallow vs. deep crawl logic
- [ ] Add deduplication hook

### Task 5: Create Relevance Report & Update-Check Hook
- [ ] Generate post-crawl Relevance Report
- [ ] Implement update-check hook for duplicates
- [ ] Store metadata with each entry
- [ ] Test silent mode reporting

### Task 6: Integration & End-to-End Testing
- [ ] Test full pipeline
- [ ] Verify shallow crawl (default)
- [ ] Verify deep crawl (with related-links)
- [ ] Test duplicate detection
- [ ] Test edge cases (non-HTML, large crawls, network errors)
- [ ] Verify knowledge base persistence
- [ ] Test semantic search

## Progress Log

- **2025-12-24**: Repository created with initial documentation

## Next Steps

1. Set up local development environment
2. Begin Task 1: Enable knowledge feature
3. Create steering documents (Task 2)
4. Build agent configuration (Task 3)
