# Guía de instalación y verificación de herramientas

Este documento te guiará paso a paso para instalar las herramientas que necesitas para el taller. No te preocupes si nunca has usado estas herramientas antes: explicaremos todo de forma clara y sencilla.

## ¿Qué vamos a instalar y para qué sirve cada cosa?

Antes de empezar, aquí está un resumen de lo que instalarás:

| Herramienta | ¿Para qué sirve? 
|-------------|------------------|
| **Git** | Guardar diferentes versiones de tu código y colaborar con otros | Como "ctrl+Z infinito" + Google Docs para código |
| **Python** | El lenguaje de programación que usaremos, el medio a través del cual en que le hablas a la computadora y le pides que ejecute instrucciones|
| **VS Code** | Espacio donde escribirás código | Como Microsoft Word, pero para programadores |
| **oTree** | Herramienta para crear experimentos económicos | Una "plantilla lista para usar" que facilita tu trabajo |
| **SSH con GitHub** | Conectar tu computadora con GitHub sin contraseñas | Como guardar tu contraseña de Netflix para no escribirla cada vez |

## Acerca de las "terminales" o "líneas de comandos"

**¿Qué es una terminal?** Es una ventana donde escribes instrucciones de texto para tu computadora.

Existen diferentes tipos:

### 💻 En Windows:
- **Command Prompt** ("Símbolo del sistema"): La terminal básica de Windows
  - *Cómo abrirla*: Presiona la tecla Windows + escribe "cmd" + Enter
- **Git Bash**: Terminal que se instala con Git (similar a Mac/Linux)
  - *Cómo abrirla*: Presiona la tecla Windows + escribe "Git Bash" + Enter

### 🍎 En macOS/Linux:
- **Terminal**: La aplicación nativa de tu sistema
  - *Cómo abrirla en Mac*: Aplicaciones → Utilidades → Terminal

**📌 Regla de oro**: Cuando el tutorial diga "abre [tipo de terminal]", abre exactamente ese tipo. Algunos comandos solo funcionan en terminales específicas.

---

# 1. Instalar Git

## ¿Qué es Git?

Git es un **sistema de control de versiones**. Piensa en esto como un "historial de cambios" para tu código que te permite:
- 📸 Guardar "fotografías" de tu código en diferentes momentos
- ⏮️ Regresar a versiones anteriores si algo sale mal
- 👥 Trabajar en equipo sin sobrescribir el trabajo de otros

---

## Instalación en Windows

**Paso 1: Descargar**
1. Abre tu navegador
2. Ve a: **https://git-scm.com/download/win**
3. Selecciona "Git for Windows/x64 Setup"

**Paso 2: Instalar**
1. Haz doble clic en el archivo descargado
2. Verás varias pantallas con opciones

**Paso 3: Configurar (opciones importantes)**

Durante la instalación verás estas pantallas. Estas son las opciones recomendadas:

| Pantalla | ¿Qué elegir? | ¿Por qué? |
|----------|--------------|-----------|
| **PATH environment** | "Git from the command line and also from 3rd-party software" | Permite usar Git desde cualquier terminal |
| **Editor por defecto** | Visual Studio Code (o el que prefieras) | Para editar mensajes de Git |
| **Cliente SSH** | "Use bundled OpenSSH" | Es la opción más compatible |

Para las demás opciones, simplemente acepta los valores recomendados (haz clic en "Next").

**Paso 4: Finalizar**
- Haz clic en "Install"
- Espera a que termine
- Haz clic en "Finish"

---

## Instalación en macOS

**Opción más simple (recomendada)**:

1. Abre **Terminal**
   - Ve a Aplicaciones → Utilidades → Terminal
   
2. Escribe esto y presiona Enter:
   ```bash
   git --version
   ```

3. Si Git no está instalado, macOS te preguntará: *"¿Deseas instalar las herramientas de línea de comandos?"*
   - Haz clic en **"Instalar"**
   - Espera a que termine

**Opción alternativa (si usas Homebrew)**:

Si ya tienes Homebrew instalado, escribe en Terminal:
```bash
brew install git
```

---

## Instalación en Linux (Ubuntu/Debian)

Abre **Terminal** y copia estos comandos uno por uno:

```bash
sudo apt update
sudo apt install git
```

Te pedirá tu contraseña (la misma que usas para iniciar sesión en tu computadora).

---

## ✅ Verificar que Git se instaló correctamente

**Dónde hacer esto**:
- Windows: Abre **Command Prompt** o **Git Bash**
- Mac/Linux: Abre **Terminal**

**Comando**:
```bash
git --version
```

**¿Qué deberías ver?**  
Algo como: `git version 2.45.1`

✅ Si ves un número de versión = ¡éxito!  
❌ Si dice "command not found" = ve a "Solución de problemas" al final

---

# 2. Instalar Python

## ¿Qué es Python?

Python es un **lenguaje de programación** (como inglés o español, pero para computadoras). Lo usaremos para:
- Escribir las instrucciones de nuestro experimento
- Hacer que oTree funcione

---

## Instalación en Windows

**Paso 1: Descargar**
1. Ve a: **https://www.python.org/downloads/**
2. Haz clic en el botón amarillo grande que dice "Download Python 3.X.X"

**Paso 2: Instalar (MUY IMPORTANTE)**

1. Haz doble clic en el archivo descargado
2. **⚠️ ANTES de hacer clic en "Install Now"**:
   - Marca la casilla ✅ **"Add python.exe to PATH"**
   - Esta es LA opción más importante (sin ella, Python no funcionará desde la terminal)

3. Haz clic en **"Install Now"**
4. Espera a que termine

**¿Por qué es importante "Add to PATH"?**  
Sin esta opción, tu computadora no sabrá dónde está Python cuando escribas comandos. Es como si tuvieras un teléfono pero no estuviera en tu lista de contactos.

---

## Instalación en macOS

**Opción 1: Desde el sitio oficial**
1. Ve a: **https://www.python.org/downloads/**
2. Descarga el instalador para Mac
3. Haz doble clic e instala normalmente

**Opción 2: Con Homebrew** (si ya lo tienes):
```bash
brew install python
```

---

## Instalación en Linux

Muchas distribuciones ya traen Python instalado. Para asegurarte de tener todo, abre **Terminal** y ejecuta:

```bash
sudo apt update
sudo apt install python3 python3-venv python3-pip
```

---

## ✅ Verificar que Python se instaló correctamente

**Dónde hacer esto**:
- Windows: **Command Prompt** (cierra y abre uno nuevo)
- Mac/Linux: **Terminal**

**Comando**:
```bash
python --version
```

O si el anterior no funciona, intenta:
```bash
python3 --version
```

**¿Qué deberías ver?**  
`Python 3.10.5` (o cualquier versión 3.8 o superior)

✅ Si ves la versión = ¡éxito!  
❌ Si dice "command not found" = cierra y abre una nueva terminal. Si persiste, ve a "Solución de problemas"

---

# 3. Instalar Visual Studio Code (VS Code)

## ¿Qué es VS Code?

VS Code es un **editor de código** (como Microsoft Word, pero diseñado para programadores). Te ayuda a:
- Escribir código con colores que facilitan la lectura
- Detectar errores antes de ejecutar tu código
- Organizar tus archivos de proyecto

---

## Instalación (Windows, Mac, Linux)

**Paso 1: Descargar**
1. Ve a: **https://code.visualstudio.com/**
2. Haz clic en el botón grande de descarga
   - El sitio detectará automáticamente tu sistema operativo

**Paso 2: Instalar en Windows**
1. Ejecuta el instalador descargado
2. **Importante**: Durante la instalación, marca estas opciones:
   - ✅ "Add to PATH"
   - ✅ "Create a desktop icon" (opcional, pero útil)

**Paso 2: Instalar en Mac**
1. Abre el archivo descargado
2. Arrastra VS Code a tu carpeta de Aplicaciones

**Paso 2: Instalar en Linux**
1. Para Ubuntu/Debian, descarga el archivo `.deb`
2. Haz doble clic para instalar con el centro de software

---

## Configurar el comando `code` (para abrir VS Code desde la terminal)

### En Mac/Linux:

1. Abre VS Code
2. Presiona `Cmd+Shift+P` (Mac) o `Ctrl+Shift+P` (Linux)
   - Esto abre el "Command Palette" (una barra de búsqueda interna)
3. Escribe: `shell command`
4. Selecciona: **"Shell Command: Install 'code' command in PATH"**
5. Presiona Enter
6. Te pedirá tu contraseña de administrador
7. Cierra y vuelve a abrir la Terminal

### En Windows:

Si marcaste "Add to PATH" durante la instalación, el comando ya debería funcionar.

Si no funciona:
1. Busca "environment variables" en el menú de inicio
2. Edita la variable "Path" del usuario
3. Agrega esta ruta (ajusta según tu instalación):
   ```
   C:\Users\TuUsuario\AppData\Local\Programs\Microsoft VS Code\bin
   ```

---

## ✅ Verificar la instalación

**Dónde**:
- Windows: Command Prompt
- Mac/Linux: Terminal

**Comando**:
```bash
code --version
```

**¿Qué deberías ver?**  
```
1.85.0
5437499feb04f7a586f677b155b039bc2b3669eb
x64
```

✅ Si ves números de versión = ¡éxito!

**Probar abriendo una carpeta**:
```bash
code .
```
(El punto significa "carpeta actual")

Esto debería abrir VS Code en la carpeta donde estás.

---

# 4. Instalar oTree

## ¿Qué es oTree?

oTree es un **framework** (conjunto de herramientas) para crear experimentos económicos y de ciencias sociales. Es como una "plantilla" que ya tiene muchas cosas listas:
- Interfaz web donde los participantes responden
- Sistema de roles y rondas
- Base de datos para guardar respuestas
- Herramientas para exportar datos

---

## Instalación

oTree se instala usando `pip`, que es el instalador de paquetes de Python.

**Dónde ejecutar**:
- Windows: **Command Prompt**
- Mac/Linux: **Terminal**

**Comando**:
```bash
pip install otree
```

O si tu sistema usa `pip3`:
```bash
pip3 install otree
```

**¿Qué pasará?**  
Verás texto desfilando mientras se descargan e instalan varios componentes. Esto puede tomar 1-2 minutos.

**En Linux, si ves un error sobre "externally-managed-environment"**:
```bash
pip install otree --break-system-packages
```

---

## ✅ Verificar que oTree se instaló correctamente

**Comando**:
```bash
otree --version
```

**¿Qué deberías ver?**  
`5.10.4` (o cualquier versión 5.X.X)

✅ Si ves la versión = ¡éxito!

---

# 4.5. Crear tu cuenta de GitHub

## ¿Qué es GitHub?

GitHub es una **plataforma en línea** donde puedes:
- 📦 Guardar tus proyectos de código (repositorios)
- 👥 Colaborar con otras personas
- 📜 Ver el historial completo de cambios
- 🔄 Sincronizar tu trabajo entre diferentes computadoras

Piensa en GitHub como "Google Drive para código" o "Dropbox con superpoderes para programadores".

---

## Paso 1: Crear tu cuenta de GitHub

**Es completamente gratis** (existe una versión de pago con funciones extra, pero no la necesitaremos).

### Instrucciones:

1. **Abre tu navegador** y ve a: **https://github.com**

2. **Haz clic en "Sign up"** (Registrarse) en la esquina superior derecha

3. **Completa el formulario** con la siguiente información:
   - **Email**: Usa un correo que revises frecuentemente
   - **Password**: Crea una contraseña segura (mínimo 15 caracteres o 8 con un número y una letra minúscula)
   - **Username**: Elige un nombre de usuario
     - 💡 **Consejo**: Usa algo profesional (evita nombres muy casuales)
     - 💡 Este nombre aparecerá en tus repositorios públicos
     - Ejemplos buenos: `maria-rodriguez`, `carlos-datos`, `ana-economista`
     - Ejemplos a evitar: `chicagamer123`, `elpepe2024`

4. **Verificación humana**:
   - GitHub te pedirá resolver un pequeño acertijo para verificar que no eres un robot

5. **Verifica tu correo**:
   - GitHub enviará un código de verificación a tu correo
   - Revisa tu bandeja de entrada y copia el código
   - Pégalo en GitHub

6. **Personalización (opcional)**:
   - GitHub te hará algunas preguntas sobre cómo planeas usar la plataforma
   - Puedes responderlas o hacer clic en "Skip personalization" (Omitir personalización)

¡Listo! Ya tienes tu cuenta de GitHub. 🎉

---

## Paso 2: Configurar tu nombre y correo en Git (local)

Antes de continuar con SSH, necesitas decirle a Git quién eres (esto aparecerá en tus commits).

**Dónde ejecutar**:
- Windows: **Command Prompt** o **Git Bash**
- Mac/Linux: **Terminal**

**Comandos** (reemplaza con TU información):

```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu_correo@ejemplo.com"
```

**Ejemplo real**:
```bash
git config --global user.name "María Rodríguez"
git config --global user.email "maria.rodriguez@universidad.edu"
```

**⚠️ Importante**: 
- Usa el **mismo correo** que usaste para crear tu cuenta de GitHub
- El nombre puede ser tu nombre real o tu username de GitHub

**Verificar que se guardó**:
```bash
git config --global user.name
git config --global user.email
```

Deberías ver tu nombre y correo.

---

## ¿Por qué necesitamos una cuenta de GitHub?

Durante el taller:
- 📥 Descargaremos (clonaremos) proyectos de ejemplo desde GitHub
- 📤 Subiremos nuestro código a GitHub para compartirlo
- 🔑 Usaremos SSH para conectarnos sin escribir contraseña cada vez

**Siguiente paso**: Ahora que ya tienes tu cuenta, configuraremos SSH para que tu computadora se pueda conectar a GitHub de forma segura y automática.

---

# 5. Configurar SSH con GitHub

## ¿Qué es SSH y por qué lo necesito?

**SSH** (Secure Shell) es un protocolo que te permite conectarte de forma segura a servidores remotos.

**¿Por qué usar SSH con GitHub?**

Cuando trabajas con GitHub, hay dos formas de conectarte:
1. **HTTPS**: Requiere escribir tu usuario y contraseña cada vez que subes o descargas código
2. **SSH**: Una vez configurado, te conectas automáticamente (sin contraseñas)

Usaremos SSH porque es:
- ✅ Más rápido (no escribes contraseña cada vez)
- ✅ Más seguro (usa encriptación de clave pública/privada)
- ✅ La opción recomendada por GitHub

---

## Paso 1: Verificar si ya tienes llaves SSH

Antes de crear llaves nuevas, verifica si ya tienes algunas.

**Dónde ejecutar**:
- Windows: **Git Bash**
- Mac/Linux: Terminal

**Comando**:
```bash
ls -al ~/.ssh
```

**¿Qué deberías ver?**

**Si NO tienes llaves** (es lo más común si nunca has usado SSH):
```
ls: cannot access '/Users/tu_usuario/.ssh': No such file or directory
```
→ Continúa con el Paso 2

**Si YA tienes llaves**:
Verás archivos como:
```
id_rsa
id_rsa.pub
id_ed25519
id_ed25519.pub
```
→ Puedes usar las existentes (salta al Paso 4) o crear nuevas

---

## Paso 2: Crear llaves SSH nuevas

**Dónde ejecutar**:
- Windows: **Git Bash**
- Mac/Linux: Terminal

**Comando** (reemplaza con TU correo de GitHub):
```bash
ssh-keygen -t ed25519 -C "tu_correo@ejemplo.com"
```

**¿Qué significa este comando?**
- `ssh-keygen` = programa para crear llaves
- `-t ed25519` = tipo de encriptación (la más moderna y segura)
- `-C "tu_correo@ejemplo.com"` = una etiqueta para identificar esta llave

**Qué pasará**:

1. Te preguntará: `Enter a file in which to save the key`
   - Simplemente presiona **Enter** (acepta la ubicación por defecto)

2. Te preguntará: `Enter passphrase`
   - Puedes:
     - **Dejar vacío** (presiona Enter dos veces) = más cómodo, menos seguro
     - **Escribir una contraseña** = más seguro, pero tendrás que escribirla al usar la llave

3. Verás un "randomart" (dibujo de caracteres) = ¡Se creó exitosamente!

**Resultado**: Ahora tienes DOS archivos en `~/.ssh/`:
- `id_ed25519` = llave privada (NUNCA compartas este archivo)
- `id_ed25519.pub` = llave pública (este SÍ lo compartirás con GitHub)

---

## Paso 3: Agregar tu llave al "ssh-agent"

**¿Qué es el ssh-agent?**  
Es un programa que "recuerda" tus llaves SSH para que no tengas que escribir la passphrase cada vez.

**Dónde ejecutar**:
- Windows: Git Bash
- Mac/Linux: Terminal

**Paso 3a: Iniciar el ssh-agent**

```bash
eval "$(ssh-agent -s)"
```

**¿Qué deberías ver?**  
`Agent pid 12345` (algún número)

**Paso 3b: Agregar tu llave**

```bash
ssh-add ~/.ssh/id_ed25519
```

(Si usaste RSA en lugar de ed25519, reemplaza `id_ed25519` con `id_rsa`)

**¿Qué deberías ver?**  
`Identity added: /Users/tu_usuario/.ssh/id_ed25519`

---

## Paso 4: Copiar tu llave pública

Necesitamos copiar el contenido de tu llave PÚBLICA (el archivo `.pub`) para pegarlo en GitHub.

**Dónde ejecutar**:
- Windows: Git Bash
- Mac/Linux: Terminal

### Opción A: Copiar automáticamente (Mac)
```bash
pbcopy < ~/.ssh/id_ed25519.pub
```
¡Listo! El contenido ya está copiado.

### Opción B: Ver y copiar manualmente (Windows/Linux)
```bash
cat ~/.ssh/id_ed25519.pub
```

Verás algo como:
```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIMx... tu_correo@ejemplo.com
```

**Selecciona TODO el texto** (desde `ssh-ed25519` hasta tu correo) y cópialo.

---

## Paso 5: Agregar la llave a tu cuenta de GitHub

**En tu navegador**:

1. Ve a **github.com** e inicia sesión

2. Haz clic en tu **foto de perfil** (esquina superior derecha)

3. Selecciona **"Settings"** (Configuración)

4. En el menú lateral izquierdo, haz clic en **"SSH and GPG keys"**

5. Haz clic en el botón verde **"New SSH key"**

6. Llena el formulario:
   - **Title**: Ponle un nombre descriptivo, por ejemplo:
     - "Laptop Personal"
     - "Computadora del Trabajo"
     - "MacBook CIDE"
   - **Key**: Pega aquí el contenido que copiaste (tu llave pública)

7. Haz clic en **"Add SSH key"**

8. GitHub te pedirá tu contraseña para confirmar

¡Listo! Tu llave SSH está ahora registrada en GitHub.

---

## Paso 6: Probar la conexión

Vamos a verificar que todo funciona correctamente.

**Dónde ejecutar**:
- Windows: Git Bash
- Mac/Linux: Terminal

**Comando**:
```bash
ssh -T git@github.com
```

**La primera vez**, verás este mensaje:
```
The authenticity of host 'github.com' can't be established.
Are you sure you want to continue connecting (yes/no)?
```

Escribe **`yes`** y presiona Enter.

**Si todo está bien, verás**:
```
Hi tu_usuario_github! You've successfully authenticated, but GitHub does not provide shell access.
```

✅ Esto significa: ¡Tu configuración SSH funciona perfectamente!

❌ Si ves un error, revisa:
- ¿Copiaste toda la llave pública completa?
- ¿El ssh-agent está ejecutándose?
- ¿Agregaste la llave al ssh-agent?

---

# Verificación final completa

¡Ya casi terminas! Ahora vamos a verificar que todo funcione.

**Abre una nueva terminal** (Command Prompt en Windows o Terminal en Mac/Linux) y ejecuta estos comandos uno por uno:

## 1. Git
```bash
git --version
```
✅ Deberías ver: `git version 2.45.1` (o similar)

## 2. Python
```bash
python --version
```
✅ Deberías ver: `Python 3.10.5` (o superior a 3.8)

## 3. VS Code
```bash
code --version
```
✅ Deberías ver: `1.85.0` (o similar)

## 4. oTree
```bash
otree --version
```
✅ Deberías ver: `5.10.4` (o cualquier 5.X.X)

## 5. GitHub SSH
```bash
ssh -T git@github.com
```
✅ Deberías ver: `Hi tu_usuario! You've successfully authenticated...`

---

# 🎉 Si todos los comandos anteriores funcionan:

**¡Felicidades! Ya tienes todo instalado y configurado.**

Ahora puedes:
- ✅ Clonar repositorios de GitHub
- ✅ Escribir código en VS Code
- ✅ Ejecutar oTree
- ✅ Colaborar usando Git sin contraseñas
---

# 6. Clonar el repositorio del taller

## ¿Qué significa "clonar" un repositorio?

**Clonar** es simplemente descargar una copia completa de un proyecto desde GitHub a tu computadora. Es como hacer "copy-paste" de una carpeta, pero con superpoderes:

- 📁 Copias todos los archivos del proyecto
- 📜 Incluye todo el historial de cambios
- 🔗 Mantiene la conexión con GitHub para futuras actualizaciones

## ¿Qué repositorio vamos a clonar?

Vamos a descargar el proyecto del taller:

| Información | Detalle |
|-------------|---------|
| **Nombre del repositorio** | taller-otree-pgg |
| **URL** | https://github.com/DonovanDiazcide/taller-otree-pgg |
| **Contenido** | Archivos y ejemplos para el taller de oTree |

---

## Paso 1: Decidir dónde guardar el proyecto

Antes de clonar, piensa en **dónde quieres guardar** el proyecto en tu computadora.

### Recomendaciones:

| Sistema | Ubicación sugerida | Ejemplo completo |
|---------|-------------------|------------------|
| Windows | Carpeta Documentos | `C:\Users\TuUsuario\Documents\taller-otree` |
| macOS | Carpeta Documentos | `/Users/TuUsuario/Documents/taller-otree` |
| Linux | Carpeta home | `/home/TuUsuario/taller-otree` |

💡 **Consejo**: Evita ubicaciones con:
- Espacios en el nombre de la carpeta (mejor `mi-proyecto` que `mi proyecto`)
- Caracteres especiales como ñ, tildes, o símbolos
- Carpetas sincronizadas como OneDrive o Dropbox (pueden causar conflictos)

---

## Paso 2: Abrir la terminal y navegar a la carpeta

### 💻 En Windows

**Opción A: Usar Git Bash (recomendado)**

1. Abre el **Explorador de archivos**
2. Navega hasta la carpeta donde quieres guardar el proyecto (por ejemplo, `Documentos`)
3. Haz **clic derecho** dentro de la carpeta
4. Selecciona **"Open Git Bash here"** o **"Git Bash Here"**
5. Se abrirá Git Bash directamente en esa ubicación

**Opción B: Navegar desde Git Bash manualmente**

1. Abre **Git Bash** (tecla Windows → escribe "Git Bash" → Enter)
2. Escribe este comando para ir a Documentos:

**Dónde ejecutar:** Git Bash
```bash
cd ~/Documents
```

3. Presiona **Enter**

**Opción C: Usar Command Prompt**

1. Abre **Command Prompt** (tecla Windows → escribe "cmd" → Enter)
2. Navega a tu carpeta de Documentos:

**Dónde ejecutar:** Command Prompt
```cmd
cd C:\Users\TuUsuario\Documents
```
⚠️ Reemplaza `TuUsuario` con tu nombre de usuario real de Windows

3. Presiona **Enter**

### 🍎 En macOS

1. Abre **Terminal** (Aplicaciones → Utilidades → Terminal)
2. Escribe este comando para ir a Documentos:

**Dónde ejecutar:** Terminal
```bash
cd ~/Documents
```

3. Presiona **Enter**

### 🐧 En Linux

1. Abre **Terminal**
2. Escribe este comando para ir a tu carpeta home:

**Dónde ejecutar:** Terminal
```bash
cd ~
```

O si prefieres una carpeta específica:

**Dónde ejecutar:** Terminal
```bash
cd ~/Documents
```

3. Presiona **Enter**

---

## Paso 3: (Opcional) Crear una carpeta específica para el taller

Si quieres organizar mejor tus archivos, puedes crear una carpeta específica.

### Comando para crear una carpeta:

**Dónde ejecutar en Windows:** Git Bash
```bash
mkdir taller-otree
cd taller-otree
```

**Dónde ejecutar en Windows (alternativa):** Command Prompt
```cmd
mkdir taller-otree
cd taller-otree
```

**Dónde ejecutar en macOS:** Terminal
```bash
mkdir taller-otree
cd taller-otree
```

**Dónde ejecutar en Linux:** Terminal
```bash
mkdir taller-otree
cd taller-otree
```

¿Qué hacen estos comandos?
- `mkdir taller-otree` = **M**a**k**e **Dir**ectory → Crea una carpeta llamada "taller-otree"
- `cd taller-otree` = **C**hange **D**irectory → Entra a esa carpeta

---

## Paso 4: Clonar el repositorio

Ahora sí, ¡vamos a clonar!

**Dónde ejecutar en Windows:** Git Bash (recomendado) o Command Prompt
```bash
git clone git@github.com:DonovanDiazcide/taller-otree-pgg.git
```

**Dónde ejecutar en macOS:** Terminal
```bash
git clone git@github.com:DonovanDiazcide/taller-otree-pgg.git
```

**Dónde ejecutar en Linux:** Terminal
```bash
git clone git@github.com:DonovanDiazcide/taller-otree-pgg.git
```

### ¿Qué significa este comando?

| Parte del comando | Significado |
|-------------------|-------------|
| `git clone` | Instrucción para clonar un repositorio |
| `git@github.com:` | Conexión SSH a GitHub |
| `DonovanDiazcide/` | Usuario dueño del repositorio |
| `taller-otree-pgg.git` | Nombre del repositorio |

### ¿Qué deberías ver?

```
Cloning into 'taller-otree-pgg'...
remote: Enumerating objects: XX, done.
remote: Counting objects: 100% (XX/XX), done.
remote: Compressing objects: 100% (XX/XX), done.
remote: Total XX (delta X), reused XX (delta X), pack-reused X
Receiving objects: 100% (XX/XX), XX.XX KiB | XX.XX MiB/s, done.
Resolving deltas: 100% (X/X), done.
```

✅ Si ves este mensaje = ¡El repositorio se clonó exitosamente!

---

## Paso 5: Entrar a la carpeta del proyecto

Después de clonar, se habrá creado una carpeta nueva con el nombre del repositorio.

**Dónde ejecutar en Windows:** Git Bash o Command Prompt
```bash
cd taller-otree-pgg
```

**Dónde ejecutar en macOS:** Terminal
```bash
cd taller-otree-pgg
```

**Dónde ejecutar en Linux:** Terminal
```bash
cd taller-otree-pgg
```

### Verificar que estás en la carpeta correcta:

**Dónde ejecutar en Windows:** Git Bash
```bash
ls
```

**Dónde ejecutar en Windows (alternativa):** Command Prompt
```cmd
dir
```

**Dónde ejecutar en macOS:** Terminal
```bash
ls
```

**Dónde ejecutar en Linux:** Terminal
```bash
ls
```

Deberías ver los archivos del proyecto (como `settings.py`, carpetas con los experimentos, etc.)

---

## Paso 6: Abrir el proyecto en VS Code

Ahora que tienes el proyecto en tu computadora, ábrelo en VS Code para explorarlo.

**Dónde ejecutar en Windows:** Git Bash o Command Prompt (estando dentro de la carpeta del proyecto)
```bash
code .
```

**Dónde ejecutar en macOS:** Terminal (estando dentro de la carpeta del proyecto)
```bash
code .
```

**Dónde ejecutar en Linux:** Terminal (estando dentro de la carpeta del proyecto)
```bash
code .
```

El punto (`.`) significa "la carpeta actual".

### ¿Qué debería pasar?
VS Code se abrirá mostrando todos los archivos del proyecto en el panel izquierdo.

### Alternativa manual:
1. Abre VS Code
2. Haz clic en **File** → **Open Folder** (o **Archivo** → **Abrir Carpeta**)
3. Navega hasta la carpeta `taller-otree-pgg`
4. Haz clic en **Seleccionar carpeta**

---

## ✅ Verificación: Confirmar que todo está listo

Vamos a verificar que el repositorio se clonó correctamente y que oTree funciona.

### 1. Verificar que estás en la carpeta correcta

**Dónde ejecutar en Windows:** Git Bash
```bash
pwd
```

**Dónde ejecutar en Windows (alternativa):** Command Prompt
```cmd
cd
```

**Dónde ejecutar en macOS:** Terminal
```bash
pwd
```

**Dónde ejecutar en Linux:** Terminal
```bash
pwd
```

**¿Qué deberías ver?**
Una ruta que termine en `taller-otree-pgg`, por ejemplo:
- Windows: `C:\Users\TuUsuario\Documents\taller-otree-pgg`
- Mac: `/Users/TuUsuario/Documents/taller-otree-pgg`
- Linux: `/home/TuUsuario/taller-otree-pgg`

### 2. Ver los archivos del proyecto

**Dónde ejecutar en Windows:** Git Bash
```bash
ls -la
```

**Dónde ejecutar en Windows (alternativa):** Command Prompt
```cmd
dir
```

**Dónde ejecutar en macOS:** Terminal
```bash
ls -la
```

**Dónde ejecutar en Linux:** Terminal
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
✅ oTree listo para ejecutar los experimentos  

**Estás completamente listo para el taller.**

---

## 🔧 Solución de problemas al clonar

### Problema: "Permission denied (publickey)"

```
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.
```

**Causa:** Tu llave SSH no está configurada correctamente.

**Solución:**
1. Verifica que configuraste SSH (Sección 5 de este documento)
2. Prueba tu conexión SSH:

**Dónde ejecutar en Windows:** Git Bash
```bash
ssh -T git@github.com
```

**Dónde ejecutar en macOS:** Terminal
```bash
ssh -T git@github.com
```

**Dónde ejecutar en Linux:** Terminal
```bash
ssh -T git@github.com
```

3. Si no funciona, repite los pasos de la Sección 5

### Problema: "Repository not found"

```
ERROR: Repository not found.
fatal: Could not read from remote repository.
```

**Causa:** La URL del repositorio está mal escrita o el repositorio no existe.

**Solución:**
1. Verifica que escribiste el comando correctamente
2. Copia y pega este comando exacto:

**Dónde ejecutar en Windows:** Git Bash o Command Prompt
```bash
git clone git@github.com:DonovanDiazcide/taller-otree-pgg.git
```

**Dónde ejecutar en macOS:** Terminal
```bash
git clone git@github.com:DonovanDiazcide/taller-otree-pgg.git
```

**Dónde ejecutar en Linux:** Terminal
```bash
git clone git@github.com:DonovanDiazcide/taller-otree-pgg.git
```

### Problema: "fatal: destination path 'taller-otree-pgg' already exists"

```
fatal: destination path 'taller-otree-pgg' already exists and is not an empty directory.
```

**Causa:** Ya existe una carpeta con ese nombre en la ubicación actual.

**Solución:**

**Opción A:** Elimina la carpeta existente y clona de nuevo:

**Dónde ejecutar en Windows:** Git Bash
```bash
rm -rf taller-otree-pgg
git clone git@github.com:DonovanDiazcide/taller-otree-pgg.git
```

**Dónde ejecutar en Windows (alternativa):** Command Prompt
```cmd
rmdir /s /q taller-otree-pgg
git clone git@github.com:DonovanDiazcide/taller-otree-pgg.git
```

**Dónde ejecutar en macOS:** Terminal
```bash
rm -rf taller-otree-pgg
git clone git@github.com:DonovanDiazcide/taller-otree-pgg.git
```

**Dónde ejecutar en Linux:** Terminal
```bash
rm -rf taller-otree-pgg
git clone git@github.com:DonovanDiazcide/taller-otree-pgg.git
```

**Opción B:** Clona con un nombre diferente:

**Dónde ejecutar en Windows:** Git Bash o Command Prompt
```bash
git clone git@github.com:DonovanDiazcide/taller-otree-pgg.git taller-otree-nuevo
```

**Dónde ejecutar en macOS:** Terminal
```bash
git clone git@github.com:DonovanDiazcide/taller-otree-pgg.git taller-otree-nuevo
```

**Dónde ejecutar en Linux:** Terminal
```bash
git clone git@github.com:DonovanDiazcide/taller-otree-pgg.git taller-otree-nuevo
```

### Problema: Clonar funciona pero oTree da error

**Causa:** Puede que falten dependencias del proyecto.

**Solución:**
1. Asegúrate de estar dentro de la carpeta del proyecto:

**Dónde ejecutar en Windows:** Git Bash o Command Prompt
```bash
cd taller-otree-pgg
```

**Dónde ejecutar en macOS:** Terminal
```bash
cd taller-otree-pgg
```

**Dónde ejecutar en Linux:** Terminal
```bash
cd taller-otree-pgg
```

2. Instala las dependencias si hay un archivo `requirements.txt`:

**Dónde ejecutar en Windows:** Git Bash o Command Prompt
```bash
pip install -r requirements.txt
```

**Dónde ejecutar en macOS:** Terminal
```bash
pip install -r requirements.txt
```
O si el anterior no funciona:
```bash
pip3 install -r requirements.txt
```

**Dónde ejecutar en Linux:** Terminal
```bash
pip install -r requirements.txt
```
O si el anterior no funciona:
```bash
pip3 install -r requirements.txt
```

### Alternativa: Clonar usando HTTPS (si SSH no funciona)

Si tienes problemas con SSH y necesitas clonar urgentemente, puedes usar HTTPS como alternativa:

**Dónde ejecutar en Windows:** Git Bash o Command Prompt
```bash
git clone https://github.com/DonovanDiazcide/taller-otree-pgg.git
```

**Dónde ejecutar en macOS:** Terminal
```bash
git clone https://github.com/DonovanDiazcide/taller-otree-pgg.git
```

**Dónde ejecutar en Linux:** Terminal
```bash
git clone https://github.com/DonovanDiazcide/taller-otree-pgg.git
```

⚠️ **Nota:** Con HTTPS te pedirá tu usuario y contraseña de GitHub cada vez que interactúes con el repositorio. Por eso recomendamos SSH para el uso regular.

---

## Resumen de comandos

Aquí tienes todos los comandos de esta sección en orden:

### Para Windows (usando Git Bash):

**Dónde ejecutar:** Git Bash
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

# 6. (Opcional) Probar oTree
otree devserver
```

### Para macOS:

**Dónde ejecutar:** Terminal
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

# 6. (Opcional) Probar oTree
otree devserver
```

### Para Linux:

**Dónde ejecutar:** Terminal
```bash
# 1. Ir a tu carpeta home o Documentos
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

# 6. (Opcional) Probar oTree
otree devserver
```
---

## 🎉 ¡Felicidades!

Si llegaste hasta aquí, ya tienes:

✅ El repositorio del taller clonado en tu computadora  
✅ Acceso a todos los archivos del proyecto  
✅ El proyecto abierto en VS Code  
✅ oTree listo para ejecutar los experimentos  

**Estás completamente listo para el taller.**

---

## 🔧 Solución de problemas al clonar

### Problema: "Permission denied (publickey)"

```
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.
```

**Causa:** Tu llave SSH no está configurada correctamente.

**Solución:**
1. Verifica que configuraste SSH (Sección 5 de este documento)
2. Prueba tu conexión SSH:
   ```bash
   ssh -T git@github.com
   ```
3. Si no funciona, repite los pasos de la Sección 5

### Problema: "Repository not found"

```
ERROR: Repository not found.
fatal: Could not read from remote repository.
```

**Causa:** La URL del repositorio está mal escrita o el repositorio no existe.

**Solución:**
1. Verifica que escribiste el comando correctamente
2. Copia y pega este comando exacto:
   ```bash
   git clone git@github.com:DonovanDiazcide/taller-otree-pgg.git
   ```

### Problema: "fatal: destination path 'taller-otree-pgg' already exists"

```
fatal: destination path 'taller-otree-pgg' already exists and is not an empty directory.
```

**Causa:** Ya existe una carpeta con ese nombre en la ubicación actual.

**Solución:**
- **Opción A:** Elimina la carpeta existente y clona de nuevo:
  ```bash
  rm -rf taller-otree-pgg
  git clone git@github.com:DonovanDiazcide/taller-otree-pgg.git
  ```
- **Opción B:** Clona con un nombre diferente:
  ```bash
  git clone git@github.com:DonovanDiazcide/taller-otree-pgg.git taller-otree-nuevo
  ```

### Problema: Clonar funciona pero oTree da error

**Causa:** Puede que falten dependencias del proyecto.

**Solución:**
1. Asegúrate de estar dentro de la carpeta del proyecto:
   ```bash
   cd taller-otree-pgg
   ```
2. Instala las dependencias si hay un archivo `requirements.txt`:
   ```bash
   pip install -r requirements.txt
   ```
   O en algunos sistemas:
   ```bash
   pip3 install -r requirements.txt
   ```

### Alternativa: Clonar usando HTTPS (si SSH no funciona)

Si tienes problemas con SSH y necesitas clonar urgentemente, puedes usar HTTPS como alternativa:

```bash
git clone https://github.com/DonovanDiazcide/taller-otree-pgg.git
```

⚠️ **Nota:** Con HTTPS te pedirá tu usuario y contraseña de GitHub cada vez que interactúes con el repositorio. Por eso recomendamos SSH para el uso regular.

---

## Resumen de comandos

Aquí tienes todos los comandos de esta sección en orden:

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

# 6. (Opcional) Probar oTree
otree devserver
```

# 🔧 Solución de problemas comunes

## Problema: "command not found" o "no se reconoce como comando"

**Causa**: El programa no está en el PATH o no reiniciaste la terminal.

**Solución**:
1. **Cierra completamente** tu terminal
2. Abre una **nueva** terminal
3. Intenta el comando de nuevo

Si persiste:
- Verifica que marcaste "Add to PATH" durante la instalación
- En Windows: busca "variables de entorno" y verifica que la ruta del programa esté en PATH

---

## Problema: Python funciona escribiendo `python3` pero no `python`

**Causa**: En algunos sistemas, Python 3 se llama `python3` para distinguirlo de Python 2.

**Solución**:
- Usa `python3` en lugar de `python` para todos los comandos
- Ejemplo: `python3 --version` o `python3 -m pip install otree`

---

## Problema: En Linux pip dice "error: externally-managed-environment"

**Causa**: Ubuntu y otras distribuciones modernas protegen el Python del sistema.

**Solución**:
Agrega `--break-system-packages` al final de tus comandos pip:
```bash
pip install otree --break-system-packages
```

---

## Problema: SSH no funciona en Windows Command Prompt

**Causa**: Command Prompt no tiene los comandos SSH por defecto.

**Solución**:
- Usa **Git Bash** para todos los comandos SSH
- Git Bash se instaló automáticamente con Git

---

## Problema: VS Code no abre con `code .`

**Causa**: No se agregó al PATH o no reiniciaste la terminal.

**Solución**:
1. Cierra todas las terminales
2. Abre una nueva
3. Intenta de nuevo

Si persiste:
- **Mac**: Sigue los pasos de "Configurar el comando `code`" en la Sección 3
- **Windows**: Verifica que marcaste "Add to PATH" durante la instalación

---

# Referencias y recursos adicionales

Este tutorial se basa en las documentaciones oficiales:

- **Git**: https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
- **Python**: https://docs.python.org/3/using/
- **VS Code**: https://code.visualstudio.com/docs
- **oTree**: https://otree.readthedocs.io/
- **GitHub SSH**: https://docs.github.com/en/authentication/connecting-to-github-with-ssh

---

**Última actualización**: Diciembre 2024  
**Versión**: 3.1 - Actualizada con sección de GitHub

---

## Glosario de términos técnicos

Si encuentras algún término que no entiendas, aquí está una guía rápida:

| Término | Significado simple |
|---------|-------------------|
| **Terminal / Línea de comandos** | Ventana donde escribes instrucciones de texto |
| **PATH** | Lista de carpetas donde tu computadora busca programas |
| **SSH** | Método seguro para conectarse a servidores |
| **Framework** | Conjunto de herramientas que facilitan programar |
| **Repository** | Carpeta de proyecto guardada en GitHub |
| **Clone** | Copiar un proyecto de GitHub a tu computadora |
| **pip** | Instalador de paquetes para Python |
| **Package** | Programa o herramienta adicional para Python |
