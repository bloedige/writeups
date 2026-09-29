# IA para CTF — hallar la flag rápido

Trabajamos con **Claude Code** u **OpenCode**: la IA **ejecuta los comandos ella misma**, no te los dicta.
Objetivo único: **encontrar la flag.** Donde más sirve: **Forensics, Reversing, Exploiting, Stego.**

---

## 0. Preparar

Todo esto se hace **por línea de comandos (terminal)**.

1. **Instalar OpenCode** (una sola vez), en tu **Kali por terminal**:
   ```bash
   curl -fsSL https://opencode.ai/v2/install | bash
   ```
   Al terminar, **cierra la terminal y ábrela de nuevo** para que se cargue el comando.
2. **Una carpeta por reto** (créala y mete ahí los archivos **por terminal**). Nada más.
   ```bash
   mkdir reto1 && mv archivo_del_reto reto1/ && cd reto1
   ```
3. Ya dentro de la carpeta, arranca la IA:
   ```bash
   opencode
   ```
   **La primera vez** hay que conectar un proveedor (no pide "cuenta", pide API key). Dentro de OpenCode:
   ```
   /connect
   ```
   Elige el proveedor, ve a **opencode.ai/auth**, inicia sesión, copia tu **API key** y pégala cuando la pida. Esto se hace una sola vez.
4. Cuando **pierda el contexto**, reinicia sesión limpia (o cierra y vuelve a abrir):
   ```
   /new
   ```
5. Si **se acaban los tokens**, cambia de modelo sin salir:
   ```
   /model
   ```
   Selecciona el modelo → **Enter**.
6. Si la IA **pide permiso** para ejecutar algo, dale **allow / allow once** (o la opción de "no volver a preguntar") para que no se detenga a cada rato.

---

## 1. Arrancar

Dale el reto y déjala trabajar:

```
Reto CTF, categoría <Forensics/Reversing/Exploiting/Stego>.
Enunciado: "<literal>"
Archivos: <los que se encuentren en la capeta si tiene> s<opcional>
Encuentra la flag. Ejecuta lo que necesites y ve directo al grano.
```

---

## 2. Si empieza a perderse (lo importante)

La IA a veces **alucina, pierde el foco y toca mil cosas** sin centrarse en el reto.
Cuando pase:

1. **Sesión nueva** (`/new` o reabrir).
2. Rescata lo **concreto que ya había encontrado** (pistas reales, no teoría).
3. Dáselo directo y acótalo:

```
Ya encontraste estas pistas: <wallets / cadenas en base64 / archivos ocultos / ...>.
Trabaja SOLO sobre eso para dar con la flag. No te desvíes.
```

> Regla: una sesión nueva + pistas concretas > seguir peleando con una sesión perdida.

---

## 3. Mantenerla enfocada

- Un reto por sesión.
- Si se va por las ramas → recórtala: *"céntrate solo en esto"*.
- Si se atasca en 2 intentos → pídele **otro enfoque**, no que repita.

---

## 4. Cuando aparece la flag → GUARDAR (siempre al final)

Dentro de la carpeta del reto, crea un `solucion.md` con **TODOS los pasos concretos** que llevaron a la flag, en orden. **No omitas ningún paso.** Sin explicar el porqué.

### Plantilla (`solucion.md`)

```
# <reto>  (<categoría>)
Flag: <la flag exacta>

Pasos:
1. <lo que se hizo>
2. <lo que se hizo>
3. <lo que se hizo>
```

---

## ⚠️ Importante

**Un reto por carpeta. No mezclar.**
Así la IA trabaja bien: no tiene que ir buscando carpeta por carpeta ni se confunde con archivos de otros retos.

**Todo se maneja desde la terminal (línea de comandos).** OpenCode vive ahí y ejecuta los comandos por ti.

---

## Skill: `ctf-solver`

En la carpeta [`ctf-solver/`](ctf-solver/) está el skill que automatiza todo esto: hace recon,
clasifica la categoría (Forensics, Reversing, Exploiting, Stego, Crypto, Web, OSINT, Misc),
aplica el playbook correcto, halla la flag y guarda `solucion.md`.

Para usarlo, cópialo donde tu IA lea skills (p. ej. `~/.claude/skills/ctf-solver` en Claude Code)
y pídele que resuelva el reto; o simplemente entra a la carpeta del reto y dile a OpenCode
*"resuelve este reto, saca la flag"*.
