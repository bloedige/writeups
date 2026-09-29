# IA para CTF — hallar la flag rápido

Trabajamos con **OpenCode**: la IA **ejecuta los comandos ella misma**, no te los dicta.
Objetivo único: **encontrar la flag.** Donde más sirve: **Forensics, Reversing, Exploiting, Stego.**
**TODO DENTRO DE KALILINUX (o SO de hacking de uso)**

---

## 0. Instalar OpenCode (desde cero, una sola vez)

Todo **por terminal** en tu **Kali**. Sigue los pasos en orden hasta el final.

_**Si algo no se entiende, revisa la [guía oficial de OpenCode en español](https://opencode.ai/docs/es).**_

1. **Abre una terminal** (icono de terminal en la barra de Kali, o `Ctrl + Alt + T`).

2. **Instala `curl`** (en Kali casi siempre viene, pero por si acaso):
   ```bash
   sudo apt update && sudo apt install -y curl
   ```
   Te pedirá tu contraseña de Kali: escríbela (no se ve mientras escribes, es normal) y pulsa **Enter**.

3. **Descarga e instala OpenCode:**
   ```bash
   curl -fsSL https://opencode.ai/v2/install | bash
   ```
   Espera a que termine. No cierres la terminal a mitad.

4. **Cierra la terminal y ábrela de nuevo** para que se cargue el comando `opencode`.

5. **Comprueba que se instaló:**
   ```bash
   opencode --version
   ```
   Si sale un número de versión, está bien. Si dice `command not found`, cierra y abre la terminal otra vez; si sigue igual, repite el paso 3.

6. **Consigue tu API key** (en el navegador):
   - Entra a **https://opencode.ai/auth**.
   - Inicia sesión.
   - Copia tu **API key** (una cadena larga de letras y números). Guárdala, no la compartas.

7. **Abre OpenCode** en la terminal:
   ```bash
   opencode
   ```

8. **Conecta la API key.** Dentro de OpenCode escribe y pulsa **Enter**:
   ```
   /connect
   ```
   - Con las flechas elige **opencode** → **Enter**.
   - Cuando pida la API key, pégala con **Ctrl + Shift + V** (en la terminal `Ctrl + V` no pega) → **Enter**.

9. **Elige el modelo:**
   ```
   /model
   ```
   Selecciona **Big Pickle** → **Enter**. Es el que viene por defecto y el que mejor nos ha funcionado para CTF.

10. **Prueba que funciona.** Escribe algo simple y pulsa **Enter**:
    ```
    hola, ¿funcionas?
    ```
    Si la IA responde, **OpenCode ya está listo**. Si da error de API key, repite el paso 8 y pega la key de nuevo.

11. Para salir de OpenCode:
    ```
    /exit
    ```
    (o `Ctrl + C`). La conexión queda guardada: **no hay que repetir esto nunca más.**

---

## 1. Preparar para resolver (cada reto)

1. **Crea una carpeta para el reto** y entra en ella:
   ```bash
   mkdir reto1
   cd reto1
   ```
   En esta carpeta **descarga todos los archivos del reto** (los adjuntos del enunciado: binarios, imágenes, pcap, zip, etc.). Solo los de este reto, nada más.
2. Ya dentro de la carpeta, arranca la IA:
   ```bash
   opencode
   ```
3. ⚠️ **SIEMPRE EN MODO `BUILD`, NUNCA EN `PLAN`.** Mira abajo en la pantalla de OpenCode qué modo está activo; si dice `Plan`, pulsa **Tab** hasta que diga `Build`. En `Plan` la IA solo propone y **no ejecuta comandos**, así que no saca la flag.
4. ⚠️ **SI PIDE PERMISO → DALE `ALWAYS ALLOW`.** Así ya no vuelve a preguntar y no se detiene a cada rato.
5. Cuando **pierda el contexto**, reinicia sesión limpia (o cierra y vuelve a abrir):
   ```
   /new
   ```
6. Si **se acaban los tokens**, cambia de modelo sin salir:
   ```
   /model
   ```
   Selecciona otro modelo → **Enter**.

---

## 2. Arrancar

Dale el reto y déjala trabajar:

```
Reto CTF, categoría <Forensics/Reversing/Exploiting/Stego>.
Enunciado: "<literal>"
Archivos: <los que se encuentren en la capeta si tiene> s<opcional>
Encuentra la flag. Ejecuta lo que necesites y ve directo al grano.
```

---

## 3. Si empieza a perderse (lo importante)

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

## 4. Mantenerla enfocada

- Un reto por sesión.
- Si se va por las ramas → recórtala: *"céntrate solo en esto"*.
- Si se atasca en 2 intentos → pídele **otro enfoque**, no que repita.

---

## 5. Cuando aparece la flag → GUARDAR (siempre al final)

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

## Extra: instala el skill `ctf-solver` (resuelve el reto de principio a fin)

En la carpeta [`ctf-solver/`](ctf-solver/) está el skill que automatiza todo esto: hace recon,
clasifica la categoría (Forensics, Reversing, Exploiting, Stego, Crypto, Web, OSINT, Misc),
aplica el playbook correcto, halla la flag y guarda `solucion.md`.

### Instalación (Kali + OpenCode, una sola vez)

Copia y pega en la terminal:

```bash
git clone https://github.com/bloedige/ctf-solver.git     # 1. descarga el skill en la carpeta actual
mkdir -p ~/.config/opencode/skills                       # 2. crea la carpeta donde OpenCode busca los skills
cp -r ctf-solver ~/.config/opencode/skills/              # 3. copia el skill a esa carpeta
ls ~/.config/opencode/skills/ctf-solver                  # 4. verifica: debe salir SKILL.md y references/
```

Después **cierra y vuelve a abrir OpenCode** para que cargue el skill.

### Usarlo

1. Entra a la carpeta del reto y arranca OpenCode:
   ```bash
   cd reto1
   opencode
   ```
2. Pídele el reto:
   ```
   Usa el skill ctf-solver. Resuelve este reto y saca la flag.
   Enunciado: "<literal>"
   ```
3. Al terminar deja `solucion.md` en la carpeta del reto.

### Actualizar

Cuando haya cambios en el skill, repite los pasos 1, 3 y 4. Antes de hacerlo, borra la copia vieja con `rm -rf ctf-solver ~/.config/opencode/skills/ctf-solver`.

