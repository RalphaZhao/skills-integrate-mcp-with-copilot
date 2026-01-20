# GitHub Copilot Agents

This directory contains custom GitHub Copilot agent configurations for the Mergington High School Activities project.

## Available Agents

### Dev RAG Agent (`dev-rag-agent.yml`)

A development-focused Retrieval-Augmented Generation (RAG) agent designed to assist with coding tasks by leveraging context from the entire codebase.

#### What is RAG?

RAG (Retrieval-Augmented Generation) combines:
- **Retrieval**: Searching through your codebase, documentation, and git history
- **Augmented**: Adding that context to the AI's knowledge
- **Generation**: Creating responses based on both the AI's training and your specific code

This means the agent provides suggestions that are:
- ✅ Consistent with your existing code style
- ✅ Aware of your project's structure and patterns  
- ✅ Based on your actual implementation details
- ✅ Contextually relevant to your codebase

#### Capabilities

The Dev RAG agent excels at:

1. **Code Understanding**
   - Explain complex code sections
   - Trace data flow through the application
   - Identify dependencies and relationships

2. **Code Generation**
   - Create new API endpoints following existing patterns
   - Generate model classes consistent with the codebase
   - Write functions that match the project's style

3. **Debugging Assistance**
   - Analyze error messages with full context
   - Suggest fixes based on similar code in the repo
   - Identify potential edge cases

4. **Documentation**
   - Generate docstrings matching existing format
   - Create API documentation
   - Update README files with accurate information

5. **Code Review**
   - Suggest improvements following project patterns
   - Identify inconsistencies with existing code
   - Recommend best practices from the codebase

#### How It Works

The agent uses RAG to:

1. **Index your codebase**: Analyzes all Python, HTML, JS, and CSS files
2. **Understand context**: Reads documentation and configuration files
3. **Search intelligently**: Finds relevant code when you ask questions
4. **Generate responses**: Combines AI knowledge with your code context

#### Usage Examples

##### Example 1: Adding a New Feature
```
@dev-rag-agent Add a new endpoint to get activities by schedule day
```

The agent will:
- Search for existing endpoint patterns in `src/app.py`
- Use the same FastAPI decorators and response format
- Follow the existing validation and error handling style

##### Example 2: Debugging
```
@dev-rag-agent Why might the signup endpoint fail for some activities?
```

The agent will:
- Analyze the `signup_for_activity` function
- Check validation logic and error conditions
- Reference the activities data structure

##### Example 3: Documentation
```
@dev-rag-agent Add docstrings to all functions in app.py
```

The agent will:
- Review existing docstring style (if any)
- Generate consistent documentation
- Include parameter types and return values

#### Configuration

The agent is configured with:

- **Context Sources**: All code files, documentation, and configuration
- **Max Context Files**: 10 most relevant files per query
- **Relevance Threshold**: 0.7 (only highly relevant context is used)
- **Model**: GPT-4 for high-quality responses
- **Temperature**: 0.7 (balanced between creativity and accuracy)

#### Integration with MCP

The Dev RAG agent works seamlessly with the GitHub MCP server configured in `.vscode/mcp.json`, providing:

- Access to GitHub issues and pull requests
- Repository search capabilities
- Code review functionality
- Commit history analysis

#### Best Practices

To get the most out of the Dev RAG agent:

1. **Be Specific**: "Add a DELETE endpoint for activities" is better than "add something"
2. **Reference Context**: Mention specific files or functions when relevant
3. **Iterate**: Start with small tasks and build up to complex features
4. **Review Suggestions**: The agent is helpful but always review generated code
5. **Provide Feedback**: If a suggestion isn't quite right, explain why

#### Limitations

The agent:
- Does not modify git configuration or the `.git` directory
- Does not access secrets or environment variables
- Will not make breaking changes without explicit confirmation
- Follows the existing project architecture and patterns

#### RAG Source Configuration

The agent indexes these paths by default:

```yaml
- src/**/*.py       # All Python files
- src/**/*.html     # All HTML templates
- src/**/*.js       # All JavaScript files
- src/**/*.css      # All stylesheets
- **/*.md           # All documentation
- requirements.txt  # Dependencies
- .vscode/**        # Editor configuration
- .github/**        # GitHub workflows and configs
```

You can modify these paths in `dev-rag-agent.yml` to customize what context the agent uses.

#### Troubleshooting

**Agent responses seem generic:**
- Check that RAG sources are correctly configured
- Ensure the codebase paths exist and contain files
- Verify the relevance threshold isn't too high

**Agent isn't finding relevant code:**
- Lower the `relevance_threshold` in settings
- Increase `max_context_files` to see more context
- Check that your question relates to indexed files

**Agent suggestions don't match code style:**
- Ensure the relevant example files are in the RAG sources
- Provide explicit style guidelines in your prompt
- Reference specific files as examples

## Adding More Agents

To add new agents:

1. Create a new `.yml` file in this directory
2. Follow the structure in `dev-rag-agent.yml`
3. Define the agent's purpose, capabilities, and configuration
4. Update this README with usage instructions

## Learn More

- [Model Context Protocol (MCP)](https://modelcontextprotocol.io/)
- [GitHub Copilot Documentation](https://docs.github.com/en/copilot)
- [Skills: Integrate MCP with Copilot](https://github.com/skills/integrate-mcp-with-copilot)
