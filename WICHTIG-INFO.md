# Git: Pull ja, versehentlicher Push nach `origin` nein

Damit nicht versehentlich nach dem Upstream-Repo gepusht wird, ist die **Push-URL** von `origin` auf eine ungültige Adresse gesetzt:

```bash
git remote set-url --push origin https://example.invalid/no-push
```

**Was das bewirkt**

- **Fetch / Pull** nutzen weiterhin die normale **Fetch-URL** (`https://github.com/browser-use/video-use`). `git fetch` und `git pull` funktionieren wie gewohnt — Aktualisierungen vom Original kommen an.
- **`git push origin …`** schlägt fehl (Host `example.invalid` ist absichtlich nicht auflösbar). So landet nichts aus Versehen bei `browser-use/video-use`.

**Kontrolle**

```bash
git remote -v
```

Dort sollten zwei Zeilen für `origin` stehen: eine mit `github.com/.../video-use` (fetch), eine mit `example.invalid` (push).

**Push wieder erlauben** (z. B. eigener Fork oder bewusst Upstream)

```bash
git remote set-url --push origin https://github.com/<account>/<repo>.git
```

Optional separaten Remote fürs eigene Repo: `git remote add fork …` und nur `git push fork <branch>` nutzen — dann kann `origin` Push blockiert bleiben.

**`uv.lock` lokal angepasst**

Eigene Lockfile-Änderungen vor dem Mergen/Pullen am besten **auf einem lokalen Branch committen**, damit sie bei Konflikten mit Upstream nicht „untergehen“. Bei Konflikt in `uv.lock` gezielt auflösen oder nach Anpassung von `pyproject.toml` mit `uv lock` neu erzeugen.
