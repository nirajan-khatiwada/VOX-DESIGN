# Vox Doc: installer (instructions for Antigravity & Claude Code)

> Note: This skill has been adapted for **Antigravity**. The primary instructions are in [GEMINI.md](file:///C:/Users/Nirajan/Downloads/lastone/vox-doc/GEMINI.md).

The user has opened this unzipped folder, or pointed you at it or at its zip.
Whatever they typed ("start", "continue", "hi", "install", a topic), do this:

1. **Install both skills globally**, so they work from any chat:
   - For Antigravity:
     - Copy `skills/vox-doc/` to `~/.gemini/config/skills/vox-doc/` (Windows: `C:\Users\<me>\.gemini\config\skills\vox-doc\`).
     - Copy `skills/vox-doc-sound/` to `~/.gemini/config/skills/vox-doc-sound/` (Windows: `C:\Users\<me>\.gemini\config\skills\vox-doc-sound\`).
   - For Claude Code (if running under Claude):
     - Copy `skills/vox-doc/` to `~/.claude/skills/vox-doc/`.
     - Copy `skills/vox-doc-sound/` to `~/.claude/skills/vox-doc-sound/`.
   - If a folder already exists, update it by copying over it. Never delete
     anything already there: the user's `.env`, `SETUP_STATUS.md`,
     `CATALOG.tsv` and their own sounds must survive.
   - Check that both `SKILL.md` files are in place.
2. Tell the user, in their language (the one they wrote in, often Hinglish):
   - it is installed globally
   - from now on, in **any** chat, they can type "vox doc start"
     or "vox doc continue", or name a topic
3. Skills load when a session starts, so this session may not list them yet.
   Read `SKILL.md` directly now and follow it. It
   runs the one-time setup (resuming if it was half done) and then makes the
   video.

Prerequisite: the Flow Connector extension and CLI, set up from the
extension's own setup prompt. Never give a Flow web address. If `flow-auto doctor --site flow` fails, tell the user to do
that first and then say "continue".

Never ask for API keys or tokens in the chat. Keys go in `.env` files the user
edits.
