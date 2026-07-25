## main components:
the CLI - commands, modes, key shortcuts
the harness - the machinery, all the special local files, system & custom tools, prompt assembly, the protocol, *.jsonl, stability gradient, skill injection
the API - auth, KV cache and TTL, rate limiting
the model - the read phase, the write phase, layers, parameters, quantization

## general topics:
high level architecture
usage patterns
costs
auth methods
MCP
skills 
commands
hooks
subagents
agent teams
dynamic workflows
useful built in skills and commands
mid-session changes
caching - 5m vs 1h, on disk
compaction
permissions
rules

# how does using the bedrock vs anthropic infra affect each of the topics?

## instructions:
Don't assume my knowledge level, but prepare questions to all levels from the absolute beginner to expert
Don't use examples from my computer, use general examples
The main point of view should be that the user usually uses models from AWS bedrock and only sometimes Anthropic, so when there is an actual difference, frame it properly
Target Claude Code and the Anthropic models as the basis for harsness based and model based questions

## sources
harness: /Users/urikeselman/Documents/Learn/learn-harness
model: /Users/urikeselman/Documents/Learn/learn-local-llm (don't ask questions about local LLMs but pick the general ones as examples)