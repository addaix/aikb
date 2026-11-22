# aikb.base

Core module: protocols, stores, providers, and factory functions for aikb.

### Functions

| [`ClaudeProject`](#aikb.base.ClaudeProject)(project_id, \*[, session_key, ...])   | Create a Claude Project knowledge store.   |
|------------------------------------------------------------------------------------------------------|--------------------------------------------|
| [`LocalKb`](#aikb.base.LocalKb)([rootdir, project_id])                      | Create a local filesystem knowledge store. |

### Classes

| [`ClaudeProjects`](#aikb.base.ClaudeProjects)(\*[, session_key, ...])         | Read-only mapping of Claude project names to KnowledgeFiles stores.   |
|-------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------|
| [`ClaudeProjectsProvider`](#aikb.base.ClaudeProjectsProvider)([session_key, ...])     | Claude Projects provider wrapping ClaudeSync.                         |
| [`KnowledgeBaseProvider`](#aikb.base.KnowledgeBaseProvider)(\*args, \*\*kwargs)      | Structural interface for knowledge base backends.                     |
| [`KnowledgeFiles`](#aikb.base.KnowledgeFiles)(provider, \*, project_id)       | Dict-like access to knowledge files in an AI project.                 |
| [`LocalFilesProvider`](#aikb.base.LocalFilesProvider)(rootdir, \*[, create_dirs]) | Local filesystem provider for knowledge files.                        |

### aikb.base.ClaudeProject(project_id, , session_key=None, organization_id=None)

Create a Claude Project knowledge store.

Requires `pip install aikb[claude]`.

Session key is resolved automatically (env var, ClaudeSync config,
browser cookies). Pass *session_key* explicitly to override.

* **Return type:**
  [`KnowledgeFiles`](#aikb.base.KnowledgeFiles)

### *class* aikb.base.ClaudeProjects(, session_key=None, organization_id=None, include_archived=False)

Bases: [`Mapping`](https://docs.python.org/3/library/collections.abc.html#collections.abc.Mapping)

Read-only mapping of Claude project names to KnowledgeFiles stores.

```pycon
>>> p = ClaudeProjects(session_key='sk-ant-...')
>>> list(p)
['My Project', 'Another Project']
>>> files = p['My Project']
>>> list(files)
['context.md', 'notes.md']
```

### *class* aikb.base.ClaudeProjectsProvider(session_key=None, , organization_id=None)

Bases: [`object`](https://docs.python.org/3/builtins/functions.html#object)

Claude Projects provider wrapping ClaudeSync.

Requires `pip install aikb[claude]`.

Session key resolution priority:

1. Explicit *session_key* parameter
2. `CLAUDE_SESSION_KEY` environment variable
3. ClaudeSync stored config (`~/.claudesync/`)
4. Browser cookies (requires `pip install aikb[cookies]`)

### *class* aikb.base.KnowledgeBaseProvider(\*args, \*\*kwargs)

Bases: [`Protocol`](https://docs.python.org/3/library/typing.html#typing.Protocol)

Structural interface for knowledge base backends.

Any object implementing these four methods can be used as a provider.

### *class* aikb.base.KnowledgeFiles(provider, , project_id)

Bases: [`MutableMapping`](https://docs.python.org/3/library/collections.abc.html#collections.abc.MutableMapping)

Dict-like access to knowledge files in an AI project.

Keys are filenames (str), values are file contents (str).

```pycon
>>> import tempfile, os
>>> store = LocalKb(tempfile.mkdtemp())
>>> store['notes.md'] = '# Notes'
>>> store['notes.md']
'# Notes'
>>> list(store)
['notes.md']
>>> len(store)
1
>>> del store['notes.md']
>>> list(store)
[]
```

### *class* aikb.base.LocalFilesProvider(rootdir, , create_dirs=True)

Bases: [`object`](https://docs.python.org/3/builtins/functions.html#object)

Local filesystem provider for knowledge files.

Each `project_id` maps to a subdirectory under *rootdir*.
Files are stored as UTF-8 text.

### aikb.base.LocalKb(rootdir=None, , project_id='default')

Create a local filesystem knowledge store.

Files are stored in `rootdir / project_id /`.
Defaults to `~/.local/share/aikb/localkb_files/` or the
`AIKB_LOCAL_DIR` environment variable.

```pycon
>>> import tempfile
>>> store = LocalKb(tempfile.mkdtemp())
>>> store['ideas.md'] = '# Ideas\nFirst idea'
>>> store['ideas.md']
'# Ideas\nFirst idea'
```

* **Return type:**
  [`KnowledgeFiles`](#aikb.base.KnowledgeFiles)
