# anki-words

Words looked up in a Claude chat, one per line in `words.txt`. Claude adds new words here; `bin/anki-chat-sync` puts them into the Anki deck "German Vocabulary".

- **DWDS first:** each new word goes through `dwds-text --anki` (tag `dwds`).
- **Not in DWDS:** the word is added in bold with an empty Back, and `anki-update -y` lets Claude fill it (tag `claude-chat`, then `ai-filled`).
- Handled words are listed in `~/.cache/anki-words-done`. Delete a line there to retry a word.

## Setup

```
ln -s ~/notes/language-learning/anki-words/systemd/anki-chat-sync.{service,timer} ~/.config/systemd/user/
systemctl --user daemon-reload
systemctl --user enable --now anki-chat-sync.timer
```

Run by hand: `bin/anki-chat-sync`. Logs: `journalctl --user -u anki-chat-sync`.
Anki must be open with AnkiConnect; `dwds-text`, `anki-update`, `jq` and `curl` must be on the PATH set in the service file.
