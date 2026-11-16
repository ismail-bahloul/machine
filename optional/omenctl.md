# omenctl

Outil séparé (ventilateurs EC + profils de puissance). Le repo est **public**
(`gh repo view ismail-bahloul/omenctl`) ; un clone local reste utile pour une
fresh install hors ligne.

- repo : `https://github.com/ismail-bahloul/omenctl.git`
- dernière ref publiée : `v0.5.0` (pas de release GitHub, seulement un tag)

## Installer

Depuis un checkout contenant `omenctl.py`, `omenctl-run.sh` et
`omenctl.service` :

```bash
sudo install -Dm755 omenctl.py /usr/local/bin/omenctl
sudo install -Dm755 omenctl-run.sh /usr/local/bin/omenctl-run.sh
sudo install -Dm644 omenctl.service /etc/systemd/system/omenctl.service
sudo systemctl daemon-reload
sudo systemctl enable --now omenctl.service
```

Les units `omenctl-profile.service`, `omenctl-profile.timer`,
`omenctl-watch.service` et les règles udev sont déjà dans `../system/etc/`
(`install.sh --system` les copie).

Vérifier :

```bash
omenctl status
systemctl status omenctl.service
```

## Si tu n'utilises pas omenctl

L'ancien repo masquait `power-profiles-daemon`. Sans omenctl, remets-le :

```bash
sudo systemctl unmask power-profiles-daemon.service
sudo systemctl enable --now power-profiles-daemon.service
```

Sans l'un ou l'autre, les ventilateurs restent sur la courbe EC par défaut
(plus bruyante à haute température), ce qui est sûr mais moins agréable.
