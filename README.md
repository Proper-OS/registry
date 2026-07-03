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
**plugin**, set `"host": "<module>"` on the entry (so tooling can gate the
install on the host being present) **and** list the host in `dependencies`
(so installing the plugin pulls it in). At runtime the `host` field in
`proper.json` is what makes it a plugin.

## Third-party and trusted registries

Anyone can host a registry (any URL serving an `index.json` in this format)
and users add them with `proper registry add <url>`. Results from those
registries show as **Third-party** unless the URL is listed in this repo's
[`trusted.json`](trusted.json) — the **Trusted** tag is granted by the
ProperOS project, never self-declared. To request it, open a pull request
adding your registry's URL to `trusted.json` with a short description of
who runs it and what it serves.

The official registry is always queried first, so a third-party registry can
never shadow a module name published here.

Point a workspace at a different primary registry with
`proper config set registry.url <url>`.
