# Codex Mid-line Slash Completion

Mid-line slash autocomplete for OpenAI Codex CLI commands, skills, plugins, and apps.

Codex normally discovers slash commands only when `/` is the first character. This patch adds a unified fuzzy-search popup when you type a slash token anywhere in the composer.

```text
Review this /seo-
            ↓
/seo-naver-upgrade  [Skill]
            ↓ Enter
Review this $seo-naver-upgrade
```

The slash is discovery syntax. Skill, plugin, and app selections are inserted using Codex's native atomic `$mention` format.

## What it adds

- Mid-sentence `/query` autocomplete
- One popup for built-in commands, skills, plugins, and apps
- Fuzzy matching with source labels such as `[Skill]` and `[Plugin]`
- URL-aware parsing, so `https://...` does not open the popup
- Occurrence-aware dismissal when the same slash query appears more than once
- Focused parser, ranking, insertion, and snapshot tests

## Compatibility

Tested against upstream [`rust-v0.145.0`](https://github.com/openai/codex/tree/rust-v0.145.0).

The patch is version-specific because it modifies Codex TUI internals. Check the patch before applying it to a newer revision.

## Apply

```bash
git clone https://github.com/openai/codex.git
cd codex
git switch --detach rust-v0.145.0
git apply --check /path/to/codex-midline-slash-completion-rust-v0.145.0.patch
git apply /path/to/codex-midline-slash-completion-rust-v0.145.0.patch
```

Patch file: [`patches/codex-midline-slash-completion-rust-v0.145.0.patch`](patches/codex-midline-slash-completion-rust-v0.145.0.patch)

## Build and verify

From the upstream `codex-rs` directory:

```bash
cargo test -p codex-tui bottom_pane::chat_composer::slash_input::tests
cargo test -p codex-tui bottom_pane::command_popup::tests
cargo build --release -p codex-cli
```

The focused completion tests pass on `rust-v0.145.0`. The upstream full TUI suite at that tag currently has unrelated stored-snapshot version differences, so this repository does not claim a clean upstream-wide test run.

## Behavior notes

- Leading slash commands keep their native Codex behavior.
- A built-in command selected in the middle of text is completed as text; it is not dispatched as a leading command.
- `/ test` remains ordinary text.
- This repository ships source changes only. It does not distribute a Codex binary, credentials, or personal configuration.

## 한국어 요약

Codex 입력창의 문장 중간에서 `/검색어`를 입력하면 기본 명령어와 스킬·플러그인·앱을 한 번에 검색해 주는 TUI 패치야. 항목을 선택하면 스킬 계열은 Codex가 원래 지원하는 `$이름` 멘션으로 삽입되고, URL의 슬래시는 자동완성을 띄우지 않아.

## Search keywords

OpenAI Codex CLI, slash commands, mid-line autocomplete, command palette, Codex skills, Codex plugins, Codex apps, terminal UI, TUI, Rust, fuzzy search, AI agent workflow, developer productivity.

## License and attribution

Apache License 2.0. The patch is derived from OpenAI Codex and retains the upstream [`LICENSE`](LICENSE) and [`NOTICE`](NOTICE). This is an unofficial community patch and is not affiliated with or endorsed by OpenAI.
