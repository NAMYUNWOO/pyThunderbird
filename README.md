# pyThunderbird
python based access to Thunderbird mail

[![pypi](https://img.shields.io/pypi/pyversions/pyThunderbird)](https://pypi.org/project/pyThunderbird/)
[![Github Actions Build](https://github.com/WolfgangFahl/pyThunderbird/actions/workflows/build.yml/badge.svg)](https://github.com/WolfgangFahl/pyThunderbird/actions/workflows/build.yml)
[![PyPI Status](https://img.shields.io/pypi/v/pyThunderbird.svg)](https://pypi.python.org/pypi/pyThunderbird/)
[![Downloads](https://pepy.tech/badge/pyThunderbird)](https://pepy.tech/project/pyThunderbird)
[![GitHub issues](https://img.shields.io/github/issues/WolfgangFahl/pyThunderbird.svg)](https://github.com/WolfgangFahl/pyThunderbird/issues)
[![GitHub closed issues](https://img.shields.io/github/issues-closed/WolfgangFahl/pyThunderbird.svg)](https://github.com/WolfgangFahl/pyThunderbird/issues/?q=is%3Aissue+is%3Aclosed)
[![API Docs](https://img.shields.io/badge/API-Documentation-blue)](https://WolfgangFahl.github.io/pyThunderbird/)
[![License](https://img.shields.io/github/license/WolfgangFahl/pyThunderbird.svg)](https://www.apache.org/licenses/LICENSE-2.0)

Demo
============
[Demo](http://pythunderbird-demo.bitplan.com/)

**Home page**
<img width="1729" alt="grafik" src="https://github.com/zauberzeug/nicegui/assets/1336221/11c946bf-7502-46ad-97a1-25d4514811fe">
**Search**
<img width="1504" alt="grafik" src="https://github.com/zauberzeug/nicegui/assets/1336221/194ba50e-4110-41b2-99e0-f4495600903f">
**Mail display**
<img width="822" alt="grafik" src="https://github.com/zauberzeug/nicegui/assets/1336221/5df06a16-c3c5-45ad-9dde-2b571a39de04">

Installation
============
```bash
pip install pyThunderbird
```

## IMAP Support Enhancement

This fork adds IMAP mail folder support to pyThunderbird. The original library only supported `Mail/Local Folders`, but this version can now index and search emails from both Local Folders and IMAP accounts.

### Key Features Added
- ✅ Auto-detection of IMAP accounts in `ImapMail/` directory
- ✅ Scanning and indexing IMAP mailbox files (e.g., `INBOX`, `Sent`)
- ✅ Hybrid approach: supports both Local Folders and IMAP folders
- ✅ CLI options to control IMAP inclusion (`--show-imap`, `--no-imap`)

### Changes to Thunderbird Class

```python
# New attributes
self.imap_root = f"{self.profile}/ImapMail"
self.imap_accounts = self._detect_imap_accounts()

# Updated method signature
def get_mailboxes(self, include_imap=True, restore_toc=False, progress_bar=None):
    """
    Get mailboxes from Local Folders and optionally IMAP folders.

    Args:
        include_imap (bool): If True, also scan IMAP folders
        restore_toc (bool): If True, restore table of contents
        progress_bar: Optional progress bar for tracking

    Returns:
        Dict[str, ThunderbirdMailbox]: Dictionary of mailboxes
    """
```

### Usage Example

```python
from thunderbird.mail import Thunderbird

# Initialize with IMAP support
tb = Thunderbird.get("username")

# Show detected IMAP accounts
print(f"IMAP accounts: {tb.imap_accounts}")

# Get mailboxes (includes IMAP by default)
mailboxes = tb.get_mailboxes(include_imap=True)

# Get only Local Folders (exclude IMAP)
local_only = tb.get_mailboxes(include_imap=False)
```

### IMAP Folder Structure

```
~/Library/Thunderbird/Profiles/[profile]/
├── Mail/Local Folders/        # Original support
│   ├── Inbox
│   ├── Sent
│   └── ...
└── ImapMail/                   # NEW: IMAP support
    ├── outlook.office365.com/  # Auto-detected
    │   ├── INBOX              # Indexed
    │   ├── Sent               # Indexed
    │   └── ...
    └── gmail.com/              # Auto-detected
        ├── INBOX
        └── ...
```

### Backward Compatibility

All changes are backward compatible. If you don't want IMAP support, simply pass `include_imap=False` to `get_mailboxes()` or use the `--no-imap` CLI flag.

Get Sources
===========
```bash
git clone https://github.com/WolfgangFahl/pyThunderbird
cd pyThunderbird
scripts/install
```

Testing
=======
```bash
scripts/test
```

## Documentation
[Wiki](http://wiki.bitplan.com/index.php/PyThunderbird)

### Authors
* [Wolfgang Fahl](http://www.bitplan.com/Wolfgang_Fahl)
