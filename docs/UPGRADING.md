# Upgrading from an earlier release

Earlier releases used legacy identifiers: Python package `deerflow`, env vars `DEER_FLOW_*`, data dir `.deer-flow`, Compose project `deer-flow-dev` and containers `deer-flow-*`. Code changes alone break nothing — for one release:

- `import deerflow…` and `use: deerflow.x:Y` paths in `config.yaml` still resolve (deprecated alias of `nova`);
- every `NOVA_X` falls back to `DEER_FLOW_X` when unset;
- an existing `.deer-flow` data dir is used when no `.nova` dir exists (data is never moved automatically).

The infrastructure cutover (Compose project, containers, network and volumes, Postgres data included) is a separate, operator-run step:

```bash
scripts/migrate-deerflow-to-nova.sh            # dry run (default): prints the plan, changes nothing
```

The script backs up first and copies volumes rather than moving them; the old `deer-flow-*` volumes are never deleted, so rolling back means bringing the old project up again. See the script's `--help` and the rename entry in [NOVA_CHANGELOG.md](./NOVA_CHANGELOG.md).


### Claude Code Integration

```bash
npx skills add https://github.com/Ali-Labs-X/nova --skill claude-to-deerflow
```
