# KB-CREATOR End-to-End Test

## Test 1: Knowledge Base Setup
Testing knowledge feature and basic workflow with available tools.

### Step 1: Verify Knowledge Feature
```bash
kiro-cli settings chat.enableKnowledge
kiro-cli settings knowledge.indexType
```

### Step 2: Test Content Extraction with web_fetch
Using web_fetch as primary tool since Playwright MCP may not be configured.

### Step 3: Test Relevance Scoring
Apply kb-standards.md criteria to extracted content.

### Step 4: Test Knowledge Base Addition
Add structured content to target knowledge base directory.

## Test URLs
- High-value: https://docs.aws.amazon.com/lambda/latest/dg/lambda-intro.html
- Low-value: https://aws.amazon.com/pricing/

## Expected Results
- High-value content extracted and added to knowledge base
- Low-value content rejected with relevance score < 6
- Metadata generated with content hash and processing stats
- Files created in target directory: /Users/bryanchasko/Code/kiro-cli-custom-agent-screenpal-video-transcription/knowledge/
