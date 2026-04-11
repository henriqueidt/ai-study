# Hooks

- Hooks are user-defined commands to enforce a behavior at specific points of Claude's lifecycle
- Regular hooks are deterministic, aka usually a shell command, or something that is very well defined

Example of a notification hook:

```JSON
{
  "hooks": {
    "Notification": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "osascript -e 'display notification \"Claude Code needs your attention\" with title \"Claude Code\"'"
          }
        ]
      }
    ]
  }
}
```

## Prompt-based Hooks

- Hooks that are not 100% deterministic and require decisioning
- Claude will send the prompt you define, along with the hook input data to a model to make the decision

Example of a hook that asks the model if all tasks are complete

```JSON
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Check if all tasks are complete. If not, respond with {\"ok\": false, \"reason\": \"what remains to be done\"}.",
            "model": "Haiku"
          }
        ]
      }
    ]
  }
}
```

## Agent-based Hooks

- Agent hooks will spawn a subagent that will perform actions

Example of a hook that checks if all tests are passing before allowing claude to stop

```JSON
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "agent",
            "prompt": "Verify that all unit tests pass. Run the test suite and check the results. $ARGUMENTS",
            "timeout": 120
          }
        ]
      }
    ]
  }
}
```

## HTTP Hooks

- HTTP Hooks will POST the event data to an HTTP endpoint
- Useful for auditing

Example of a hook sending events to an audit service

```JSON
{
  "hooks": {
    "PostToolUse": [
      {
        "hooks": [
          {
            "type": "http",
            "url": "http://localhost:8080/hooks/tool-use",
            "headers": {
              "Authorization": "Bearer $MY_TOKEN"
            },
            "allowedEnvVars": ["MY_TOKEN"]
          }
        ]
      }
    ]
  }
}
```
