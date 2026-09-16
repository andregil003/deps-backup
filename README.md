# deps-backup — Backup de dependencias críticas de terceros

Repo privado de respaldo para dependencias **tipo 2** (repos de referencia de
código / binarios de release) de las que dependemos. Blindaje ante la
desaparición, privatización o abandono del upstream.

> **Regla:** si se instala con `pip`/`npm`/`cargo` → el registry es el backup,
> no va aquí. Este repo es SOLO para repos que clonamos/descargamos como
> referencia o binario. Ver skill `dependency-backup` en PuckStuff.

## Dependencias blindadas

| Dependencia | Versión | Licencia | Origen | Fecha | Tamaño | Por qué es crítica |
|---|---|---|---|---|---|---|
| **camofox-browser** | v1.16.0 | MIT | https://github.com/jo-inc/camofox-browser | 2026-09-15 | 3.2 MB (sin node_modules) | Navegador stealth para agentes de IA — research/scraping anti-detección |
| **ntfy** | v2.28.0 | Apache-2.0 | https://github.com/binwiederhier/ntfy | 2026-09-15 | 21.3 MB | Servidor/CLI de notificaciones push — sistema de avisos de PUCK (puck-listener, puck-slave) |
| **llmfit** | 1.1.14 | Apache-2.0 | https://github.com/AlexsJones/llmfit | 2026-09-15 | 24.1 MB (sin target/) | Herramienta de fine-tuning/benchmark de LLMs locales |

## Notas de limpieza

- **camofox-browser**: se excluyó `node_modules/` (regenerable con `npm install`).
- **ntfy**: copia completa del repo (sin `.git`).
- **llmfit**: se excluyó `target/` (artefactos de compilación Rust, regenerables
  con `cargo build`; un `.rlib` de 101 MB superaba el límite de GitHub).

## Cómo actualizar una dependencia

1. Re-clonar el upstream (o usar el clon local existente).
2. Copiar a `deps-backup/<nombre>` (excluyendo `.git`, `node_modules`, `target/`).
3. Actualizar la tabla de arriba (versión, fecha).
4. Commit + push.

## Cómo restaurar una dependencia

Si el upstream desapareció: copiar la carpeta desde este repo a su ubicación
original y seguir las instrucciones de instalación de la skill correspondiente
(en PuckStuff/skills-all).

## Licencias

Cada carpeta conserva su LICENSE/NOTICE original. MIT y Apache-2.0 permiten
copiar y conservar (con atribución). Este repo es privado — no se redistribuye.