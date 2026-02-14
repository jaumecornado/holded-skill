# Holded CLI Reference

Short reference for operating Holded ERP through `holdedcli`.

## Installation and Status

```bash
brew tap jaumecornado/tap
brew install holded
holded help
holded auth status
```

## Credentials (priority)

1. `--api-key`
2. `HOLDED_API_KEY`
3. `~/.config/holdedcli/config.yaml`

## Action Discovery

List all:

```bash
holded actions list --json
```

Filter:

```bash
holded actions list --filter contacts --json
holded actions list --filter invoice --json
```

## Run an Action

```bash
holded actions run <action-id|operation-id> \
  --path key=value \
  --query key=value \
  --body '{"field":"value"}' \
  --json
```

Payload options:

- `--body '<json>'`
- `--body-file payload.json`

Do not use both at the same time.

## Risk Classification

- Read: `GET`, `HEAD`
- Write: `POST`, `PUT`, `PATCH`, `DELETE`

If the action is a write operation, request explicit confirmation before execution.

## Examples

Read a contact:

```bash
holded actions run invoice.get-contact --path contactId=abc123 --json
```

Update a contact (requires prior confirmation):

```bash
holded actions run invoice.update-contact \
  --path contactId=abc123 \
  --body-file payload.json \
  --json
```

## Common Errors

- `MISSING_API_KEY`: missing API key.
- `ACTION_NOT_FOUND`: unknown action; review `actions list`.
- `INVALID_BODY`: invalid JSON or incompatible flags.
- `API_ERROR`: HTTP error returned by Holded.

## Sources

- Holded CLI: <https://github.com/jaumecornado/holdedcli>
- Holded web: <https://www.holded.com>
- Holded API docs: <https://developers.holded.com/reference/api-key>

## OpenClaw Config Example

Use this minimal snippet in your OpenClaw config to explicitly enable the skill and inject the API key value to `HOLDED_API_KEY` via `primaryEnv`:

```json
{
    "skills": {
      "entries": {
      "holded-skill": {
        "enabled": true,
        "apiKey": "YOUR_HOLDED_API_KEY"
      }
    }
  }
}
```
