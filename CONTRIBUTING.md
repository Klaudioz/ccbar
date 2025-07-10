# Contributing to ccbar

Thank you for your interest in contributing to ccbar! This document provides guidelines for contributing to the project.

## Getting Started

1. **Fork the repository** on GitHub
2. **Clone your fork** locally
3. **Create a feature branch** from `main`
4. **Make your changes** following the guidelines below
5. **Test your changes** thoroughly
6. **Submit a pull request** with a clear description

## Development Environment

### Prerequisites

- macOS 13.0 or later
- Xcode 16+ (tested with Xcode 26 beta 3)
- Swift 6.0+
- Node.js (for ccusage testing)

### Setup

```bash
# Clone your fork
git clone https://github.com/yourusername/ccbar.git
cd ccbar

# Open in Xcode
open SidebarApp.xcodeproj

# Install ccusage for testing
npm install -g ccusage
```

## Code Style

- Follow Swift naming conventions
- Use 2 spaces for indentation
- Run `swift format -i <path>` to format code
- Write clear, self-documenting code
- Add comments for complex logic only

## Architecture Guidelines

- Use **The Composable Architecture (TCA)** for state management
- Use **Dependencies library** for dependency injection
- Prefer **SwiftUI** over AppKit unless necessary
- Follow **Apple Human Interface Guidelines**

## Testing

- Write unit tests for business logic
- Test ccusage integration thoroughly
- Verify UI behavior across different screen sizes
- Test with various ccusage output scenarios

## Pull Request Guidelines

### Before Submitting

- [ ] Code follows project style guidelines
- [ ] All tests pass
- [ ] New features include appropriate tests
- [ ] Documentation is updated if needed
- [ ] Changes are backwards compatible

### PR Description

Please include:

- **What** changes were made
- **Why** the changes were necessary
- **How** to test the changes
- **Screenshots** for UI changes
- **Breaking changes** if any

### Example PR Title

```
feat: add real-time usage monitoring in menu bar
fix: resolve ccusage detection on M1 Macs
docs: update installation instructions for Homebrew
```

## Feature Requests

Before implementing new features:

1. **Check existing issues** to avoid duplicates
2. **Open an issue** to discuss the feature
3. **Wait for feedback** from maintainers
4. **Implement** after approval

## Bug Reports

When reporting bugs, please include:

- **ccbar version**
- **macOS version**
- **ccusage version** (`ccusage --version`)
- **Steps to reproduce**
- **Expected behavior**
- **Actual behavior**
- **Screenshots** if applicable
- **Console logs** if relevant

## Code of Conduct

- Be respectful and inclusive
- Focus on constructive feedback
- Help others learn and grow
- Maintain a welcoming environment

## Questions?

- Open an issue for feature discussions
- Check existing issues before asking
- Be patient waiting for responses

Thank you for contributing to ccbar! 