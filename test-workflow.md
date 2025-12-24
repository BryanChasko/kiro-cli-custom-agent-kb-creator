# KB-CREATOR Workflow Test

## Test URLs for Validation

### High-Value Technical Content (Should be included)
- https://docs.aws.amazon.com/lambda/latest/dg/lambda-intro.html
- https://kubernetes.io/docs/concepts/overview/what-is-kubernetes/
- https://docs.docker.com/get-started/

### Low-Value Content (Should be rejected)
- https://aws.amazon.com/pricing/
- https://kubernetes.io/
- https://docker.com/company/

## Workflow Steps to Test

1. **Extract**: Navigate to URL and capture accessibility tree
2. **Prune**: Remove navigation/ads using DOM pruning script
3. **Evaluate**: Apply relevance scoring from kb-standards.md
4. **Index**: Check for duplicates with knowledge search
5. **Persist**: Add to knowledge base with metadata

## Expected Metadata Fields

- timestamp (ISO 8601)
- contentHash (SHA-256 of inner text)
- nodeCountBefore/After
- relevanceScore (0-10)
- extractionMethod (playwright)
- sourceUrl
- accessibilityTreeDepth

## Test Commands

```bash
# Launch KB-CREATOR agent
kiro-cli chat --agent kb-creator

# Test with high-value URL targeting Screenpal knowledge base
"Extract Screenpal documentation to /Users/bryanchasko/Code/kiro-cli-custom-agent-screenpal-video-transcription/knowledge/"

# Test deduplication
"Extract the same URL again to test duplicate detection"

# Test low-value URL rejection  
"Extract content from https://aws.amazon.com/pricing/ and explain why it should be rejected"
```

## Target Knowledge Base Structure

The extracted content should be organized in the separate repository:
```
kiro-cli-custom-agent-screenpal-video-transcription/
└── knowledge/
    ├── screenpal-api/          # API documentation
    ├── transcription-tools/    # Video transcription tools
    ├── workflow-automation/    # Automation patterns
    └── best-practices/        # Industry guidelines
```
