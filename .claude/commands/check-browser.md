Read the current state of Safari using the "Claude" browser profile to inspect what is open in TradingView or any other page.

## Prerequisite

The Safari window from the **"Claude" profile** must be the frontmost window before running this skill. Switch to it manually if needed (Safari > profile selector in the tab bar).

## Steps

1. Get the active tab URL and title:

```bash
osascript -e 'tell application "Safari" to get {URL, name} of front document'
```

2. Read the visible text content of the page:

```bash
osascript -e 'tell application "Safari" to do JavaScript "document.body.innerText" in front document'
```

3. Report what you found:
   - Active URL and page title
   - Summarize the visible content in context of what the user is working on
   - If it is TradingView: note the symbol, timeframe, and any visible Pine Script editor content or error messages
   - Flag anything relevant to the current trading strategy work

## Notes

- Do NOT activate or bring Safari to the foreground
- Always use the "Claude" Safari profile window, not the personal profile
- If $ARGUMENTS contains a specific thing to look for, focus the report on that
