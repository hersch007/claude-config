---
name: feedback-encoding-fixes
description: "How to safely fix the platform's widespread mojibake (double-encoding) bug in PHP source files"
metadata: 
  node_type: memory
  type: feedback
  originSessionId: bce3fcb0-21e9-482d-b4c5-fd9cda46bc99
---

Many plugin source files across this platform have mojibake — special characters (em dashes, arrows, play triangles, etc.) that were UTF-8 originally, got misread as Windows-1252 at some point (likely an editor/tool saving with the wrong encoding), and were re-saved as UTF-8. This shows up as garbage sequences like `â€"`, `â–¶`, `â”€â”€` in source and sometimes leaks into the actual UI (e.g. "▶ Start Workflow" rendering as "â-¶ Start Workflow", reported by Richard 2026-07-05).

**The fix that works:** read the file as UTF-8 text, re-encode that text using Windows-1252 (`[System.Text.Encoding]::GetEncoding(1252)`) to recover the original corrupted byte sequence, then decode those bytes as UTF-8 again to get the correct original text. **Windows-1252, not ISO-8859-1** — they differ in the 0x80–0x9F range where the actual printable characters (em dash, curly quotes, etc.) live; ISO-8859-1 there just gives C1 control codes and fails to reverse the corruption.

**Critical gotcha — do NOT re-run this fix on a file that's already been partially corrected.** If a file has a mix of already-correct UTF-8 (e.g. from an earlier manual fix) and still-corrupted mojibake, running the whole-file transform again will *destroy* the already-correct parts (they become `�` U+FFFD replacement characters — genuinely unrecoverable, not just re-mojibake'd). This happened 2026-07-05 to `start-performance-tickets.php`, `start-performance-sales.php`, and `start-performance-operations.php` — their Plugin Name header em dashes had been fixed earlier and got destroyed by a second blind pass; had to manually retype the known-correct text via direct `Edit` calls since the transform is lossy once it hits `�`.

**How to apply:** Before running the Windows-1252 roundtrip fix on a batch of files, check each file isn't already partially fixed (e.g. `grep` for the corruption pattern first — if a file shows 0 matches for the *specific* corrupted substrings you're targeting, don't run the blind transform on it, even if it still contains OTHER unrelated corrupted substrings you haven't accounted for in your search pattern). After running, always scan for `�` (U+FFFD) to catch any new damage before considering the job done — grep pattern: `$'\xef\xbf\xbd'` in Bash.

Some cosmetic corruption (`// ── comment ──` divider lines) in `start-performance-tickets.php` resisted the single Windows-1252 roundtrip — deeper/different corruption there. Left as-is since it's source-comment-only, never rendered to any user; not worth the risk of further blind transforms for zero user-facing benefit.
