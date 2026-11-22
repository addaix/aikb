# aikb.sync

### aikb.sync(a, b, \*, manifest=None, manifest_path=None, on_conflict=ConflictPolicy.RAISE, dry_run=False, hash_fn=<function content_hash>)

Bidirectional sync with three-way reconciliation.

Requires a *manifest* (or *manifest_path*) for correct conflict detection.
If neither is provided, uses an empty manifest (first sync — everything
looks “created”).

After successful propagation, updates the manifest to reflect the new
synchronized state.

* **Return type:**
  [`list`](https://docs.python.org/3/builtins/stdtypes.html#list)[`SyncAction`]
