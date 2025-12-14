# Guía completa de instalación para desarrollo en macOS (2025)

**Git, Python, Visual Studio Code, oTree y SSH con GitHub** — Esta guía proporciona un camino claro desde cero hasta tener un entorno de desarrollo completamente funcional para experimentos económicos con oTree en tu Mac. El factor más importante a considerar: **oTree solo funciona con Python 3.7-3.11**, no con versiones más recientes, lo que determina el orden óptimo de instalación.

La estrategia recomendada es instalar primero las herramientas base (Git, Python 3.11, VS Code), luego oTree en un ambiente virtual, y finalmente configurar SSH para conectar con GitHub. Todo el proceso toma aproximadamente **45-60 minutos** para usuarios nuevos.

---

## Antes de comenzar: conoce tu Mac

Antes de instalar cualquier herramienta, necesitas identificar dos características fundamentales de tu computadora que afectarán algunos pasos de instalación.

### Verificar el tipo de procesador

Abre la aplicación **Terminal** (búscala en Spotlight con `Cmd + Espacio` y escribe "Terminal") y ejecuta:

```bash
uname -m
```

- Si muestra `arm64`: tienes un **Mac con Apple Silicon** (M1, M2, M3 o M4)
- Si muestra `x86_64`: tienes un **Mac con procesador Intel**

### Verificar la versión de macOS

En el menú de Apple () → **Acerca de esta Mac**, verás tu versión. Esta guía cubre:

| Versión | Nombre | Soporte |
|---------|--------|---------|
| 15.x | Sequoia | ✅ Completo |
| 14.x | Sonoma | ✅ Completo |
| 13.x | Ventura | ✅ Completo |
| 12.x | Monterey | ⚠️ Funcional con limitaciones |

### El shell predeterminado: zsh

Desde macOS Catalina (10.15), el shell predeterminado es **zsh**, no bash. Esto significa que la configuración de variables de entorno va en el archivo `~/.zshrc` (o `~/.zprofile` para variables que se cargan al iniciar sesión). La tilde (`~`) representa tu carpeta de usuario, equivalente a `/Users/tu_nombre_de_usuario`.

---

## Parte 1: Instalación de Git

Git es el sistema de control de versiones que te permite rastrear cambios en tu código y colaborar con otros. Es requisito fundamental para trabajar con GitHub.

### Caso HABITUAL: Instalación mediante Xcode Command Line Tools

Este es el método más sencillo y el que **el 70-80% de usuarios Mac seguirán**. Apple incluye Git como parte de sus herramientas de desarrollo.

> *"Apple ships a binary package of Git with Xcode Command Line Tools. You can install this via: `$ xcode-select --install`"*
> — Documentación oficial de Git (git-scm.com/download/mac)

**Paso 1:** Abre Terminal y ejecuta:

```bash
xcode-select --install
```

**Paso 2:** Aparecerá una ventana emergente preguntando si deseas instalar las herramientas de desarrollo. Haz clic en **"Instalar"** y acepta los términos de licencia.

**Paso 3:** Espera la descarga e instalación (aproximadamente 1 GB, puede tomar 10-20 minutos dependiendo de tu conexión).

**Paso 4:** Verifica la instalación:

```bash
git --version
```

Deberías ver algo como: `git version 2.39.3 (Apple Git-146)`

**Nota importante:** La versión de Git que Apple incluye suele estar algunas versiones por detrás de la más reciente, pero es perfectamente funcional para la mayoría de usos.

**Método alternativo (automático):** Si nunca has instalado las herramientas de desarrollo, simplemente escribir `git` en Terminal disparará automáticamente el instalador:

```bash
git
```

### Caso MEDIANAMENTE HABITUAL: Instalación mediante Homebrew

Si ya tienes **Homebrew** instalado (el gestor de paquetes más popular para Mac), o si necesitas la **versión más reciente de Git**, este método es preferible.

> *"Install homebrew if you don't already have it, then: `$ brew install git`"*
> — Documentación oficial de Git

**Si aún no tienes Homebrew instalado:**

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

**Importante para Apple Silicon (M1/M2/M3/M4):** Después de instalar Homebrew, debes agregar su ruta al PATH. El instalador te mostrará instrucciones específicas, pero generalmente es:

```bash
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zshrc
eval "$(/opt/homebrew/bin/brew shellenv)"
```

**Para Macs Intel**, Homebrew se instala en `/usr/local` y generalmente no requiere configuración adicional del PATH.

**Instalar Git con Homebrew:**

```bash
brew install git
```

**Verificar que estás usando la versión de Homebrew:**

```bash
which git
```

- Apple Silicon: debe mostrar `/opt/homebrew/bin/git`
- Intel: debe mostrar `/usr/local/bin/git`

### Configuración inicial de Git (OBLIGATORIA para todos)

Después de instalar Git, debes configurar tu identidad. Esta información se incluye en cada commit que hagas.

> *"The first thing you should do when you install Git is to set your user name and email address. This is important because every Git commit uses this information, and it's immutably baked into the commits you start creating."*
> — Pro Git Book (git-scm.com/book)

```bash
# Tu nombre (usa tu nombre real o el que uses profesionalmente)
git config --global user.name "Tu Nombre Completo"

# Tu correo (usa el mismo correo de tu cuenta de GitHub)
git config --global user.email "tu_correo@ejemplo.com"

# Configurar la rama por defecto como "main" (estándar moderno)
git config --global init.defaultBranch main

# Verificar la configuración
git config --list
```

---

## Parte 2: Instalación de Python

Python es el lenguaje de programación en el que está escrito oTree. La elección de versión es **crítica** porque oTree tiene requisitos específicos de compatibilidad.

### Consideración fundamental: compatibilidad con oTree

> *"This version of oTree is only compatible with these Python versions: 3.7, 3.8, 3.9, 3.10, 3.11"*
> — Mensaje de error de oTree / Foro oTreeHub

**Python 3.12 y 3.13 NO son compatibles con oTree.** Por esta razón, la guía recomienda instalar específicamente **Python 3.11**, que es la versión más reciente compatible y tiene soporte activo.

### Estado de Python en macOS moderno

> *"Recent versions of macOS include a python3 command in /usr/bin/python3 that links to a usually older and incomplete version of Python provided by and for use by the Apple development tools. You should never modify or attempt to delete this installation."*
> — Documentación oficial de Python (docs.python.org/3/using/mac.html)

- **Python 2.7 fue eliminado** a partir de macOS 12.3 (Monterey)
- macOS incluye Python 3.9.6 a través de Xcode Command Line Tools
- Esta versión del sistema **no debe usarse para desarrollo**; instala tu propia versión

### Caso HABITUAL: Instalación desde python.org

Este método es el más directo y el recomendado para usuarios que no tienen Homebrew o prefieren un instalador gráfico tradicional.

> *"Current installers provide a universal2 binary build of Python which runs natively on all Macs (Apple Silicon and Intel) that are supported by a wide range of macOS versions."*
> — Documentación oficial de Python

**Paso 1:** Ve a https://www.python.org/downloads/macos/

**Paso 2:** Descarga el instalador de **Python 3.11.x** (la versión específica más reciente de la serie 3.11). Busca el archivo que termine en `-macos11.pkg`.

**Paso 3:** Abre el archivo `.pkg` descargado y sigue el asistente de instalación. Acepta los valores predeterminados.

**Paso 4:** Al finalizar la instalación, se abrirá una carpeta con dos archivos importantes:
- `Install Certificates.command` — **Ejecútalo haciendo doble clic**. Esto instala certificados SSL necesarios para que pip funcione correctamente.
- `Update Shell Profile.command` — Actualiza el PATH automáticamente.

**Paso 5:** Cierra y vuelve a abrir Terminal, luego verifica:

```bash
python3 --version
# Debe mostrar: Python 3.11.x

pip3 --version
# Debe mostrar la versión de pip y la ruta de Python 3.11
```

**Ubicación de la instalación:** `/Library/Frameworks/Python.framework/Versions/3.11/`

### Caso MEDIANAMENTE HABITUAL: Instalación mediante Homebrew

Si ya usas Homebrew, este método mantiene todas tus herramientas gestionadas en un solo lugar.

```bash
# Instalar Python 3.11 específicamente
brew install python@3.11

# Verificar instalación
python3.11 --version

# Verificar pip
pip3.11 --version
```

**Crear alias para facilitar el uso** (agregar a `~/.zshrc`):

```bash
echo 'alias python3="/opt/homebrew/opt/python@3.11/bin/python3.11"' >> ~/.zshrc
echo 'alias pip3="/opt/homebrew/opt/python@3.11/bin/pip3.11"' >> ~/.zshrc
source ~/.zshrc
```

### Configuración del PATH

Si después de la instalación los comandos `python3` o `pip3` no funcionan, necesitas agregar Python al PATH. Abre `~/.zshrc` con cualquier editor de texto:

```bash
nano ~/.zshrc
```

Agrega la siguiente línea (ajusta la versión si es diferente):

```bash
export PATH="/Library/Frameworks/Python.framework/Versions/3.11/bin:$PATH"
```

Guarda (`Ctrl + O`, luego `Enter`) y cierra (`Ctrl + X`). Recarga la configuración:

```bash
source ~/.zshrc
```

### Verificación completa de Python

Ejecuta estos comandos para confirmar que todo está correctamente configurado:

```bash
# Versión de Python
python3 --version

# Ubicación del ejecutable
which python3

# Versión de pip
pip3 --version

# Verificar arquitectura (útil para Apple Silicon)
python3 -c "import platform; print(platform.machine())"
# arm64 = Apple Silicon, x86_64 = Intel
```

---

## Parte 3: Instalación de Visual Studio Code

Visual Studio Code (VS Code) es un editor de código gratuito de Microsoft, extremadamente popular por su flexibilidad y extensiones. Es ideal para desarrollar proyectos de oTree.

### Caso HABITUAL: Descarga e instalación manual

> *"Download Visual Studio Code for macOS. Open the browser's download list and locate the downloaded app. Drag Visual Studio Code.app to the Applications folder, making it available in the macOS Launchpad."*
> — Documentación oficial de Microsoft (code.visualstudio.com/docs/setup/mac)

**Paso 1:** Ve a https://code.visualstudio.com/download

**Paso 2:** Descarga la versión para Mac. Hay tres opciones:
- **Universal** (recomendada): funciona en cualquier Mac
- **Intel Chip**: solo para Macs con procesador Intel
- **Apple Silicon**: optimizada para M1/M2/M3/M4

**Paso 3:** Abre el archivo `.zip` descargado (generalmente se descomprime automáticamente).

**Paso 4:** Arrastra `Visual Studio Code.app` a tu carpeta **Aplicaciones**.

**Paso 5:** Abre VS Code desde Aplicaciones o Launchpad. La primera vez, macOS te preguntará si deseas abrir una aplicación descargada de Internet; haz clic en **"Abrir"**.

### Caso MEDIANAMENTE HABITUAL: Instalación mediante Homebrew

```bash
brew install --cask visual-studio-code
```

Este método tiene la ventaja de configurar automáticamente el comando `code` en Terminal.

### Configurar el comando 'code' en Terminal

El comando `code` te permite abrir archivos y carpetas en VS Code directamente desde Terminal. Por ejemplo, `code .` abre la carpeta actual.

> *"Open the Command Palette (Cmd+Shift+P), type 'shell command', and run the Shell Command: Install 'code' command in PATH command."*
> — Documentación de VS Code

**Paso 1:** Abre VS Code.

**Paso 2:** Presiona `Cmd + Shift + P` para abrir la Paleta de Comandos.

**Paso 3:** Escribe "shell command" y selecciona **"Shell Command: Install 'code' command in PATH"**.

**Paso 4:** Cierra y vuelve a abrir Terminal.

**Paso 5:** Verifica que funciona:

```bash
code --version
```

### Solución de problemas con Gatekeeper

Si VS Code no abre y ves un mensaje sobre "desarrollador no identificado" (raro con descargas oficiales):

**Método 1 — Control + clic:**
1. En Finder, ve a Aplicaciones
2. Mantén presionada la tecla `Control` y haz clic en Visual Studio Code
3. Selecciona "Abrir" del menú contextual
4. Confirma que deseas abrirla

**Método 2 — Ajustes del Sistema:**
1. Ve a **Ajustes del Sistema → Privacidad y Seguridad**
2. En la sección "Seguridad", busca el mensaje sobre VS Code
3. Haz clic en **"Abrir de todos modos"**

### Extensiones recomendadas para desarrollo con Python/oTree

Abre VS Code y ve a la pestaña de Extensiones (`Cmd + Shift + X`). Instala:

- **Python** (ms-python.python): Extensión oficial de Microsoft. Proporciona IntelliSense, debugging y más.
- **Pylance** (ms-python.vscode-pylance): Servidor de lenguaje que mejora el autocompletado.

Configuración recomendada para Python (abre Configuración con `Cmd + ,` y busca "settings json"):

```json
{
  "python.languageServer": "Pylance",
  "editor.formatOnSave": true,
  "[python]": {
    "editor.defaultFormatter": "ms-python.python"
  }
}
```

---

## Parte 4: Instalación de oTree

oTree es un framework de código abierto para crear experimentos de comportamiento económico, encuestas y juegos interactivos. Se ejecuta en el navegador y permite experimentos tanto en laboratorio como en línea.

### Requisitos previos verificados

Antes de continuar, confirma que tienes:

```bash
# Python 3.11 (NO 3.12 o superior)
python3 --version

# pip funcionando
pip3 --version
```

### Caso HABITUAL: Instalación en ambiente virtual

**Los ambientes virtuales son esenciales** para oTree. Aíslan las dependencias del proyecto y evitan conflictos con otros paquetes de Python.

> *"You can install many versions of oTree using separate Python virtual environments."*
> — Documentación de oTree

**Paso 1:** Crea una carpeta para tus proyectos de oTree:

```bash
mkdir ~/oTree-proyectos
cd ~/oTree-proyectos
```

**Paso 2:** Crea un ambiente virtual:

```bash
python3 -m venv venv
```

Esto crea una carpeta `venv` que contiene una instalación aislada de Python.

**Paso 3:** Activa el ambiente virtual:

```bash
source venv/bin/activate
```

Tu prompt de Terminal cambiará para mostrar `(venv)` al inicio, indicando que el ambiente está activo.

**Paso 4:** Actualiza pip dentro del ambiente virtual:

```bash
pip install --upgrade pip
```

**Paso 5:** Instala oTree:

```bash
pip install otree
```

**Paso 6:** Verifica la instalación:

```bash
otree --version
```

Deberías ver algo como `5.11.1` (la versión estable actual).

### Crear y ejecutar tu primer proyecto oTree

**Paso 1:** Crea un nuevo proyecto:

```bash
otree startproject mi_experimento
```

Cuando pregunte si deseas incluir juegos de ejemplo, responde `y` (sí). Estos ejemplos son excelentes para aprender.

**Paso 2:** Entra a la carpeta del proyecto:

```bash
cd mi_experimento
```

**Paso 3:** Inicia el servidor de desarrollo:

```bash
otree devserver
```

**Paso 4:** Abre tu navegador en http://localhost:8000

Verás la interfaz de administración de oTree con los juegos de ejemplo disponibles.

**Para detener el servidor:** Presiona `Ctrl + C` en Terminal.

### Flujo de trabajo diario con oTree

Cada vez que quieras trabajar en tu proyecto:

```bash
# 1. Navega a tu carpeta de proyectos
cd ~/oTree-proyectos

# 2. Activa el ambiente virtual
source venv/bin/activate

# 3. Entra a tu proyecto
cd mi_experimento

# 4. Inicia el servidor
otree devserver

# 5. Cuando termines, desactiva el ambiente
deactivate
```

### Estructura de un proyecto oTree

Cuando creas un proyecto, oTree genera esta estructura:

```
mi_experimento/
├── _rooms/              → Configuración de salas de laboratorio
├── _static/             → Archivos CSS, JavaScript, imágenes
├── _templates/          → Plantillas HTML globales
├── settings.py          → Configuración principal del proyecto
├── requirements.txt     → Lista de dependencias
└── [carpetas de apps]   → Cada juego/experimento es una app
```

---

## Parte 5: Configuración de SSH con GitHub

SSH (Secure Shell) te permite conectarte a GitHub sin ingresar tu usuario y contraseña cada vez. Una vez configurado, puedes hacer push, pull y otras operaciones de Git de forma segura y automática.

### Caso HABITUAL: Configuración completa paso a paso

#### Paso 1: Verificar llaves SSH existentes

Primero, revisa si ya tienes llaves SSH:

```bash
ls -al ~/.ssh
```

Busca archivos como `id_ed25519.pub`, `id_rsa.pub`, o `id_ecdsa.pub`. Si existen, podrías reutilizarlos. Si ves el error "No such file or directory", no tienes llaves y debes crear una.

#### Paso 2: Generar una nueva llave SSH

GitHub recomienda el algoritmo **Ed25519** por su seguridad y eficiencia.

> *"If you are using a legacy system that doesn't support the Ed25519 algorithm, use RSA."*
> — Documentación de GitHub

```bash
ssh-keygen -t ed25519 -C "tu_correo@ejemplo.com"
```

Usa el **mismo correo** que tienes en tu cuenta de GitHub.

El sistema te preguntará:
1. **Ubicación del archivo:** Presiona `Enter` para aceptar la ubicación predeterminada (`~/.ssh/id_ed25519`)
2. **Passphrase:** Escribe una contraseña segura (se recomienda, aunque es opcional). La passphrase protege tu llave privada.

#### Paso 3: Iniciar el agente SSH

El agente SSH gestiona tus llaves y te evita escribir la passphrase repetidamente.

```bash
eval "$(ssh-agent -s)"
```

Verás algo como: `Agent pid 59566`

#### Paso 4: Crear el archivo de configuración SSH

Este archivo le dice a SSH cómo manejar conexiones a GitHub en macOS.

```bash
touch ~/.ssh/config
nano ~/.ssh/config
```

Agrega este contenido (el archivo de configuración **debe estar exactamente así**):

```text
Host github.com
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_ed25519
```

**Notas importantes:**
- `UseKeychain yes` guarda tu passphrase en el Llavero de macOS
- Si **no** pusiste passphrase a tu llave, omite la línea `UseKeychain yes`

Guarda y cierra (`Ctrl + O`, `Enter`, `Ctrl + X`).

#### Paso 5: Agregar tu llave al agente SSH

El comando varía según tu versión de macOS:

**macOS Monterey (12.0) o posterior:**
```bash
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```

**macOS Big Sur (11.0) o anterior:**
```bash
ssh-add -K ~/.ssh/id_ed25519
```

> *"The `--apple-use-keychain` option stores the passphrase in your keychain. In macOS versions prior to Monterey (12.0), the flag used the syntax `-K`."*
> — Documentación de GitHub

#### Paso 6: Copiar la llave pública al portapapeles

```bash
pbcopy < ~/.ssh/id_ed25519.pub
```

Este comando copia el contenido de tu llave pública. **Nunca compartas tu llave privada** (el archivo sin `.pub`).

#### Paso 7: Agregar la llave a tu cuenta de GitHub

1. Ve a https://github.com y asegúrate de haber iniciado sesión
2. Haz clic en tu **foto de perfil** → **Settings** (Configuración)
3. En el menú lateral, bajo "Access", selecciona **SSH and GPG keys**
4. Haz clic en **New SSH key**
5. En **Title**, escribe un nombre descriptivo (ej: "MacBook Pro Personal 2025")
6. En **Key type**, selecciona "Authentication Key"
7. En **Key**, pega la llave que copiaste (`Cmd + V`)
8. Haz clic en **Add SSH key**
9. Confirma con tu contraseña de GitHub si se solicita

#### Paso 8: Probar la conexión

```bash
ssh -T git@github.com
```

La primera vez verás una advertencia sobre la autenticidad del host. Escribe `yes` para continuar.

**Respuesta exitosa:**
```
Hi tu_usuario! You've successfully authenticated, but GitHub does not provide shell access.
```

¡Felicidades! SSH está configurado correctamente.

### Permisos correctos de archivos SSH

Si tienes problemas de permisos, asegúrate de que tus archivos tengan la configuración correcta:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
chmod 600 ~/.ssh/config
```

---

## ANEXO A: Casos RAROS y solución de problemas

### Git: Error "xcrun: error: invalid active developer path"

Este error aparece frecuentemente **después de actualizar macOS**, porque la actualización puede invalidar las herramientas de desarrollo.

```
xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools),
missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun
```

**Solución:**
```bash
xcode-select --install
```

Si persiste:
```bash
sudo xcode-select --reset
```

### Git: Conflicto entre versión de Apple y Homebrew

**Síntoma:** `git --version` muestra una versión antigua aunque instalaste una nueva con Homebrew.

**Diagnóstico:**
```bash
which -a git
# Mostrará todas las ubicaciones de git instaladas
```

**Solución:** Asegúrate de que Homebrew esté antes en el PATH. Agrega a `~/.zshrc`:

Para Apple Silicon:
```bash
export PATH="/opt/homebrew/bin:$PATH"
```

Para Intel:
```bash
export PATH="/usr/local/bin:$PATH"
```

### Python: Error "externally-managed-environment"

En macOS Sonoma (14+) y Python 3.12+, pip puede mostrar este error al intentar instalar paquetes globalmente.

**Solución:** **Siempre usa ambientes virtuales** (lo cual ya haces si sigues esta guía para oTree):

```bash
python3 -m venv mi_ambiente
source mi_ambiente/bin/activate
pip install paquete
```

### Python: Certificados SSL

Si ves errores de certificados SSL al usar pip o hacer requests HTTPS:

**Solución:** Ejecuta el instalador de certificados que viene con Python:

```bash
# Ajusta la versión según lo que tengas instalado
/Applications/Python\ 3.11/Install\ Certificates.command
```

### oTree: Error "This version of oTree is only compatible with..."

**Problema:** Intentaste instalar oTree con Python 3.12 o superior.

**Solución:** Instala Python 3.11 y crea un nuevo ambiente virtual:

```bash
# Si usas Homebrew
brew install python@3.11

# Crea ambiente virtual con esa versión específica
/opt/homebrew/opt/python@3.11/bin/python3.11 -m venv venv

# Activa y reinstala oTree
source venv/bin/activate
pip install otree
```

### oTree: Puerto 8000 ocupado

**Síntoma:** Error "Address already in use" al ejecutar `otree devserver`.

**Solución:**
```bash
# Encontrar qué proceso usa el puerto
lsof -i :8000

# Terminar ese proceso (reemplaza PID con el número que aparece)
kill -9 PID

# O usar un puerto diferente
otree devserver 8080
```

### SSH: Error "Permission denied (publickey)"

**Causas comunes y soluciones:**

1. **Llave no agregada al agent:**
```bash
ssh-add -l  # Lista las llaves cargadas
# Si está vacío:
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```

2. **Llave no agregada a GitHub:** Ve a GitHub Settings → SSH Keys y verifica que tu llave aparezca.

3. **Permisos incorrectos:**
```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
```

4. **Debug detallado:**
```bash
ssh -vT git@github.com
```

### SSH: Error "ssh-add: illegal option -- apple-use-keychain"

**Causa:** Estás usando una versión de `ssh-add` que no es la de Apple (posiblemente instalada por Homebrew).

**Solución:** Usa explícitamente la versión de Apple:

```bash
/usr/bin/ssh-add --apple-use-keychain ~/.ssh/id_ed25519
```

### VS Code: El comando 'code' no funciona después de reinstalar

**Solución:** Vuelve a instalarlo desde VS Code:
1. Abre VS Code
2. `Cmd + Shift + P`
3. Escribe "Shell Command: Install 'code' command in PATH"
4. Reinicia Terminal

**Alternativa manual** — agrega a `~/.zshrc`:
```bash
export PATH="$PATH:/Applications/Visual Studio Code.app/Contents/Resources/app/bin"
```

---

## ANEXO B: Diferencias Apple Silicon vs Intel

| Aspecto | Apple Silicon (M1/M2/M3/M4) | Intel |
|---------|----------------------------|-------|
| Homebrew ubicación | `/opt/homebrew` | `/usr/local` |
| Requiere configurar PATH para Homebrew | Sí | Generalmente no |
| Arquitectura reportada | `arm64` | `x86_64` |
| Python de python.org | Universal (funciona nativo) | Universal (funciona nativo) |
| VS Code versión recomendada | Universal o Apple Silicon | Universal o Intel |

### Verificar arquitectura de cualquier programa

```bash
file $(which python3)
# Mostrará si es arm64, x86_64, o universal
```

---

## ANEXO C: Referencia rápida de archivos de configuración en macOS

| Archivo | Propósito | Cuándo se carga |
|---------|-----------|-----------------|
| `~/.zshrc` | Configuración de zsh | Cada vez que abres Terminal |
| `~/.zprofile` | Variables de entorno | Al iniciar sesión |
| `~/.ssh/config` | Configuración de SSH | Al usar comandos SSH |
| `~/.gitconfig` | Configuración global de Git | Al usar comandos Git |

### Recargar configuración sin cerrar Terminal

```bash
source ~/.zshrc
```

---

## ANEXO D: Comandos útiles de referencia

### Git
```bash
git --version              # Ver versión
git config --list          # Ver configuración
git init                   # Inicializar repositorio
git clone URL              # Clonar repositorio
git status                 # Ver estado de cambios
git add .                  # Agregar todos los cambios
git commit -m "mensaje"    # Crear commit
git push                   # Subir cambios a GitHub
git pull                   # Bajar cambios de GitHub
```

### Python
```bash
python3 --version           # Ver versión
which python3               # Ubicación del ejecutable
python3 -m venv nombre      # Crear ambiente virtual
source nombre/bin/activate  # Activar ambiente virtual
deactivate                  # Desactivar ambiente virtual
pip install paquete         # Instalar paquete
pip list                    # Ver paquetes instalados
pip freeze > requirements.txt  # Exportar dependencias
```

### oTree
```bash
otree startproject nombre   # Crear proyecto
otree startapp nombre       # Crear aplicación
otree devserver             # Iniciar servidor desarrollo
otree devserver 8080        # Usar puerto alternativo
otree resetdb               # Reiniciar base de datos
```

### SSH
```bash
ssh-keygen -t ed25519 -C "email"  # Generar llave
eval "$(ssh-agent -s)"             # Iniciar agente
ssh-add --apple-use-keychain ~/.ssh/id_ed25519  # Agregar llave
ssh -T git@github.com              # Probar conexión
```

---

## Conclusión

Al completar esta guía tendrás un entorno de desarrollo profesional en tu Mac: **Git** para control de versiones, **Python 3.11** compatible con oTree, **Visual Studio Code** como editor de código, **oTree** para crear experimentos económicos, y **SSH** configurado para trabajar fluidamente con GitHub.

El punto más crítico a recordar es la **compatibilidad de versiones**: oTree requiere Python 3.7-3.11, y usar ambientes virtuales no es opcional sino esencial para evitar conflictos. Con esta configuración, puedes desarrollar, versionar y compartir tus experimentos de manera profesional y reproducible.

La próxima vez que necesites comenzar un nuevo proyecto, simplemente: crea una carpeta, crea un ambiente virtual con Python 3.11, actívalo, instala oTree, y ejecuta `otree startproject`. Todo el ecosistema que configuraste hoy seguirá funcionando.