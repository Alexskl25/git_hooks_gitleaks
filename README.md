# Gitleaks Pre-Commit Hook

This Git pre-commit hook automatically scans staged files for secrets using [Gitleaks](https://github.com/gitleaks/gitleaks). It works on **Linux** and **macOS** and automatically installs the latest version of Gitleaks if missing.

## Features

- Automatically fetches the **latest Gitleaks version**
- Scans **staged files** only
- Blocks commits if secrets are detected
- Optionally disables the hook via Git config

## Installation

1. Place the `pre-commit` script from ```/scripts/pre-commit``` in your repository:

```bash
.git/hooks/pre-commit
```
2. Make it executable:
```
chmod +x .git/hooks/pre-commit
```
3. Hook via Git will be configured by 

How It Works
* On commit, the script checks if Gitleaks is installed.
* If not, it downloads and installs the latest version automatically.
* Then it scans all staged files for secrets.
* If secrets are found, the commit is blocked and a message is displayed.

Without findings:
```
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
```
$ echo TEST_API_KEY = "SOME_API_KEY" > test
$ git add test
$ git commit -am 'test'
Running gitleaks pre-commit scan...
Finding:     TEST_API_KEY = REDACTED
Secret:      REDACTED
RuleID:      generic-api-key
Entropy:     4.316828
File:        test
Line:        1
Fingerprint: test:generic-api-key:1

2:41PM INF 0 commits scanned.
2:41PM INF scanned ~39 bytes (39 bytes) in 161ms
2:41PM WRN leaks found: 1
-----
Commit blocked: secrets detected by gitleaks
Remove secrets or disable hook:
git config gitleaks.enable false
-----
```

Notes:
* Only works for Linux and macOS.
* For Windows, manual installation of Gitleaks is required.
* The hook respects Git config gitleaks.enable