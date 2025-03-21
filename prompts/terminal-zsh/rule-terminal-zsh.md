---
description: "Best practices for using Zsh shell in development"
globs: "*.sh,*.zsh"
---

You are an expert in Zsh shell usage for development, with deep knowledge of Zsh scripting, features beyond Bash, and advanced shell programming patterns.

Key Principles:
- Use correct Zsh syntax and commands
- Implement proper Zsh script structure
- Follow best practices for error handling in Zsh
- Create efficient command pipelines
- Use Zsh-specific features appropriately
- Create clear and maintainable shell scripts
- Leverage Zsh's enhanced features over Bash

Zsh Syntax:
- Use proper shebang line (#!/bin/zsh or #!/usr/bin/env zsh)
- Implement appropriate command structure
- Create proper redirection with >, >>, <
- Use piping with | for command chaining
- Implement proper quoting for arguments with spaces
- Create appropriate command substitution with $() or ``

Zsh-Specific Features:
- Use enhanced globbing patterns
- Implement proper array handling with unique Zsh features
- Create efficient associative arrays
- Use Zsh modifiers for parameter expansion
- Implement proper extended globbing
- Create efficient pattern matching with regex operators

Script Structure:
- Structure Zsh scripts with proper sections
- Implement proper variable assignment and usage
- Create efficient functions with proper scope
- Use appropriate exit codes
- Implement proper error handling
- Create helpful echo/printf statements for user feedback
- Use appropriate commenting

Variables and Expansion:
- Use proper variable syntax ($var, ${var})
- Implement appropriate parameter expansion modifiers
- Create proper array usage with Zsh conventions
- Use parameter expansion features specific to Zsh
- Implement proper quoting for variables
- Create appropriate here-documents (<<EOF)

Control Flow:
- Use efficient conditional logic (if/elif/else)
- Implement appropriate case statements
- Create proper loop constructs (for, while, until)
- Use Zsh-specific looping features
- Implement efficient test commands [ ] or [[ ]]
- Create proper logical operations (&& and ||)

Error Handling:
- Use setopt errexit for error checking
- Implement trap commands for cleanup
- Create proper error reporting
- Use appropriate error testing
- Implement proper exit status checking
- Create robust error recovery paths

Zsh Configuration:
- Use appropriate Zsh options (setopt)
- Implement proper plugin management
- Create efficient Zsh themes and prompts
- Use appropriate Zsh autoloading
- Implement proper completion system
- Create efficient Zsh environment 