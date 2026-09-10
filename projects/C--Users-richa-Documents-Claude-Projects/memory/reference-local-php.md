---
name: reference-local-php
description: "PHP 8.4 CLI is installed on Richard's laptop via winget (Sep 9 2026); path for php -l linting of WordPress plugins before building zips"
metadata: 
  node_type: memory
  type: reference
  originSessionId: 29804b27-f62f-4bc7-9443-ea5f12f8d5d8
  modified: 2026-09-09T11:12:38.365Z
---

PHP 8.4.24 CLI was installed 2026-09-09 with `winget install --id PHP.PHP.8.4`. It is on PATH for new shells, but in-session Git Bash may not see it. Absolute path:

`/c/Users/richa/AppData/Local/Microsoft/WinGet/Packages/PHP.PHP.8.4_Microsoft.Winget.Source_8wekyb3d8bbwe/php.exe`

**How to apply:** Before building any WordPress plugin zip (LawnAce chatbot, Start Performance, GSC, AI addon), run `php -l` over every .php file with that path. Do not scp plugin files to the Riverside VPS just to lint; the auto-mode classifier blocks that anyway. See [[project-lawnace-chatbot]].
