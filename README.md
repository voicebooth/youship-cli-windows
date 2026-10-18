# youship-cli-windows

YouShip command line for Windows.

## Installation

Install the `YouShip.CLI` package from [winget](https://learn.microsoft.com/windows/package-manager/winget/) using PowerShell:

```powershell
winget install YouShip.CLI
```

Open a new terminal afterwards so the updated `PATH` is picked up, then check it works:

```powershell
youship version
```

To upgrade or remove it later:

```powershell
winget upgrade YouShip.CLI
winget uninstall YouShip.CLI
```

## Usage

```
youship <command> [options]
```

| Command | Description |
| --- | --- |
| `youship login` | Sign in to a workspace |
| `youship logout` | Remove locally stored credentials |
| `youship whoami` | Show the signed-in user and workspace |
| `youship create project` | Create a project |
| `youship create release` | Create a release of a project |
| `youship deploy` | Deploy a release to an environment |
| `youship version` | Print the CLI version |
| `youship help` | Show usage |

Options can be given as `--name value` or `--name=value`. Add `--help` (or `-h`) to any command to see its usage, e.g. `youship deploy --help`.

Projects, environments and tenants can be given by name or by id.

### `login`

```
youship login --workspace <workspace> [--email <email>] [--no-browser]
```

| Option | Description |
| --- | --- |
| `--workspace <workspace>` | Workspace to sign in to, e.g. `myworkspace.youship.app`. If omitted in an interactive session you are prompted (the previously used workspace is offered as the default); in a non-interactive session it is required. |
| `--email <email>` | Guard against signing in as the wrong account. If you sign in as a different user, nothing is saved and the command fails. |
| `--no-browser` | Don't open a browser automatically. |

### `logout`

```
youship logout
```

### `whoami`

```
youship whoami
```

### `create project`

```
youship create project <name> [--group <group>] [--description <text>]
```

| Option | Description |
| --- | --- |
| `<name>` | Name of the new project (required). |
| `--group <group>` | Project group to create the project in. Optional when the workspace has exactly one group; otherwise required. |
| `--description <text>` | Project description. |

### `create release`

```
youship create release --project <project> --version <version> [--package-version <v>] [--tag <tag>] [--notes <text>]
```

| Option | Description |
| --- | --- |
| `--project <project>` | Project to release (required). |
| `--version <version>` | Release version (required). |
| `--package-version <v>` | Package version to pin for registered-package steps. |
| `--tag <tag>` | Image/version tag for container steps and registry-sourced package steps. Defaults to the release version. |
| `--notes <text>` | Release notes. |

### `deploy`

```
youship deploy --project <project> --version <version> [--environment <env>] [--tenant <tenant>] [--no-wait]
```

| Option | Description |
| --- | --- |
| `--project <project>` | Project to deploy (required). |
| `--version <version>` | Release version to deploy (required). The release must already exist. |
| `--environment <env>` | Target environment. Optional only when the workspace has exactly one environment. |
| `--tenant <tenant>` | Deploy for a specific tenant. |
| `--no-wait` | Return as soon as the deployment is queued instead of streaming its progress. |

While waiting, press Ctrl+C once to stop watching (the deployment keeps running on the server); press it a second time to kill the process. The exit code is `0` when the deployment succeeds and `1` otherwise.

## Example

```powershell
youship login --workspace myworkspace.youship.app
youship create project my-app --group Web
youship create release --project my-app --version 1.0.0
youship deploy --project my-app --version 1.0.0 --environment Production
```

## Configuration

Credentials are stored in `~/.youship/credentials.json`. Set the `YOUSHIP_HOME` environment variable to use a different folder.
