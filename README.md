# deps-backup — Backup de dependencias críticas de terceros

Repositorio público de respaldo para dependencias **tipo 2** (repos de
referencia de código / binarios de release) de las que dependemos en nuestros
proyectos. Blindaje ante la desaparición, privatización o abandono del
upstream.

> **¿Por qué existe?** Si un repositorio open source del que dependemos
> desaparece, se vuelve privado o deja de mantenerse, y no lo tenemos clonado,
> perdemos la herramienta. Clonar un repo open source (MIT/Apache/AGPL) antes
> de que desaparezca te da derecho a conservar esa copia para siempre bajo su
> licencia. Este repo es esa copia de seguridad.

## Clasificación de dependencias

| Tipo | Ejemplo | ¿Necesita backup? |
|---|---|---|
| **1. Instalables vía package manager** | `pip install`, `npm install`, `cargo` | ❌ NO — el registry (PyPI/npm/crates.io) ES el backup. El paquete sigue instalable aunque el repo GitHub del autor muera. |
| **2. Repos de referencia de código** (no instalables, o binarios de release) | camofox-browser, ntfy, llmfit | ✅ SÍ — si el upstream muere, no hay registry que salve. Clonar YA. |
| **3. Modelos de Hugging Face** | modelos de IA | ⚠️ Parcial — pueden desaparecer o volverse gated. El backup natural es la descarga local. |

**Regla de oro:** si se instala con `pip`/`npm`/`cargo` → no clonar (registry).
Si es un repo que clonamos/descargamos como referencia o binario → **clonar**.

## Dependencias blindadas

| Dependencia | Versión | Licencia | Origen | Fecha | Tamaño | Por qué es crítica |
|---|---|---|---|---|---|---|
| **camofox-browser** | v1.16.0 | MIT | https://github.com/jo-inc/camofox-browser | 2026-09-15 | 3.2 MB (sin node_modules) | Navegador stealth (anti-detección) para agentes de IA — research/scraping de sitios que bloquean bots |
| **ntfy** | v2.28.0 | Apache-2.0 | https://github.com/binwiederhier/ntfy | 2026-09-15 | 21.3 MB | Servidor/CLI de notificaciones push (pub-sub por HTTP) — sistema de avisos en tiempo real |
| **llmfit** | 1.1.14 | Apache-2.0 | https://github.com/AlexsJones/llmfit | 2026-09-15 | 24.1 MB (sin target/) | Herramienta de fine-tuning/benchmark de LLMs locales (Rust, TUI + web + desktop) |

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
original y seguir las instrucciones de instalación del README del propio repo
(instalación, build, configuración).

## Licencias

Cada carpeta conserva su LICENSE/NOTICE original. MIT y Apache-2.0 permiten
copiar y conservar (con atribución). Este repo es público — cualquier uso debe
respetar la licencia de cada dependencia y atribuir su origen (tabla de arriba).

## Contribuir / reportar

Si detectas que una dependencia de este repo quedó desactualizada, o conoces
otra herramienta crítica que debería estar respaldada aquí, abre un issue o un
PR actualizando la tabla.