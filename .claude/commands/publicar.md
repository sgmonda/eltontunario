---
description: Publica el siguiente episodio pendiente (crea el post, calcula duración/bytes, commit y push)
allowed-tools: Bash, Read, Write, Glob
---

Publica el siguiente episodio del podcast que tenga audio en `mp3/` pero todavía no
tenga post en `_posts/`. Sigue estos pasos en orden y para si algo falla.

## 1. Localizar el siguiente episodio pendiente

- Lista `mp3/*.mp3` y `_posts/*.md`. Un mp3 `X.mp3` ya está publicado si existe algún
  fichero `_posts/YYYY-MM-DD-X.md` (el slug del post coincide con el nombre base del mp3).
- De los mp3 SIN post, ordénalos por temporada y episodio (formato `NxNN`: temporada N,
  episodio NN; o el formato antiguo `sNNeNN`) y quédate con el **primero** en ese orden.
- Si no hay ninguno pendiente, dilo y termina sin tocar nada.
- Deriva del slug la `season` y el `episode` (ej. `2x08` → season 2, episode 8).

## 2. Calcular duración y tamaño

```bash
# Duración en formato HH:MM:SS
secs=$(ffprobe -v error -show_entries format=duration -of default=noprint_wrappers=1:nokey=1 "mp3/<slug>.mp3")
printf '%02d:%02d:%02d\n' $((${secs%.*}/3600)) $((${secs%.*}%3600/60)) $((${secs%.*}%60))
# Tamaño exacto en bytes (macOS)
stat -f%z "mp3/<slug>.mp3"
```

`length` = los bytes exactos del paso anterior. `duration` = el HH:MM:SS calculado.

## 3. Calcular la fecha de publicación (siguiente martes libre)

Parte del próximo martes **estrictamente futuro** (si hoy es martes, el de la semana
siguiente) y, mientras ya exista un post programado para ese martes, **salta al
siguiente** hasta dar con uno libre. Un martes está ocupado si algún fichero de
`_posts/` empieza por esa fecha (`_posts/<FECHA>-*.md`).

```bash
dow=$(date +%u)                 # 1=lunes … 7=domingo
days=$(( (2 - dow + 7) % 7 ))   # martes = 2
[ "$days" -eq 0 ] && days=7
fecha=$(date -v+${days}d +%F)
# Saltar martes ya ocupados por otro post
while find _posts -maxdepth 1 -name "${fecha}-*.md" | grep -q .; do
  fecha=$(date -j -v+7d -f %F "$fecha" +%F)
done
echo "$fecha"
```

## 4. Pedir title y description, y crear el post

- Antes de escribir nada, **pregúntame en texto** el `title` y la `description` del
  episodio y espera mi respuesta. No los inventes.
- Crea `_posts/<FECHA>-<slug>.md` (FECHA del paso 3, slug del paso 1) con SOLO front
  matter, igual que los posts existentes (mira `_posts/2026-03-04-2x07.md` como modelo):

```yaml
---
layout: post
season: <N>
episode: <NN>
title: <title que te he dado>
description: <description que te he dado>
mp3: /mp3/<slug>.mp3
duration: "<HH:MM:SS>"
length: <bytes>
---
```

## 5. Commit y push

- `git add` el nuevo post y el mp3 (`mp3/<slug>.mp3`), y la imagen `img/<slug>.png` si
  existe y no está versionada.
- Commit con mensaje `feat: <slug>` (ej. `feat: 2x08`), siguiendo la convención del repo.
  No añadas firma de Co-Authored-By ni menciones a Claude (ver skill `commit` del repo).
- `git push`.
- Resume al final: episodio publicado, fecha programada, duración y tamaño.
