# Guía Rápida de Comandos (Cheat Sheet)

Referencia rápida de comandos de Git agrupados por necesidad práctica del día a día.

---

## 🚀 1. Inicio y Configuración

| Tarea | Comando |
| :--- | :--- |
| Configurar usuario | `git config --global user.name "Tu Nombre"` |
| Configurar email | `git config --global user.email "tu@email.com"` |
| Configurar rama inicial | `git config --global init.defaultBranch main` |
| Iniciar repositorio local | `git init` |
| Clonar repositorio | `git clone <url>` |
| Ver remotos configurados | `git remote -v` |
| Conectar con remoto | `git remote add origin <url>` |

---

## 📝 2. Estado y Preparación de Cambios

| Tarea | Comando |
| :--- | :--- |
| Ver estado | `git status` o `git status -s` |
| Ver diferencias en archivos | `git diff` |
| Ver diferencias en Staging | `git diff --staged` |
| Preparar un archivo | `git add <archivo>` |
| Preparar todo | `git add .` |
| Preparación interactiva | `git add -p` |
| Quitar del Staging | `git restore --staged <archivo>` |
| Descartar cambios en disco | `git restore <archivo>` |

---

## 💾 3. Commits y Registro

| Tarea | Comando |
| :--- | :--- |
| Crear commit | `git commit -m "feat: mensaje"` |
| Preparar y commitear directo | `git commit -am "fix: mensaje"` |
| Modificar último commit | `git commit --amend --no-edit` |
| Historial en una línea | `git log --oneline` |
| Historial gráfico en árbol | `git log --graph --oneline --all` |
| Ver últimos N commits | `git log -n 5` |
| Historial con cambios de código | `git log -p` |

---

## 🌿 4. Ramas (Branches)

| Tarea | Comando |
| :--- | :--- |
| Listar ramas | `git branch` (locales) / `git branch -a` (todas) |
| Crear y cambiar de rama | `git switch -c <nueva-rama>` |
| Cambiar de rama | `git switch <nombre-rama>` |
| Renombrar rama actual | `git branch -m <nuevo-nombre>` |
| Borrar rama local fusionada | `git branch -d <nombre-rama>` |
| Forzar borrado de rama | `git branch -D <nombre-rama>` |
| Borrar rama remota | `git push origin --delete <nombre-rama>` |

---

## 🔀 5. Fusiones, Squash y Rebase

| Tarea | Comando |
| :--- | :--- |
| Merge estándar | `git merge <rama>` |
| Merge forzando commit | `git merge --no-ff <rama>` |
| **Merge Squash** | `git merge --squash <rama>` seguido de `git commit -m "..."` |
| Rebase sobre rama base | `git rebase main` |
| Continuar Rebase tras conflicto | `git rebase --continue` |
| Abortar Merge en conflicto | `git merge --abort` |
| Abortar Rebase | `git rebase --abort` |

---

## ☁️ 6. Sincronización Remota

| Tarea | Comando |
| :--- | :--- |
| Enviar cambios con tracking | `git push -u origin <rama>` |
| Enviar cambios regulares | `git push` |
| Descargar sin fusionar | `git fetch --all` |
| Descargar y fusionar | `git pull` |
| Descargar con Rebase (Limpio) | `git pull --rebase` |

---

## 🧰 7. Stash y Operaciones Especiales

| Tarea | Comando |
| :--- | :--- |
| Guardar cambios temporalmente | `git stash push -m "mensaje"` |
| Listar stashes | `git stash list` |
| Recuperar y borrar último stash | `git stash pop` |
| Recuperar sin borrar stash | `git stash apply` |
| Borrar un stash | `git stash drop stash@{0}` |
| Revertir commit público | `git revert <hash-commit>` |
| Reset suave (mantiene staging) | `git reset --soft HEAD~1` |
| Reset duro (**Borra cambios**) | `git reset --hard HEAD~1` |
| Aplicar un commit específico | `git cherry-pick <hash-commit>` |
| Ver registro de rescate | `git reflog` |

---

## 🧭 Navegación

- Volver al índice general: **[[Index|Índice de Git]]**
