# ansible-inventory-auditor

Find hosts without an owner group in a normalized inventory.

## Run

Python 3.8 or later; no third-party packages or cloud credentials are required.

```sh
python3 tool.py examples/input.json
cat examples/input.json | python3 tool.py -
python3 -m unittest -v
```

## Example

Input:

```json
{
  "hosts": [
    "web1",
    "db1"
  ],
  "groups": {
    "web": [
      "web1",
      "old1"
    ]
  }
}
```

Result:

```json
{
  "ungrouped_hosts": [
    "db1"
  ],
  "unknown_group_members": [
    "old1"
  ]
}
```

The command writes JSON to standard output. Exit 0 means evaluation completed;
a finding in the output may still require attention. Invalid input returns exit 2,
with an error on standard error and no result on standard output.

## Operational scope

Uses a flat JSON projection; Ansible child groups, patterns, and variable precedence are not evaluated.

The tool evaluates supplied data locally. It does not change hosts, deploy
resources, or establish production compliance. Example identifiers and observations
are test fixtures. Keep credentials and private inventory out of Git.

## Design and verification

See [NOTES.md](NOTES.md) for the input contract, verification, and integration notes.
Related utilities: [maintenance-window-planner](https://github.com/sal-s-root/maintenance-window-planner), [least-privilege-file-auditor](https://github.com/sal-s-root/least-privilege-file-auditor).
