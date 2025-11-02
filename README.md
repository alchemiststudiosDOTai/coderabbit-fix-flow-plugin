# CodeRabbit Fix Flow Plugin

A Claude Code plugin that provides a systematic workflow for processing CodeRabbit code review feedback with MCP-powered analysis and automated fixes.

## Overview

This plugin introduces the `coderabbit-fix-flow` agent skill that transforms code review feedback into actionable improvements. It combines the power of two cutting-edge MCP servers to analyze issues deeply and research best practices before implementing fixes.

## Why This Plugin?

Code reviews generate valuable feedback, but acting on that feedback systematically can be challenging. This plugin:

- **Captures and archives** CodeRabbit feedback in structured QA documents
- **Analyzes issues** using advanced reasoning to categorize and prioritize
- **Researches best practices** from current web resources and documentation
- **Implements fixes** with minimal code changes and proper validation
- **Tracks progress** with automatic todo management

## Key Features

### Powered by Advanced MCP Servers

#### Sequential Thinking MCP
**What it does:** Provides Claude with enhanced reasoning capabilities through a "chain of thought" approach that breaks down complex problems into logical steps.

**Why it's useful here:** When analyzing CodeRabbit feedback containing multiple issues across different files, Sequential Thinking helps:
- Categorize issues by type (type errors, logic bugs, performance issues, etc.)
- Identify dependencies between fixes (what must be fixed first)
- Plan the optimal order of operations
- Consider edge cases and potential side effects
- Reason through complex type-system constraints

**Real feedback example:** Instead of rushing to fix a type error, the skill uses Sequential Thinking to analyze whether the error indicates a deeper architectural issue, what related code might be affected, and whether a simple type cast or a more fundamental refactor is needed.

#### Exa MCP
**What it does:** Provides AI-powered web search capabilities that understand semantic meaning, not just keywords. Exa searches return highly relevant code examples, documentation, and best practices.

**Why it's useful here:** When fixing code issues, having current, relevant examples is crucial. Exa helps:
- Find language-specific best practices for the exact framework version you're using
- Discover how other developers solved similar type-safety issues
- Locate official documentation for unfamiliar APIs
- Research modern patterns that have replaced deprecated approaches
- Verify that proposed fixes align with current community standards

**Real feedback example:** If CodeRabbit flags a TypeScript type issue with React hooks, Exa searches for "TypeScript React useCallback type inference 2024" and returns actual blog posts, Stack Overflow answers, and documentation showing the current best practice—not outdated solutions from 2018.

### Together They Transform Code Review

1. **Sequential Thinking** analyzes: "This type error in the authentication hook suggests the callback signature doesn't match the context type. Let me break down the type flow..."

2. **Exa** researches: "What's the current best practice for typing context providers with generic constraints in TypeScript 5.x?"

3. **Result**: A targeted fix that solves the root cause, follows modern patterns, and includes explanatory comments.

## Installation

### Prerequisites

1. **Claude Code CLI** - Latest version
2. **Exa API Key** - Get yours from [dashboard.exa.ai/api-keys](https://dashboard.exa.ai/api-keys)
3. **CodeRabbit CLI** - Install from [coderabbit.ai](https://coderabbit.ai)

### Install the Plugin

```bash
# Add the marketplace
/plugin marketplace add tunahorse/coderabbit-fix-flow-plugin

# Install the plugin
/plugin install coderabbit-fix-flow
```

### Configure Exa API Key

Add your Exa API key to your environment:

```bash
# Add to ~/.bashrc, ~/.zshrc, or your shell config
export EXA_API_KEY="your-api-key-here"
```

Or set it temporarily for the current session:

```bash
export EXA_API_KEY="your-api-key-here"
```

### Restart Claude Code

After installation, restart Claude Code to activate the MCP servers:

```bash
# Exit Claude Code and restart
claude code
```

## Usage

### Step 1: Generate CodeRabbit Feedback

Run CodeRabbit on your changes:

```bash
coderabbit --plain
```

### Step 2: Invoke the Skill

Simply tell Claude:

```
Use coderabbit-fix-flow to process the CodeRabbit feedback
```

Or trigger it automatically by providing CodeRabbit output when asking for help with code review issues.

### Step 3: Review and Commit

The skill will:
1. Save feedback to `memory-bank/qa/coderabbit/cr-qa-{timestamp}.md`
2. Analyze issues with Sequential Thinking
3. Research best practices with Exa
4. Create a todo list for systematic fixes
5. Implement fixes one by one
6. Validate each fix with appropriate linters

### What Gets Created

```
your-project/
├── memory-bank/
│   └── qa/
│       └── coderabbit/
│           └── cr-qa-2025-02-11T15-30-45.md  # Archived feedback
└── [your code files with fixes applied]
```

## How It Works

1. **Capture**: CodeRabbit feedback is saved with timestamp
2. **Analyze**: Sequential Thinking categorizes issues by severity and type
3. **Research**: Exa finds current best practices for each issue category
4. **Plan**: Creates a prioritized fix list with dependencies considered
5. **Implement**: Applies minimal code changes with explanatory comments
6. **Validate**: Runs type checkers, linters, and tests as appropriate
7. **Document**: Updates QA document with resolution notes

## MCP Server Details

### Sequential Thinking MCP

- **Package**: `@modelcontextprotocol/server-sequential-thinking`
- **Installation**: Automatic via npx (no local install needed)
- **Purpose**: Enhances Claude's reasoning for complex problem-solving
- **Usage**: Transparent - automatically invoked when skill needs deep analysis

### Exa MCP

- **Package**: `exa-mcp-server`
- **Installation**: Automatic via npx (no local install needed)
- **API Key**: Required (free tier available)
- **Purpose**: Semantic web search for code examples and documentation
- **Usage**: Transparent - automatically invoked when skill needs to research

## Troubleshooting

### Plugin Not Available

If the skill doesn't appear after installation:

```bash
# Check plugin status
/plugin list

# Restart Claude Code
```

### Exa API Key Issues

```bash
# Verify environment variable
echo $EXA_API_KEY

# If empty, set it and restart Claude Code
export EXA_API_KEY="your-key"
```

### MCP Server Not Starting

Check Claude Code logs:

```bash
# Claude Code typically logs to ~/.claude/logs/
tail -f ~/.claude/logs/latest.log
```

## Examples

### TypeScript Type Errors

```
User: I got CodeRabbit feedback about type errors in my React components

Claude (using coderabbit-fix-flow):
1. Saving feedback to memory-bank/qa/coderabbit/cr-qa-2025-02-11T15-30-45.md
2. Using Sequential Thinking to analyze 12 type errors across 5 files
3. Researching TypeScript 5.x best practices with Exa
4. Creating fix plan: Props interface → Hook types → Component signatures
5. Implementing fix 1/12: Updating UserProfile props interface...
```

### Python Linting Issues

```
User: Process this CodeRabbit feedback about my Django views

Claude (using coderabbit-fix-flow):
1. Analyzed with Sequential Thinking: Mix of import ordering, type hints, and exception handling
2. Researched with Exa: Django 5.0 type annotation patterns
3. Implementing systematic fixes following PEP 8 and Django best practices
```

## Benefits

- **Consistency**: Every fix follows researched best practices
- **Context**: Fixes include explanatory comments with reasoning
- **Traceability**: All feedback archived in memory-bank for future reference
- **Learning**: Exa research results show you current patterns to adopt
- **Efficiency**: Automated todo tracking ensures no issue is forgotten

## Contributing

Contributions welcome! Please feel free to submit pull requests or open issues.

## License

MIT License - See LICENSE file for details

## Links

- **GitHub**: [tunahorse/coderabbit-fix-flow-plugin](https://github.com/tunahorse/coderabbit-fix-flow-plugin)
- **Exa AI**: [dashboard.exa.ai](https://dashboard.exa.ai)
- **CodeRabbit**: [coderabbit.ai](https://coderabbit.ai)
- **Sequential Thinking MCP**: [@modelcontextprotocol/server-sequential-thinking](https://www.npmjs.com/package/@modelcontextprotocol/server-sequential-thinking)

## Support

For issues or questions:
1. Check the [Troubleshooting](#troubleshooting) section
2. Review [closed issues](https://github.com/tunahorse/coderabbit-fix-flow-plugin/issues?q=is%3Aissue+is%3Aclosed)
3. Open a [new issue](https://github.com/tunahorse/coderabbit-fix-flow-plugin/issues/new)

---

Built with Claude Code | Powered by Sequential Thinking MCP + Exa MCP