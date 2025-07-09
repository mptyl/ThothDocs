# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the documentation repository for **Thoth**, an AI-powered SQL generation application built with Django backend and frontend. The project aims to facilitate AI-generated SQL queries by providing enhanced context through schema documentation, hints, and example questions.

## Documentation Structure

This is a **MkDocs-based documentation site** with Material theme. The documentation is organized in a three-tier structure:

1. **Install and Basic Setup** - Installation methods (Docker, local, sources) and initial configuration
2. **Getting Started** - Frontend setup and process execution
3. **Reference Manual** - Comprehensive setup guides, workflow documentation, and feature references

## Key Architecture Components

### Core Concepts
- **Hints**: Contextual information stored in vector database to help AI understand database schema and jargon
- **Questions**: Three-part documents (question, hint, SQL) that serve as few-shot examples for SQL generation
- **Comments**: Documentation for database schema elements
- **Preprocessing**: Essential step to enhance database schema with metadata for better AI comprehension

### Documentation Organization
- `docs/1-install_and_basic_setup/` - Installation and initial setup procedures
- `docs/2-getting_started/` - User onboarding and basic usage
- `docs/3-reference_manual/` - Detailed technical documentation including:
  - Authentication (groups, users, profiles)
  - AI Models and Agents configuration
  - SQL Database setup (databases, tables, columns, relationships)
  - Workflow processes
  - Logging with Logfire integration

## Common Commands

### Documentation Development
```bash
# Serve documentation locally
mkdocs serve

# Build documentation
mkdocs build

# Deploy documentation
mkdocs gh-deploy
```

### Content Management
- All documentation is written in **Italian** as specified in the project requirements
- Use numbered section formatting (1, 1.1, 1.2, 2, 2.1, etc.) with " - " separator
- Focus on user manual content rather than technical implementation details

## Key Files

- `mkdocs.yml` - Main configuration file defining site structure and navigation
- `docs/index.md` - Home page with project overview in Italian
- `docs/assets/` - Images and static assets
- `istruzioni*.txt` - Instruction files for different documentation sections

## Related Projects

This documentation covers the Thoth ecosystem which includes:
- Main Thoth application: `/Users/mp/DjangoExperimental/Thoth` (Django backend)
- ThothSL: `/Users/mp/PythonProjects/ThothSL` (Related component)

## Documentation Standards

- Write all content in English
- Structure content as user manual, avoiding technical implementation details
- Use clear chapter and subsection numbering
- Focus on functionality and purpose rather than code specifics
- Include Mermaid diagrams support where applicable