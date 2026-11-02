# claude-settings

The [Claude Code settings](https://docs.claude.com/en/docs/claude-code/settings) of AYÇİÇEK Labs. They enforce the Git safety rules of [`ai-rules`](https://github.com/AYCICEK-Labs/ai-rules) instead of only asking for them.

| Setting            | Effect                                                                                          |
| ------------------ | ----------------------------------------------------------------------------------------------- |
| `attribution`      | No `Co-Authored-By` line in commits and no "Generated with Claude Code" line in pull requests.  |
| `permissions.ask`  | Every `git push` waits for approval.                                                            |
| `permissions.deny` | Blocks force-push, `--no-verify`, changing `user.name` / `user.email` and reading `.env` files. |

Permission rules match command prefixes. They are a safety net, not a sandbox: the rules in `AGENTS.md` still apply.

## Use it in a project

As `.claude/settings.json`, committed, so it also applies in cloud sessions:

```sh
mkdir -p .claude
curl -fsSL https://raw.githubusercontent.com/AYCICEK-Labs/claude-settings/main/settings.json -o .claude/settings.json
```

## Use it on every project of a machine

Merge it into `~/.claude/settings.json`, keeping the settings already there:

```sh
curl -fsSL https://raw.githubusercontent.com/AYCICEK-Labs/claude-settings/main/settings.json -o /tmp/aycicek-settings.json
mkdir -p ~/.claude && [ -s ~/.claude/settings.json ] || echo "{}" > ~/.claude/settings.json
jq -s 'def m($a; $b): if ($a|type) == "object" and ($b|type) == "object" then reduce ($b|keys[]) as $k ($a; .[$k] = m(.[$k]; $b[$k])) elif ($a|type) == "array" and ($b|type) == "array" then reduce ($a + $b)[] as $x ([]; if any(.[]; . == $x) then . else . + [$x] end) else $b end; m(.[0]; .[1])' ~/.claude/settings.json /tmp/aycicek-settings.json > /tmp/merged.json && mv /tmp/merged.json ~/.claude/settings.json
```

## License

[MIT](LICENSE) © Yunus Emre AYÇİÇEK
