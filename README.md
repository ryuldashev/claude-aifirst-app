# AI-First App - Claude Code Plugin Marketplace

Tools for building AI-first startups with Claude Code.

## Installation

```bash
claude /plugin marketplace add https://github.com/ryuldashev/claude-aifirst-app
```

Then enable the plugin:
```bash
claude /plugin enable startup-spec@aifirst-app
```

## Available Plugins

### startup-spec

AI-First Startup Spec Generator with two commands:

#### `/rapid-spec` - Quick Validation (1-2 weeks)

Creates actionable documentation for rapid idea validation:
- 6 spec documents (BRIEF, VALIDATION, ECONOMICS, PRODUCT, GROWTH, NEXT-STEPS)
- Public landing page for user signups
- Investor pitch page for angel investors

```bash
/rapid-spec "AI chess tutor for kids 3-8 years old"
```

#### `/startup-spec` - Full Documentation (for scale)

Creates comprehensive startup documentation:
- AI thesis and first principles analysis
- Product specs (vision, features, personas, journeys)
- Technical architecture
- Business model and projections
- Growth strategy
- Operations, legal, support, content

```bash
/startup-spec "AI chess tutor for kids"
/startup-spec --from-rapid  # Expand rapid-spec to full spec
/startup-spec --review      # Review existing spec
```

## Philosophy

**Rapid Spec** = Validate fast (1-2 weeks)
**Startup Spec** = Scale (after validation)

Start with `/rapid-spec`, validate your idea, then expand with `/startup-spec --from-rapid`.

## Author

[ryuldashev](https://github.com/ryuldashev)
