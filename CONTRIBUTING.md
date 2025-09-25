# Contributing to DIIP

Welcome to the Decentralized Identity Interop Profile (DIIP) project! We appreciate your interest in contributing to the specification.

## Overview

DIIP is an open specification developed by the community for the community. We welcome contributions from anyone interested in improving decentralized identity interoperability.

## Release Cycle

DIIP follows a **6-month release cycle**:

- **Draft Development**: Active development happens on the `develop` branch
- **Release Promotion**: At the end of each 6-month cycle, the draft version from the `develop` branch is promoted to the `main` branch
- **Published Version**: The content in the `main` branch becomes the next officially published DIIP profile

## 🔗 Where to Find Versions

- **Latest Published Version**: [https://FIDEScommunity.github.io/DIIP](https://FIDEScommunity.github.io/DIIP)
- **Latest Draft**: [https://FIDEScommunity.github.io/DIIP/draft](https://FIDEScommunity.github.io/DIIP/draft)

## How to Contribute

### Opening Pull Requests

Anyone is free to open Pull Requests (PRs) for inclusion in the next version of the DIIP profile. We encourage contributions that:

- Improve clarity and readability
- Add missing technical details
- Fix errors or inconsistencies
- Enhance examples and use cases
- Address security considerations

### PR Preview System

Every PR automatically generates a **preview** so you can see your changes in a rendered format before they're merged. This allows reviewers and contributors to:

- Visualize the final output
- Verify formatting and styling
- Test links and references
- Ensure examples render correctly

The preview link will be posted as a comment on your PR once the build completes.

### Review Process

- **Anyone can review**: All community members are encouraged to provide review comments and feedback on PRs
- **Editor approval required**: Only designated editors of the specification can merge PRs
- **Current editors** (as defined in `CODEOWNERS`):
    - @nklomp
    - @samuelmr
    - @surfnet-niels
    - @k00ij
    - @eklaver
    - @TimoGlastra

## Ways to Engage

We welcome interaction through multiple channels:

### GitHub Issues

Use Issues for:

- Bug reports
- Feature requests
- Questions about the specification
- Proposing new sections or improvements

### Pull Requests

Submit PRs for:

- Direct contributions to the specification
- Documentation improvements
- Example updates
- Technical corrections

### GitHub Discussions

Join Discussions for:

- High-level design conversations
- Community feedback
- Implementation experiences
- Future roadmap discussions

## Development Setup

To work with the specification locally:

```bash
# Clone the repository
git clone https://github.com/FIDEScommunity/DIIP.git
cd DIIP

# Install dependencies
npm install

# Start development server (with auto-reload)
npm run dev

# Render the specification
npm run render
```

After rendering, open `index.html` in your browser to view the generated specification.

## Writing Guidelines

When contributing to the specification:

1. **Be Clear**: Use precise, unambiguous language
2. **Be Consistent**: Follow existing terminology and style
3. **Provide Examples**: Include practical examples where helpful
4. **Consider Security**: Address security implications of proposed changes
5. **Link References**: Use proper citations and references

## PR Checklist

Before submitting your PR:

- [ ] Your changes are focused and address a specific issue
- [ ] You've tested the rendered output locally
- [ ] Examples are accurate and functional
- [ ] Links and references are working
- [ ] You've checked for typos and formatting issues
- [ ] Your commit messages are descriptive

## Questions?

If you have questions about contributing:

1. Check existing [Issues](https://github.com/FIDEScommunity/DIIP/issues)
2. Start a [Discussion](https://github.com/FIDEScommunity/DIIP/discussions)
3. Reach out to the editors

## License

By contributing to DIIP, you agree that your contributions will be licensed under the same license as the project (see [LICENSE](LICENSE)).

Thank you for contributing to the future of decentralized identity! 🚀