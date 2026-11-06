# Updating AskAI Instructions via the CTM API

A practical, tested guide for reading and updating **AskAI capture actions** on
CTM triggers through the REST API — including the sub-account pattern for
agencies managing many accounts.

This exists because the two things most likely to bite you here **fail
silently**: the update returns `200 OK` whether or not it destroyed your
configuration.

> **The one thing to know before you start:** a trigger update **replaces the
> action, it does not merge**. Send back only the field you changed and every
> other field is set to `null` — including the one that tells AskAI where to
> write its answer. Read [§4](GUIDE.md#4-%EF%B8%8F-the-one-rule-that-matters)
> before writing anything.

---

## Contents

| | |
|---|---|
| [1. Before you start](GUIDE.md#1-before-you-start) | Agency key, sub-account id |
| [2. The API pattern](GUIDE.md#2-the-api-pattern) | Read and update calls |
| [3. Where the instructions live](GUIDE.md#3-where-the-instructions-live) | `ask_question` and the action object |
| [4. ⚠️ The one rule that matters](GUIDE.md#4-%EF%B8%8F-the-one-rule-that-matters) | Replace vs merge, and the safe pattern |
| [5. Other things that trip you up](GUIDE.md#5-other-things-that-will-trip-you-up) | Missing endpoints, no dry-run |
| [6. Response codes](GUIDE.md#6-response-codes) | What 400 / 404 / 403 mean here |
| [7. Verify your change](GUIDE.md#7-verify-your-change) | Diff script + rollback |
| [8. Working across multiple sub-accounts](GUIDE.md#8-working-across-multiple-sub-accounts) | Looping, per-account trigger ids |
| [9. Quick reference](GUIDE.md#9-quick-reference) | Cheat sheet |

---

## Full guide

The complete guide is **[GUIDE.md](GUIDE.md)**.

---

## Using this with an AI assistant

This file is written to be handed to an AI coding assistant as instructions.
That's a perfectly good way to use it if you'd rather not write the calls by
hand.

Paste `GUIDE.md` into the chat, then ask for something concrete:

> Read this guide. Then write me the exact curl commands to update the AskAI
> instructions on trigger `AUT...` in account `12345`.

Three habits worth keeping when you work this way:

1. **Do the read step first.** Ask the assistant to fetch the trigger and show
   you where the instructions live *before* it changes anything.
2. **Ask for the diff before the write.** "Show me exactly which fields would
   change." If it doesn't offer that, ask for it.
3. **Keep the snapshot.** The guide has you save the trigger before every
   change. That file is your undo button — don't skip it.

---

## Scope and status

- Documents the read and update operations for an existing AskAI
  (`ai_capture` / `TriggerAiCaptureV2`) action.
- The "replace, not merge" behaviour and the missing sub-resource endpoints were
  confirmed against a live account.
- `PUT` is not documented here — use `PATCH`, and assume `PUT` is at least as
  destructive.
- No credentials, account ids, or customer data are included. Bring your own
  agency key.

## Contributing

Corrections and additions welcome — especially if you hit API behaviour that
contradicts what's written here. Please don't include real account ids,
customer names, keys, or captured API responses in issues or pull requests.

## License

MIT — see [LICENSE](LICENSE).
