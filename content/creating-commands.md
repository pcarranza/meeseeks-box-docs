---
title: "Configuring commands"
anchor: "configuring-commands"
weight: 30
---

## Configuring Commands

Simply add those commands to the yaml configuration file, like this:

```yaml
---
commands:
  echo:
    command: "echo"
      auth_strategy: any
      timeout: 5
      help: command that prints back the arguments passed
```

Add as many commands as you need, with the only caveat tha they each command needs to have a different name.

To invoke this command you will need to restart the process (still no hot configuration reloaded supported) and then write `@bot-name echo argument1 argument2` in a channel where the bot has already been invited.

When invoking this command, the execution will be translated into calling the `echo` binary, passing through the arguments the user wrote in slack.

As stated before, the `command` has to be an executable, it has to be inside the path and can only be a single word.


### Options

A command can be configured the following way:

- `command`: the command to execute
- `args`: list of arguments to always prepend to the command
- `timeout`: how long we allow the command to run until we cancel it, in
  seconds, 60 by default
- `auth_strategy`: defined the authorization strategy
  - `any`: everyone will be allowed to run this command
  - `none`: no user will be allowed to run this command (default value,
    permissions have to be explicit and conscious)
  - `group`: use `allowed_groups` to control who has access to this command
- `allowed_groups`: list of groups allowed to run this command
- `help`: help to be printed when using the builtin `help` command
- `templates`: adds the capacity to change how the replies from this command
  are represented, check the Templating help for more details.

### A slightly more complex example

Running a command with docker, for example, would look like this

```
groups:
  docker: ["pablo"]
commands:
  run-command:
    command: docker
    args:
      - "run"
      - "-it"
      - "--rm"
      - "container-image:latest"
    auth_strategy: group
    allowed_groups: ["docker"]
    help: "Run the container-image docker image passing arguments in"
```

This will launch a container image every time the command in invoked.