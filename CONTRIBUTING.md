# Contributing to AI Support Operations Workflow

Thank you for contributing to this project! Here's how to help:

## 📝 How to Contribute

### 1. Found a Bug or Issue?
- Create an issue describing the problem
- Include: What went wrong, where, and how to reproduce
- Label: `bug`

### 2. Have a Feature Request?
- Create an issue with the request
- Explain why you need it and how it would help
- Label: `enhancement`

### 3. Improving Documentation?
- Edit the markdown files in `/docs`
- Add examples or clarifications
- Submit a pull request with changes

### 4. Updating Runbooks?
- Test changes in your environment first
- Update the relevant runbook in `/docs`
- Include: What changed and why
- Label: `docs`

---

## 📋 Contribution Guidelines

### Documentation
- Use clear, simple language (no jargon)
- Include examples where helpful
- Keep sections short (under 1000 words)
- Test any instructions before submitting

### Code/Scripts
- Comment your code
- Follow existing patterns
- Include usage examples
- Document dependencies

### Testing
- If you modify automation rules, test with 5-10 sample tickets
- Verify triage accuracy (target >90%)
- Check response draft quality
- Run KB search tests

### Git Commit Messages
- Start with action verb: "Add", "Fix", "Update", "Improve"
- Be specific: "Add Shopify integration guide" not "Update docs"
- Example: "Add FAQ section to response drafting runbook"

---

## 🔄 Pull Request Process

1. **Fork the repo** (if external contributor)
2. **Create a branch**: `git checkout -b fix/your-fix-name`
3. **Make changes** and commit with clear messages
4. **Push to your fork**: `git push origin fix/your-fix-name`
5. **Create Pull Request** with description of changes
6. **Address feedback** (implementation lead will review)
7. **Merge** once approved

---

## 📞 Questions?

- **Documentation questions?** → Check `/docs` folder
- **Setup questions?** → See `IMPLEMENTATION_GUIDE.md`
- **Bug reports?** → Create an issue
- **Slack?** → #ai-workflows channel

---

## ⚠️ What NOT to Commit

**Never commit:**
- API keys or tokens (use `.env` files, add to `.gitignore`)
- Customer data or PII
- Internal company secrets
- Passwords or credentials
- Large binary files
- Sensitive system configurations

---

## 🎓 Resources

- [Main README](../README.md) — Overview and getting started
- [Implementation Guide](./docs/IMPLEMENTATION_GUIDE.md) — Setup instructions
- [Ticket Triage Runbook](./TICKET_TRIAGE_RUNBOOK.md) — Classification guide
- [Response Drafting Runbook](./RESPONSE_DRAFTING_RUNBOOK.md) — Quality standards

---

## 📈 Recognition

Contributors who improve documentation, fix bugs, or add features will be:
- Mentioned in the CHANGELOG
- Added to contributors list
- Thanked in project updates

Thank you for helping make this workflow better! 🚀
