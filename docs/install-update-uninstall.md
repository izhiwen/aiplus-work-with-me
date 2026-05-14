# Install, Update, Disable, Uninstall

Install:

```bash
aiplus profile install aiplus-work-with-you --user --yes
```

Dry run:

```bash
aiplus profile install aiplus-work-with-you --user --dry-run
```

Status and doctor:

```bash
aiplus profile status
aiplus profile doctor
```

Disable for future sessions:

```bash
aiplus profile disable aiplus-work-with-you --user --yes
```

Uninstall:

```bash
aiplus profile uninstall aiplus-work-with-you --user --yes
```

All profile writes must stay under user-owned AiPlus config locations and must
not write secret values.
