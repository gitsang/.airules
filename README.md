# AI Rules

## 1. Usage

1. Clone the repo to `~/.airules`
2. Add prompts to the tools to make it read the rules files:

### 1.1 prompt

```txt
To read the rules, run the following command at the beginning of each task. Make sure you know what the rules file contains.

`find ~/.airules -type f -name "*.md" -exec cat {} \; && find .airules -type f -name "*.md" -exec cat {} \;`
```
