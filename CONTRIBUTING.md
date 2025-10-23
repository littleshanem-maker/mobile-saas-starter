# Contributing to Mobile Micro-SaaS Starter Kit

Thank you for your interest in contributing! This project is open source and welcomes contributions from the community.

## Ways to Contribute

### 1. Report Bugs
Found a bug? Please create an issue with:
- Clear description of the bug
- Steps to reproduce
- Expected vs actual behavior
- Your environment (OS, Node version, etc.)

### 2. Suggest Enhancements
Have an idea? Open an issue with:
- Clear description of the enhancement
- Use cases and benefits
- Potential implementation approaches

### 3. Improve Documentation
Documentation improvements are always welcome:
- Fix typos and grammar
- Add missing information
- Improve clarity and examples
- Add troubleshooting tips

### 4. Submit Code
Want to code? Please:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## Code Standards

### TypeScript
- Use TypeScript strictly (no `any`)
- Add proper type definitions
- Include JSDoc comments for functions

### Formatting
- Use Prettier for code formatting
- Use ESLint for linting
- Follow the existing code style

### Naming
- Use descriptive names
- Use camelCase for variables/functions
- Use PascalCase for components/types
- Prefix boolean variables with `is` or `has`

### Comments
- Comment WHY, not WHAT
- Keep comments up to date
- Use clear, professional language

### Testing
- Test your changes thoroughly
- Test on multiple devices/simulators
- Test dark mode
- Test network failures

## Pull Request Process

1. Update documentation if needed
2. Add comments for complex logic
3. Test all changes
4. Submit PR with clear description
5. Link related issues
6. Wait for review

## Development Setup

```bash
# Clone and setup
git clone <repo>
cd mobile-saas-starter
npm install

# Create your branch
git checkout -b feature/your-feature

# Make changes and test
npm run ios    # or android
expo start -c

# Format and lint
npm run format
npm run lint

# Commit
git commit -m "feat: add your feature"
git push origin feature/your-feature
```

## Code of Conduct

### Our Pledge
We are committed to providing a welcoming and inspiring community for all.

### Our Standards
- Be respectful and inclusive
- Welcome different opinions
- Focus on what is best for the community
- Show empathy towards others

### Our Responsibilities
Maintainers are responsible for:
- Clarifying standards of behavior
- Removing inappropriate comments
- Banning abusive members

## Questions?

- 📖 Check the [documentation](./docs/)
- 💬 Open a discussion
- 📧 Email the maintainers

## Recognition

Contributors will be recognized in:
- README.md
- Release notes
- Project credits

---

Thank you for helping make this project better! 🎉
