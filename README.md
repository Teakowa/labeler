# labeler

Manage labels on GitHub as code, more info in [tonglil/labeler](https://github.com/tonglil/labeler)

## Installation

Get binaries for OS X / Linux / Windows from the latest [release].

Or use `go get`:

```
go get -u github.com/tonglil/labeler
```

[release]: https://github.com/tonglil/labeler/releases

## Usage

First, set a [GitHub token][tokens] in the environment (optional, the token can be set as an cli argument as well).

```
export GITHUB_TOKEN=xxx
```

> - The token for public repos need the `public_repo` scope.
> - The token for private repos need the `repo` scope.

[tokens]: https://github.com/settings/tokens

### Scanning labels

To scan existing labels from a repository and save it to a file:
```
labeler scan labels.yaml --repo owner/name
```

Which when run against a "new" repo created on GitHub, will:
- Fetch `bug` with color `fc2929`
- Fetch `duplicate` with color `cccccc`
- Fetch `enhancement` with color `84b6eb`
- Fetch `invalid` with color `e6e6e6`
- Fetch `question` with color `cc317c`
- Fetch `wontfix` with color `ffffff`

And write them into `labels.yaml`, creating the file if it doesn't exist, otherwise overwriting its contents.

### Applying labels

To apply labels to a repository:
```
labeler apply labels.yaml --dryrun
```

Where `labels.yaml` is like:
```yml
repo: owner/name
labels:
  - name: bug
    color: fc2929
  - name: help wanted
    color: 000000
  - name: fix
    color: cccccc
    from: wontfix
  - name: notes
    color: fbca04
```

Which when run against a "new" repo created on GitHub, will:
- Rename `wontfix` to `fix` with color `ffffff` to `ffffff`
- Update `help wanted` with color `159818` to `000000`
- Create `notes` with color `fbca04`
- Delete `duplicate` with color `cccccc`
- Delete `enhancement` with color `84b6eb`
- Delete `invalid` with color `e6e6e6`
- Delete `question` with color `cc317c`

When run again, rename changes will not be run because the label already exists.
In this manner, this tool is idempotent.

## Usage options

```
$ labeler
Labeler is a CLI application for managing labels on Github as code.

With the ability to scan and apply label changes, repository maintainers can
empower contributors to submit PRs and improve the project management
process/label system!

Usage:
  labeler [command]

Available Commands:
  apply       Apply a YAML label definition file
  completion  Output shell completion code for tab completion
  scan        Save a repository's labels into a YAML definition file
  version     Print the version information

Use "labeler [command] --help" for more information about a command.
```
