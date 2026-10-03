# The Tkinter `<FocusOut>` trap

An inline `Entry` created over a `Treeview` cell can appear and
disappear instantly — no error, no trace.

## Cause

    entry.focus_set()
    entry.bind("<FocusOut>", commit)

`focus_set()` is a polite request. The `Treeview` still holds focus,
so the entry never receives it. Tkinter fires `<FocusOut>` anyway,
the handler commits, and the entry destroys itself.

## Fix

    entry.focus_force()
    entry.bind("<Return>", commit)
    entry.bind("<KP_Enter>", commit)
    entry.bind("<Escape>", cancel)

Use `focus_force()` (takes focus regardless of the current owner)
and drop the `<FocusOut>` binding entirely.

## Summary

| | |
|---|---|
| Symptom | Inline editor appears and disappears instantly |
| Cause   | `<FocusOut>` fires on an entry that never received focus |
| Fix     | `focus_force()` + no `<FocusOut>` binding |

## Files

- `TKINTER_ISSUE_AI_REMINDER.html` — illustrated note.