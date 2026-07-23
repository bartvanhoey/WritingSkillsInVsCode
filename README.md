# Writing Claude Code Skills In Visual Studio Code

In this guide, we will explore how to enhance your writing **Claude Code Skills** using Visual Studio Code (VS Code). Whether you're a developer, writer, or student, VS Code offers a variety of extensions and features that can help you improve your writing efficiency and quality.

## Getting Started

First you need to install [Visual Studio Code](https://code.visualstudio.com/) if you haven't already. Once installed, make sure you have **AutoSave** enabled in VS Code. You can do this by going to `File > Auto Save` or by pressing `Ctrl+Shift+P` and searching for "Auto Save".

Add VS Code to your taskbar for easy access. You can do this by right-clicking the VS Code icon in your taskbar and selecting "Pin to taskbar".

When you right-click the VS Code icon in your taskbar, you can also pin a folder/file to the list, that way you can quickly open your project folder in VS Code.

> **Pro-tip**
>
> You can quickly open the current folder in VS Code by pressing `CTRL+L` to focus on the address bar, typing `code .`, and hitting Enter.

## Extensions

| Name | Id | Description | Publisher |
| - | - | - | - |
| Claude Code for VS Code | `Anthropic.claude-code` | Claude Code for VS Code: Harness the power of Claude Code without leaving your IDE | Anthropic |
| Code Spell Checker | `streetsidesoftware.code-spell-checker` | Spelling checker for source code | streetsidesoftware |
| German - Code Spell Checker | `streetsidesoftware.code-spell-checker-german` | German dictionary extension for VS Code | streetsidesoftware |
| Markdown All in One | `yzhang.markdown-all-in-one` | All you need to write Markdown (keyboard shortcuts, table of contents, auto preview and more) | Yu Zhang |
| Markdown Preview Enhanced | `shd101wyy.markdown-preview-enhanced` | Markdown Preview Enhanced ported to vscode | Yiyi Wang |
| Prettier - Code formatter | `esbenp.prettier-vscode` | Code formatter using prettier | Prettier |
| YAML | `redhat.vscode-yaml` | YAML Language Support, used to validate the YAML frontmatter in a skill's `SKILL.md` | Red Hat |

> **Pro-tip**
>
> In the Claude Code extension, click on the gear-settings icon and select "Extension Settings" to customize the extension.  In the settings, click on Claude Code: Use Terminal to enable the use of the terminal for Claude Code.  This will allow you to run Claude Code commands directly from the terminal in VS Code instead of the native UI.

To make sure everyone who opens this project gets prompted to install the extensions above, add a `.vscode/extensions.json` file to the project root:

```json
{
  "recommendations": [
    "Anthropic.claude-code",
    "streetsidesoftware.code-spell-checker",
    "streetsidesoftware.code-spell-checker-german",
    "yzhang.markdown-all-in-one",
    "shd101wyy.markdown-preview-enhanced",
    "esbenp.prettier-vscode",
    "redhat.vscode-yaml"
  ]
}
```

When someone opens this workspace in VS Code, they'll see a notification offering to install any recommended extensions they're missing — no need to copy the table by hand.

## Command Palette

In VS Code, Ctrl+Shift+P opens the Command Palette — a dropdown that appears at the top center of the window with a text input where you can type and run any available command (e.g. "Claude Code: Open in Terminal", "Git: Commit", extension commands, settings). It's prefixed with > by default.

To change the keyboard shortcut for the Command Palette, go to `File > Preferences > Keyboard Shortcuts` and search for "Command Palette" or `(workbench.action.showCommands)`. You can then change the keybinding to your preferred combination.

## Split Screen

I always use split screen when writing Claude Code Skills. This allows me to have the skill open to the left of the screen and the Claude Code terminal open to the right of the screen.

## Anthropic skill-creator skill

When you have the Claude Code terminal open, you can also install the skill-creator skill from the Anthropic marketplace. This skill will help you by creating and editing skills directly from VS Code.

```bash
/plugin marketplace add anthropics/claude-plugins-official
/plugin install skill-creator@claude-plugins-official
/reload-plugins
```

when the plugin is installed, you can use the following command to call the skill-creator skill:

```bash
/skill-creator 
```

## Anatomy of a Skill

A Claude Code skill is a folder containing at minimum a `SKILL.md` file. The file starts with a YAML frontmatter block, followed by the skill's instructions in Markdown:

```markdown
---
name: my-skill
description: One-line summary of what this skill does and when to use it — this is what Claude reads to decide whether to trigger it.
---

Instructions for Claude go here, written like you're briefing a colleague.
```

A skill can also include extra files alongside `SKILL.md`, referenced from the instructions:

- `references/` — supporting docs the skill can point Claude to for more detail
- `scripts/` — helper scripts the skill can tell Claude to run

Because the `description` field is what Claude matches against your request, it's worth spending the most editing time on that single line — vague descriptions cause skills to trigger too rarely or too often.

> **Pro-tip**
>
> Since `SKILL.md` frontmatter is YAML, install the **YAML** extension (see [Extensions](#extensions)) so VS Code flags indentation and syntax errors in the frontmatter block before you ever try to load the skill.

## Snippets for scaffolding skills

You can add a VS Code user snippet so typing a short prefix inserts the frontmatter block above instead of retyping it each time. Open the Command Palette → "Snippets: Configure User Snippets" → "markdown.json" and add:

```json
{
  "Skill frontmatter": {
    "prefix": "skillfront",
    "body": [
      "---",
      "name: ${1:skill-name}",
      "description: ${2:One-line summary of what this skill does and when to use it.}",
      "---",
      "",
      "$0"
    ],
    "description": "Scaffold a Claude Code SKILL.md frontmatter block"
  }
}
```

Now, in any Markdown file, typing `skillfront` and pressing Tab will expand into the snippet above, with your cursor jumping between the placeholders.

## Markdown Preview mode by default

To open Markdown files in preview mode by default (instead of the raw editor), add a workspace or user setting that associates .md files with the preview editor. Open Settings (JSON) via the Command Palette → "Preferences: Open User Settings (JSON)" and add:

```json
"workbench.editorAssociations": {
  "*.md": "vscode.markdown.preview.editor"
}
```

To open Markdown files in text editor mode, you need to right-click the file and select "Open With" → "Text Editor".

## Terminal

VS Code has an integrated terminal that you can use to run command-line tools without leaving the editor. To open the terminal, go to `View > Terminal` or press Ctrl + SHIFT + `ö`

To change the keyboard shortcut for the Terminal: create new Terminal, go to `File > Preferences > Keyboard Shortcuts` and search for "Terminal: Create New Terminal" or `(workbench.action.terminal.new)`. You can then change the keybinding to your preferred combination.

When working on Skills, the terminal comes in handy when you want to run git commands:

checkout the main branch and pull the latest changes:

```bash
git checkout main
git pull origin main
```

create a new branch for your changes:

```bash
git checkout -b your-branch-name
```

```bash
git add .
git commit -m "Your commit message"
git push origin your-branch-name
```

> **Pro-tip**
>
> when using GitHub, it can be useful to install the GitHub CLI. Open a terminal and run the following command to install it:

```bash
winget install --id GitHub.cli

# you need to authenticate with GitHub by running the following command:
gh auth login

# you can then create a pull request using the following command:
gh pr create --title "Your PR title" --body "Your PR description"

# you can merge the pull request using the following command:
gh pr merge --squash --delete-branch
```

## Ignore local skill artifacts with .gitignore

While you're developing and testing a skill, tools like `skill-creator` or Claude Code itself may drop local cache, log, or state files into your project (for example, a `.claude/` folder or `*.log` files). These are machine-specific and shouldn't be committed alongside your skill's source files.

Add a `.gitignore` file to your project root to keep them out of version control:

```gitignore
# Claude Code / skill-creator local artifacts
.claude/
*.log
```

> **Pro-tip**
>
> If you've already accidentally committed one of these files before adding `.gitignore`, adding the rule alone won't remove it from the repo — you'll also need to `git rm --cached <file>` to untrack it.

## Ignore Markdown issues

When writing Markdown files, you may encounter issues with formatting or rendering. To ignore these issues, you can add a .markdownlint.json file to your project root with the following content:

```json
{
  "default": true,
  "MD012": false
}
```
