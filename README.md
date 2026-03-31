# Which Dystopia Are You Living In?

Literary personality quiz — single-page app (vanilla HTML/CSS/JS, no build).

**Repository:** [github.com/bananasutra/which-dystopia](https://github.com/bananasutra/which-dystopia)

**Live site (after GitHub Pages is enabled):** [bananasutra.github.io/which-dystopia](https://bananasutra.github.io/which-dystopia/)

## What ships

| Deliverable | Notes |
|-------------|--------|
| `index.html` | Entire app: questions, dual-dimension scoring (world vs coping), split or unified result layouts, share via URL hash, clipboard + “bird site” intent, `<details>` reference table, accessible “eyebrow” contrast, terminal aesthetic |
| `which-dystopia-spec.md` | Full product spec (kept in sync with shipped behavior) |

## Requirements (runtime)

- **Browser:** Modern evergreen (ES5-style JS, CSS `color-mix` with fallback, `<details>`/`<summary>`).
- **Hosting:** Static HTTPS (e.g. **GitHub Pages**). Clipboard copy for “Transmit result” needs a secure context; `file://` may not support it.
- **No** Node, bundler, or backend — deploy the file as-is.

## Deploy on GitHub Pages

1. Push `index.html` (and this `README.md`) to the `main` branch of `bananasutra/which-dystopia`.
2. Repo **Settings → Pages → Build and deployment**: source **Deploy from a branch**, branch **`main`**, folder **`/ (root)`**.
3. Site URL: `https://bananasutra.github.io/which-dystopia/` (propagation usually under a minute).

## Share links

Results use both dimensions in the fragment:

```text
https://bananasutra.github.io/which-dystopia/#world=atwood&coping=kafka
```

Legacy `#archetype=…` (single value) is still accepted for old bookmarks.

## License / credit

Content and concept by the project author; built for GitHub Pages delivery.
