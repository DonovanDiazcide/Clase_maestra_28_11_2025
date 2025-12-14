# Guía de instalación completa para Windows 2025: Git, Python, VS Code, oTree y SSH

Esta guía verificada con fuentes oficiales de 2025 proporciona instrucciones precisas para configurar un entorno de desarrollo completo en **Windows 10 y 11** en español. Cada paso incluye citas textuales de la documentación oficial, comandos exactos y soluciones a problemas comunes organizados por frecuencia.

---

## 1. Git en Windows

Git es el sistema de control de versiones que permite gestionar el código y colaborar con GitHub. La versión actual es **Git 2.52.0** (noviembre 2025).

### Caso HABITUAL: Instalación estándar

**Paso 1: Descargar el instalador oficial**

Visita https://git-scm.com/download/win y descarga el instalador de 64 bits. Según la documentación oficial:

> "Click here to download the latest (2.52.0) x64 version of Git for Windows. This is the most recent maintained build."
> — *Fuente: git-scm.com/download/win*

**Paso 2: Ejecutar el instalador con opciones recomendadas**

Durante la instalación, aparecerán varias pantallas de configuración. Las opciones **críticas** son:

| Pantalla | Opción recomendada | Por qué |
|----------|-------------------|---------|
| **Select Components** | ✅ Mantener valores por defecto | Incluye Git Bash e integración con Explorer |
| **Default Editor** | Visual Studio Code o Notepad++ | Vim puede ser difícil para principiantes |
| **PATH environment** | ✅ "Git from the command line and also from 3rd-party software" | **CRÍTICO**: Permite usar `git` desde cualquier terminal |
| **SSH executable** | ✅ "Use bundled OpenSSH" | Evita conflictos con otras versiones |
| **Line ending conversions** | ✅ "Checkout Windows-style, commit Unix-style" | Maneja automáticamente las diferencias CRLF/LF |
| **Terminal emulator** | ✅ "Use MinTTY" | Terminal moderna con mejor soporte |

Sobre la opción de PATH, la documentación indica:

> "The PATH is the default set of directories included when you run a command from the command line. Keep the middle (recommended) selection."
> — *Fuente: git-scm.com/book/en/v2/Getting-Started-Installing-Git*

Sobre los finales de línea (CRLF):

> "If you're on a Windows machine, set it to true — this converts LF endings into CRLF when you check out code."
> — *Fuente: git-scm.com/book/en/v2/Customizing-Git-Git-Configuration*

**Paso 3: Verificar la instalación**

Abre una **nueva** terminal (Command Prompt, PowerShell, o Git Bash) y ejecuta:

```cmd
git --version
```

Salida esperada: `git version 2.52.0.windows.1`

**Paso 4: Configuración inicial obligatoria**

Según el libro oficial Pro Git:

> "The first thing you should do when you install Git is to set your user name and email address. This is important because every Git commit uses this information."
> — *Fuente: git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup*

Ejecuta estos comandos (reemplaza con tu información):

```bash
git config --global user.name "Tu Nombre Completo"
git config --global user.email "tu@email.com"
git config --global init.defaultBranch main
```

Para verificar la configuración:

```bash
git config --list
```

### Git Bash vs Command Prompt vs PowerShell

La documentación de Git for Windows explica:

> "Git for Windows provides a BASH emulation used to run Git from the command line. *NIX users should feel right at home, as the BASH emulation behaves just like the 'git' command in LINUX and UNIX environments."
> — *Fuente: gitforwindows.org*

| Terminal | Cuándo usarla |
|----------|---------------|
| **Git Bash** | ✅ Recomendada para SSH y scripts. Compatible con tutoriales de Linux/Mac |
| **PowerShell** | Buena para usuarios que prefieren entorno Windows nativo |
| **CMD** | Funcional pero con menos características |

**Diferencia importante en rutas:**
- Git Bash: `cd /c/Users/nombre/proyectos`
- CMD/PowerShell: `cd C:\Users\nombre\proyectos`

### Caso MEDIANAMENTE HABITUAL: Problemas comunes

**Problema: "git no reconocido como comando"**

Causa: La terminal se abrió antes de instalar Git o el PATH no se configuró.

Soluciones:
1. **Cerrar y reabrir** la terminal (obligatorio después de instalar)
2. Si persiste, agregar manualmente al PATH:
   - Win + R → `sysdm.cpl` → Opciones avanzadas → Variables de entorno
   - Editar `Path` en Variables del sistema
   - Agregar: `C:\Program Files\Git\cmd`

**Problema: Git extremadamente lento**

Causa: Windows Defender escanea cada archivo.

Solución con PowerShell (ejecutar como administrador):
```powershell
Add-MpPreference -ExclusionPath "C:\Program Files\Git"
Add-MpPreference -ExclusionPath "C:\Users\TuUsuario\proyectos"
Add-MpPreference -ExclusionProcess "git.exe"
```

**Problema: Advertencia "LF will be replaced by CRLF"**

Esta advertencia es **normal** e indica que Git está convirtiendo correctamente los finales de línea. Para silenciarla, confirma que tienes:

```bash
git config --global core.autocrlf true
```

---

## 2. Python en Windows

Para oTree, necesitas **Python 3.11.x** específicamente. La versión más reciente con instaladores binarios es **Python 3.11.9**.

### ⚠️ Advertencia crítica sobre versiones

El código fuente de oTree especifica explícitamente:

> "Error: This version of oTree is only compatible with these Python versions: 3.7, 3.8, 3.9, 3.10, 3.11"
> — *Fuente: github.com/oTree-org/otree-core/blob/master/setup.py*

**NO instales Python 3.12 o superior** si vas a usar oTree.

### Caso HABITUAL: Instalación de Python 3.11.9

**Paso 1: Descargar el instalador correcto**

Descarga desde: https://www.python.org/ftp/python/3.11.9/python-3.11.9-amd64.exe

Según python.org:

> "Python 3.11.9 was the last full bugfix release of Python 3.11 with binary installers."
> — *Fuente: python.org/downloads/release/python-3119/*

**Paso 2: Instalar con "Add Python to PATH" activado**

Al ejecutar el instalador, en la **primera pantalla** verás dos opciones cruciales en la parte inferior:

**✅ MARCA OBLIGATORIAMENTE**: "Add python.exe to PATH"

Según la documentación oficial:

> "When you first install a runtime, you will likely be prompted to add a directory to your PATH. This is optional, if you prefer to use the py command, but is offered for those who prefer the full range of aliases."
> — *Fuente: docs.python.org/3/using/windows.html*

**¿Qué pasa si NO marcas esta opción?**
- El comando `python` no funcionará desde ninguna terminal
- Aparecerá el error: `'python' is not recognized as an internal or external command`
- O Windows abrirá la Microsoft Store ofreciendo instalar otra versión

Opciones de instalación:
| Opción | Significado |
|--------|-------------|
| "Install Now" | Instala para el usuario actual con configuración estándar |
| "Customize installation" | Permite elegir ubicación y opciones adicionales |

Para la mayoría de usuarios, **"Install Now"** con PATH activado es suficiente.

**Paso 3: Verificar la instalación**

**IMPORTANTE**: Cierra y vuelve a abrir la terminal antes de verificar.

```cmd
python --version
```
Salida esperada: `Python 3.11.9`

```cmd
pip --version
```
Salida esperada: `pip 24.x.x from ... (python 3.11)`

### Caso MEDIANAMENTE HABITUAL: Problemas con Python

**Problema: "python was not found" o abre Microsoft Store**

Causa: Alias de Microsoft Store activos compitiendo con tu instalación.

Solución según la documentación:

> "Click Start, open 'Manage app execution aliases', and check that the aliases for 'Python (default)' are enabled. If they already are, try disabling and re-enabling."
> — *Fuente: docs.python.org/3/using/windows.html*

Pasos:
1. Configuración → Aplicaciones → Alias de ejecución de aplicaciones
2. **Desactivar** los alias "python.exe" y "python3.exe" de "App Installer"

**Problema: Agregar Python al PATH manualmente (si se olvidó)**

1. Win + R → `sysdm.cpl` → pestaña "Opciones avanzadas"
2. Clic en "Variables de entorno"
3. En "Variables del usuario", selecciona "Path" → "Editar"
4. Agrega estas dos rutas (ajusta según tu usuario):
   ```
   C:\Users\TuUsuario\AppData\Local\Programs\Python\Python311\
   C:\Users\TuUsuario\AppData\Local\Programs\Python\Python311\Scripts\
   ```
5. Acepta y **cierra todas las terminales** antes de probar

**Problema: Múltiples versiones de Python instaladas**

Usa el Python Launcher para especificar versión:
```cmd
py -3.11 -m venv mi_ambiente
py -3.11 script.py
```

Para ver todas las versiones instaladas:
```cmd
py --list
```

---

## 3. Visual Studio Code en Windows

VS Code es el editor de código recomendado para desarrollo con Python y oTree.

### Caso HABITUAL: Instalación estándar

**Paso 1: Descargar el instalador correcto**

Descarga desde: https://code.visualstudio.com/download

Según la documentación oficial de Microsoft:

> "**User setup:** Does not require administrator privileges to run, as the location is under your user Local AppData folder. **This is the preferred way to install VS Code on Windows.**"
> — *Fuente: code.visualstudio.com/docs/setup/windows*

Elige **"User Installer x64"** (recomendado) en lugar del System Installer.

**Paso 2: Opciones de instalación importantes**

Durante la instalación, **marca estas opciones**:

- ✅ **"Add to PATH"** — Permite usar el comando `code` desde terminal
- ✅ "Register Code as an editor for supported file types"
- ✅ "Add 'Open with Code' action to Windows Explorer context menu"

Sobre la opción de PATH, la documentación indica:

> "Setup adds Visual Studio Code to your %PATH% environment variable, to let you type 'code .' in the console to open VS Code on that folder. You need to restart your console after the installation for the change to the %PATH% environmental variable to take effect."
> — *Fuente: code.visualstudio.com/docs/setup/windows*

**Paso 3: Verificar la instalación**

Cierra y abre una nueva terminal:
```cmd
code --version
```

Para abrir VS Code en la carpeta actual:
```cmd
code .
```

**Paso 4: Instalar la extensión de Python**

La extensión oficial de Microsoft para Python es esencial. Según la documentación:

> "The Python extension will automatically install the following extensions by default: Pylance (ms-python.vscode-pylance), Python Debugger (ms-python.debugpy), Python Environments (ms-python.vscode-python-envs)"
> — *Fuente: marketplace.visualstudio.com/items?itemName=ms-python.python*

**Instalación desde línea de comandos:**
```cmd
code --install-extension ms-python.python
```

**Instalación desde VS Code:**
1. Presiona `Ctrl+Shift+X` para abrir extensiones
2. Busca "Python"
3. Instala la extensión de Microsoft (identificador: `ms-python.python`)

**Extensiones recomendadas adicionales:**

| Extensión | ID | Propósito |
|-----------|---|-----------|
| Pylance | `ms-python.vscode-pylance` | Autocompletado inteligente |
| Python Debugger | `ms-python.debugpy` | Depuración |
| Jupyter | `ms-toolsai.jupyter` | Notebooks (opcional) |

### Seleccionar el intérprete de Python correcto

Según la documentación:

> "The Python extension automatically detects Python interpreters that are installed in standard locations. It also detects virtual environments in the workspace folder."
> — *Fuente: code.visualstudio.com/docs/languages/python*

Para seleccionar manualmente:
1. Presiona `Ctrl+Shift+P`
2. Escribe "Python: Select Interpreter"
3. Elige tu instalación de Python 3.11 o el ambiente virtual

### Caso MEDIANAMENTE HABITUAL: Problemas con VS Code

**Problema: Windows Defender SmartScreen bloquea la instalación**

Solución:
1. Clic en "Más información"
2. Clic en "Ejecutar de todos modos"

El instalador oficial de VS Code está firmado digitalmente por Microsoft y es seguro.

**Problema: El comando `code` no funciona**

La documentación indica:

> "'code' is not recognized as an internal or external command: Your OS cannot find the VS Code binary on its path. Try uninstalling and reinstalling VS Code."
> — *Fuente: code.visualstudio.com/docs/configure/command-line*

Solución manual: Agregar al PATH la ruta:
```
C:\Users\TuUsuario\AppData\Local\Programs\Microsoft VS Code\bin
```

---

## 4. oTree en Windows

oTree es el framework para crear experimentos económicos. Se instala mediante pip dentro de un ambiente virtual.

### Caso HABITUAL: Instalación de oTree

**Requisito previo**: Python 3.11.x instalado y funcionando.

**Paso 1: Crear carpeta para el proyecto**
```cmd
mkdir mi_proyecto_otree
cd mi_proyecto_otree
```

**Paso 2: Crear ambiente virtual**
```cmd
python -m venv venv
```

**Paso 3: Activar ambiente virtual**

En CMD:
```cmd
venv\Scripts\activate.bat
```

En PowerShell:
```powershell
.\venv\Scripts\Activate.ps1
```

Sabrás que está activo porque aparece `(venv)` al inicio del prompt.

**Paso 4: Instalar oTree**
```cmd
pip install otree
```

Según la documentación oficial:

> "pip3 install -U otree"
> — *Fuente: otree.readthedocs.io/en/latest/install-nostudio.html*

**Nota Windows**: En Windows usa `pip` en lugar de `pip3`:

> "Hi awallen, just use pip instead. pip3 is typically used for Mac OS."
> — *Fuente: otreehub.com/forum/1030/*

**Paso 5: Verificar instalación**
```cmd
otree --version
```

**Paso 6: Crear proyecto oTree**
```cmd
otree startproject nombre_proyecto
```

El comando preguntará si deseas incluir juegos de ejemplo (responde "y" o "n").

**Paso 7: Ejecutar servidor de desarrollo**
```cmd
cd nombre_proyecto
otree devserver
```

Abre tu navegador en: **http://localhost:8000**

Deberías ver la interfaz de administración de oTree.

### Comandos principales de oTree

| Comando | Descripción |
|---------|-------------|
| `otree devserver` | Servidor de desarrollo (solo acceso local) |
| `otree prodserver` | Servidor de producción (acceso desde red) |
| `otree startproject nombre` | Crear nuevo proyecto |
| `otree startapp nombre` | Crear nueva aplicación dentro del proyecto |
| `otree resetdb` | Reiniciar base de datos |
| `otree --version` | Ver versión instalada |

### Diferencia devserver vs prodserver

> "With otree devserver, I can only access oTree via http://localhost:8000. If I want to access oTree from other computers, I have to use otree prodserver."
> — *Fuente: BonnEconLab, otreehub.com/forum/1337/*

### Caso MEDIANAMENTE HABITUAL: Problemas con oTree

**Problema: Error de versión de Python**

Si ves este error:
```
Error: This version of oTree is only compatible with these Python versions: 3.7, 3.8, 3.9, 3.10, 3.11
```

Solución: Instala Python 3.11.9 y crea un nuevo ambiente virtual:
```cmd
py -3.11 -m venv venv_otree
venv_otree\Scripts\activate
pip install otree
```

**Problema: Puerto 8000 en uso**

> "This means that there is another server process already running on that port."
> — *Fuente: groups.google.com/g/otree*

Soluciones:
1. Cerrar todas las terminales y reintentar
2. Encontrar el proceso:
   ```cmd
   netstat -ano | findstr :8000
   ```
3. Terminar el proceso (reemplaza PID con el número):
   ```cmd
   taskkill /PID [numero_pid] /F
   ```

**Problema: Puerto bloqueado por firewall**

Abrir puerto en Firewall de Windows (ejecutar como administrador):
```powershell
netsh advfirewall firewall add rule name="oTree Port 8000" dir=in action=allow protocol=TCP localport=8000
```

**Problema: Python se congela con PowerShell**

> "If Python crashes when you run this command, and you're using PowerShell, try using CMD instead."
> — *Fuente: ibsen-otree-docs.ibsen-h2020.eu/install.html*

**Problema: Caracteres especiales en rutas**

Evita carpetas con espacios, acentos o caracteres especiales:
- ✅ Correcto: `C:\Users\Juan\proyectos\otree`
- ❌ Evitar: `C:\Users\José María\mis proyectos\oTree experimento`

---

## 5. SSH con GitHub en Windows

SSH permite conectarte de forma segura a GitHub sin escribir tu contraseña cada vez.

### Caso HABITUAL: Configuración SSH completa

GitHub recomienda usar **Git Bash** para SSH en Windows:

> "Open Terminal Terminal **Git Bash**."
> — *Fuente: docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent*

**Paso 1: Abrir Git Bash**

Busca "Git Bash" en el menú inicio o haz clic derecho en cualquier carpeta → "Git Bash Here"

**Paso 2: Generar la llave SSH**

Según la documentación oficial de GitHub:

> "Paste the text below, replacing the email used in the example with your GitHub email address.
> `ssh-keygen -t ed25519 -C "your_email@example.com"`
> Note: If you are using a legacy system that doesn't support the Ed25519 algorithm, use:
> `ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`"
> — *Fuente: docs.github.com/en/authentication/connecting-to-github-with-ssh*

Ejecuta en Git Bash:
```bash
ssh-keygen -t ed25519 -C "tu@email.com"
```

Cuando pregunte ubicación, presiona Enter para aceptar el valor por defecto (`/c/Users/TuUsuario/.ssh/id_ed25519`).

Cuando pregunte por passphrase, puedes:
- **Dejar vacío** (presiona Enter dos veces) — Más conveniente
- **Escribir una contraseña** — Más seguro

Sobre el passphrase, GitHub indica:

> "If your key has a passphrase and you don't want to enter the passphrase every time you use the key, you can add your key to the SSH agent."
> — *Fuente: docs.github.com*

**Paso 3: Iniciar ssh-agent y agregar la llave**

En Git Bash:
```bash
eval "$(ssh-agent -s)"
```
Salida esperada: `Agent pid XXXXX`

Agregar la llave:
```bash
ssh-add ~/.ssh/id_ed25519
```

**Paso 4: Copiar la llave pública**

En Git Bash:
```bash
clip < ~/.ssh/id_ed25519.pub
```

Según la documentación:

> "$ clip < ~/.ssh/id_ed25519.pub
> Copies the contents of the id_ed25519.pub file to your clipboard"
> — *Fuente: docs.github.com*

**Nota para PowerShell/Windows Terminal**: El operador `<` no funciona igual:

> "On newer versions of Windows that use the Windows Terminal, or anywhere else that uses the PowerShell command line, you may receive a ParseError. In this case, the following alternative command should be used:
> `$ cat ~/.ssh/id_ed25519.pub | clip`"
> — *Fuente: docs.github.com*

**Paso 5: Agregar la llave a GitHub**

1. Abre https://github.com/settings/keys
2. Clic en "New SSH key"
3. En "Title", escribe un nombre descriptivo (ej: "Laptop Windows personal")
4. En "Key", pega el contenido copiado (Ctrl+V)
5. Clic en "Add SSH key"

Según la documentación:

> "In the upper-right corner of any page on GitHub, click your profile picture, then click Settings. In the 'Access' section of the sidebar, click SSH and GPG keys. Click New SSH key."
> — *Fuente: docs.github.com*

**Paso 6: Probar la conexión**

```bash
ssh -T git@github.com
```

La primera vez verás una advertencia sobre la autenticidad del host:

> "The authenticity of host 'github.com (IP ADDRESS)' can't be established.
> ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
> Are you sure you want to continue connecting (yes/no)?"
> — *Fuente: docs.github.com*

Escribe `yes` y presiona Enter.

**Mensaje de éxito esperado:**
```
Hi USERNAME! You've successfully authenticated, but GitHub does not provide shell access.
```

### Archivo de configuración SSH (opcional pero recomendado)

Crea o edita el archivo `~/.ssh/config` (en Windows: `C:\Users\TuUsuario\.ssh\config`):

```
Host github.com
  HostName github.com
  User git
  IdentityFile ~/.ssh/id_ed25519
  AddKeysToAgent yes
```

Esto asegura que siempre se use la llave correcta y se agregue automáticamente al agente.

### Caso MEDIANAMENTE HABITUAL: Problemas con SSH

**Problema: "Permission denied (publickey)"**

> "A 'Permission denied' error means that the server rejected your connection."
> — *Fuente: docs.github.com/en/authentication/troubleshooting-ssh/error-permission-denied-publickey*

Verificar que la llave está cargada:
```bash
ssh-add -l -E sha256
```

Soluciones:
1. Verificar que la llave pública está correctamente agregada en GitHub
2. Asegurarse de conectar como usuario `git` (no tu username de GitHub)
3. Reiniciar ssh-agent y volver a agregar la llave

**Problema: ssh-agent no está corriendo**

En Git Bash:
```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

En PowerShell (como administrador):
```powershell
Get-Service ssh-agent | Set-Service -StartupType Manual
Start-Service ssh-agent
ssh-add $env:USERPROFILE\.ssh\id_ed25519
```

**Problema: Conflicto entre Git SSH y Windows OpenSSH**

Git for Windows incluye su propio cliente SSH. Para usar el OpenSSH de Windows en su lugar:
```bash
git config --global core.sshCommand "C:/Windows/System32/OpenSSH/ssh.exe"
```

### Git Bash vs PowerShell para SSH

| Característica | Git Bash | PowerShell |
|---------------|----------|------------|
| Iniciar ssh-agent | `eval "$(ssh-agent -s)"` | `Start-Service ssh-agent` |
| Copiar llave | `clip < ~/.ssh/id_ed25519.pub` | `cat ~/.ssh/id_ed25519.pub \| clip` |
| Rutas | `~/.ssh/` | `$env:USERPROFILE\.ssh\` |
| Documentación GitHub | ✅ Recomendada | ✅ Soportada |

**Recomendación**: Git Bash es la opción más sencilla y mejor documentada por GitHub.

---

## Resumen de instalación completa

### Orden de instalación recomendado

1. **Git** → Incluye Git Bash necesario para SSH
2. **Python 3.11.9** → Con "Add to PATH" activado
3. **Visual Studio Code** → Con extensión de Python
4. **Ambiente virtual** → Para aislar oTree
5. **oTree** → Dentro del ambiente virtual
6. **SSH con GitHub** → Usando Git Bash

### Comandos de verificación rápida

```bash
# Git
git --version

# Python  
python --version
pip --version

# VS Code
code --version

# oTree (con ambiente virtual activado)
otree --version

# SSH
ssh -T git@github.com
```

### Ubicaciones importantes en Windows

| Elemento | Ruta típica |
|----------|-------------|
| Git | `C:\Program Files\Git\` |
| Python 3.11 | `C:\Users\TuUsuario\AppData\Local\Programs\Python\Python311\` |
| VS Code | `C:\Users\TuUsuario\AppData\Local\Programs\Microsoft VS Code\` |
| Llaves SSH | `C:\Users\TuUsuario\.ssh\` |
| Configuración Git | `C:\Users\TuUsuario\.gitconfig` |

---

## Anexos: Casos raros

### Windows en modo S

Windows en modo S solo permite aplicaciones de Microsoft Store. Para instalar estas herramientas necesitas salir del modo S (gratuito pero irreversible) o usar la versión de Python de Microsoft Store (no recomendada para oTree por problemas de compatibilidad).

### WSL como alternativa

> "If you are on Windows, WSL is a great way to do Python development."
> — *Fuente: code.visualstudio.com/docs/languages/python*

WSL (Windows Subsystem for Linux) permite ejecutar un entorno Linux completo dentro de Windows. Puede ser útil para usuarios avanzados, pero esta guía se enfoca en la instalación nativa de Windows que es más sencilla para principiantes.

### Instalación de Git con Winget

Alternativa para usuarios que prefieren línea de comandos:
```powershell
winget install --id Git.Git -e --source winget
```

> "Install winget tool if you don't already have it, then type this command in command prompt or Powershell."
> — *Fuente: git-scm.com*

### Instalación de VS Code con Winget

```powershell
winget install Microsoft.VisualStudioCode
```

### oTree 6.0 Beta (Octubre 2025)

> "October 2025 update: oTree 6.0 beta is available with AI integration, better wait pages, custom welcome pages and consent forms, back button, better admin interface, and many other features."
> — *Fuente: otree.readthedocs.io*

Para instalar la versión beta:
```cmd
pip install otree==6.0.0b1
```

Solo usar para probar nuevas características; para producción usar la versión estable.

### Problemas con certificados SSL en pip

Si pip muestra errores de certificados SSL:
```cmd
pip install --trusted-host pypi.org --trusted-host files.pythonhosted.org otree
```

### Configuración de ssh-agent automático en Git Bash

Agregar al archivo `~/.bashrc` (crear si no existe):

```bash
SSH_ENV="$HOME/.ssh/environment"

function run_ssh_env {
  . "${SSH_ENV}" > /dev/null
}

function start_ssh_agent {
  echo "Initializing SSH agent..."
  ssh-agent | sed 's/^echo/#echo/' > "${SSH_ENV}"
  chmod 600 "${SSH_ENV}"
  run_ssh_env;
  ssh-add ~/.ssh/id_ed25519;
}

if [ -f "${SSH_ENV}" ]; then
  run_ssh_env;
  ps -ef | grep ${SSH_AGENT_PID} | grep ssh-agent$ > /dev/null || {
    start_ssh_agent;
  }
else
  start_ssh_agent;
fi
```

Esto inicia automáticamente ssh-agent cada vez que abres Git Bash.

---

## Conclusión

Esta guía proporciona un camino verificado y documentado para configurar un entorno de desarrollo completo en Windows 10/11. Los puntos más críticos a recordar son:

La opción **"Add Python to PATH"** es absolutamente esencial durante la instalación de Python; sin ella, ningún comando de Python funcionará correctamente. Para oTree, usar exclusivamente **Python 3.11.x** evita errores de compatibilidad que pueden ser difíciles de diagnosticar. Los **ambientes virtuales** no son opcionales para proyectos serios; protegen tu sistema de conflictos de dependencias. Y finalmente, **Git Bash** es la terminal preferida para operaciones SSH porque sigue exactamente la documentación oficial de GitHub.

Siempre cierra y reabre las terminales después de modificar el PATH o instalar nuevas herramientas. Este simple paso evita la mayoría de los problemas de "comando no reconocido" que frustran a los principiantes.
