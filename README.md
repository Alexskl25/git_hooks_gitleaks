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

How it works:
```
> git commit -am 'Simple pre-commit script' 
Running gitleaks pre-commit scan...
2:11PM INF 0 commits scanned.
2:11PM INF scanned ~1552 bytes (1.55 KB) in 160ms
2:11PM INF no leaks found
No secrets detected
[main 020c429] Simple pre-commit script
 1 file changed, 50 insertions(+)
 create mode 100755 scripts/pre-commit
```