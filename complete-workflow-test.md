# KB-CREATOR Complete Workflow Test

## Test Scenario: Extract AWS Lambda Documentation

This test demonstrates the complete Extract → Prune → Evaluate → Index → Persist workflow.

### Step 1: Launch KB-CREATOR Agent
```bash
kiro-cli chat --agent kb-creator
```

### Step 2: Extract and Process Content
**User Input:**
```
Extract technical content from https://docs.aws.amazon.com/lambda/latest/dg/lambda-intro.html for the Screenpal video transcription knowledge base. Apply relevance scoring and create structured knowledge entry.
```

**Expected Agent Actions:**
1. Use `web_fetch` to extract content from AWS Lambda documentation
2. Apply relevance scoring from `~/.kiro/steering/kb-standards.md`
3. Generate content hash from extracted text
4. Create structured markdown file in target directory
5. Use `/knowledge add` command to index the content

### Step 3: Test Deduplication
**User Input:**
```
Extract the same AWS Lambda URL again to test duplicate detection.
```

**Expected Agent Actions:**
1. Generate content hash from new extraction
2. Compare with existing hash
3. Detect duplicate and offer update check
4. Skip addition if content unchanged

### Step 4: Test Low-Value Content Rejection
**User Input:**
```
Extract content from https://aws.amazon.com/pricing/ and explain why it should be rejected.
```

**Expected Agent Actions:**
1. Extract content using `web_fetch`
2. Apply relevance scoring (should score < 6)
3. Reject content with explanation
4. Log rejection in Relevance Report

### Step 5: Verify Knowledge Base
**User Input:**
```
/knowledge show
```

**Expected Results:**
- Show indexed AWS Lambda documentation
- Display metadata and indexing status
- Confirm semantic indexing (Best) is active

## Success Criteria

### Content Quality
- ✅ High-value technical content extracted and indexed
- ✅ Low-value marketing content rejected
- ✅ Structured markdown format with Summary, Key Entities, Usage Examples
- ✅ Complete metadata including content hash and relevance score

### Deduplication
- ✅ Duplicate detection working via content hash comparison
- ✅ Update-check hook offered for existing content
- ✅ No duplicate entries in knowledge base

### Knowledge Base Integration
- ✅ Content added to separate target directory
- ✅ Semantic indexing (Best) applied
- ✅ Natural language search capabilities enabled
- ✅ Agent-specific knowledge base isolation

### Reporting
- ✅ Relevance Report generated with processing statistics
- ✅ DOM processing metrics (if Playwright available)
- ✅ Content quality breakdown (included/rejected/duplicated)

## File Locations

### Generated Knowledge Entry
```
/Users/bryanchasko/Code/kiro-cli-custom-agent-screenpal-video-transcription/knowledge/aws-lambda-intro.md
```

### Knowledge Base Storage
```
~/.kiro/knowledge_bases/kb-creator_<code>/
├── contexts.json
└── context-id-*/
    ├── data.json
    └── bm25_data.json (if Fast indexing)
```

## Performance Metrics to Track
- Content extraction time
- Relevance scoring accuracy
- Knowledge base indexing speed
- Search query response time
- Memory usage during processing
