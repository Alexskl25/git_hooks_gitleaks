# Gitleaks Pre-Commit Hook for Telegram token

This Git pre-commit hook automatically scans staged files for secrets (including Telegram token) using [Gitleaks](https://github.com/gitleaks/gitleaks). It works on **Linux** and **macOS** and automatically installs the latest version of Gitleaks if missing.

## Features

- Automatically fetches the **latest Gitleaks version**
- Scans **staged files** only
- Blocks commits if secrets are detected
- Auto enable gitleaks.enable true Git config
- Scans Telegram token if ```.gitleaks.toml``` correctly setuped

## Installation

1. Place the `pre-commit` script from ```/scripts/pre-commit``` and ```.gitleaks.toml``` to repo root in your repository:
```bash
.gitleaks.toml
```
```bash
.git/hooks/pre-commit
```
2. Make it executable:
```bash
chmod +x .git/hooks/pre-commit
```
3. Hook via Git will be configured by 

How It Works
* On commit, the script checks if Gitleaks is installed.
* If not, it downloads and installs the latest version automatically.
* Then it scans all staged files for secrets.
* If secrets are found, the commit is blocked and a message is displayed.

Without findings:
```bash
$ git commit -am 'Simple pre-commit script' 
Running gitleaks pre-commit scan...
2:11PM INF 0 commits scanned.
2:11PM INF scanned ~1552 bytes (1.55 KB) in 160ms
2:11PM INF no leaks found
No secrets detected
[main 020c429] Simple pre-commit script
 1 file changed, 50 insertions(+)
 create mode 100755 scripts/pre-commit
```

With finding:
```bash
$ echo "TELE_TOKEN=123456789:ABCDEF1234567890abcdefABCDEF12345" > test_secret.txt
$ git add test_secret.txt
$ git commit -m "test telegram leak"
Running gitleaks pre-commit scan...
Finding:     REDACTED
Secret:      REDACTED
RuleID:      telegram-bot-token
Entropy:     4.729257
File:        test_secret.txt
Line:        1
Fingerprint: test_secret.txt:telegram-bot-token:1

3:04PM INF 0 commits scanned.
3:04PM INF scanned ~55 bytes (55 bytes) in 2.86ms
3:04PM WRN leaks found: 1
-----
Commit blocked: secrets detected by gitleaks
Remove secrets or disable hook:
git config gitleaks.enable false
-----
```

Notes:
* Only works for Linux and macOS.
* For Windows, manual installation of Gitleaks is required.
