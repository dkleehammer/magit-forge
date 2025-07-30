# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Magit Forge is an Emacs Lisp package that provides Git forge integration for Magit. It allows users to work with issues, pull requests, and other forge features from within Emacs, supporting multiple forge types including GitHub, GitLab, Gitea, Forgejo, and others.

## Development Commands

### Building and Compilation
- `make` or `make all` - Build lisp files and documentation
- `make lisp` - Compile Emacs Lisp files and generate autoloads
- `make redo` - Clean and recompile all lisp files
- `make clean` - Remove compiled files and generated documentation

### Documentation
- `make docs` - Generate all documentation formats (texi, info, html, pdf)
- `make info` - Generate info manual
- `make html` - Generate HTML manual
- `make texi` - Generate texi source from org files

### Development Workflow
- Emacs Lisp files are in `lisp/` directory
- Each `.el` file compiles to a corresponding `.elc` file
- Dependencies between files are defined in `lisp/Makefile`
- Use `make check-declare` to verify function declarations

## Code Architecture

### Core Components

The codebase follows a modular architecture with clear separation of concerns:

**Database Layer** (`forge-db.el`):
- SQLite database using `closql` and `emacsql`
- Stores forge data (repositories, issues, pull requests, etc.)
- Schema versioning and migration support
- Database file location: `~/.emacs.d/forge-database.sqlite`

**Core Functionality** (`forge-core.el`):
- Base classes and fundamental operations
- Forge configuration via `forge-alist`
- Object-oriented design using EIEIO classes
- Integration with Magit's transient UI system

**Data Models**:
- `forge-repo.el` - Repository objects
- `forge-post.el` - Base class for posts (comments, etc.)
- `forge-topic.el` - Base class for topics (issues, PRs)
- `forge-issue.el` - Issue-specific functionality
- `forge-pullreq.el` - Pull request functionality
- `forge-discussion.el` - Discussion/conversation features

**Forge-Specific Implementations**:
- `forge-github.el` - GitHub integration using GraphQL API
- `forge-gitlab.el` - GitLab API integration
- `forge-gitea.el`, `forge-forgejo.el`, `forge-gogs.el` - Gitea-family forges
- `forge-bitbucket.el` - Bitbucket integration
- `forge-semi.el` - Semi-forges (read-only Git hosting)

**UI Components**:
- `forge-commands.el` - Interactive commands and transients
- `forge-topics.el` - Topic list interface using tablist
- `forge-repos.el` - Repository list interface
- `forge-tablist.el` - Common tablist functionality

### Key Dependencies

The project relies heavily on:
- `magit` - Git interface integration
- `closql`/`emacsql` - Database abstraction
- `ghub` - GitHub API client library
- `transient` - Command interface framework
- `llama` - Additional utilities

### Module Dependencies

Files have strict dependency order (defined in `lisp/Makefile`):
1. `forge-db.el` - Database foundation
2. `forge-core.el` - Core classes and utilities
3. `forge.el` - Main entry point
4. Data model files (`forge-repo.el`, `forge-post.el`, etc.)
5. Forge-specific implementations
6. UI and command modules

### Configuration

The main configuration is in `forge-alist` which maps:
- Git hosts to API endpoints
- Forge types to their respective class implementations
- Supported and semi-supported forges

When modifying forge support, update this configuration along with the corresponding forge-specific module.