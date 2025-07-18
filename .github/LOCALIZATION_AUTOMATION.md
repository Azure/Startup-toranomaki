# Automated Localization System

This repository includes a GitHub Actions workflow that automatically detects changes to the `docs/` directory and creates localization issues.

## 🔄 System Operation

### Triggers

The workflow executes under the following conditions:

1. When files are pushed to the `docs/` directory on the `main` branch
2. When Pull Requests with changes to the `docs/` directory are merged into the `main` branch

### Automated Processes

1. **Change Detection**: Identifies files changed within `docs/`
2. **Issue Creation**: Creates issues for each changed file for target languages (Japanese & Chinese)
3. **Auto Assignment**: Automatically assigns Copilot to created issues
4. **Labeling**: Automatically applies appropriate labels (`localization`, `lang:ja_jp`, `lang:zh_cn`, etc.)

## 📁 File Structure

```text
.github/
├── workflows/
│   └── auto-localization-issue.yml    # Automated localization workflow
└── ISSUE_TEMPLATE/
    └── localization-task.md           # Localization issue template

docs/                                   # Source documents (English)
├── ai-service-guides.md
├── architecture-patterns.md
└── ...

Localization/                          # Localization destination
├── ja_jp/                            # Japanese version
│   └── docs/
│       ├── ai-service-guides.md
│       └── ...
└── zh_cn/                            # Chinese version
    └── docs/
        ├── ai-service-guides.md
        └── ...
```

## 🏷️ Labels Used

| Label | Description |
|--------|-------------|
| `localization` | Localization-related tasks |
| `lang:ja_jp` | Japanese translation tasks |
| `lang:zh_cn` | Chinese translation tasks |
| `japanese` | Japanese-specific tasks |
| `chinese` | Chinese-specific tasks |
| `auto-generated` | Automatically generated issues |

## 📝 Issue Content

Automatically created issues include the following information:

- Source file path
- Target file path
- Target language
- Related commit information
- Translation guidelines
- Checklist-style tasks

## 🔧 Manual Issue Creation

In addition to automated issue creation, you can manually create localization issues by selecting the "Localization Task" template on the repository's Issues page.

## 🚀 Usage

1. Edit or add files in the `docs/` directory
2. Commit changes and push to `main` branch or merge PR
3. GitHub Actions automatically executes and creates localization issues
4. Work on translation in the created issues
5. After translation completion, place files in appropriate `Localization/[language]/docs/` path
6. Close the issue

## ⚙️ Configuration Customization

### Adding/Changing Target Languages

Edit the `languages` array in `auto-localization-issue.yml` to change target languages:

```javascript
const languages = [
  { code: 'ja_jp', name: 'Japanese', flag: '🇯🇵' },
  { code: 'zh_cn', name: 'Chinese', flag: '🇨🇳' },
  // Add new language
  { code: 'ko_kr', name: 'Korean', flag: '🇰🇷' }
];
```

### Changing Assignees

By default, Copilot is assigned, but you can change this by editing the `assignees` array:

```javascript
assignees: ['username1', 'username2']  // Specify GitHub usernames
```

## 🔍 Troubleshooting

### Common Issues

1. **Issues not created**
   - Verify files in `docs/` directory have been changed
   - Check GitHub Actions execution logs

2. **Permission errors**
   - Check appropriate permissions are set in repository Settings > Actions > General
   - Verify `GITHUB_TOKEN` has appropriate permissions

3. **Duplicate issues**
   - Multiple changes to the same file may create multiple issues
   - Manually close unnecessary issues

## 📞 Support

For questions or issues with the system, please create a repository issue or contact maintainers directly.
