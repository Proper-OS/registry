# ProperOS registry

The default module registry for [ProperOS](https://github.com/Proper-OS/ProperOS):
one [`index.json`](index.json) mapping module names to release archives.
`proper install <name>`, `proper search <term>`, and the web interface's
Discover section all read it from

```
https://raw.githubusercontent.com/Proper-OS/registry/main/index.json
```

## Publishing a module or plugin

1. Pack it with `proper.json` at the archive root:
   `tar -czf my-module-1.0.0.tgz -C my-module .`
2. Upload the archive to a GitHub Release (or any public URL).
3. Add or update your entry here in `index.json`:

```json
"my-module": {
  "description": "What it does",
  "latest": "1.0.0",
  "versions": {
    "1.0.0": {
      "url": "https://github.com/you/my-module/releases/download/v1.0.0/my-module-1.0.0.tgz",
      "dependencies": []
    }
  }
}
```

`dependencies` are bare module names, resolved to their `latest`. For a
**plugin**, list its host module here so installing the plugin pulls the
host in too (at runtime the `host` field in `proper.json` is what makes it
a plugin).

Point a workspace at a different registry with
`proper config set registry.url <url>`.
