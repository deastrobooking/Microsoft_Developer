# Developer Guide: Building VS Code Extensions and Using Copilot in VS Code

This guide helps you:
- Build, test, and publish Visual Studio Code (VS Code) extensions.
- Get the most out of GitHub Copilot inside VS Code.
- Optionally extend Copilot Chat by adding your own commands/tools via the VS Code Chat API.

References:
- VS Code Extension docs: https://code.visualstudio.com/api
- Publishing extensions: https://code.visualstudio.com/api/working-with-extensions/publishing-extension
- VS Code Chat API (agents/participants): https://code.visualstudio.com/api/extension-guides/chat
- GitHub Copilot in VS Code: https://code.visualstudio.com/docs/copilot/overview
- GitHub Copilot docs: https://docs.github.com/copilot

---

## 1) Prerequisites

- VS Code latest stable: https://code.visualstudio.com/
- Node.js LTS + npm: https://nodejs.org/
- Yeoman + VS Code extension generator: `npm i -g yo generator-code`
- VS Code Extension Manager (vsce): `npm i -g @vscode/vsce`
- GitHub account (for Copilot) and Marketplace publisher (for publishing)

Optional:
- pnpm or yarn
- OpenTelemetry/telemetry lib for extensions
- Azure or your preferred hosting if your extension calls remote APIs

---

## 2) Create your first extension

Scaffold:
```bash
yo code
# Choose: New Extension (TypeScript)  (recommended)
# Answer prompts (name, description, etc.)
cd <your-extension>
npm install
```

Project layout (key files):
- package.json — manifest (contributions, activation events, commands)
- src/extension.ts — entry point with `activate`/`deactivate`
- .vscode/launch.json — debug config for Extension Development Host
- README.md — marketplace page and usage
- CHANGELOG.md — release notes

Add a command (manifest):
```json
{
  "name": "hello-world",
  "displayName": "Hello World",
  "publisher": "your-publisher",
  "engines": { "vscode": "^1.90.0" },
  "activationEvents": ["onCommand:hello.sayHello"],
  "contributes": {
    "commands": [
      {
        "command": "hello.sayHello",
        "title": "Hello: Say Hello",
        "category": "Hello"
      }
    ]
  },
  "main": "./dist/extension.js"
}
```

Register the command (TypeScript):
```ts
// src/extension.ts
import * as vscode from 'vscode';

export function activate(context: vscode.ExtensionContext) {
  const disposable = vscode.commands.registerCommand('hello.sayHello', async () => {
    const name = await vscode.window.showInputBox({ prompt: 'Your name' });
    vscode.window.showInformationMessage(`Hello, ${name ?? 'world'}!`);
  });
  context.subscriptions.push(disposable);
}

export function deactivate() {}
```

Run and debug:
```bash
npm run compile
# Press F5 in VS Code to start the Extension Development Host
```

Add tests (optional):
- Use `@vscode/test-electron` per docs: https://code.visualstudio.com/api/working-with-extensions/testing-extension

---

## 3) Common capabilities you can add

- Commands and menus
  - Contribute to Command Palette, editor/title/context menus.
- Tree views
  - Custom explorers and side bar views with refresh, actions, icons.
- Webviews
  - Full custom UIs (HTML/CSS/JS) for rich experiences.
- Language features
  - Completion, hover, diagnostics, code actions, formatting, semantic tokens.
- Tasks and terminals
  - Create tasks, interact with terminals, stream output.
- Authentication and secrets
  - `vscode.authentication` and SecretStorage for tokens.
- Workspace and file system
  - Watch files, virtual file systems, workspace edits.

Guides: https://code.visualstudio.com/api/extension-guides

---

## 4) Quality, performance, and security

- Activation
  - Use specific `activationEvents` (e.g., onCommand, onLanguage) to avoid unnecessary startup.
- Performance
  - Defer heavy work; stream results; cache; avoid blocking the extension host.
- Security
  - Validate inputs; sanitize HTML in webviews; least-privilege tokens; store secrets in `SecretStorage`.
- Telemetry
  - Use explicit opt-in; document what you collect; minimize PII.
- Accessibility and localization
  - Accessible labels, keyboard navigation; use `vscode-nls` for i18n.

Checklist:
- [ ] Activation events are minimal
- [ ] Error handling and logs (Output channel)
- [ ] No secrets in code or settings defaults
- [ ] Docs include steps, screenshots, limitations
- [ ] Tests cover core flows

---

## 5) Package and publish to the Marketplace

Create a publisher:
- https://code.visualstudio.com/api/working-with-extensions/publishing-extension

Login and publish:
```bash
# Package a VSIX locally
vsce package

# Publish to Marketplace (requires PAT for your publisher)
vsce publish
# Or publish a specific version:
vsce publish 1.0.0
# Pre-release:
vsce publish --pre-release
```

Good Marketplace pages include:
- Clear description, animated GIF/demo, feature bullets
- Permissions rationale (if any)
- Settings and commands list
- Known issues and troubleshooting
- Links to repo, issues, changelog, privacy policy

---

## 6) GitHub Copilot in VS Code

Install and sign in:
- Install the [GitHub Copilot](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot) and [GitHub Copilot Chat](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat) extensions.
- Sign in to GitHub with a Copilot-enabled account (individual, org, or enterprise).

Core features:
- Inline completions
  - Accept with Tab; configure triggers and opacity in Settings.
- Copilot Chat
  - Use the Chat view or Quick Chat (Ctrl/Cmd+I). Ask about code, tests, refactors.
- Edit with Copilot
  - Select code → “Copilot: Explain/Refactor/Document” commands or Chat code lens.
- Context awareness
  - Copilot uses open files and workspace context; you can share files/selection to improve results.
- Terminal and Notebooks
  - Ask Copilot to craft CLI commands or explain notebook cells.

Best practices:
- Provide context (file name, selection, constraints).
- Prefer smaller, focused prompts.
- Review diffs before applying edits.
- Establish conventions for comments and TODOs Copilot can expand.

Admin/policy notes:
- Enterprise admins can manage policies, telemetry, and content exclusions.
- See [GitHub Copilot docs](https://docs.github.com/copilot) for org rollout.

Troubleshooting:
- Check Output: “GitHub Copilot” and “GitHub Copilot Chat”.
- Verify authentication and network proxies.
- Update VS Code and extensions to latest.

---

## 7) Extend Copilot Chat with your own capabilities (advanced)

VS Code’s Chat API lets extensions:
- Register chat “participants” (agents) that users can @mention.
- Add slash commands, quick picks, and streamed responses.
- Provide contextual data (e.g., workspace analysis) to LLMs.
- Bridge to your services/tools securely.

Read the guide: https://code.visualstudio.com/api/extension-guides/chat

Manifest additions (example):
```json
{
  "name": "sample-chat-agent",
  "displayName": "Sample Chat Agent",
  "publisher": "your-publisher",
  "engines": { "vscode": "^1.92.0" },
  "activationEvents": [
    "onStartupFinished"
  ],
  "contributes": {
    "commands": [
      { "command": "sample.analyzeWorkspace", "title": "Sample: Analyze Workspace" }
    ]
  }
}
```

Create a chat participant (TypeScript example):
```ts
// src/extension.ts
import * as vscode from 'vscode';

export function activate(ctx: vscode.ExtensionContext) {
  // Create an agent/participant the user can @mention (e.g., @sample)
  const participant = vscode.chat.createChatParticipant('sample', async (request, context, stream, token) => {
    // request.prompt: user text
    // context: selection, files, etc.
    // stream: send progressive output
    await stream.progress('Analyzing workspace...');
    const files = await vscode.workspace.findFiles('**/*.{ts,tsx,js,jsx}', '**/node_modules/**', 100);
    await stream.markdown(`Found ${files.length} source files.`);
    await stream.markdown('Try `/summarize` to get a module-level summary.');
    return { metadata: { handled: true } };
  });

  participant.iconPath = vscode.Uri.joinPath(ctx.extensionUri, 'media', 'icon.png');
  participant.description = 'Analyze your workspace and answer questions.';
  participant.followups = ['List largest files', 'Summarize dependencies'];

  // Optional: slash command
  participant.commands.registerCommand('summarize', 'Summarize modules', async (request, context, stream, token) => {
    await stream.progress('Summarizing...');
    // ... your logic or API call
    await stream.markdown('Summary: This project is a web app with 3 modules ...');
  });

  ctx.subscriptions.push(participant);
}

export function deactivate() {}
```

Notes:
- API surface evolves; check the Chat API docs for the exact signatures and capabilities in your VS Code version.
- You can stream output, show follow-ups, and read selection or file URIs that the user shared from the editor.
- If your participant calls remote APIs, use `SecretStorage` for tokens, support proxies, and handle cancellations via `token`.

Testing your chat participant:
- Launch the Extension Development Host.
- Open the Chat view and type `@sample What files are the largest?`
- Try your slash command: `@sample /summarize`

Security and privacy:
- Never send code externally without explicit user action or opt-in.
- Redact secrets; respect `.gitignore`/workspace privacy rules.
- Log minimal telemetry; provide a privacy policy if collecting any data.

---

## 8) Patterns for combining VS Code extensions with Copilot

- “Assistive refactors”
  - Expose extension commands; instruct Copilot to call them via @mention or slash commands for reliability.
- “Explain and verify”
  - Participant runs static checks/tests and streams results; Copilot explains outcomes and next steps.
- “Tool-augmented coding”
  - Participant calls your backend (e.g., Azure Functions) for domain knowledge or secure actions, then returns structured summaries to Copilot.

Design tips:
- Keep operations idempotent, cancellable, and with visible diffs.
- Prefer stateless remote calls; include correlation IDs in logs.
- Provide deterministic prompts with clear input/output contracts for repeatability.

---

## 9) Release and operations

- Versioning
  - Semantic versioning; use prerelease channels for breaking changes.
- Changelog
  - Keep concise, user-facing notes.
- Crash and error reporting
  - Use `vscode.window.showErrorMessage` sparingly; prefer logs and status indicators.
- Performance budget
  - Monitor activation time and command latency; avoid long extension host blocks.
- Support
  - Link to GitHub issues; add “good first issue” labels for community contributions.

---

## 10) Quick-start checklists

Build an extension:
- [ ] Scaffold with `yo code`
- [ ] Add a command and tests
- [ ] Document features in README with GIF
- [ ] Package with `vsce package`
- [ ] Publish with `vsce publish`

Use Copilot effectively:
- [ ] Install Copilot + Copilot Chat
- [ ] Sign in and verify license
- [ ] Try inline completions and “Edit with Copilot”
- [ ] Use Chat with file/selection context
- [ ] Review and tweak settings

Add a chat participant (advanced):
- [ ] Read Chat API guide
- [ ] Register a participant/agent
- [ ] Add a slash command
- [ ] Stream progress and results
- [ ] Handle cancellations and errors

---

If you share your target runtime (Node/Python/.NET) and the kind of capability you want (e.g., tree view + chat agent + webview), I can generate a complete starter repo with:
- Extension scaffolding
- Sample chat participant
- Tests and launch configs
- Marketplace-ready README and CI to package/publish
