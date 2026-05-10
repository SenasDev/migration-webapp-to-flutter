# Publishing checklist

Use this checklist before making the repository public or announcing it.

## Repository

- [ ] Repository is public on GitHub.
- [ ] Default branch is `main`.
- [ ] `SKILL.md` exists at repository root.
- [ ] README install command uses the real `SenasDev/migration-webapp-to-flutter`.
- [ ] README badge uses the real `SenasDev/migration-webapp-to-flutter`.
- [ ] License is Apache-2.0.
- [ ] GitHub repository topics include: `agent-skills`, `skills`, `skills-sh`, `flutter`, `dart`, `webapp`, `migration`.

## Validate with skills.sh CLI

List detected skills from the local repo before publishing:

```bash
npx skills add . --list
```

List detected skills from GitHub after publishing:

```bash
npx skills add SenasDev/migration-webapp-to-flutter --list
```

Install for Codex globally:

```bash
npx skills add SenasDev/migration-webapp-to-flutter -g -a codex -y
```

## After publishing

- [ ] Visit `https://skills.sh/senasdev/migration-webapp-to-flutter/migration-webapp-to-flutter`.
- [ ] Confirm badge renders: `https://skills.sh/b/senasdev/migration-webapp-to-flutter`.
- [ ] Test install from a clean directory.
- [ ] Confirm the detected skill name is `migration-webapp-to-flutter`.
