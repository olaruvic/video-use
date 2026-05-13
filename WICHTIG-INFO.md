# Git: video-use — Fork, Submodule, Upstream

Stand: `video-use` liegt unter **`test-video-editor`** als **Submodule** und zeigt in `.gitmodules` auf den Fork **`olaruvic/video-use`** (`branch = local/main`). Auf GitHub ist `video-use` dort verlinkt; lokale Arbeit läuft auf Branch **`local/main`**.

## Aktuelles Remote (Submodule / normaler Klon des Forks)

```bash
git remote -v
```

Typisch nach Submodule-Checkout:

- **`origin`** → `git@github.com:olaruvic/video-use.git` (Fetch **und** Push)

**Push zu deinem Fork** (Standard):

```bash
git push origin local/main
```

Damit landet nichts bei **`browser-use/video-use`** — das ist nur noch „Upstream“, nicht dein `origin`.

## Updates vom Original (browser-use/video-use)

Einmalig **`upstream`** hinzufügen:

```bash
git remote add upstream https://github.com/browser-use/video-use.git
```

Dann z. B.:

```bash
git fetch upstream
git merge upstream/main
# oder: git rebase upstream/main   (nach Geschmack / Policy)
```

Konflikte wie gewohnt lösen, danach **`git push origin local/main`**, damit der Fork den Stand hat, den das Studio-Repo per Submodule einbinden kann.

## Root-Repo (test-video-editor): Submodule-Zeiger aktualisieren

Wenn sich der Commit in `video-use/` geändert hat (neuer Push auf den Fork):

```bash
cd /pfad/zu/test-video-editor
cd video-use && git pull origin local/main && cd ..
git add video-use
git commit -m "[main] chore: bump video-use submodule"
git push origin main
```

## `uv.lock` lokal angepasst

Eigene Lockfile-Änderungen vor dem Zusammenführen mit Upstream am besten **auf `local/main` committen**, damit sie bei Konflikten nicht „untergehen“. Bei Konflikt in `uv.lock` gezielt auflösen oder nach Anpassung von `pyproject.toml` mit **`uv lock`** neu erzeugen.

## Älterer Stand: separater Klon nur von browser-use

Wenn du **`video-use` irgendwo allein** geklont hast (nicht als Submodule), kann dort noch die alte Variante mit **`origin`** = browser-use und **Push-Sperre** (`git remote set-url --push origin https://example.invalid/no-push`) und extra **`fork`**-Remote existieren. Remotes dann mit `git remote -v` prüfen — nicht mit diesem Submodule-Setup verwechseln.
