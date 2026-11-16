# KDE settings

## Déjà versionné (stable)

`home/dot_config/` contient les fichiers `.rc` stables :

- `dolphinrc`, `konsolerc`, `kwinrulesrc`, `kxkbrc` ;
- `create_katerc` (seed : déployé seulement si absent) ;
- `create_plasma-org.kde.plasma.desktop-appletsrc` (seed).

Ces fichiers sont écrits par `chezmoi --source home apply`.

## Non versionné (runtime)

`kwinrc`, `kdeglobals` et `kglobalshortcutsrc` sont réécrits par Plasma (et par
`kde-material-you-colors` pour les couleurs). Les versionner crée du drift.

L'ancien `run_once_08-kde-settings.sh` imposait les clés qui comptent via
`kwriteconfig6` : effets KWin (translucency, wobbly, geometry change,
better-blur-dx), règles de fenêtres, etc. Le script complet est conservé dans
le tag `v2-full` ; récupère-le si tu veux le rejouer.

À la main, les commandes sont de la forme :

```bash
kwriteconfig6 --file kwinrc --group Plugins --key translucencyEnabled true
kwriteconfig6 --file kwinrc --group Plugins --key wobblywindowsEnabled true
kwriteconfig6 --file kwinrc --group Plugins --key kwin4_effect_geometry_changeEnabled true
```

Puis `qdbus6 org.kde.KWin /KWin reconfigure` (ou déconnexion/reconnexion).

Ne lance ces commandes que si `kwriteconfig6` existe (Plasma installé).

## Terminal par défaut

Plasma n'écrit pas `TerminalApplication` par défaut. Sans cette clé, un
`.desktop` avec `Terminal=true` tombe sur `konsole`, qui n'est pas installé ici
(le terminal quotidien est kitty) : le lanceur échoue avec « Terminal konsole
not found ».

```bash
kwriteconfig6 --file kdeglobals --group General --key TerminalApplication kitty
kwriteconfig6 --file kdeglobals --group General --key TerminalService kitty.desktop
kbuildsycoca6
```

Vérification :

```bash
kreadconfig6 --file kdeglobals --group General --key TerminalApplication
kreadconfig6 --file kdeglobals --group General --key TerminalService
```

`kitty.desktop` est fourni par le paquet `kitty` dans
`/usr/share/applications/`.
