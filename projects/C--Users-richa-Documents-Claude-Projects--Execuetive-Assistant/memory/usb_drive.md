---
name: usb-drive-paths
description: "Richard's USB drive contains all Claude Projects. Drive letter varies by machine."
metadata: 
  node_type: memory
  type: reference
  originSessionId: 848129bb-33b4-40c8-807c-3b0d8cea76b1
---

All Claude Projects have been copied to a portable USB drive.

**USB path structure:** `{DRIVE}:\__Claude Projects\`

| Machine | Drive letter | Full EA path |
|---------|-------------|-------------|
| Richard's main PC | G:\ | `G:\__Claude Projects\_Execuetive Assistant` |
| Other computer | F:\ | `F:\__Claude Projects\_Execuetive Assistant` |

**How to start a session from USB:**
```
cd "G:\__Claude Projects\_Execuetive Assistant"
claude
```

**Why:** Richard wants to work portably across machines without re-copying files.

**Note:** Drive letter changes per machine — always confirm which letter the USB mounted as before saving files.
