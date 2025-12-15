# Clonar el repositorio del taller (Windows)

## ¿Qué significa "clonar" un repositorio?

Clonar es simplemente descargar una copia completa de un proyecto desde GitHub a tu computadora. Es como hacer "copy-paste" de una carpeta, pero con superpoderes:

- 📁 Copias todos los archivos del proyecto
- 📜 Incluye todo el historial de cambios
- 🔗 Mantiene la conexión con GitHub para futuras actualizaciones

---

## ¿Qué repositorio vamos a clonar?

Vamos a descargar el proyecto del taller:

| Información | Detalle |
|-------------|---------|
| **Nombre del repositorio** | taller-otree-pgg |
| **URL** | https://github.com/DonovanDiazcide/taller-otree-pgg |
| **Contenido** | Archivos y ejemplos para el taller de oTree |

---

## Paso 1: Decidir dónde guardar el proyecto

Antes de clonar, piensa en dónde quieres guardar el proyecto en tu computadora.

**Ubicación recomendada:** Carpeta Documentos

Ejemplo: `C:\Users\TuUsuario\Documents\taller-otree`

💡 **Consejo:** Evita ubicaciones con:
- Espacios en el nombre de la carpeta (mejor `mi-proyecto` que `mi proyecto`)
- Caracteres especiales como ñ, tildes, o símbolos
- Carpetas sincronizadas como OneDrive o Dropbox (pueden causar conflictos)

---

## Paso 2: Abrir Git Bash en la ubicación correcta

**Opción A: Abrir Git Bash directamente en la carpeta (RECOMENDADO)**

1. Abre el **Explorador de archivos**
2. Navega hasta tu carpeta **Documentos** (o donde quieras guardar el proyecto)
3. Haz **clic derecho** dentro de la carpeta
4. Selecciona **"Git Bash Here"** o **"Open Git Bash here"**
5. Se abrirá Git Bash directamente en esa ubicación

**Opción B: Navegar manualmente desde Git Bash**

1. Abre **Git Bash** (tecla Windows → escribe "Git Bash" → Enter)
2. Navega a tu carpeta de Documentos:

```bash
cd ~/Documents
```

3. Presiona Enter

---

## Paso 3: (Opcional) Crear una carpeta específica para el taller

Si quieres organizar mejor tus archivos, crea una carpeta específica:

```bash
mkdir taller-otree
cd taller-otree
```

**¿Qué hacen estos comandos?**
- `mkdir taller-otree` = **Make Directory** → Crea una carpeta llamada "taller-otree"
- `cd taller-otree` = **Change Directory** → Entra a esa carpeta

---

## Paso 4: Clonar el repositorio

Ahora sí, ¡vamos a clonar! Ejecuta este comando en **Git Bash**:

```bash
git clone git@github.com:DonovanDiazcide/taller-otree-pgg.git
```

**¿Qué significa este comando?**

| Parte del comando | Significado |
|-------------------|-------------|
| `git clone` | Instrucción para clonar un repositorio |
| `git@github.com:` | Conexión SSH a GitHub |
| `DonovanDiazcide/` | Usuario dueño del repositorio |
| `taller-otree-pgg.git` | Nombre del repositorio |

**¿Qué deberías ver?**

```
Cloning into 'taller-otree-pgg'...
remote: Enumerating objects: XX, done.
remote: Counting objects: 100% (XX/XX), done.
remote: Compressing objects: 100% (XX/XX), done.
remote: Total XX (delta X), reused XX (delta X), pack-reused X
Receiving objects: 100% (XX/XX), XX.XX KiB | XX.XX MiB/s, done.
Resolving deltas: 100% (X/X), done.
```

✅ **Si ves este mensaje** = ¡El repositorio se clonó exitosamente!

---

## Paso 5: Entrar a la carpeta del proyecto

Después de clonar, se habrá creado una carpeta nueva con el nombre del repositorio:

```bash
cd taller-otree-pgg
```

**Verificar que estás en la carpeta correcta:**

```bash
ls
```

Deberías ver los archivos del proyecto (como `settings.py`, carpetas con los experimentos, etc.)

---

## Paso 6: Abrir el proyecto en VS Code

Ahora que tienes el proyecto en tu computadora, ábrelo en VS Code:

```bash
code .
```

El punto (`.`) significa "la carpeta actual".

**¿Qué debería pasar?**

VS Code se abrirá mostrando todos los archivos del proyecto en el panel izquierdo.

**Alternativa manual:**
1. Abre VS Code
2. Haz clic en **File → Open Folder** (o **Archivo → Abrir Carpeta**)
3. Navega hasta la carpeta `taller-otree-pgg`
4. Haz clic en **Seleccionar carpeta**

---

## ✅ Verificación: Confirmar que todo está listo

### 1. Verificar que estás en la carpeta correcta

En Git Bash:

```bash
pwd
```

**¿Qué deberías ver?** Una ruta que termine en `taller-otree-pgg`, por ejemplo:
```
C:\Users\TuUsuario\Documents\taller-otree-pgg
```

### 2. Ver los archivos del proyecto

```bash
ls -la
```

Deberías ver archivos como `settings.py` y otras carpetas del proyecto.

---

## 🎉 ¡Felicidades!

Si llegaste hasta aquí, ya tienes:

✅ El repositorio del taller clonado en tu computadora  
✅ Acceso a todos los archivos del proyecto  
✅ El proyecto abierto en VS Code  
✅ Todo listo para ejecutar los experimentos de oTree

**Estás completamente listo para el taller.**

---

## 🔧 Solución de problemas

### Problema: "Permission denied (publickey)"

```
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.
```

**Causa:** Tu llave SSH no está configurada correctamente con GitHub.

**Solución:**
1. Verifica que configuraste SSH correctamente (sección anterior del tutorial)
2. Prueba tu conexión SSH:
   ```bash
   ssh -T git@github.com
   ```
3. Si no funciona, repite los pasos de configuración SSH

### Problema: "Repository not found"

```
ERROR: Repository not found.
fatal: Could not read from remote repository.
```

**Causa:** La URL del repositorio está mal escrita.

**Solución:**
Copia y pega exactamente este comando:
```bash
git clone git@github.com:DonovanDiazcide/taller-otree-pgg.git
```

### Problema: "destination path already exists"

```
fatal: destination path 'taller-otree-pgg' already exists and is not an empty directory.
```

**Causa:** Ya existe una carpeta con ese nombre en la ubicación actual.

**Solución:**

**Opción A:** Elimina la carpeta existente y clona de nuevo:
```bash
rm -rf taller-otree-pgg
git clone git@github.com:DonovanDiazcide/taller-otree-pgg.git
```

**Opción B:** Clona con un nombre diferente:
```bash
git clone git@github.com:DonovanDiazcide/taller-otree-pgg.git taller-otree-nuevo
```

### Alternativa: Clonar usando HTTPS (si SSH no funciona)

Si tienes problemas con SSH y necesitas clonar urgentemente, puedes usar HTTPS:

```bash
git clone https://github.com/DonovanDiazcide/taller-otree-pgg.git
```

⚠️ **Nota:** Con HTTPS te pedirá tu usuario y contraseña de GitHub cada vez que interactúes con el repositorio. Por eso recomendamos SSH para el uso regular.

---

## Resumen de comandos

Aquí tienes todos los comandos en orden:

```bash
# 1. Ir a tu carpeta de Documentos
cd ~/Documents

# 2. (Opcional) Crear carpeta para el taller
mkdir taller-otree
cd taller-otree

# 3. Clonar el repositorio
git clone git@github.com:DonovanDiazcide/taller-otree-pgg.git

# 4. Entrar a la carpeta del proyecto
cd taller-otree-pgg

# 5. Abrir en VS Code
code .
```
