
# Guía de instalación y verificación de herramientas

Este documento explica **qué es cada herramienta**, **cómo instalarla** y **cómo verificar** que todo quedó bien configurado para el taller.


## 0. Verificación rápida (después de instalar todo)

Cuando termines los pasos de instalación, abre una terminal y ejecuta:

```bash
git --version
python --version
otree --version
code --version
ssh -T git@github.com
````

**Resultados esperados (ejemplo):**

```text
git version 2.45.1
Python 3.11.6
otree 5.10.0
1.90.0
Hi tuUsuarioGitHub! You've successfully authenticated, but GitHub does not provide shell access.
```

Si todos los comandos responden algo razonable (sin errores), estás listo/a ✅

---

# 1. Git

Git es el sistema de control de versiones que usaremos para colaborar.

### 1.1 Instalación en Windows

1. Busca en internet **“Git for Windows”** (página oficial de Git).
2. Descarga el instalador `.exe`.
3. Durante la instalación, acepta los valores por defecto, salvo:

   * Deja marcada la opción que agrega Git al **PATH del sistema**.
4. Termina la instalación.

### 1.2 Instalación en macOS

* Opción A (recomendada):

  1. Abre la app **Terminal**.
  2. Escribe:

     ```bash
     git --version
     ```
  3. Si no está instalado, macOS ofrecerá instalar las herramientas de desarrollador de Xcode. Acepta.

* Opción B (si usas Homebrew):

  ```bash
  brew install git
  ```

### 1.3 Instalación en Linux (Ubuntu/Debian)

```bash
sudo apt update
sudo apt install git
```

### 1.4 Verificación

En cualquier sistema:

```bash
git --version
```

Debe mostrar algo como `git version 2.x.x`.

---

# 2. Python 3.8+

Python será el lenguaje base para oTree.

### 2.1 Windows

1. Busca **“Download Python”** (sitio oficial).
2. Descarga el instalador de la versión **3.x** (3.9, 3.10, 3.11, etc.).
3. MUY IMPORTANTE: en la primera pantalla, marca la casilla:

   * **“Add Python to PATH”**
4. Acepta e instala.

### 2.2 macOS

* Opción A: Instalar desde el sitio oficial de Python.
* Opción B (con Homebrew):

  ```bash
  brew install python
  ```

### 2.3 Linux (Ubuntu/Debian)

La mayoría ya trae Python 3 instalado. Para asegurarte:

```bash
sudo apt update
sudo apt install python3 python3-venv python3-pip
```

### 2.4 Verificación

```bash
python --version
```

En algunos sistemas puede ser:

```bash
python3 --version
```

Debes ver algo como `Python 3.10.x` o superior.

---

# 3. Visual Studio Code (VS Code)

Usaremos VS Code como editor de código.

### 3.1 Instalación

* Ve al sitio oficial de **Visual Studio Code**.
* Descarga la versión para tu sistema (Windows, macOS o Linux).
* Instala con las opciones por defecto.

### 3.2 Habilitar el comando `code` en la terminal

* En **macOS / Linux**:

  1. Abre VS Code.
  2. Ve a **View → Command Palette** (o `Ctrl+Shift+P` / `Cmd+Shift+P`).
  3. Escribe: `Shell Command: Install 'code' command in PATH` y ejecútalo.
  4. Cierra y vuelve a abrir la terminal.

* En **Windows**:

  * Si usaste el instalador estándar, normalmente ya queda configurado.
  * Si no funciona, revisa durante la instalación la opción **“Add to PATH”** o reinstala activándola.

### 3.3 Verificación

```bash
code --version
```

Debe mostrar algo como `1.xx.x`.

---

# 4. oTree 5.x

oTree es el framework que usaremos para el experimento (Public Goods Game).

### 4.1 Instalar oTree

```bash
pip install --upgrade pip
pip install "otree>=5,<6"
```

### 4.2 Verificación

```bash
otree --version
```

Debe mostrar algo como `5.x.x`.

---

# 5. Configurar SSH con GitHub

Esto sirve para que puedas hacer `git push` y `git pull` sin escribir usuario/contraseña cada vez.

## 5.1 Revisar si ya tienes una llave SSH (abre git bash)

```bash
ls ~/.ssh
```

Si ves archivos como `id_ed25519` o `id_rsa`, quizá ya tienes una llave.
Si no ves nada o no estás seguro, puedes crear una nueva sin problema.

---

## 5.2 Crear una nueva llave SSH

En la terminal:

```bash
ssh-keygen -t ed25519 -C "tu_correo@ejemplo.com"
```

* Cuando pregunte dónde guardar el archivo, presiona **Enter** para aceptar la ruta por defecto (`~/.ssh/id_ed25519`).
* Cuando pregunte por passphrase, puedes dejarlo vacío (Enter) o definir una contraseña (más seguro, pero un poco menos cómodo).

Esto generará 2 archivos:

* `~/.ssh/id_ed25519`  → **llave privada** (NO se comparte).
* `~/.ssh/id_ed25519.pub` → **llave pública** (sí se comparte con GitHub).

---

## 5.3 Agregar la llave al ssh-agent

En macOS / Linux:

```bash
# Iniciar el agente
eval "$(ssh-agent -s)"

# Agregar la llave (usa el nombre correcto si cambiaste el default)
ssh-add ~/.ssh/id_ed25519
```

En Windows (Git Bash):

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

---

## 5.4 Registrar la llave en GitHub

1. Copia el contenido de tu llave pública:

   ```bash
   cat ~/.ssh/id_ed25519.pub
   ```

   Copia TODO el texto que empieza con `ssh-ed25519` hasta el final.

2. Ve a GitHub:

   * Click en tu foto (arriba a la derecha) → **Settings**.
   * En la barra lateral: **SSH and GPG keys**.
   * Botón **“New SSH key”**.
   * Pega el contenido de `id_ed25519.pub`.
   * Ponle un título descriptivo, por ejemplo: `Laptop CIDE`.
   * Guarda.

---

## 5.5 Probar la conexión con GitHub

Ahora sí, ejecuta:

```bash
ssh -T git@github.com
```

La primera vez te preguntará algo como:

```text
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Escribe:

```text
yes
```

Si todo está bien, deberías ver algo similar a:

```text
Hi tuUsuarioGitHub! You've successfully authenticated, but GitHub does not provide shell access.
```

Eso significa que tu configuración SSH está correcta ✅

---

## 6. Resumen final

Si todos estos comandos funcionan:

```bash
git --version
python --version
otree --version
code --version
ssh -T git@github.com
```

…entonces ya tienes todo lo necesario para:

* Clonar el repositorio del taller.
* Abrirlo en VS Code.
* Correr oTree (`otree devserver`).
* Colaborar usando Git y GitHub con SSH.

A partir de aquí, puedes seguir con la sección **“PARTE 1: SETUP INICIAL”** del taller.

```
```

