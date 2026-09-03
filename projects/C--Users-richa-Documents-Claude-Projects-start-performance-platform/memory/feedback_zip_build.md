---
name: feedback-zip-build
description: How to build plugin zips so WordPress can install them (forward-slash paths)
metadata: 
  node_type: memory
  type: feedback
  originSessionId: f256f0f7-3dd7-41b0-92c9-aaf30a545038
---

PowerShell 5.1's `Compress-Archive` (and even .NET `ZipFile.CreateFromDirectory` on this machine) writes Windows **backslash** path separators into the zip. Linux/WordPress cannot read backslash paths, causing **"Plugin file does not exist."** on activation.

**Why:** The ZIP spec requires forward slashes. Backslash entries mean WordPress never finds `start-performance/start-performance.php`.

**How to apply:** Build zips by creating entries manually with explicit forward slashes:

```powershell
Add-Type -AssemblyName System.IO.Compression
Add-Type -AssemblyName System.IO.Compression.FileSystem
$src = "...\plugin\start-performance"
$out = "...\plugin\start-performance-X.Y.Z.zip"
if (Test-Path $out) { Remove-Item $out }
$bs = [char]92; $fwd = [char]47
$zip = [System.IO.Compression.ZipFile]::Open($out, [System.IO.Compression.ZipArchiveMode]::Create)
$srcLen = $src.Length
Get-ChildItem $src -Recurse -File | ForEach-Object {
    $rel = "start-performance/" + $_.FullName.Substring($srcLen + 1).Replace($bs, $fwd)
    $entry = $zip.CreateEntry($rel, [System.IO.Compression.CompressionLevel]::Optimal)
    $es = $entry.Open(); $fh = [System.IO.File]::OpenRead($_.FullName)
    $fh.CopyTo($es); $fh.Dispose(); $es.Dispose()
}
$zip.Dispose()
```

**Critical:** The zip MUST contain a root folder matching the plugin slug (e.g. `start-performance-intelligence/start-performance-intelligence.php`). Without the root folder WordPress treats it as a new plugin instead of overwriting the existing one. `Compress-Archive -Path "plugin/foo/*"` strips the root folder — always use the manual `ZipFile::Open` method above with an explicit `pluginslug/` prefix on every entry.

Always verify: the zip must contain `start-performance/start-performance.php` with forward slashes at the root. Build output zips into the `plugin\` folder (where the user expects them). See [[feedback-version-bump]].
