# Brief: translating Odoo 20.0 UI strings into Georgian (ka)

You are translating Odoo user-interface strings from English into Georgian.

## Input and output
- Each input is `/home/im/odoo/work/chunks/<name>.json`, a JSON array of items:
  `{id, ctx, msgid, plural, flags, where, note}`.
  - `msgid` is the English text to translate.
  - `plural` is the English plural form, when present.
  - `where` and `note` give context: the model field, the view, the JS file.
- Write `/home/im/odoo/work/chunks/<name>.out.json`: one JSON object mapping each id (as a string) to its Georgian translation.
  Example: `{"0": "...", "1": "..."}`. Include **every** id.
- For plural items, give ONE Georgian string, used for all counts. Georgian puts the noun in the singular after a numeral: "%s records" → "%s ჩანაწერი".
- The output must be valid JSON. Escape `"` as `\"`, newlines as `\n`, and backslashes as `\\`. The simplest and safest way is a small Python script that builds the dict and calls `json.dump(d, f, ensure_ascii=False, indent=0)`.

## Terminology
Read `/home/im/odoo/work/glossary.csv` first. Its terms and RULE lines are **mandatory**. Also note these user decisions:
- Discard = გაუქმება
- Message and Notification = შეტყობინება
- Discuss = დისკუსია
- Report = ანგარიშგება
- Customer = კლიენტი
- User = მომხმარებელი

## Hard rules (a validator enforces them)
1. Keep every placeholder exactly as written, the same number of times: `%s`, `%d`, `%(name)s`, `{name}`, `{0}`, `{}`, `{{ expr }}`, `${expr}`. You may reorder them to suit Georgian word order. Never translate the name inside a placeholder.
2. Keep HTML/XML markup: the same tags and the same `t-*`, `class`, `href`, `src`, `role`, `name`, `style`, `id` and `type` attributes with identical values. Translate only the human-readable text between tags, plus `title`, `placeholder`, `alt` and `aria-label` attribute text. Keep entities such as `&nbsp;`, `&amp;` and `&gt;`.
3. Keep the leading and trailing whitespace exactly the same, including newlines and indentation. If the source contains `\n`, the translation must also contain at least one.
4. Keep untranslated: code, identifiers, Python/JS expressions, field technical names (`res.partner`, `x_name`), file extensions, URLs, email addresses, format codes (`%Y-%m-%d`), keyboard keys (Ctrl, Shift, Enter, Alt), and brand names (Odoo, Google, Gmail, Outlook, WhatsApp, QWeb, Python, JSON, XML, CSV, API, SMTP, IMAP, HTML, GIF, EDI).
5. If an item is purely technical (only code, a placeholder, a symbol or a proper name, e.g. "%s (copy)" is NOT one), return the source text unchanged.
6. Emoji names (the "face", "hand", "heart" items in mail): translate them naturally and briefly, e.g. "grinning face" → "მომღიმარი სახე". Country names, language names and timezones: use the standard Georgian exonyms (Germany → გერმანია, French → ფრანგული).
7. Currency-unit words used for amount-to-text (Cents, Centimes, Fils, Peso, Rupee…): use the standard Georgian singular form (ცენტი, სანტიმი, ფილსი, პესო, რუპია).

## Style
- Use a formal register (თქვენ).
- Buttons and menu items use verbal nouns (შენახვა, წაშლა).
- Use sentence case; Georgian has no capital letters.
- Use natural, fluent Georgian, not word-for-word calques. Keep strings about as short as the English, since UI space is limited.
- Use „…“ for quotations in prose.

## Validate before finishing
Run:
`/home/im/odoo/.venv/bin/python /home/im/odoo/tools/check_chunk.py /home/im/odoo/work/chunks/<name>.out.json ...`
Fix every reported problem, including "missing", and re-run until each file reports `0 problems`.
Do not edit any other file.
Report only one line per chunk with its final counts. Put no translations in your report.
