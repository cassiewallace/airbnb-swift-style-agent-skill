# Airbnb Swift Style Guide Agent Skill

Enforcement of the [Airbnb iOS Swift Style Guide](https://github.com/airbnb/swift) for any AI coding tool that supports the [Agent Skills open format](https://github.com/AvdLee/Swift-Concurrency-Agent-Skill).

---

## Based on the Airbnb Swift Style Guide

This skill is a direct implementation of the **[Airbnb Swift Style Guide](https://github.com/airbnb/swift)** — Airbnb's open-source guide for writing clean, consistent Swift code. Every rule, example, and convention in this skill traces back to that guide. If you find a discrepancy, the upstream guide is the source of truth.

---

## Who this is for

- iOS and Swift developers who want every line of new or reviewed code to follow Airbnb's conventions automatically
- Teams doing pull request reviews who want a consistent style baseline without manual style debates
- Anyone migrating an existing codebase to match the Airbnb Swift guide and looking for targeted, rule-by-rule feedback

---

## How to Use This Skill

### Option A: Using skills.sh (Recommended)

```bash
npx skills add https://github.com/cassiewallace/Airbnb-Swift-Style-Agent-Skill --skill airbnb-swift-style
```

Visit [skills.sh](https://skills.sh) to learn more about the Agent Skills format.

### Option B: Claude Code Plugin

#### Personal Usage

```
/plugin marketplace add cassiewallace/Airbnb-Swift-Style-Agent-Skill
/plugin install airbnb-swift-style@airbnb-swift-style-agent-skill
```

#### Project Configuration

Add the following to your `.claude/settings.json` to enable this skill for everyone on the team:

```json
{
  "enabledPlugins": {
    "airbnb-swift-style@airbnb-swift-style-agent-skill": true
  },
  "extraKnownMarketplaces": {
    "airbnb-swift-style-agent-skill": {
      "source": {
        "source": "github",
        "repo": "cassiewallace/Airbnb-Swift-Style-Agent-Skill"
      }
    }
  }
}
```

Committing this file means every team member gets the skill automatically with no manual setup.

### Option C: Manual Install

1. Clone this repository:
   ```bash
   git clone https://github.com/cassiewallace/Airbnb-Swift-Style-Agent-Skill.git
   ```
2. Symlink or copy the `airbnb-swift-style/` skill folder into your agent's skills directory.
3. Restart your agent or reload skills.

#### Where to Save Skills

Refer to your tool's documentation for the exact path:

- **Claude Code**: [Claude Code Skills docs](https://docs.claude.com)
- **Cursor**: [Cursor Rules docs](https://docs.cursor.com)
- **Windsurf**: [Windsurf docs](https://docs.windsurf.com)

Once installed, verify by asking your agent: *"What Swift style rules do you know about?"*

---

## What This Skill Offers

This skill gives your AI agent deep, rule-by-rule knowledge of the Airbnb Swift Style Guide so it can catch issues automatically, explain them clearly, and fix them precisely.

### Review and Lint Code
- Scan any Swift file or snippet for style violations
- Report findings grouped by category (Naming, Formatting, Style, Patterns)
- Quote the exact offending line and provide a corrected version for each issue
- Summarize total violations and severity at the end

### Enforce Naming Conventions
- Catch incorrect casing (PascalCase vs. lowerCamelCase)
- Flag missing boolean prefixes (`is`, `has`, `does`)
- Enforce acronym capitalization rules (`URL`, `ID`, `urlValidator`)
- Identify event handler naming that doesn't follow past-tense convention (`didTap` not `handleTap`)
- Remove Objective-C-style class prefixes

### Enforce Formatting Rules
- Flag lines exceeding 100 characters and show how to break them
- Detect tab indentation and convert to 2-space indentation
- Identify trailing whitespace

### Apply Style Patterns
- Remove redundant `self` references
- Apply type inference where types are easily inferred
- Replace force-unwrapping (`!`) with safe alternatives
- Convert nested `if let` to `guard` early-exit patterns
- Enforce most-restrictive access control

### Rewrite Code
- Apply all applicable rules and return a clean, corrected version of any Swift snippet
- Include a summary of every change made

---

## What Makes This Skill Different

**Grounded in the actual guide.** Every rule in this skill maps directly to a section of the [Airbnb Swift Style Guide](https://github.com/airbnb/swift). Nothing is invented or inferred — if a rule is here, it's in the guide.

**Explains the why.** The skill doesn't just flag violations — it tells you what rule was broken, why the rule exists, and shows a corrected version. This helps teams learn the conventions rather than just mechanically fixing them.

**Works alongside your linters.** SwiftFormat and SwiftLint handle mechanical formatting automatically. This skill handles the higher-level review: evaluating naming intent, assessing pattern choices, reasoning about access control, and coaching new team members on the reasoning behind each rule.

**Agent-framework agnostic.** Built on the Agent Skills open format, this skill works with any AI coding agent — Claude Code, Cursor, Windsurf, and others — without modification.

---

## Skill Structure

```
Airbnb-Swift-Style-Agent-Skill/
├── .claude-plugin/
│   └── manifest.json          # Plugin metadata for Claude Code marketplace
├── airbnb-swift-style/        # The skill folder (this is what gets installed)
│   ├── SKILL.md               # Main skill: decision trees and quick-reference rules
│   └── references/
│       └── rules.md           # Complete rule catalog with ✅/❌ code examples
├── LICENSE
└── README.md
```

---

## Contributing

Contributions are welcome and AI-assisted contributions are encouraged — use this skill itself when writing Swift examples in the rule catalog.

Please refer to the contributing guidelines before opening a PR:

- Keep all rule examples grounded in the [upstream Airbnb Swift Style Guide](https://github.com/airbnb/swift)
- Add before/after code examples for any new rule entries
- Update `SKILL.md` if a new rule category is introduced
- Run SwiftFormat on any Swift code examples before submitting


---

## License

MIT — see [LICENSE](LICENSE)

