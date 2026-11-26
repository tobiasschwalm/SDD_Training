# Templates

Files to copy into the to-do app in the root folder of the learning environment. In the learning environment the templates are in `exercises/templates/`. None of these files is part of OpenSpec.

| File | Purpose | Used in |
|---|---|---|
| `config/01-todo-app.yaml` | replaces the content of `openspec/config.yaml`: project, tech stack (HTML5, JavaScript, LocalStorage, node:test), conventions and non-goals as context, no rules | Set up OpenSpec, all later exercises |
| `config/06-todo-app-layers.yaml` | the same plus the architect's decision as the section “Architecture” (UI components, application logic, persistence, `src/main.js`), replaces the content of `openspec/config.yaml` | Layers without behavior change |

To copy a config.yaml template, for example:

```bash
cp exercises/templates/config/01-todo-app.yaml openspec/config.yaml
```

`openspec init` creates its own `config.yaml`. The template replaces it.



## License

Our own texts are under MIT, see `LICENSE` in the root folder of the learning environment. Third-party parts state their origin and license in their own folder.

OpenSpec, Claude Code and OpenFastTrace are names or trademarks of their owners. This material is independent of their makers.
