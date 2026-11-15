# anki-words

Words looked up in a Claude chat, one per line in `words.txt`. Claude adds new words here; `bin/anki-chat-sync` puts them into the Anki deck "German Vocabulary".

Each word goes through `anki-de-add`, the same as pressing `a` in the `dwds` popup:

- **DWDS first:** `dwds-text --anki` (tag `dwds`).
- **Not in DWDS, or DWDS has only grammar and no meanings:** the word is added in bold with an empty Back, and `anki-update -y` lets Claude fill it (tag `claude-chat`, then `ai-filled`).
- Handled words are listed in `~/.cache/anki-words-done`. Delete a line there to retry a word.

## Setup

```
ln -s ~/notes/language-learning/anki-words/systemd/anki-chat-sync.{service,timer} ~/.config/systemd/user/
systemctl --user daemon-reload
systemctl --user enable --now anki-chat-sync.timer
```

Run by hand: `bin/anki-chat-sync`. Logs: `journalctl --user -u anki-chat-sync`.
If Anki isn't open, the script starts it and closes it again afterwards (which syncs to AnkiWeb). For that, your sway config needs `exec systemctl --user import-environment WAYLAND_DISPLAY DISPLAY`. `dwds-text`, `anki-update`, `jq` and `curl` must be on the PATH set in the service file.
