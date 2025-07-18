# Contributing to Startup-toranomaki

First of all, thank you for considering contributing to Startup-toranomaki! 🙌

This document provides guidelines and instructions for contributing to this project. By participating in this project, you agree to abide by its terms.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How Can I Contribute?](#how-can-i-contribute)
  - [Reporting Bugs](#reporting-bugs)
  - [Suggesting Enhancements](#suggesting-enhancements)
  - [Adding Content](#adding-content)
  - [Improving Documentation](#improving-documentation)
  - [Contributing to Localization](#contributing-to-localization)
- [Contribution Workflow](#contribution-workflow)
  - [Setting Up the Development Environment](#setting-up-the-development-environment)
  - [Creating a Pull Request](#creating-a-pull-request)
  - [Pull Request Review Process](#pull-request-review-process)
- [Style Guidelines](#style-guidelines)
  - [Documentation Style](#documentation-style)
  - [Markdown Style](#markdown-style)
  - [Code Examples Style](#code-examples-style)

## Code of Conduct

This project and everyone participating in it is governed by our Code of Conduct. By participating, you are expected to uphold this code. Please report unacceptable behavior to the project maintainers.

## How Can I Contribute?

### Reporting Bugs

If you find a bug in the documentation or code examples:

1. Check the [GitHub Issues](https://github.com/Azure/Startup-toranomaki/issues) to see if the bug has already been reported.
2. If not, create a new issue using the bug report template.
3. Provide detailed information about the bug, including steps to reproduce, expected behavior, and actual behavior.

### Suggesting Enhancements

If you have an idea for an enhancement:

1. Check the [GitHub Issues](https://github.com/Azure/Startup-toranomaki/issues) to see if the enhancement has already been suggested.
2. If not, create a new issue using the feature request template.
3. Clearly describe your suggestion, providing as much context as possible.

### Adding Content

If you want to add new content:

1. Ensure the content is relevant to startups using Microsoft Azure and AI technologies.
2. Follow the existing structure and style of the repository.
3. Include practical examples and actionable insights where applicable.
4. Provide references to official Microsoft documentation when necessary.

### Improving Documentation

If you want to improve existing documentation:

1. Check for typos, grammar issues, or unclear explanations.
2. Update outdated information.
3. Enhance explanations with additional context or examples.

### Contributing to Localization

If you want to help with localization:

> **Important:** For i18n content, since we perform automatic translation, please add main content in English.

1. Check the existing localized content in the `Localization` directory.
2. If you're adding new localized content, ensure it follows the same structure as the English version.
3. If you're improving existing localized content, make sure it remains consistent with the English version.
4. For new languages, create a new directory under `Localization` using the appropriate language code (e.g., `fr_fr` for French).

## Contribution Workflow

### Setting Up the Development Environment

1. Fork the repository on GitHub.
2. Clone your fork locally:
   ```
   git clone https://github.com/YOUR_USERNAME/Startup-toranomaki.git
   cd Startup-toranomaki
   ```
3. Add the original repository as an upstream remote:
   ```
   git remote add upstream https://github.com/Azure/Startup-toranomaki.git
   ```
4. Create a new branch for your changes:
   ```
   git checkout -b feature/your-feature-name
   ```

### Creating a Pull Request

1. Make your changes in your branch.
2. Test your changes to ensure they work as expected.
3. Commit your changes with a clear commit message:
   ```
   git commit -m "Add detailed description of changes"
   ```
4. Push your branch to your fork:
   ```
   git push origin feature/your-feature-name
   ```
5. Open a pull request from your branch to the original repository's `main` branch.
6. Provide a clear description of the changes and reference any related issues.

### Pull Request Review Process

1. Maintainers will review your pull request.
2. They may request changes or suggest improvements.
3. Once the pull request is approved, it will be merged into the main codebase.

## Style Guidelines

### Documentation Style

- Use clear and concise language.
- Avoid jargon or explain technical terms when first used.
- Use active voice rather than passive voice.
- Focus on providing actionable insights and practical guidance.

### Markdown Style

- Use ATX-style headers with a space after the hash marks (e.g., `# Header` not `#Header`).
- Use dashes `-` for unordered lists.
- Use fenced code blocks (``` language) with the appropriate language specified.
- Use reference-style links when possible for better readability of the Markdown source.

### Code Examples Style

- Include comments to explain complex or non-obvious code.
- Follow best practices for the specific language or platform.
- Keep examples concise and focused on demonstrating a specific concept.
- Include error handling in your examples when appropriate.

Thank you for contributing to Startup-toranomaki! Your efforts help make this resource better for startups around the world.