# KB-CREATOR Agent Workflow Documentation

## Overview
The KB-CREATOR agent implements a local-first Extract → Prune → Evaluate → Index → Persist pipeline using Playwright MCP for browser automation and zero external dependencies.

## Workflow Steps

### 1. Extract (Browser Navigation)
```javascript
// Navigate to target URL
await navigate_page({url: targetUrl, type: "url"});

// Capture accessibility tree
const snapshot = await take_snapshot();
```

### 2. Prune (DOM Filtering)
```javascript
// Execute DOM pruning script
const prunedContent = await evaluate_script({
  function: fs_read("~/.kiro/steering/dom-pruning.js")
});
```

### 3. Evaluate (Relevance Scoring)
```javascript
// Apply relevance criteria from kb-standards.md
const relevanceScore = calculateRelevanceScore(
  prunedContent.textContent,
  prunedContent.codeBlocks,
  prunedContent.title
);

// Reject if score < 6
if (relevanceScore < 6) {
  return {status: "rejected", reason: "Low technical relevance"};
}
```

### 4. Index (Deduplication Check)
```bash
# Search for existing content
kiro-cli knowledge search --query "extracted topic keywords"

# Generate content hash
content_hash=$(echo -n "$extracted_text" | shasum -a 256 | cut -d' ' -f1)

# Compare with existing entries
if existing_hash_matches; then
  offer_update_check();
fi
```

### 5. Persist (Knowledge Base Storage)
```bash
# Configure target knowledge base path
TARGET_KB_PATH="/Users/bryanchasko/Code/kiro-cli-custom-agent-screenpal-video-transcription/knowledge"

# Create structured markdown file in target directory
cat > "${TARGET_KB_PATH}/temp_knowledge_entry.md" << EOF
# ${title}

## Summary
${three_sentence_summary}

## Key Entities
${extracted_entities}

## Usage Examples
${code_blocks_and_examples}

## Metadata
- Source: ${sourceUrl}
- Extracted: ${timestamp}
- Hash: ${contentHash}
- Relevance: ${relevanceScore}/10
- DOM Nodes: ${nodeCountBefore} → ${nodeCountAfter}
EOF

# Add to knowledge base using /knowledge command in chat session
# /knowledge add --name "${digest_name}" --path "${TARGET_KB_PATH}/temp_knowledge_entry.md" --index-type Best
```

## Knowledge Base Commands (In Chat Session)

### Add Knowledge Entry
```bash
/knowledge add --name "AWS Lambda Introduction" --path "/path/to/knowledge/entry.md" --index-type Best
```

### Search Existing Knowledge
```bash
/knowledge show
# Search for duplicates before adding new content
```

### Remove Knowledge Entry
```bash
/knowledge remove "entry-name"
```

## Target Path Configuration

### Public Example (Screenpal Agent)
```bash
TARGET_KB_PATH="/Users/bryanchasko/Code/kiro-cli-custom-agent-screenpal-video-transcription/knowledge"
```

### Private Production Pattern
```bash
TARGET_KB_PATH="/Users/bryanchasko/Code/private-agent-knowledge-bases/my-company-agent/knowledge"
```

### Environment Variable Pattern
```bash
export KB_CREATOR_TARGET_PATH="/path/to/private/knowledge/base"
TARGET_KB_PATH="${KB_CREATOR_TARGET_PATH:-/default/public/path}"
```

## Error Handling

### Browser Navigation Failures
1. Retry navigation once
2. Fallback to web_fetch for simple HTML
3. Log failure and continue with remaining URLs

### DOM Processing Errors
1. Log JavaScript execution warnings
2. Continue with basic HTML extraction
3. Reduce DOM complexity if > 10,000 nodes

### Content Quality Issues
1. Reject non-technical content (score < 6)
2. Log rejection reason in Relevance Report
3. Continue processing remaining URLs

## Performance Metrics

Track and report:
- DOM processing time (ms)
- Node count reduction (before/after)
- Content extraction size (characters)
- Accessibility tree depth
- Code blocks discovered
- Relevance score distribution

## Reporting Format

### Relevance Report
```
KB-CREATOR Extraction Report
============================

Processed: 5 URLs
Added: 3 entries
Rejected: 1 entry (low relevance)
Duplicates: 1 entry (update available)

DOM Processing:
- Total nodes processed: 15,420
- Nodes removed: 11,180 (72.5%)
- Average processing time: 850ms

Content Quality:
- High relevance (8-10): 2 entries
- Medium relevance (6-7): 1 entry
- Rejected (0-5): 1 entry

Performance:
- Total extraction time: 4.2s
- Average per URL: 840ms
- Code blocks extracted: 12
```

## Integration Points

### Docker-Gateway
- Containerized browser instances for scalability
- Parallel processing of multiple URLs
- Resource isolation and cleanup

### Strands SDK
- Local orchestration framework
- Task queuing and management
- Performance monitoring and metrics

### Knowledge Base
- Semantic indexing with all-minilm-l6-v2
- Natural language query support
- Structured metadata storage
- Content versioning and updates
