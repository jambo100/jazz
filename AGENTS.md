# AGENTS.md

## User Info
- Windows 11, PowerShell default shell
- Timezone: Asia/Shanghai (UTC+8)
- Mixed usage: coding/debugging + daily productivity tasks

## Language & Style
- Reply in Chinese
- Be detailed: explain the why, alternatives considered, underlying principles
- Keep personality but prioritize information density
- Use Markdown formatting for readability; prefer short paragraphs over walls of text

## Dev Environment
- Default shell: PowerShell (NOT cmd)
- Tools: git, Node.js, npm/pnpm
- Python 3.12.9：已安装到 C:\Python312，pip + openpyxl 3.1.5 已预装
  - 若 python 指向 Windows Store 占位符：设置 → 应用 → 应用执行别名 → 关闭 python.exe


- Avoid bash-only syntax (&& chains, export, source); use PowerShell equivalents
- File paths: use PowerShell-compatible syntax when possible
- Chrome 路径：`C:\Program Files\Google\Chrome\Application\chrome.exe`（正版 Chrome）
  - `@playwright/cli` 硬编码查找 `C:\Users\OMEN\AppData\Local\Google\Chrome\Application\chrome.exe`
  - 通过 Junction 桥接：`C:\Users\OMEN\AppData\Local\Google\Chrome\Application` → `C:\Program Files\Google\Chrome\Application`
  - 若 Junction 丢失，重建命令：
    ```powershell
    New-Item -ItemType Directory -Path "C:\Users\OMEN\AppData\Local\Google\Chrome" -Force -ErrorAction SilentlyContinue
    New-Item -ItemType Junction -Path "C:\Users\OMEN\AppData\Local\Google\Chrome\Application" -Target "C:\Program Files\Google\Chrome\Application"
    ```
  - 删除 Junction 需用 `cmd.exe /c rmdir`，PowerShell `Remove-Item` 对 Junction 不稳定
  - **Playwright CLI 调用**：`npx @playwright/cli@latest <command>`（Windows 不用 `.sh` wrapper）
  - 不推荐 Edge
  - **PowerShell 执行策略**可能拦截 `npx`（.ps1），改用 `npx.cmd` 绕过：`npx.cmd @playwright/cli@latest <command>`

## Java & IntelliJ IDEA
- IntelliJ IDEA 2023.1.2: E:\Java\IntelliJ IDEA 2023.1.2\bin\idea64.exe
- Primary JDK: Java 8 (JAVA_HOME=C:\Program Files\Java\jdk1.8.0_202, JDK 1.8.0_202)
- Fallback JDK: Java 25 at E:\Java (JDK 25.0.2 LTS)
- **After writing Java code**: open the project in IntelliJ and verify it compiles and runs correctly
- For standalone .java files: place them inside an existing IntelliJ project's src/ directory, or create a new module
- If the project doesn't compile: go to File -> Project Structure -> Project SDK, select JDK 8 first; fall back to JDK 25 only if JDK 8 is incompatible. Do NOT download a new JDK unless both existing versions are incompatible
- Use the project's existing build system (Maven/Gradle) if present; do not bypass it
- After configuration changes, run the project from within IntelliJ to confirm everything works

## Coding Conventions
- JavaScript/TypeScript: single quotes, always semicolons
- Naming: camelCase for variables/functions, PascalCase for classes/components
- Comments: explain complex logic blocks; skip self-evident statements

## Interaction Rules
- Default to action: read the problem, then implement the fix directly ?? do not stop at analysis
- Ask before destructive operations (rm -rf, git reset --hard, force push)
- **Significant changes**: before making important modifications to code or documents, explain the plan in Chinese first ?? what you intend to change and why. Wait for the user to approve before proceeding. Routine/trivial edits (typo fixes, formatting, minor adjustments) do not require this step
- When an approach fails, try a different one; never blindly retry the same failing action
- After editing files, do a basic sanity check (lint, syntax, quick test)
- **Sandbox escalation**: when blocked from opening software, browsers, or accessing network resources, ask the user for permission. If the user says yes (e.g. "????", "????", "??"), proceed with the escalation to bypass the sandbox

## Daily Productivity

### Document & Report Writing
- Word documents (.docx): use the docx skill ?? supports formatting, TOC, headers, page numbers, tracked changes
- Markdown reports: default to clean GH-flavored Markdown; render with code blocks and tables as needed
- PDF: read via pdf skill, generate via docx -> PDF pipeline or reportlab
- When user asks for a "report" or "memo", confirm output format then draft directly

### Presentations (PPT)
- Use the pptx skill for .pptx creation ?? supports layouts, speaker notes, templates
- Default to clean, professional designs; avoid clip-art and excessive animations
- For slide content: short bullets, one idea per slide, consistent heading hierarchy

### Document Formatting & Beautification
- When beautifying existing .docx files, preserve original content; only improve layout, fonts, spacing, headings
- Default font: use the document's existing font family; fall back to system defaults (Calibri/????)
- For Chinese documents: ensure proper Chinese punctuation alignment and line spacing

### Data & Spreadsheets
- Excel (.xlsx/.csv)：使用 xlsx skill（Python + openpyxl，已就绪）

- Default to clean tables with headers, auto-fit columns, and number formatting

### Image Generation
- Use the imagegen skill for bitmap/photo generation, mockups, textures, transparent cutouts

### Audio & Speech
- Speech generation: use the speech skill (OpenAI TTS) for narration, voiceovers, accessibility reads
- Transcription: use the transcribe skill for audio/video to text, with speaker diarization support

### Video Editing
- Codex does not have built-in video editing capability (no timeline, no NLE integration)
- Can assist with: video scripting, subtitle file (.srt) creation, ffmpeg command generation for cuts/merges/format conversion
- For full video editing, recommend: DaVinci Resolve (free, powerful) or CapCut (simple, Chinese-friendly)

## Maintenance
- ?????? C ????????? %LOCALAPPDATA%\Temp ?? Edge ????

## Security
- Never auto-execute git push
- Never auto-delete files/dirs the user did not explicitly target
- **File deletion rule**: before deleting ANY file or document, you MUST ask the user for permission. Explain in Chinese what you want to delete and why. Do NOT delete anything until the user explicitly approves. This applies to all files ?? code, documents, temp files, caches, and configuration files
- Never write secrets (keys, tokens, passwords) into any file

