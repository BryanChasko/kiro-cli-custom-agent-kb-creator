# Contributing to KB-CREATOR Agent

## Local-First Development Principles

This project follows a strict local-first architecture to ensure zero external API costs and complete self-hosting capabilities. **KB-CREATOR is designed to populate knowledge bases for other custom Kiro CLI agents, not store knowledge within this repository.**

### Knowledge Base Separation Strategy

- **This Repository**: Contains KB-CREATOR agent logic, configuration, and operational steering
- **Target Repositories**: Separate directories/repos for each agent's knowledge base
- **Example Structure**:
  ```
  ~/Code/
  ├── GHeL-Gemini/                                    # KB-CREATOR (this repo)
  └── kiro-cli-custom-agent-screenpal-*/             # Target knowledge bases
      └── knowledge/                                  # Populated by KB-CREATOR
  ```

### Privacy Best Practices

- **Public Examples**: Use for demonstration and open-source agents
- **Private Production**: Keep proprietary knowledge bases in private repositories
- **Configurable Paths**: Design workflows to easily redirect to private directories
- **Access Control**: Separate agent logic (public) from sensitive data (private)

### Core Technologies
- **Playwright MCP**: Primary browser automation engine
- **Fetch MCP**: Fallback for simple HTTP requests  
- **Kiro CLI**: Agent orchestration and knowledge management
- **Docker-Gateway**: Scalable containerized processing
- **Strands SDK**: Local orchestration framework

### Development Workflow

1. **Setup Local Environment**
   ```bash
   # Enable knowledge feature
   kiro-cli settings chat.enableKnowledge true
   kiro-cli settings knowledge.indexType Best
   
   # Launch agent for testing
   kiro-cli chat --agent kb-creator
   ```

2. **Browser Automation Guidelines**
   - Use `take_snapshot` for accessibility tree extraction
   - Prioritize inner text over visual elements
   - Use `evaluate_script` for DOM manipulation and pruning
   - Fallback to `web_fetch` only when browser automation fails

3. **Content Processing Standards**
   - Extract clean, structured data from accessibility trees
   - Remove navigation, ads, and non-technical elements via DOM pruning
   - Generate SHA-256 hashes from extracted inner text
   - Track DOM node counts before/after processing

### Code Quality Requirements

- **Zero External Dependencies**: No paid SaaS or external APIs
- **Accessibility-First**: Prioritize a11y tree and inner text extraction
- **Performance Tracking**: Log DOM processing statistics and extraction metrics
- **Error Handling**: Graceful fallbacks for browser automation failures
- **Deduplication**: Content hash comparison before knowledge base additions

### Testing Guidelines

1. **Local Browser Testing**
   - Test navigation to various technical documentation sites
   - Validate DOM pruning removes non-technical content
   - Verify accessibility tree extraction captures structured data

2. **Content Quality Validation**
   - Ensure relevance scoring filters non-technical content
   - Validate structured markdown output format
   - Test deduplication with content hash comparison

3. **Performance Benchmarking**
   - Measure DOM processing time and node count reduction
   - Track memory usage during large page extraction
   - Validate scalability with Docker-Gateway orchestration

### File Structure

```
# KB-CREATOR Agent (this repository)
~/.kiro/agents/kb-creator.json     # Agent configuration
~/.kiro/steering/kb-standards.md   # Technical relevance criteria
~/.kiro/steering/AGENTS.md         # Operational logic
steering-agent-plan.md             # Implementation tracking
README.md                          # Project overview
CONTRIBUTING.md                    # This file

# Target Knowledge Bases (separate repositories)
../kiro-cli-custom-agent-*/
├── README.md                      # Agent-specific documentation
├── knowledge/                     # Knowledge base populated by KB-CREATOR
│   ├── api-docs/                 # API documentation
│   ├── tutorials/                # Implementation guides
│   └── best-practices/           # Industry standards
└── .kiro/agents/                 # Agent configuration (optional)
```

### Contribution Process

1. **Feature Development**
   - Update steering documents first
   - Modify agent configuration as needed
   - Test with local browser automation
   - Update implementation plan progress

2. **Bug Fixes**
   - Identify root cause in browser automation or content processing
   - Test fix with multiple URL types
   - Update error handling documentation

3. **Performance Improvements**
   - Benchmark current performance
   - Implement optimization
   - Validate improvement with metrics
   - Update documentation

### Docker-Gateway Integration

When contributing to orchestration features:
- Use containerized browser instances for scalability
- Integrate with Strands SDK for local processing
- Maintain zero external API dependencies
- Test with multiple concurrent extraction tasks

### Reporting Issues

Include in bug reports:
- URL that failed processing
- Browser automation logs
- DOM processing statistics
- Expected vs actual content extraction
- Local environment details (OS, browser version)

### Local Development Commands

```bash
# Test agent configuration
kiro-cli chat --agent kb-creator

# Check knowledge base status  
kiro-cli knowledge show

# Validate steering documents
cat ~/.kiro/steering/AGENTS.md
cat ~/.kiro/steering/kb-standards.md

# Monitor extraction performance
# (Add logging to evaluate_script calls)
```
