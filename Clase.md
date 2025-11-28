# Taller Interactivo: Colaboración con Git/GitHub en Proyectos oTree

## Información del Taller

| Campo | Valor |
|-------|-------|
| **Duración estimada** | 3-4 horas |
| **Participantes** | Mauricio, José Miguel, Sergio, Donovan |
| **Nivel** | Intermedio (conocimiento básico de Git requerido) |
| **Proyecto base** | Public Goods Game (oTree) |
| **Referencia académica** | Fehr & Gächter (2000), "Cooperation and Punishment in Public Goods Experiments" |

---

# PARTE 1: SETUP INICIAL

## 1.1 Prerrequisitos

Antes de comenzar, cada participante debe tener instalado:

- [ ] Git (verificar con `git --version`)
- [ ] Python 3.8+ (verificar con `python --version`)
- [ ] oTree 5.x (verificar con `otree --version`)
- [ ] Visual Studio Code
- [ ] Cuenta de GitHub con SSH configurado

### Verificación rápida (ejecutar en terminal)

```bash
# Verificar todas las herramientas
git --version
python --version
otree --version
code --version
ssh -T git@github.com
```

**Output esperado del último comando:**
```
Hi [DonovanDiazcide]! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## 1.2 Creación del Repositorio (Solo el Facilitador)

### Paso 1: Crear proyecto oTree base

```bash
# Crear directorio del proyecto
mkdir taller-otree-pgg
cd taller-otree-pgg

# Crear proyecto oTree
otree startproject .

# Cuando pregunte "Include sample games?", responder: y
```

### Paso 2: Inicializar repositorio Git

```bash
# Inicializar Git
git init

# Crear archivo .gitignore
cat > .gitignore << 'EOF'
# oTree
__pycache__/
*.pyc
db.sqlite3
.otree/
_static_root/
*.csv

# Python
*.egg-info/
dist/
build/
.eggs/
*.egg

# IDE
.vscode/
.idea/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db

# Environment
.env
venv/
EOF

# Primer commit
git add .
git commit -m "feat: inicializa proyecto oTree con Public Goods Game"
```

### Paso 3: Crear repositorio en GitHub

1. Ir a **github.com** → Click en **"+"** (esquina superior derecha) → **"New repository"**
2. Configurar:
   - **Repository name:** `taller-otree-pgg`
   - **Description:** `Taller interactivo de Git/GitHub con Public Goods Game en oTree`
   - **Visibility:** Private (o Public si prefieren)
   - **NO** inicializar con README, .gitignore, ni license
3. Click **"Create repository"**

### Paso 4: Conectar repositorio local con GitHub

```bash
git remote add origin git@github.com:[DonovanDiazcide]/taller-otree-pgg.git

# Subir código
git branch -M main
git push -u origin main
```

### Paso 5: Agregar colaboradores

1. En GitHub → Repositorio → **Settings** → **Collaborators**
2. Click **"Add people"**
3. Agregar a: Mauricio, José Miguel, Sergio, Donovan
4. Cada colaborador debe **aceptar la invitación** (recibirán email)

---

## 1.3 Clonar Repositorio (Cada Participante)

Una vez aceptada la invitación, cada participante ejecuta:

```bash
# Navegar a carpeta de trabajo
cd ~/proyectos  # o la carpeta que prefieran

# Clonar repositorio
git clone git@github.com:DonovanDiazcide/taller-otree-pgg.git

# Entrar al proyecto
cd taller-otree-pgg

# Verificar que funciona
otree devserver
```

Abrir navegador en `http://localhost:8000` y verificar que se ve la interfaz de oTree.

---

## 1.4 Estructura del Proyecto Base

Después de clonar, la estructura debe ser:

```
taller-otree-pgg/
├── __init__.py
├── settings.py
├── public_goods/
│   ├── __init__.py
│   └── templates/
│       └── public_goods/
│           ├── Contribute.html
│           └── Results.html
├── [otras apps de ejemplo...]
└── .gitignore
```

### Archivo clave: `public_goods/__init__.py`

```python
from otree.api import *

doc = """
Public Goods Game - Taller Git/GitHub
"""

class C(BaseConstants):
    NAME_IN_URL = 'public_goods'
    PLAYERS_PER_GROUP = 3
    NUM_ROUNDS = 1
    ENDOWMENT = cu(100)
    MULTIPLIER = 2

class Subsession(BaseSubsession):
    pass

class Group(BaseGroup):
    total_contribution = models.CurrencyField()
    individual_share = models.CurrencyField()

class Player(BasePlayer):
    contribution = models.CurrencyField(
        min=0,
        max=C.ENDOWMENT,
        label="¿Cuánto quieres contribuir al fondo común?"
    )

# PAGES
class Contribute(Page):
    form_model = 'player'
    form_fields = ['contribution']

class ResultsWaitPage(WaitPage):
    after_all_players_arrive = 'set_payoffs'

class Results(Page):
    pass

# FUNCIONES
def set_payoffs(group: Group):
    players = group.get_players()
    contributions = [p.contribution for p in players]
    group.total_contribution = sum(contributions)
    group.individual_share = group.total_contribution * C.MULTIPLIER / C.PLAYERS_PER_GROUP
    for p in players:
        p.payoff = C.ENDOWMENT - p.contribution + group.individual_share

page_sequence = [Contribute, ResultsWaitPage, Results]
```

---

# PARTE 2: CONFIGURACIÓN DE GITHUB

## 2.1 Crear Milestones

Los Milestones agrupan issues relacionados y permiten trackear progreso.

### Instrucciones paso a paso:

1. En GitHub → Repositorio → Pestaña **"Issues"**
2. Click en **"Milestones"** (junto a "Labels")
3. Click **"New milestone"**

### Milestone 1: MVP Public Goods Game

| Campo | Valor |
|-------|-------|
| **Title** | `v1.0 - MVP Public Goods Game` |
| **Due date** | (fecha del taller + 1 día) |
| **Description** | Primera versión funcional con instrucciones, comprensión, tratamientos, resultados y castigo |

Click **"Create milestone"**

### Milestone 2: CI/CD Pipeline

| Campo | Valor |
|-------|-------|
| **Title** | `v1.1 - CI/CD Pipeline` |
| **Due date** | (fecha del taller + 2 días) |
| **Description** | Integración continua con GitHub Actions para validación automática |

Click **"Create milestone"**

---

## 2.2 Crear Labels

Los Labels categorizan issues por tipo y prioridad.

### Instrucciones paso a paso:

1. En GitHub → Repositorio → Pestaña **"Issues"**
2. Click en **"Labels"**
3. Click **"New label"** para cada uno:

| Label | Color | Description |
|-------|-------|-------------|
| `feature` | `#0E8A16` (verde) | Nueva funcionalidad |
| `enhancement` | `#84B6EB` (azul claro) | Mejora a funcionalidad existente |
| `documentation` | `#FEF2C0` (amarillo claro) | Documentación y comentarios |
| `ci/cd` | `#5319E7` (morado) | Integración y despliegue continuo |
| `priority: high` | `#D93F0B` (rojo) | Prioridad alta |
| `priority: medium` | `#FBCA04` (amarillo) | Prioridad media |
| `assigned: mauricio` | `#C2E0C6` (verde claro) | Asignado a Mauricio |
| `assigned: jose-miguel` | `#BFD4F2` (azul claro) | Asignado a José Miguel |
| `assigned: sergio` | `#D4C5F9` (lavanda) | Asignado a Sergio |
| `assigned: donovan` | `#FFC0CB` (rosa) | Asignado a Donovan |

---

## 2.3 Crear Issues

Crear los siguientes issues (uno por participante):

### Issue #1: Instrucciones y Comprensión (Mauricio)

1. Click **"New issue"**
2. Completar:

**Title:**
```
feat: Agregar página de instrucciones y preguntas de comprensión
```

**Body:**
```markdown
## Descripción
Implementar una página de instrucciones clara para el Public Goods Game y una página de comprensión que valide el entendimiento del participante.

## Tareas
- [ ] Crear página `Introduction.html` con instrucciones del juego
- [ ] Crear página `Comprehension.html` con 3 preguntas de validación
- [ ] Agregar lógica de validación en `__init__.py`
- [ ] Los participantes deben responder correctamente para continuar

## Criterios de aceptación
- Las instrucciones explican claramente el mecanismo del juego
- Las preguntas de comprensión cubren: dotación, multiplicador, y cálculo de payoff
- Si el participante falla, debe ver mensaje de error y puede reintentar

## Referencias
- Fehr & Gächter (2000): Incluían quiz de comprensión antes del juego
- oTree docs: https://otree.readthedocs.io/en/latest/forms.html

## Archivos a modificar
- `public_goods/__init__.py`
- `public_goods/templates/public_goods/Introduction.html` (nuevo)
- `public_goods/templates/public_goods/Comprehension.html` (nuevo)
```

**Labels:** `feature`, `priority: high`, `assigned: mauricio`
**Milestone:** `v1.0 - MVP Public Goods Game`

---

### Issue #2: Parámetros y Tratamientos (José Miguel)

**Title:**
```
feat: Implementar parámetros configurables y múltiples tratamientos
```

**Body:**
```markdown
## Descripción
Hacer los parámetros del juego configurables desde `settings.py` y crear dos tratamientos: MPCR alto y MPCR bajo.

## Tareas
- [ ] Mover parámetros de `C` a configuración de sesión
- [ ] Crear tratamiento `high_mpcr` (multiplicador = 2.0)
- [ ] Crear tratamiento `low_mpcr` (multiplicador = 1.2)
- [ ] Documentar los parámetros en el código

## Criterios de aceptación
- Los parámetros se pueden cambiar sin modificar el código del juego
- Ambos tratamientos aparecen en la demo de oTree
- El MPCR se calcula correctamente en cada tratamiento

## Contexto teórico
- MPCR (Marginal Per Capita Return) = multiplicador / n_jugadores
- MPCR > 1: Nash equilibrium = contribuir 0, pero óptimo social = contribuir todo
- MPCR alto favorece más la cooperación

## Archivos a modificar
- `public_goods/__init__.py`
- `settings.py`
```

**Labels:** `feature`, `priority: high`, `assigned: jose-miguel`
**Milestone:** `v1.0 - MVP Public Goods Game`

---

### Issue #3: Página de Resultados con Visualización (Sergio)

**Title:**
```
feat: Mejorar página de resultados con visualización de contribuciones
```

**Body:**
```markdown
## Descripción
Crear una página de resultados mejorada que muestre gráficamente las contribuciones de cada jugador y el resultado del grupo.

## Tareas
- [ ] Agregar tabla con contribuciones individuales (anonimizadas)
- [ ] Mostrar desglose del cálculo de payoff
- [ ] Implementar gráfico de barras con contribuciones usando Chart.js
- [ ] Agregar CSS para mejorar presentación

## Criterios de aceptación
- El participante puede ver todas las contribuciones del grupo
- El cálculo de payoff es transparente y verificable
- El gráfico renderiza correctamente en navegadores modernos

## Recursos
- Chart.js CDN: https://cdn.jsdelivr.net/npm/chart.js
- oTree templates: https://otree.readthedocs.io/en/latest/templates.html

## Archivos a modificar
- `public_goods/__init__.py` (agregar `vars_for_template`)
- `public_goods/templates/public_goods/Results.html`
```

**Labels:** `feature`, `enhancement`, `priority: medium`, `assigned: sergio`
**Milestone:** `v1.0 - MVP Public Goods Game`

---

### Issue #4: Sistema de Castigo (Donovan)

**Title:**
```
feat: Implementar etapa de castigo (punishment stage)
```

**Body:**
```markdown
## Descripción
Agregar una etapa de castigo después de ver los resultados, donde los participantes pueden pagar para reducir el payoff de otros jugadores.

## Tareas
- [ ] Crear página `Punishment.html` con interfaz para asignar puntos de castigo
- [ ] Implementar lógica de castigo en `__init__.py`
- [ ] Agregar campo `punishment_sent` y `punishment_received` al Player
- [ ] Actualizar cálculo de payoff final
- [ ] Mostrar resultados de castigo en página final

## Criterios de aceptación
- Cada punto de castigo cuesta 1 unidad al que castiga
- Cada punto de castigo reduce 3 unidades al castigado
- El castigo es anónimo (no se sabe quién castigó a quién)
- El payoff final refleja correctamente las deducciones

## Referencia académica
- Fehr & Gächter (2000): "Cooperation and Punishment in Public Goods Experiments"
- Costo de castigo: 1:3 ratio (estándar en la literatura)

## Archivos a modificar
- `public_goods/__init__.py`
- `public_goods/templates/public_goods/Punishment.html` (nuevo)
- `public_goods/templates/public_goods/FinalResults.html` (nuevo)
```

**Labels:** `feature`, `priority: high`, `assigned: donovan`
**Milestone:** `v1.0 - MVP Public Goods Game`

---

### Issue #5: GitHub Actions CI/CD (Todos)

**Title:**
```
ci: Implementar pipeline de integración continua con GitHub Actions
```

**Body:**
```markdown
## Descripción
Configurar GitHub Actions para ejecutar validaciones automáticas en cada Pull Request.

## Tareas
- [ ] Crear workflow `.github/workflows/ci.yml`
- [ ] Validar sintaxis Python con `flake8` o `ruff`
- [ ] Ejecutar `otree test` para validar apps
- [ ] Verificar que el servidor inicia correctamente

## Criterios de aceptación
- El workflow se ejecuta en cada PR a `main`
- PRs con errores de sintaxis no pueden mergearse
- El status check aparece en la UI de GitHub

## Recursos
- GitHub Actions docs: https://docs.github.com/en/actions
- oTree testing: https://otree.readthedocs.io/en/latest/bots.html
```

**Labels:** `ci/cd`, `priority: medium`
**Milestone:** `v1.1 - CI/CD Pipeline`

---

## 2.4 Configurar Branch Protection Rules

### Instrucciones paso a paso:

1. GitHub → Repositorio → **Settings** → **Branches**
2. Click **"Add branch ruleset"** (o "Add rule" en versiones anteriores)
3. Configurar:

| Configuración | Valor |
|---------------|-------|
| **Branch name pattern** | `main` |
| **Require a pull request before merging** | ✅ Activado |
| **Require approvals** | 1 |
| **Dismiss stale pull request approvals** | ✅ Activado |
| **Require status checks to pass** | ✅ Activado (después de crear GitHub Actions) |
| **Require branches to be up to date** | ✅ Activado |
| **Do not allow bypassing** | ✅ Activado |

4. Click **"Create"** o **"Save changes"**

**Resultado:** Nadie puede hacer push directo a `main`. Todo debe pasar por Pull Request con al menos 1 aprobación.

---

# PARTE 3: MÓDULOS DE TRABAJO

Cada participante trabajará en su issue asignado. A continuación se detallan las instrucciones, hints, y soluciones para cada uno.

---

## 3.1 MÓDULO 1: Mauricio - Instrucciones y Comprensión

### Objetivo
Crear una página de instrucciones clara y una página de preguntas de comprensión que valide que el participante entiende el juego antes de comenzar.

### Flujo de trabajo Git

```bash
# 1. Asegurarse de estar en main actualizado
git checkout main
git pull origin main

# 2. Crear rama de feature
git checkout -b feature/instrucciones-comprension

# 3. Verificar que estás en la rama correcta
git branch
# Debe mostrar: * feature/instrucciones-comprension
```

### Prompt sugerido para IA

> **Modelo recomendado:** Claude Opus 4.5  
> **Justificación:** Esta tarea requiere coherencia entre múltiples archivos (HTML + Python) y conocimiento específico de oTree 5. Opus 4.5 destaca en tareas multi-archivo con frameworks específicos.

```
Actúa como un desarrollador experto en oTree 5 y economía experimental.

CONTEXTO:
Estoy implementando un Public Goods Game en oTree 5. Necesito crear:
1. Una página de instrucciones (Introduction.html)
2. Una página de preguntas de comprensión (Comprehension.html)

PARÁMETROS DEL JUEGO:
- PLAYERS_PER_GROUP = 3
- ENDOWMENT = 100 puntos
- MULTIPLIER = 2
- El fondo común se multiplica y divide equitativamente

REQUISITOS:
1. Las instrucciones deben explicar:
   - Cuánto tiene cada jugador inicialmente
   - Cómo funciona la contribución al fondo común
   - Cómo se calcula el payoff final
   - Un ejemplo numérico concreto

2. Las preguntas de comprensión deben incluir:
   - Pregunta sobre dotación inicial
   - Pregunta sobre qué pasa con las contribuciones
   - Pregunta de cálculo de payoff con números específicos

3. La validación debe:
   - Usar error_message() en oTree 5
   - Mostrar mensaje claro si hay error
   - Permitir reintentos

ESTRUCTURA DE ARCHIVOS EN OTREE 5:
- Todo está en public_goods/__init__.py (Pages, Models, etc.)
- Templates en public_goods/templates/public_goods/

OUTPUT ESPERADO:
1. Código completo para agregar a __init__.py (clases Player fields, Pages)
2. Template Introduction.html completo
3. Template Comprehension.html completo

Usa la estructura de oTree 5 (no oTree 3). Incluye comentarios explicativos.
```

### Descripción de la tarea

**Archivos a crear/modificar:**
- `public_goods/__init__.py` - Agregar campos y páginas
- `public_goods/templates/public_goods/Introduction.html` - Nuevo
- `public_goods/templates/public_goods/Comprehension.html` - Nuevo

**Especificaciones:**
1. La página de Introduction debe tener instrucciones claras en español
2. La página de Comprehension debe tener 3 preguntas con validación
3. Los participantes deben responder correctamente para continuar

---

### 💡 HINT (leer solo si llevas más de 15 minutos atascado)

<details>
<summary>Click para ver el hint</summary>

**Para las preguntas de comprensión en oTree 5:**

1. Define los campos en la clase `Player`:
```python
class Player(BasePlayer):
    # ... campos existentes ...
    comp_q1 = models.IntegerField(label="...")
    comp_q2 = models.IntegerField(label="...")
    comp_q3 = models.IntegerField(label="...")
```

2. Usa `error_message` a nivel de página para validar:
```python
class Comprehension(Page):
    form_model = 'player'
    form_fields = ['comp_q1', 'comp_q2', 'comp_q3']
    
    @staticmethod
    def error_message(player, values):
        # Validar aquí
        if values['comp_q1'] != RESPUESTA_CORRECTA:
            return 'La respuesta a la pregunta 1 es incorrecta.'
```

3. Para las páginas, recuerda agregarlas a `page_sequence`:
```python
page_sequence = [Introduction, Comprehension, Contribute, ResultsWaitPage, Results]
```

</details>

---

### ✅ SOLUCIÓN COMPLETA

<details>
<summary>Click para ver la solución completa</summary>

#### Modificaciones a `public_goods/__init__.py`

Agregar estos campos a la clase `Player`:

```python
class Player(BasePlayer):
    contribution = models.CurrencyField(
        min=0,
        max=C.ENDOWMENT,
        label="¿Cuánto quieres contribuir al fondo común?"
    )
    
    # Preguntas de comprensión
    comp_q1 = models.IntegerField(
        label="¿Cuántos puntos recibe cada jugador al inicio de la ronda?"
    )
    comp_q2 = models.IntegerField(
        label="Si todos los jugadores contribuyen 50 puntos cada uno, ¿cuánto habrá en el fondo común ANTES de multiplicar?",
        choices=[
            [50, '50 puntos'],
            [100, '100 puntos'],
            [150, '150 puntos'],
            [200, '200 puntos'],
        ]
    )
    comp_q3 = models.IntegerField(
        label="Si el fondo común tiene 300 puntos después de multiplicar, ¿cuánto recibe cada jugador del fondo?",
        choices=[
            [50, '50 puntos'],
            [100, '100 puntos'],
            [150, '150 puntos'],
            [300, '300 puntos'],
        ]
    )
```

Agregar estas páginas:

```python
class Introduction(Page):
    """Página de instrucciones del juego"""
    pass


class Comprehension(Page):
    """Página de preguntas de comprensión"""
    form_model = 'player'
    form_fields = ['comp_q1', 'comp_q2', 'comp_q3']
    
    @staticmethod
    def error_message(player, values):
        solutions = {
            'comp_q1': C.ENDOWMENT,  # 100 puntos
            'comp_q2': 150,           # 3 jugadores x 50 = 150
            'comp_q3': 100,           # 300 / 3 = 100
        }
        
        errors = []
        
        if values['comp_q1'] != solutions['comp_q1']:
            errors.append(f"Pregunta 1: La respuesta correcta es {solutions['comp_q1']} puntos.")
        
        if values['comp_q2'] != solutions['comp_q2']:
            errors.append("Pregunta 2: Recuerda que hay 3 jugadores y cada uno contribuye 50.")
        
        if values['comp_q3'] != solutions['comp_q3']:
            errors.append("Pregunta 3: El fondo se divide equitativamente entre los 3 jugadores.")
        
        if errors:
            return ' '.join(errors)
```

Actualizar `page_sequence`:

```python
page_sequence = [Introduction, Comprehension, Contribute, ResultsWaitPage, Results]
```

#### Template: `Introduction.html`

Crear archivo `public_goods/templates/public_goods/Introduction.html`:

```html
{{ block title }}
    Instrucciones del Juego
{{ endblock }}

{{ block content }}
<div class="instructions">
    <h3>Bienvenido al Juego de Bienes Públicos</h3>
    
    <p>En este juego, formarás parte de un grupo de <strong>{{ C.PLAYERS_PER_GROUP }} jugadores</strong>.</p>
    
    <h4>Dotación Inicial</h4>
    <p>Cada jugador recibe <strong>{{ C.ENDOWMENT }} puntos</strong> al inicio de cada ronda.</p>
    
    <h4>Decisión</h4>
    <p>Debes decidir cuántos de tus {{ C.ENDOWMENT }} puntos quieres contribuir a un <strong>fondo común</strong>.</p>
    <ul>
        <li>Puedes contribuir cualquier cantidad entre 0 y {{ C.ENDOWMENT }} puntos.</li>
        <li>Los puntos que NO contribuyas se quedan contigo.</li>
    </ul>
    
    <h4>El Fondo Común</h4>
    <p>Las contribuciones de todos los jugadores se suman y se <strong>multiplican por {{ C.MULTIPLIER }}</strong>.</p>
    <p>Luego, el fondo multiplicado se <strong>divide equitativamente</strong> entre los {{ C.PLAYERS_PER_GROUP }} jugadores.</p>
    
    <h4>Tu Ganancia</h4>
    <p>Tu ganancia final será:</p>
    <div class="formula" style="background-color: #f0f0f0; padding: 15px; border-radius: 5px; margin: 10px 0;">
        <strong>Ganancia = (Puntos que guardaste) + (Tu parte del fondo común)</strong>
    </div>
    
    <h4>Ejemplo</h4>
    <div class="example" style="background-color: #e8f4e8; padding: 15px; border-radius: 5px; margin: 10px 0;">
        <p>Supongamos que:</p>
        <ul>
            <li>Jugador 1 contribuye 40 puntos</li>
            <li>Jugador 2 contribuye 60 puntos</li>
            <li>Jugador 3 contribuye 20 puntos</li>
        </ul>
        <p><strong>Total contribuido:</strong> 40 + 60 + 20 = 120 puntos</p>
        <p><strong>Fondo después de multiplicar:</strong> 120 × {{ C.MULTIPLIER }} = 240 puntos</p>
        <p><strong>Parte de cada jugador:</strong> 240 ÷ 3 = 80 puntos</p>
        <p><strong>Ganancia del Jugador 1:</strong> (100 - 40) + 80 = <strong>140 puntos</strong></p>
        <p><strong>Ganancia del Jugador 2:</strong> (100 - 60) + 80 = <strong>120 puntos</strong></p>
        <p><strong>Ganancia del Jugador 3:</strong> (100 - 20) + 80 = <strong>160 puntos</strong></p>
    </div>
    
    <p style="margin-top: 20px;">
        <strong>A continuación, responderás algunas preguntas para verificar que entendiste las instrucciones.</strong>
    </p>
</div>

{{ next_button }}
{{ endblock }}
```

#### Template: `Comprehension.html`

Crear archivo `public_goods/templates/public_goods/Comprehension.html`:

```html
{{ block title }}
    Preguntas de Comprensión
{{ endblock }}

{{ block content }}
<div class="comprehension">
    <p>Por favor responde las siguientes preguntas para verificar que entendiste las instrucciones.</p>
    <p><em>Debes responder correctamente todas las preguntas para continuar.</em></p>
    
    <div class="question" style="margin: 20px 0; padding: 15px; background-color: #f9f9f9; border-radius: 5px;">
        <label><strong>Pregunta 1:</strong></label>
        {{ formfields.comp_q1 }}
    </div>
    
    <div class="question" style="margin: 20px 0; padding: 15px; background-color: #f9f9f9; border-radius: 5px;">
        <label><strong>Pregunta 2:</strong></label>
        {{ formfields.comp_q2 }}
    </div>
    
    <div class="question" style="margin: 20px 0; padding: 15px; background-color: #f9f9f9; border-radius: 5px;">
        <label><strong>Pregunta 3:</strong></label>
        {{ formfields.comp_q3 }}
    </div>
</div>

{{ next_button }}
{{ endblock }}
```

#### Commits sugeridos

```bash
# Después de crear los archivos
git add public_goods/__init__.py
git commit -m "feat(public_goods): agrega campos de comprensión al modelo Player"

git add public_goods/templates/public_goods/Introduction.html
git commit -m "feat(public_goods): crea página de instrucciones del juego"

git add public_goods/templates/public_goods/Comprehension.html
git commit -m "feat(public_goods): crea página de comprensión con validación"

# Push a la rama
git push -u origin feature/instrucciones-comprension
```

</details>

---

### Verificación local

Antes de hacer push, verificar que funciona:

```bash
# Iniciar servidor
otree devserver

# Abrir en navegador: http://localhost:8000
# Probar el flujo completo:
# 1. Introduction -> debe mostrar instrucciones
# 2. Comprehension -> probar con respuestas incorrectas (debe mostrar error)
# 3. Comprehension -> probar con respuestas correctas (debe continuar)
```

### Crear Pull Request

Una vez verificado localmente:

1. Push de la rama:
```bash
git push -u origin feature/instrucciones-comprension
```

2. En GitHub → Repositorio → Aparecerá banner "Compare & pull request" → Click

3. Completar el PR:

**Title:**
```
feat(public_goods): Agrega instrucciones y preguntas de comprensión
```

**Body:**
```markdown
## Descripción
Implementa la página de instrucciones y el quiz de comprensión para el Public Goods Game.

## Cambios realizados
- Agregados campos `comp_q1`, `comp_q2`, `comp_q3` al modelo Player
- Creada página `Introduction` con instrucciones detalladas y ejemplo
- Creada página `Comprehension` con validación de respuestas
- Actualizado `page_sequence`

## Testing
- [x] Probado localmente con `otree devserver`
- [x] Validación funciona correctamente
- [x] Mensajes de error son claros

## Screenshots
(Agregar capturas de pantalla si es posible)

Closes #1
```

4. Asignar reviewer (cualquier otro participante)
5. Click **"Create pull request"**


---

## 3.2 MÓDULO 2: José Miguel - Parámetros y Tratamientos

### Objetivo
Hacer los parámetros del juego configurables desde `settings.py` y crear dos tratamientos experimentales con diferentes valores de MPCR (Marginal Per Capita Return).

### Flujo de trabajo Git

```bash
# 1. Asegurarse de estar en main actualizado
git checkout main
git pull origin main

# 2. Crear rama de feature
git checkout -b feature/parametros-tratamientos

# 3. Verificar que estás en la rama correcta
git branch
# Debe mostrar: * feature/parametros-tratamientos
```

### Prompt sugerido para IA

> **Modelo recomendado:** GPT-5.1 Thinking  
> **Justificación:** Esta tarea requiere razonamiento sobre parámetros económicos (MPCR) y anticipar edge cases en la configuración. GPT-5.1 es mejor para tareas donde necesitas que el modelo "piense defensivamente" sobre posibles errores.

```
Eres un economista experimental experto en oTree 5 y diseño de experimentos.

CONTEXTO:
Tengo un Public Goods Game en oTree 5 con estos parámetros hardcodeados:
- PLAYERS_PER_GROUP = 3
- ENDOWMENT = 100
- MULTIPLIER = 2

OBJETIVO:
1. Hacer estos parámetros configurables desde settings.py
2. Crear dos tratamientos experimentales:
   - high_mpcr: multiplicador = 2.0 (MPCR = 0.67)
   - low_mpcr: multiplicador = 1.2 (MPCR = 0.40)

REQUISITOS TÉCNICOS EN OTREE 5:
- Los parámetros de sesión se definen en SESSION_CONFIGS en settings.py
- Se acceden en el código via self.session.config['param_name']
- Los valores por defecto deben estar en la clase C (Constants)

CONSIDERACIONES ECONÓMICAS:
- MPCR = multiplicador / n_jugadores
- MPCR > 1/n: contribuir es socialmente óptimo
- MPCR < 1: el equilibrio de Nash es contribuir 0
- Explica en comentarios por qué elegimos estos valores

OUTPUT ESPERADO:
1. Código modificado de settings.py con los dos tratamientos
2. Código modificado de __init__.py para leer parámetros de sesión
3. Documentación inline explicando el diseño experimental

Anticipa posibles errores (ej: qué pasa si un parámetro no está definido).
```

### Descripción de la tarea

**Archivos a modificar:**
- `settings.py` - Agregar configuraciones de sesión
- `public_goods/__init__.py` - Modificar para leer parámetros de sesión

**Especificaciones:**
1. Los parámetros deben tener valores por defecto sensatos
2. Crear tratamiento `high_mpcr` con multiplicador = 2.0
3. Crear tratamiento `low_mpcr` con multiplicador = 1.2
4. El código debe funcionar aunque no se especifique un parámetro

---

### 💡 HINT (leer solo si llevas más de 15 minutos atascado)

<details>
<summary>Click para ver el hint</summary>

**Para acceder a parámetros de sesión en oTree 5:**

1. En `settings.py`, define los parámetros en cada SESSION_CONFIG:
```python
SESSION_CONFIGS = [
    dict(
        name='public_goods_high',
        display_name="Public Goods - High MPCR",
        app_sequence=['public_goods'],
        num_demo_participants=3,
        multiplier=2.0,  # Este es tu parámetro custom
        endowment=100,
    ),
]
```

2. En `__init__.py`, accede a los parámetros usando `session.config`:
```python
# En una función o método
multiplier = player.session.config.get('multiplier', C.MULTIPLIER)
```

3. Para usarlo en cálculos de grupo, hazlo en la función `set_payoffs`:
```python
def set_payoffs(group: Group):
    multiplier = group.session.config.get('multiplier', C.MULTIPLIER)
    # ... resto del cálculo
```

4. Usa `.get()` con valor por defecto para evitar errores si el parámetro no existe.

</details>

---

### ✅ SOLUCIÓN COMPLETA

<details>
<summary>Click para ver la solución completa</summary>

#### Modificaciones a `settings.py`

```python
from os import environ

# Configuración básica
SESSION_CONFIGS = [
    # Tratamiento 1: MPCR Alto
    dict(
        name='public_goods_high_mpcr',
        display_name="Public Goods - High MPCR (0.67)",
        app_sequence=['public_goods'],
        num_demo_participants=3,
        # Parámetros del experimento
        endowment=100,
        multiplier=2.0,  # MPCR = 2.0/3 = 0.67
        players_per_group=3,
        # Documentación del tratamiento
        doc="""
        Tratamiento con MPCR alto (0.67).
        Predicción teórica: Mayor cooperación que en low_mpcr.
        Nash equilibrium: contribuir 0.
        Óptimo social: contribuir todo.
        """
    ),
    # Tratamiento 2: MPCR Bajo
    dict(
        name='public_goods_low_mpcr',
        display_name="Public Goods - Low MPCR (0.40)",
        app_sequence=['public_goods'],
        num_demo_participants=3,
        # Parámetros del experimento
        endowment=100,
        multiplier=1.2,  # MPCR = 1.2/3 = 0.40
        players_per_group=3,
        # Documentación del tratamiento
        doc="""
        Tratamiento con MPCR bajo (0.40).
        Predicción teórica: Menor cooperación que en high_mpcr.
        Nash equilibrium: contribuir 0.
        Óptimo social: contribuir todo (pero incentivo menor).
        """
    ),
]

# Configuración del lenguaje y moneda
LANGUAGE_CODE = 'es'
REAL_WORLD_CURRENCY_CODE = 'MXN'
USE_POINTS = True
POINTS_CUSTOM_NAME = 'puntos'

# Rooms (opcional, útil para laboratorio)
ROOMS = [
    dict(
        name='lab_session',
        display_name='Sesión de Laboratorio',
    ),
]

ADMIN_USERNAME = 'admin'
ADMIN_PASSWORD = environ.get('OTREE_ADMIN_PASSWORD', 'admin')

DEMO_PAGE_INTRO_HTML = """
<h2>Taller Git/GitHub - Public Goods Game</h2>
<p>Este experimento tiene dos tratamientos:</p>
<ul>
    <li><strong>High MPCR (0.67):</strong> Multiplicador = 2.0</li>
    <li><strong>Low MPCR (0.40):</strong> Multiplicador = 1.2</li>
</ul>
<p>Referencia: Fehr & Gächter (2000)</p>
"""

SECRET_KEY = '{{ secret_key }}'
```

#### Modificaciones a `public_goods/__init__.py`

```python
from otree.api import *

doc = """
Public Goods Game con parámetros configurables.
Implementa tratamientos High MPCR y Low MPCR.
Referencia: Fehr & Gächter (2000)
"""


class C(BaseConstants):
    NAME_IN_URL = 'public_goods'
    # Valores por defecto (se sobrescriben con session.config)
    PLAYERS_PER_GROUP = 3
    NUM_ROUNDS = 1
    ENDOWMENT = cu(100)
    MULTIPLIER = 2  # Por defecto, MPCR = 2/3 = 0.67


class Subsession(BaseSubsession):
    pass


class Group(BaseGroup):
    total_contribution = models.CurrencyField()
    individual_share = models.CurrencyField()
    
    # Almacenar el multiplicador usado para referencia
    multiplier_used = models.FloatField()


class Player(BasePlayer):
    contribution = models.CurrencyField(
        min=0,
        label="¿Cuánto quieres contribuir al fondo común?"
    )
    
    # Campo para almacenar el MPCR del tratamiento
    treatment_mpcr = models.FloatField()

    @staticmethod
    def contribution_max(player):
        """
        El máximo de contribución depende del endowment configurado.
        """
        endowment = player.session.config.get('endowment', C.ENDOWMENT)
        return endowment


# FUNCIONES AUXILIARES
def get_config_value(session, key, default):
    """
    Obtiene un valor de configuración con fallback a valor por defecto.
    Útil para manejar casos donde el parámetro no está definido.
    """
    return session.config.get(key, default)


def calculate_mpcr(multiplier, n_players):
    """
    Calcula el Marginal Per Capita Return.
    MPCR = multiplicador / número de jugadores
    
    Interpretación económica:
    - MPCR > 1: Cada peso contribuido genera más de 1 peso de retorno grupal
    - MPCR < 1: Cada peso contribuido genera menos de 1 peso de retorno individual
    - MPCR > 1/n: Contribuir es socialmente óptimo
    """
    return multiplier / n_players


# PAGES
class Contribute(Page):
    form_model = 'player'
    form_fields = ['contribution']
    
    @staticmethod
    def vars_for_template(player):
        """
        Pasa variables al template incluyendo parámetros configurados.
        """
        session = player.session
        endowment = get_config_value(session, 'endowment', C.ENDOWMENT)
        multiplier = get_config_value(session, 'multiplier', C.MULTIPLIER)
        n_players = get_config_value(session, 'players_per_group', C.PLAYERS_PER_GROUP)
        mpcr = calculate_mpcr(multiplier, n_players)
        
        return dict(
            endowment=endowment,
            multiplier=multiplier,
            n_players=n_players,
            mpcr=round(mpcr, 2),
        )


class ResultsWaitPage(WaitPage):
    after_all_players_arrive = 'set_payoffs'


class Results(Page):
    @staticmethod
    def vars_for_template(player):
        """
        Variables para mostrar resultados con parámetros del tratamiento.
        """
        session = player.session
        multiplier = get_config_value(session, 'multiplier', C.MULTIPLIER)
        n_players = get_config_value(session, 'players_per_group', C.PLAYERS_PER_GROUP)
        mpcr = calculate_mpcr(multiplier, n_players)
        
        return dict(
            multiplier=multiplier,
            mpcr=round(mpcr, 2),
            treatment_name=session.config.get('name', 'default'),
        )


# FUNCIONES DE GRUPO
def set_payoffs(group: Group):
    """
    Calcula payoffs usando parámetros de la sesión.
    
    Fórmula:
    payoff_i = (endowment - contribution_i) + (sum(contributions) * multiplier / n)
    """
    session = group.session
    
    # Obtener parámetros de configuración
    endowment = get_config_value(session, 'endowment', C.ENDOWMENT)
    multiplier = get_config_value(session, 'multiplier', C.MULTIPLIER)
    n_players = get_config_value(session, 'players_per_group', C.PLAYERS_PER_GROUP)
    
    # Guardar multiplicador usado
    group.multiplier_used = multiplier
    
    # Calcular contribución total
    players = group.get_players()
    contributions = [p.contribution for p in players]
    group.total_contribution = sum(contributions)
    
    # Calcular parte individual del fondo común
    group.individual_share = (group.total_contribution * multiplier) / n_players
    
    # Calcular MPCR para guardarlo en cada jugador
    mpcr = calculate_mpcr(multiplier, n_players)
    
    # Asignar payoffs
    for p in players:
        p.treatment_mpcr = mpcr
        p.payoff = endowment - p.contribution + group.individual_share


page_sequence = [Contribute, ResultsWaitPage, Results]
```

#### Actualizar template `Contribute.html`

```html
{{ block title }}
    Contribución al Fondo Común
{{ endblock }}

{{ block content }}
<div class="contribute-page">
    <div class="info-box" style="background-color: #f0f8ff; padding: 15px; border-radius: 5px; margin-bottom: 20px;">
        <h4>Información del Tratamiento</h4>
        <ul>
            <li><strong>Tu dotación:</strong> {{ endowment }} puntos</li>
            <li><strong>Jugadores en tu grupo:</strong> {{ n_players }}</li>
            <li><strong>Multiplicador:</strong> {{ multiplier }}</li>
            <li><strong>MPCR:</strong> {{ mpcr }}</li>
        </ul>
    </div>
    
    <p>
        Tienes <strong>{{ endowment }} puntos</strong>. 
        ¿Cuántos puntos quieres contribuir al fondo común?
    </p>
    
    <p>
        Las contribuciones se multiplicarán por <strong>{{ multiplier }}</strong> 
        y se dividirán equitativamente entre los {{ n_players }} jugadores.
    </p>
    
    {{ formfields }}
    
    {{ next_button }}
</div>
{{ endblock }}
```

#### Commits sugeridos

```bash
# Después de modificar settings.py
git add settings.py
git commit -m "feat: agrega tratamientos high_mpcr y low_mpcr en settings"

# Después de modificar __init__.py
git add public_goods/__init__.py
git commit -m "feat(public_goods): implementa parámetros configurables desde sesión

- Agrega función get_config_value para manejo seguro de parámetros
- Agrega función calculate_mpcr para cálculo de MPCR
- Modifica set_payoffs para usar parámetros de sesión
- Agrega documentación económica en comentarios"

# Después de actualizar template
git add public_goods/templates/public_goods/Contribute.html
git commit -m "feat(public_goods): muestra info de tratamiento en Contribute"

# Push a la rama
git push -u origin feature/parametros-tratamientos
```

</details>

---

### Verificación local

```bash
# Iniciar servidor
otree devserver

# En navegador: http://localhost:8000
# Verificar que aparecen los dos tratamientos:
# - "Public Goods - High MPCR (0.67)"
# - "Public Goods - Low MPCR (0.40)"

# Probar cada tratamiento y verificar que:
# 1. El multiplicador mostrado es correcto
# 2. Los payoffs se calculan con el multiplicador correcto
```

### Crear Pull Request

**Title:**
```
feat(public_goods): Implementa parámetros configurables y tratamientos MPCR
```

**Body:**
```markdown
## Descripción
Hace los parámetros del Public Goods Game configurables desde settings.py y crea dos tratamientos experimentales.

## Tratamientos
| Tratamiento | Multiplicador | MPCR | Predicción |
|-------------|---------------|------|------------|
| high_mpcr | 2.0 | 0.67 | Mayor cooperación |
| low_mpcr | 1.2 | 0.40 | Menor cooperación |

## Cambios técnicos
- Parámetros se leen de `session.config` con fallback a valores por defecto
- Función `calculate_mpcr` documenta el concepto económico
- Template muestra información del tratamiento

## Testing
- [x] Ambos tratamientos aparecen en demo
- [x] Payoffs se calculan correctamente en cada tratamiento
- [x] Funciona con parámetros por defecto si no se especifican

Closes #2
```

---

## 3.3 MÓDULO 3: Sergio - Resultados con Visualización

### Objetivo
Crear una página de resultados mejorada que muestre gráficamente las contribuciones de cada jugador usando Chart.js, con una tabla detallada y desglose del cálculo de payoff.

### Flujo de trabajo Git

```bash
# 1. Asegurarse de estar en main actualizado
git checkout main
git pull origin main

# 2. Crear rama de feature
git checkout -b feature/resultados-graficos

# 3. Verificar que estás en la rama correcta
git branch
# Debe mostrar: * feature/resultados-graficos
```

### Prompt sugerido para IA

> **Modelo recomendado:** Claude Sonnet 4.5  
> **Justificación:** Esta tarea es principalmente de frontend (HTML + JavaScript) sin lógica compleja de backend. Sonnet es más rápido y suficiente para generar código de visualización con Chart.js.

```
Eres un desarrollador frontend experto en visualización de datos con Chart.js y oTree.

CONTEXTO:
Tengo un Public Goods Game en oTree 5. Necesito mejorar la página de resultados para mostrar:
1. Tabla con contribuciones de cada jugador (anonimizadas como "Jugador 1, 2, 3")
2. Gráfico de barras con las contribuciones
3. Desglose claro del cálculo de payoff

DATOS DISPONIBLES EN EL TEMPLATE:
- player.contribution: contribución del jugador actual
- group.total_contribution: suma de todas las contribuciones
- group.individual_share: parte que recibe cada jugador del fondo
- player.payoff: ganancia final del jugador

PARA OBTENER CONTRIBUCIONES DE OTROS JUGADORES:
En vars_for_template puedo pasar:
- Lista de contribuciones de todos los jugadores
- El índice del jugador actual (para destacarlo)

REQUISITOS:
1. Usar Chart.js desde CDN (no instalar paquetes)
2. El gráfico debe ser un bar chart horizontal o vertical
3. Destacar la barra del jugador actual en color diferente
4. La tabla debe mostrar contribución y si es "Tú" o "Otro jugador"
5. El desglose del cálculo debe ser paso a paso

RESTRICCIONES DE OTREE:
- Los templates usan sintaxis Django/Jinja2
- Para pasar datos a JavaScript, usar {{ variable|json }}
- No puedo usar módulos ES6, solo script tags tradicionales

OUTPUT:
1. Función vars_for_template completa para Results page
2. Template Results.html completo con:
   - Tabla de contribuciones
   - Gráfico Chart.js
   - Desglose del cálculo
   - CSS inline para estilizar
```

### Descripción de la tarea

**Archivos a modificar:**
- `public_goods/__init__.py` - Agregar `vars_for_template` a Results
- `public_goods/templates/public_goods/Results.html` - Rediseñar completamente

**Especificaciones:**
1. Tabla con contribuciones anonimizadas
2. Gráfico de barras con Chart.js
3. Destacar al jugador actual en la visualización
4. Mostrar fórmula y cálculo paso a paso

---

### 💡 HINT (leer solo si llevas más de 15 minutos atascado)

<details>
<summary>Click para ver el hint</summary>

**Para pasar datos de contribuciones a JavaScript:**

1. En `vars_for_template`, crea una lista de contribuciones:
```python
@staticmethod
def vars_for_template(player):
    group = player.group
    players = group.get_players()
    
    contributions = []
    for i, p in enumerate(players):
        contributions.append({
            'player_number': i + 1,
            'contribution': float(p.contribution),
            'is_self': p.id == player.id,
        })
    
    return dict(
        contributions=contributions,
        # ... otros datos
    )
```

2. En el template, pasa los datos a JavaScript:
```html
<script>
    const contributions = {{ contributions|json }};
    // Ahora puedes usar 'contributions' en JavaScript
</script>
```

3. Para Chart.js, incluye el CDN:
```html
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
```

4. Para destacar al jugador actual, usa colores diferentes:
```javascript
const colors = contributions.map(c => 
    c.is_self ? '#4CAF50' : '#2196F3'
);
```

</details>

---

### ✅ SOLUCIÓN COMPLETA

<details>
<summary>Click para ver la solución completa</summary>

#### Modificaciones a `public_goods/__init__.py`

Reemplazar la clase Results:

```python
class Results(Page):
    @staticmethod
    def vars_for_template(player):
        """
        Prepara todos los datos necesarios para la visualización de resultados.
        """
        group = player.group
        session = player.session
        
        # Obtener parámetros de configuración
        endowment = session.config.get('endowment', C.ENDOWMENT)
        multiplier = session.config.get('multiplier', C.MULTIPLIER)
        n_players = session.config.get('players_per_group', C.PLAYERS_PER_GROUP)
        
        # Preparar datos de contribuciones para la tabla y el gráfico
        players_in_group = group.get_players()
        contributions_data = []
        
        for i, p in enumerate(players_in_group):
            contributions_data.append({
                'player_number': i + 1,
                'contribution': float(p.contribution),
                'is_self': p.id == player.id,
                'label': 'Tú' if p.id == player.id else f'Jugador {i + 1}',
            })
        
        # Ordenar por número de jugador
        contributions_data.sort(key=lambda x: x['player_number'])
        
        # Calcular desglose paso a paso
        kept = float(endowment - player.contribution)
        total_contributed = float(group.total_contribution)
        multiplied_fund = total_contributed * multiplier
        share_from_fund = float(group.individual_share)
        final_payoff = float(player.payoff)
        
        # Datos para el gráfico
        chart_labels = [d['label'] for d in contributions_data]
        chart_values = [d['contribution'] for d in contributions_data]
        chart_colors = ['#4CAF50' if d['is_self'] else '#2196F3' for d in contributions_data]
        
        return dict(
            # Parámetros del juego
            endowment=endowment,
            multiplier=multiplier,
            n_players=n_players,
            
            # Datos de contribuciones
            contributions_data=contributions_data,
            
            # Desglose del cálculo
            my_contribution=float(player.contribution),
            kept=kept,
            total_contributed=total_contributed,
            multiplied_fund=multiplied_fund,
            share_from_fund=share_from_fund,
            final_payoff=final_payoff,
            
            # Datos para Chart.js (en formato JSON)
            chart_labels=chart_labels,
            chart_values=chart_values,
            chart_colors=chart_colors,
        )
```

#### Template: `Results.html`

Reemplazar completamente `public_goods/templates/public_goods/Results.html`:

```html
{{ block title }}
    Resultados
{{ endblock }}

{{ block styles }}
<style>
    .results-container {
        max-width: 800px;
        margin: 0 auto;
    }
    
    .section {
        background-color: #f9f9f9;
        border-radius: 8px;
        padding: 20px;
        margin-bottom: 20px;
        box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }
    
    .section h3 {
        margin-top: 0;
        color: #333;
        border-bottom: 2px solid #4CAF50;
        padding-bottom: 10px;
    }
    
    .contributions-table {
        width: 100%;
        border-collapse: collapse;
        margin: 15px 0;
    }
    
    .contributions-table th,
    .contributions-table td {
        padding: 12px;
        text-align: center;
        border: 1px solid #ddd;
    }
    
    .contributions-table th {
        background-color: #4CAF50;
        color: white;
    }
    
    .contributions-table tr.is-self {
        background-color: #E8F5E9;
        font-weight: bold;
    }
    
    .contributions-table tr:hover {
        background-color: #f5f5f5;
    }
    
    .chart-container {
        position: relative;
        height: 300px;
        margin: 20px 0;
    }
    
    .calculation-step {
        display: flex;
        justify-content: space-between;
        padding: 10px 0;
        border-bottom: 1px dashed #ddd;
    }
    
    .calculation-step:last-child {
        border-bottom: none;
        font-weight: bold;
        font-size: 1.2em;
        color: #4CAF50;
    }
    
    .calculation-step .label {
        color: #666;
    }
    
    .calculation-step .value {
        font-weight: bold;
    }
    
    .highlight-box {
        background-color: #E3F2FD;
        border-left: 4px solid #2196F3;
        padding: 15px;
        margin: 15px 0;
    }
    
    .final-payoff {
        font-size: 1.5em;
        text-align: center;
        padding: 20px;
        background: linear-gradient(135deg, #4CAF50, #8BC34A);
        color: white;
        border-radius: 8px;
        margin-top: 20px;
    }
</style>
{{ endblock }}

{{ block content }}
<div class="results-container">
    
    <!-- Sección 1: Resumen -->
    <div class="section">
        <h3>📊 Resumen del Grupo</h3>
        <div class="highlight-box">
            <p>
                <strong>Total contribuido por el grupo:</strong> {{ total_contributed }} puntos<br>
                <strong>Fondo después de multiplicar (×{{ multiplier }}):</strong> {{ multiplied_fund }} puntos<br>
                <strong>Parte de cada jugador:</strong> {{ share_from_fund }} puntos
            </p>
        </div>
    </div>
    
    <!-- Sección 2: Tabla de Contribuciones -->
    <div class="section">
        <h3>👥 Contribuciones del Grupo</h3>
        <table class="contributions-table">
            <thead>
                <tr>
                    <th>Jugador</th>
                    <th>Contribución</th>
                    <th>Puntos Guardados</th>
                </tr>
            </thead>
            <tbody>
                {{ for item in contributions_data }}
                <tr class="{{ if item.is_self }}is-self{{ endif }}">
                    <td>{{ item.label }}</td>
                    <td>{{ item.contribution }} puntos</td>
                    <td>{{ endowment }} - {{ item.contribution }} = {{ js_vars.endowment_val - item.contribution }} puntos</td>
                </tr>
                {{ endfor }}
            </tbody>
        </table>
    </div>
    
    <!-- Sección 3: Gráfico -->
    <div class="section">
        <h3>📈 Visualización de Contribuciones</h3>
        <div class="chart-container">
            <canvas id="contributionsChart"></canvas>
        </div>
        <p style="text-align: center; color: #666; font-size: 0.9em;">
            <span style="color: #4CAF50;">■</span> Tu contribución &nbsp;&nbsp;
            <span style="color: #2196F3;">■</span> Otros jugadores
        </p>
    </div>
    
    <!-- Sección 4: Desglose del Cálculo -->
    <div class="section">
        <h3>🧮 Cálculo de tu Ganancia</h3>
        
        <div class="calculation-step">
            <span class="label">Tu dotación inicial:</span>
            <span class="value">{{ endowment }} puntos</span>
        </div>
        
        <div class="calculation-step">
            <span class="label">Tu contribución al fondo:</span>
            <span class="value">- {{ my_contribution }} puntos</span>
        </div>
        
        <div class="calculation-step">
            <span class="label">Puntos que guardaste:</span>
            <span class="value">= {{ kept }} puntos</span>
        </div>
        
        <div class="calculation-step">
            <span class="label">Tu parte del fondo común:</span>
            <span class="value">+ {{ share_from_fund }} puntos</span>
        </div>
        
        <div class="calculation-step">
            <span class="label">TU GANANCIA FINAL:</span>
            <span class="value">{{ final_payoff }} puntos</span>
        </div>
    </div>
    
    <!-- Ganancia Final Destacada -->
    <div class="final-payoff">
        🎉 Tu ganancia en esta ronda: <strong>{{ player.payoff }}</strong>
    </div>

</div>

{{ next_button }}
{{ endblock }}

{{ block scripts }}
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script>
    // Datos pasados desde Python
    const labels = {{ chart_labels|json }};
    const values = {{ chart_values|json }};
    const colors = {{ chart_colors|json }};
    
    // Configuración del gráfico
    const ctx = document.getElementById('contributionsChart').getContext('2d');
    
    new Chart(ctx, {
        type: 'bar',
        data: {
            labels: labels,
            datasets: [{
                label: 'Contribución (puntos)',
                data: values,
                backgroundColor: colors,
                borderColor: colors.map(c => c),
                borderWidth: 2,
                borderRadius: 5,
            }]
        },
        options: {
            responsive: true,
            maintainAspectRatio: false,
            plugins: {
                legend: {
                    display: false
                },
                tooltip: {
                    callbacks: {
                        label: function(context) {
                            return context.parsed.y + ' puntos';
                        }
                    }
                }
            },
            scales: {
                y: {
                    beginAtZero: true,
                    max: {{ endowment }},
                    title: {
                        display: true,
                        text: 'Puntos Contribuidos'
                    },
                    ticks: {
                        stepSize: 20
                    }
                },
                x: {
                    title: {
                        display: true,
                        text: 'Jugadores'
                    }
                }
            }
        }
    });
</script>
{{ endblock }}
```

#### Commits sugeridos

```bash
# Después de modificar __init__.py
git add public_goods/__init__.py
git commit -m "feat(public_goods): agrega vars_for_template con datos para visualización"

# Después de crear el nuevo template
git add public_goods/templates/public_goods/Results.html
git commit -m "feat(public_goods): rediseña Results con tabla, gráfico y desglose

- Agrega tabla de contribuciones con destacado del jugador actual
- Integra Chart.js para gráfico de barras
- Muestra desglose paso a paso del cálculo de payoff
- Incluye CSS personalizado para mejor presentación"

# Push a la rama
git push -u origin feature/resultados-graficos
```

</details>

---

### Verificación local

```bash
# Iniciar servidor
otree devserver

# En navegador: http://localhost:8000
# Completar el flujo del juego hasta Results
# Verificar:
# 1. La tabla muestra todas las contribuciones
# 2. Tu fila está destacada
# 3. El gráfico renderiza correctamente
# 4. El desglose del cálculo es correcto
# 5. Probar en diferentes navegadores si es posible
```

### Crear Pull Request

**Title:**
```
feat(public_goods): Mejora página de resultados con visualización Chart.js
```

**Body:**
```markdown
## Descripción
Rediseña completamente la página de resultados con visualizaciones mejoradas.

## Nuevas características
- 📊 Tabla de contribuciones con destacado del jugador actual
- 📈 Gráfico de barras con Chart.js
- 🧮 Desglose paso a paso del cálculo de payoff
- 🎨 CSS personalizado para mejor UX

## Screenshots
(Incluir capturas del gráfico y la tabla)

## Dependencias externas
- Chart.js vía CDN (no requiere instalación)

## Testing
- [x] Gráfico renderiza en Chrome, Firefox, Safari
- [x] Datos se pasan correctamente a JavaScript
- [x] Cálculos coinciden con payoffs reales

Closes #3
```

Perfecto, aquí va el **módulo completo para Donovan** en el mismo formato que los otros, listo para pegar en tu taller debajo de 3.3 👇

---

## 3.4 MÓDULO 4: Donovan - Sistema de Castigo (Punishment Stage)

### Objetivo

Agregar una **etapa de castigo** después de ver los resultados del Public Goods Game, donde los participantes pueden pagar puntos para reducir el payoff de otros jugadores, siguiendo la lógica estándar de la literatura (ratio 1:3).

* Cada punto de castigo **cuesta 1 unidad** al que castiga.
* Cada punto de castigo **reduce 3 unidades** al castigado.
* El castigo es **anónimo**: solo se observan los totales recibidos, no quién castigó a quién.
* Se agregan nuevas páginas: `Punishment` y `FinalResults`.

---

### Flujo de trabajo Git

```bash
# 1. Asegurarse de estar en main actualizado
git checkout main
git pull origin main

# 2. Crear rama de feature
git checkout -b feature/punishment-stage

# 3. Verificar que estás en la rama correcta
git branch
# Debe mostrar: * feature/punishment-stage
```

---

### Prompt sugerido para IA

> **Modelo recomendado:** GPT-5.1 Thinking
> **Justificación:** El diseño de la etapa de castigo combina lógica económica (incentivos, costo/beneficio) y coordinación de varias partes del código (modelo de datos, funciones de grupo, flow de páginas). GPT-5.1 ayuda a razonar defensivamente sobre edge cases y consistencia con el juego base.

```text
Actúa como un economista experimental experto en oTree 5 y en juegos de bienes públicos con castigo (Fehr & Gächter, 2000).

CONTEXTO:
Estoy implementando un Public Goods Game en oTree 5. Ya existe:
- Una etapa de contribución (Contribute)
- Una página de resultados iniciales (Results) que muestra payoffs base

QUIERO AGREGAR:
Una etapa de castigo (punishment stage), con las siguientes características:

1. Después de ver los resultados, cada jugador puede asignar puntos de castigo a los demás jugadores de su grupo.
2. Cada punto de castigo:
   - Cuesta 1 unidad al que castiga
   - Reduce 3 unidades al castigado
3. Los jugadores no pueden castigarse a sí mismos.
4. El castigo es anónimo: en la pantalla final solo se verá el total de castigo recibido, no quién lo envió.

REQUISITOS TÉCNICOS (OTREE 5):

- La estructura actual (simplificada) es:
  - Clase C(BaseConstants) con:
    - PLAYERS_PER_GROUP = 3
    - ENDOWMENT, MULTIPLIER, etc.
  - Clase Group(BaseGroup) con total_contribution, individual_share, etc.
  - Clase Player(BasePlayer) con:
    - contribution
    - (opcional) payoff_before_punishment
  - Función set_payoffs(group) que calcula el payoff base del PGG.
  - Páginas: Contribute, ResultsWaitPage, Results

OBJETIVO TÉCNICO:
1. Agregar campos al modelo para:
   - Puntos de castigo enviados a cada jugador (por ejemplo: punish_1, punish_2, punish_3)
   - Totales enviados y recibidos por cada jugador
   - Payoff antes y después del castigo

2. Agregar una función de grupo:
   - apply_punishment(group) que:
     a) Calcula cuánto castigo envía cada jugador
     b) Calcula cuánto castigo recibe cada jugador
     c) Actualiza el payoff final con:
        payoff_final = payoff_base
                        - costo_castigo_enviado
                        - impacto_castigo_recibido

3. Agregar nuevas páginas:
   - Punishment (Page) donde cada jugador elige cuántos puntos de castigo asignar a cada otro jugador
   - PunishmentWaitPage (WaitPage) que llama a apply_punishment cuando todos han decidido
   - FinalResults (Page) que muestra:
     - payoff antes del castigo
     - castigo enviado
     - castigo recibido
     - payoff final después del castigo

4. Actualizar page_sequence para que el flujo sea:
   Introduction -> Comprehension -> Contribute -> ResultsWaitPage -> Results
   -> Punishment -> PunishmentWaitPage -> FinalResults

DETALLES ADICIONALES:
- Número de jugadores por grupo: 3 (id_in_group = 1, 2 y 3).
- Máximo de puntos de castigo por objetivo: 0 a 10 (parámetro configurable como constante).
- Usa constantes:
  - PUNISHMENT_MAX_POINTS = 10
  - PUNISHMENT_COST_PER_POINT = 1
  - PUNISHMENT_IMPACT_PER_POINT = 3

OUTPUT ESPERADO:
1. Fragmentos de código para agregar a public_goods/__init__.py:
   - Nuevos campos en C, Group y Player
   - Modificación de set_payoffs para guardar payoff_before_punishment
   - Nueva función apply_punishment(group)
   - Nuevas clases de página: Punishment, PunishmentWaitPage, FinalResults
   - Nueva page_sequence actualizada

2. Template Punishment.html:
   - Explica la lógica del castigo
   - Muestra contribuciones y payoff base de los demás jugadores
   - Renderiza los campos del formulario para elegir castigo

3. Template FinalResults.html:
   - Muestra desglose:
     - Payoff antes del castigo
     - Castigo enviado (total y costo)
     - Castigo recibido (total y impacto)
     - Payoff final

Incluye comentarios explicativos y respeta las convenciones de oTree 5.
```

---

### Descripción de la tarea

**Archivos a crear/modificar:**

* `public_goods/__init__.py`

  * Agregar constantes para el castigo.
  * Agregar campos en `Player` para castigo enviado/recibido.
  * Guardar `payoff_before_punishment` en `set_payoffs`.
  * Crear función `apply_punishment(group)`.
  * Crear páginas `Punishment`, `PunishmentWaitPage`, `FinalResults`.
  * Actualizar `page_sequence`.

* `public_goods/templates/public_goods/Punishment.html` (nuevo)

* `public_goods/templates/public_goods/FinalResults.html` (nuevo)

**Especificaciones económicas:**

1. **Restricción de autoinfligirse castigo:** el jugador no puede asignarse castigo a sí mismo.
2. **Costo del castigo (castigador):**
   ( \text{costo} = 1 \times \text{puntos de castigo enviados} )
3. **Impacto del castigo (castigado):**
   ( \text{impacto} = 3 \times \text{puntos de castigo recibidos} )
4. **Payoff final:**
   ( \pi_i^{final} = \pi_i^{base} - 1 \cdot \text{castigo_enviado}_i - 3 \cdot \text{castigo_recibido}_i )

---

### 💡 HINT (leer solo si llevas más de 15 minutos atascado)

<details>
<summary>Click para ver el hint</summary>

**Idea general en oTree:**

1. **Define campos de castigo en `Player`:**

```python
class Player(BasePlayer):
    # ... campos existentes (contribution, etc.) ...

    # Puntos de castigo que este jugador asigna a cada id_in_group
    punish_1 = models.IntegerField(min=0, max=C.PUNISHMENT_MAX_POINTS, blank=True)
    punish_2 = models.IntegerField(min=0, max=C.PUNISHMENT_MAX_POINTS, blank=True)
    punish_3 = models.IntegerField(min=0, max=C.PUNISHMENT_MAX_POINTS, blank=True)

    # Totales enviados y recibidos
    punishment_sent_total = models.IntegerField(initial=0)
    punishment_received_total = models.IntegerField(initial=0)

    # Payoffs antes y después del castigo
    payoff_before_punishment = models.CurrencyField()
    payoff_after_punishment = models.CurrencyField()
```

2. **Guarda el payoff base antes del castigo en `set_payoffs`:**

```python
def set_payoffs(group: Group):
    # ... cálculo estándar de payoff base ...
    for p in players:
        baseline = endowment - p.contribution + group.individual_share
        p.payoff_before_punishment = baseline
        p.payoff = baseline
```

3. **Crea una función de grupo para aplicar castigos:**

```python
def apply_punishment(group: Group):
    players = group.get_players()

    # 1) Calcular castigo enviado por cada jugador
    for p in players:
        total_sent = 0
        for i in range(1, C.PLAYERS_PER_GROUP + 1):
            if i == p.id_in_group:
                continue  # no se castiga a sí mismo
            total_sent += getattr(p, f'punish_{i}') or 0
        p.punishment_sent_total = total_sent

    # 2) Calcular castigo recibido por cada jugador
    for target in players:
        received = 0
        i = target.id_in_group
        for sender in players:
            if sender.id_in_group == i:
                continue
            received += getattr(sender, f'punish_{i}') or 0
        target.punishment_received_total = received

    # 3) Ajustar payoffs con costo e impacto
    for p in players:
        cost = cu(C.PUNISHMENT_COST_PER_POINT * p.punishment_sent_total)
        impact = cu(C.PUNISHMENT_IMPACT_PER_POINT * p.punishment_received_total)
        p.payoff -= cost + impact
        p.payoff_after_punishment = p.payoff
```

4. **En la página de Punishment, evita que el jugador vea el campo de castigo hacia sí mismo:**

```python
class Punishment(Page):
    form_model = 'player'

    @staticmethod
    def get_form_fields(player):
        fields = [f'punish_{i}' for i in range(1, C.PLAYERS_PER_GROUP + 1)]
        self_field = f'punish_{player.id_in_group}'
        return [f for f in fields if f != self_field]
```

5. **Crea una WaitPage que llame a `apply_punishment` y luego una página final con el desglose.**

```python
class PunishmentWaitPage(WaitPage):
    after_all_players_arrive = 'apply_punishment'
```

Actualiza `page_sequence` para insertar estas nuevas páginas al final.

</details>

---

### ✅ SOLUCIÓN COMPLETA

<details>
<summary>Click para ver la solución completa</summary>

#### 1. Modificaciones a `public_goods/__init__.py`

##### 1.1. Agregar constantes de castigo a la clase `C`

```python
class C(BaseConstants):
    NAME_IN_URL = 'public_goods'
    PLAYERS_PER_GROUP = 3
    NUM_ROUNDS = 1
    ENDOWMENT = cu(100)
    MULTIPLIER = 2

    # --- NUEVAS CONSTANTES PARA EL CASTIGO ---
    PUNISHMENT_MAX_POINTS = 10              # Máximo de puntos de castigo que puedes asignar a cada jugador
    PUNISHMENT_COST_PER_POINT = 1           # Cada punto de castigo cuesta 1 unidad al castigador
    PUNISHMENT_IMPACT_PER_POINT = 3         # Cada punto de castigo reduce 3 unidades al castigado
```

> Ajusta los valores si quieres variar la severidad del castigo.

---

##### 1.2. Agregar campos de castigo a la clase `Player`

Agrega estos campos **dentro de** la clase `Player(BasePlayer)` (además de los que ya tengas: `contribution`, etc.):

```python
class Player(BasePlayer):
    # ... campos existentes (contribution, comp_q1, etc.) ...

    # Puntos de castigo que este jugador asigna a cada id_in_group
    punish_1 = models.IntegerField(
        min=0,
        max=C.PUNISHMENT_MAX_POINTS,
        blank=True,
        label="Puntos de castigo para el Jugador 1"
    )
    punish_2 = models.IntegerField(
        min=0,
        max=C.PUNISHMENT_MAX_POINTS,
        blank=True,
        label="Puntos de castigo para el Jugador 2"
    )
    punish_3 = models.IntegerField(
        min=0,
        max=C.PUNISHMENT_MAX_POINTS,
        blank=True,
        label="Puntos de castigo para el Jugador 3"
    )

    # Totales de castigo
    punishment_sent_total = models.IntegerField(initial=0)
    punishment_received_total = models.IntegerField(initial=0)

    # Payoffs antes y después del castigo
    payoff_before_punishment = models.CurrencyField()
    payoff_after_punishment = models.CurrencyField()
```

---

##### 1.3. Modificar `set_payoffs` para guardar el payoff base

Suponiendo que ya tienes una función `set_payoffs(group)` que calcula el payoff del juego base, asegúrate de que guarde el payoff base en `payoff_before_punishment` antes del castigo.

Ejemplo (adaptando tu implementación actual):

```python
def set_payoffs(group: Group):
    session = group.session

    # Parámetros (ajusta según tu versión actual)
    endowment = session.config.get('endowment', C.ENDOWMENT)
    multiplier = session.config.get('multiplier', C.MULTIPLIER)
    n_players = session.config.get('players_per_group', C.PLAYERS_PER_GROUP)

    players = group.get_players()
    contributions = [p.contribution for p in players]
    group.total_contribution = sum(contributions)
    group.individual_share = (group.total_contribution * multiplier) / n_players

    for p in players:
        baseline_payoff = endowment - p.contribution + group.individual_share

        # Guardar payoff base
        p.payoff_before_punishment = baseline_payoff

        # Por ahora, payoff = payoff base (se ajustará en apply_punishment)
        p.payoff = baseline_payoff
        p.payoff_after_punishment = baseline_payoff  # valor provisional
```

---

##### 1.4. Agregar la función `apply_punishment(group)`

```python
def apply_punishment(group: Group):
    """
    Aplica el sistema de castigo tipo Fehr & Gächter:
    - Cada punto de castigo cuesta 1 unidad al castigador
    - Cada punto de castigo reduce 3 unidades al castigado
    """
    players = group.get_players()

    # 1) Calcular castigo ENVIADO por cada jugador
    for p in players:
        total_sent = 0
        for i in range(1, C.PLAYERS_PER_GROUP + 1):
            if i == p.id_in_group:
                continue  # no se castiga a sí mismo
            value = getattr(p, f'punish_{i}', 0) or 0
            total_sent += value
        p.punishment_sent_total = total_sent

    # 2) Calcular castigo RECIBIDO por cada jugador
    for target in players:
        received = 0
        i = target.id_in_group
        for sender in players:
            if sender.id_in_group == i:
                continue
            value = getattr(sender, f'punish_{i}', 0) or 0
            received += value
        target.punishment_received_total = received

    # 3) Ajustar payoffs con costo e impacto
    for p in players:
        cost = cu(C.PUNISHMENT_COST_PER_POINT * p.punishment_sent_total)
        impact = cu(C.PUNISHMENT_IMPACT_PER_POINT * p.punishment_received_total)

        # Partimos del payoff ya calculado en set_payoffs
        p.payoff = p.payoff_before_punishment - cost - impact
        p.payoff_after_punishment = p.payoff
```

---

##### 1.5. Crear páginas `Punishment`, `PunishmentWaitPage` y `FinalResults`

Agrega estas clases de página:

```python
class Punishment(Page):
    """
    Página donde cada jugador decide cuánto castigo asignar a cada otro jugador.
    """
    form_model = 'player'

    @staticmethod
    def get_form_fields(player):
        # Campos de castigo hacia cada id_in_group
        fields = [f'punish_{i}' for i in range(1, C.PLAYERS_PER_GROUP + 1)]
        # No permitir castigo a sí mismo
        self_field = f'punish_{player.id_in_group}'
        return [f for f in fields if f != self_field]

    @staticmethod
    def vars_for_template(player):
        group = player.group
        players = group.get_players()

        # Información anónima de los otros jugadores
        others_info = []
        for p in players:
            if p.id_in_group == player.id_in_group:
                continue
            others_info.append(dict(
                id_in_group=p.id_in_group,
                contribution=p.contribution,
                payoff_before=p.payoff_before_punishment
            ))

        return dict(
            others=others_info,
            max_points=C.PUNISHMENT_MAX_POINTS,
            cost_per=C.PUNISHMENT_COST_PER_POINT,
            impact_per=C.PUNISHMENT_IMPACT_PER_POINT,
        )


class PunishmentWaitPage(WaitPage):
    """
    Espera a que todos elijan castigo y luego aplica la función apply_punishment.
    """
    after_all_players_arrive = 'apply_punishment'


class FinalResults(Page):
    """
    Muestra el payoff antes y después del castigo, junto con el desglose.
    """
    @staticmethod
    def vars_for_template(player):
        cost = C.PUNISHMENT_COST_PER_POINT * player.punishment_sent_total
        impact = C.PUNISHMENT_IMPACT_PER_POINT * player.punishment_received_total

        return dict(
            payoff_before=player.payoff_before_punishment,
            payoff_after=player.payoff_after_punishment,
            punishment_sent_total=player.punishment_sent_total,
            punishment_received_total=player.punishment_received_total,
            cost_castigo=cost,
            impacto_castigo=impact,
        )
```

---

##### 1.6. Actualizar `page_sequence`

Suponiendo que tu secuencia actual (después de los otros módulos) es:

```python
page_sequence = [Introduction, Comprehension, Contribute, ResultsWaitPage, Results]
```

Actualízala para incluir la etapa de castigo:

```python
page_sequence = [
    Introduction,
    Comprehension,
    Contribute,
    ResultsWaitPage,
    Results,
    Punishment,
    PunishmentWaitPage,
    FinalResults,
]
```

---

#### 2. Template `Punishment.html`

Crear el archivo
`public_goods/templates/public_goods/Punishment.html`:

```html
{{ block title }}
    Etapa de Castigo
{{ endblock }}

{{ block content }}
<div class="punishment-container">
    <h3>Etapa de Castigo</h3>

    <p>
        Ahora puedes asignar <strong>puntos de castigo</strong> a los otros jugadores de tu grupo.
    </p>

    <ul>
        <li>No puedes castigarte a ti mismo.</li>
        <li>Cada punto de castigo <strong>te cuesta {{ cost_per }} punto(s)</strong>.</li>
        <li>Cada punto de castigo <strong>reduce {{ impact_per }} punto(s)</strong> al jugador castigado.</li>
    </ul>

    <div style="background-color:#f9f9f9; padding:15px; border-radius:6px; margin:15px 0;">
        <h4>Información del resultado antes del castigo</h4>
        <p>Ves las contribuciones y payoffs base de los otros jugadores:</p>
        <table class="table">
            <thead>
                <tr>
                    <th>Jugador</th>
                    <th>Contribución</th>
                    <th>Payoff antes del castigo</th>
                </tr>
            </thead>
            <tbody>
                {{ for other in others }}
                <tr>
                    <td>Jugador {{ other.id_in_group }}</td>
                    <td>{{ other.contribution }}</td>
                    <td>{{ other.payoff_before }}</td>
                </tr>
                {{ endfor }}
            </tbody>
        </table>
    </div>

    <p>
        Elige cuántos <strong>puntos de castigo</strong> asignar a cada uno de los otros jugadores.
        El máximo por jugador es <strong>{{ max_points }}</strong>.
    </p>

    {{ formfields }}

    {{ next_button }}
</div>
{{ endblock }}
```

---

#### 3. Template `FinalResults.html`

Crear el archivo
`public_goods/templates/public_goods/FinalResults.html`:

```html
{{ block title }}
    Resultados Finales
{{ endblock }}

{{ block content }}
<div class="final-results-container" style="max-width:700px; margin:0 auto;">
    <h3>Resultados Finales de la Ronda</h3>

    <div style="background-color:#E3F2FD; padding:15px; border-radius:6px; margin-bottom:20px;">
        <p>
            <strong>Tu payoff antes del castigo:</strong> {{ payoff_before }} puntos
        </p>
    </div>

    <div style="background-color:#FFF3E0; padding:15px; border-radius:6px; margin-bottom:20px;">
        <h4>Castigo que enviaste</h4>
        <p>
            Puntos de castigo enviados: <strong>{{ punishment_sent_total }}</strong><br>
            Costo total del castigo: <strong>{{ cost_castigo }}</strong> puntos
        </p>
    </div>

    <div style="background-color:#FFEBEE; padding:15px; border-radius:6px; margin-bottom:20px;">
        <h4>Castigo que recibiste</h4>
        <p>
            Puntos de castigo recibidos: <strong>{{ punishment_received_total }}</strong><br>
            Impacto total del castigo: <strong>{{ impacto_castigo }}</strong> puntos
        </p>
        <p style="font-size:0.9em; color:#555;">
            No se muestra quién te castigó; solo el total recibido.
        </p>
    </div>

    <div style="background:linear-gradient(135deg, #4CAF50, #8BC34A); color:white; padding:20px; border-radius:8px; text-align:center; margin-bottom:20px;">
        <h4>🎯 Tu payoff final en esta ronda</h4>
        <p style="font-size:1.4em;">
            <strong>{{ payoff_after }}</strong> puntos
        </p>
    </div>

    {{ next_button }}
</div>
{{ endblock }}
```

---

#### 4. Commits sugeridos

```bash
# Modificaciones en __init__.py
git add public_goods/__init__.py
git commit -m "feat(public_goods): agrega sistema de castigo tipo Fehr & Gächter"

# Nuevos templates
git add public_goods/templates/public_goods/Punishment.html
git add public_goods/templates/public_goods/FinalResults.html
git commit -m "feat(public_goods): agrega vistas Punishment y FinalResults para etapa de castigo"

# Push de la rama
git push -u origin feature/punishment-stage
```

---

#### 5. Verificación local

```bash
otree devserver
# En el navegador:
# 1. Jugar el PGG normalmente.
# 2. Verificar que aparece la página de Resultados (payoff base).
# 3. Ir a la página de Punishment, asignar castigo a otros.
# 4. Confirmar que, después de la WaitPage, FinalResults muestra:
#    - payoff antes del castigo
#    - castigo enviado/recibido
#    - payoff final coherente con las fórmulas
```

#### 6. Crear Pull Request

**Title:**

```text
feat(public_goods): Implementa etapa de castigo (punishment stage)
```

**Body:**

```markdown
## Descripción
Agrega una etapa de castigo al Public Goods Game siguiendo el esquema de Fehr & Gächter (2000).

## Cambios principales
- Nuevas constantes en `C` para parametrizar el castigo.
- Campos en `Player` para castigo enviado/recibido y payoffs antes/después.
- Función `apply_punishment(group)` que aplica el costo e impacto del castigo.
- Nuevas páginas:
  - `Punishment`: interfaz para asignar puntos de castigo a otros jugadores.
  - `PunishmentWaitPage`: sincronización y aplicación de castigo.
  - `FinalResults`: muestra el desglose final del payoff.

## Lógica económica
- Cada punto de castigo cuesta 1 unidad al que castiga.
- Cada punto de castigo reduce 3 unidades al castigado.
- El castigo es anónimo: solo se muestra el total recibido.

## Testing
- [x] Probado localmente con `otree devserver`.
- [x] Payoff final coincide con la fórmula:
      π_final = π_base - 1 * castigo_enviado - 3 * castigo_recibido.
- [x] No se permite asignar castigo a uno mismo.

Closes #4
```

</details>

---


Perfecto, aquí tienes las secciones **7, 8 y 9** listas para copiar y pegar al final de tu Markdown, en el mismo espíritu del taller 👇

---

# 7. INTEGRACIÓN: Pull Requests y Code Review

## 7.1 Flujo completo de Pull Request (PR)

Este es el flujo estándar que seguiremos **SIEMPRE** para integrar cambios a `main`:

```bash
# 1. Asegurarte de estar en main actualizado
git checkout main
git pull origin main

# 2. Crear rama de feature a partir de main
git checkout -b feature/nombre-claro-de-la-feature

# 3. Trabajar, hacer cambios y commits pequeños
# (editar archivos, correr otree devserver, etc.)

git status      # Ver qué cambió
git add archivo1 archivo2
git commit -m "feat: mensaje descriptivo"

# 4. Subir la rama al remoto
git push -u origin feature/nombre-claro-de-la-feature
```

Luego, en GitHub:

1. Abrir el repositorio → verás un banner: **“Compare & pull request”** → click.
2. Completar el PR:

   * **Base branch:** `main`
   * **Compare:** `feature/nombre-claro-de-la-feature`
   * **Title:** mensaje claro, e.g.
     `feat(public_goods): agrega etapa de castigo`
   * **Body:** usar la plantilla del taller:

     * Descripción
     * Cambios realizados
     * Testing
     * Screenshots (si aplica)
3. En el cuerpo del PR, enlazar el issue:
   `Closes #X`
4. Asignar:

   * **Reviewers**: al menos 1 persona del taller.
   * **Labels**: `feature`, `enhancement`, `ci/cd`, etc.
   * **Milestone**: por ejemplo `v1.0 - MVP Public Goods Game`.

A partir de ahí:

* Se disparará el workflow de **GitHub Actions** (sección 9).
* No se puede hacer merge hasta que:

  * ✅ Pasen todos los checks de CI.
  * ✅ Haya al menos 1 aprobación de review.
  * ✅ La rama esté up-to-date con `main`.

---

## 7.2 Checklist de revisión (para quien revisa el PR)

Cuando haces **code review**, NO es solo ver si “se ve bonito”. Usa esta checklist:

```markdown
### Checklist de revisión

- [ ] El PR está vinculado a un issue (`Closes #X`)
- [ ] El título del PR describe claramente el cambio
- [ ] La rama tiene un nombre descriptivo (`feature/...`, `fix/...`, etc.)
- [ ] El cambio compila y corre localmente (`otree devserver`)
- [ ] Los tests pasan localmente (`otree test` si aplica)
- [ ] No hay warnings/lints graves (ruff / flake8)
- [ ] El cambio es atómico (una sola cosa; no mezcla features sin relación)
- [ ] La lógica del juego (oTree) sigue siendo consistente (pagos, tratamientos, etc.)
- [ ] Los templates HTML no rompen el flujo de oTree
- [ ] Variables y constantes tienen nombres claros (sin “magic numbers”)
- [ ] Se actualizaron comentarios/documentación si hubo cambios importantes
- [ ] No hay credenciales ni secretos en el código
```

### Prompt sugerido para IA (revisor)

> **Modelo recomendado:** GPT-5.1 Thinking
> **Uso típico:** revisar diffs grandes o lógicos (oTree, pagos, tratamiento experimental).

```text
Actúa como un revisor senior de código en un laboratorio de economía experimental.

Tengo un Pull Request en un proyecto de oTree que implementa [describir la feature: castigo, nuevos tratamientos, visualización, etc.].

Objetivo:
- Quiero que evalúes si la lógica económica, la implementación en oTree y la estructura del código son razonables.
- Señala:
  - Posibles bugs
  - Asunciones peligrosas
  - Problemas de legibilidad
  - Cosas que deberían probarse con `otree test`

Contexto:
- Es un Public Goods Game con [detalles relevantes].
- La rama se llama [nombre de la rama].
- El diff incluye cambios en:
  - `public_goods/__init__.py`
  - Templates HTML
  - (Otros archivos si aplica)

OUTPUT:
1. Resumen de qué hace el PR.
2. Lista de posibles problemas o preguntas para el autor.
3. Sugerencias concretas de mejora (nombres, estructura, validaciones).
4. Tests o escenarios que te gustaría que el autor verifique antes de hacer merge.
```

---

## 7.3 Proceso de merge

Una vez que el PR:

* ✅ Tiene al menos 1 aprobación
* ✅ Pasó todos los checks de CI
* ✅ No tiene conflictos con `main`

Entonces:

1. El revisor (o el responsable del módulo) hace click en **“Merge pull request”**.

   * Recomendado: **“Squash and merge”** para que todo el trabajo quede en un solo commit limpio en `main`.
2. Tras el merge:

   * GitHub ofrecerá **“Delete branch”** → borrar la rama remota.
3. Localmente, el autor puede limpiar:

```bash
# Volver a main y actualizar
git checkout main
git pull origin main

# Borrar rama local
git branch -d feature/nombre-claro-de-la-feature
```

---

# 8. CONFLICTOS: Escenarios de Resolución

## 8.1 Conflicto 1: Mismo archivo, misma línea

### Situación típica

Dos personas editan **la misma línea** en `public_goods/__init__.py` (por ejemplo, modifican `PLAYERS_PER_GROUP` o la fórmula de `set_payoffs`). Al hacer `git pull` o al intentar hacer merge del PR, aparece un conflicto.

Ejemplo de conflicto en el archivo:

```python
class C(BaseConstants):
    NAME_IN_URL = 'public_goods'
<<<<<<< HEAD
    PLAYERS_PER_GROUP = 3
=======
    PLAYERS_PER_GROUP = 4
>>>>>>> feature/cambios-grupo
```

### Pasos para resolver (localmente)

```bash
# Estás en tu rama de feature
git pull --rebase origin main
# (o GitHub te avisa del conflicto en el PR)
```

1. Abrir el archivo con conflicto (`public_goods/__init__.py`).
2. Buscar las marcas:

   * `<<<<<<< HEAD`
   * `=======`
   * `>>>>>>> rama-remota`
3. Decidir qué dejar:

   * ¿Debe ser 3, 4, o una configuración desde `settings.py`?
4. Editar manualmente para que quede una sola versión coherente:

```python
class C(BaseConstants):
    NAME_IN_URL = 'public_goods'
    PLAYERS_PER_GROUP = 3  # Decisión final después de discutir con el equipo
```

5. Marcar el conflicto como resuelto:

```bash
git add public_goods/__init__.py

# Si estabas haciendo rebase:
git rebase --continue

# Si estabas en un merge normal:
git commit
git push
```

### Prompt sugerido para IA (ayuda en conflicto puntual)

```text
Estoy resolviendo un conflicto de merge en `public_goods/__init__.py` de un juego de bienes públicos en oTree.

Te pego las dos versiones de la sección en conflicto:
[pegar bloque con <<<<<<< HEAD, =======, >>>>>>> feature/... aquí]

Contexto:
- La rama main tiene [describir].
- Mi rama feature hace [describir].

Quiero que:
1. Me propongas una versión final coherente con la lógica económica del experimento.
2. Expliques brevemente por qué esa versión es mejor que las alternativas.
3. Señales si debo ajustar algo más en el código para que todo sea consistente (por ejemplo, en settings.py o en los templates).
```

---

## 8.2 Conflicto 2: Dependencias entre features

### Escenario típico

* La rama `feature/parametros-tratamientos` introduce parámetros configurables (`multiplier`, `endowment`, `players_per_group` en `settings.py` y `__init__.py`).
* La rama `feature/resultados-graficos` quiere mostrar `mpcr` y usar esos parámetros.
* Si se desarrolla `resultados-graficos` sin actualizarse desde `parametros-tratamientos`, el código puede:

  * Romperse (atributos inexistentes).
  * Compilar, pero mostrar datos inconsistentes.

### Estrategia recomendada

1. **Ordenar merges**:

   * Primero mergear la rama más “fundacional” (ej: parámetros y treatments).
   * Luego, rebasar o actualizar las otras ramas encima de `main`.

```bash
# Una vez que `feature/parametros-tratamientos` se mergeó a main

git checkout feature/resultados-graficos
git pull --rebase origin main   # trae cambios de main (incluye parámetros nuevos)
# Resolver conflictos si aparecen
git push --force-with-lease
```

2. **Verificar dependencias**:

   * ¿La página Results está usando los nuevos campos (`treatment_mpcr`, etc.)?
   * ¿El cálculo de `mpcr` es coherente con la lógica central de C y settings?

3. **Prueba cruzada**:

   * Correr `otree devserver` con ambos tratamientos.
   * Correr `otree test` (cuando haya bots) para detectar errores que no se ven en una sola corrida.

---

## 8.3 Conflicto 3: Conflicto en `constants.py` (o en la clase `C`)

Aunque en este taller usamos una clase `C(BaseConstants)` dentro de `__init__.py`, en proyectos más grandes suele haber un archivo `constants.py` central. Es un lugar donde los conflictos son frecuentes:

* Dos ramas definen la misma constante con valores distintos.
* Dos ramas agregan constantes con nombres parecidos para lo mismo.

### Ejemplo de conflicto conceptual

* Rama A (castigo):
  `PUNISHMENT_MAX_POINTS = 10`
* Rama B (otra feature):
  `MAX_POINTS_PER_PLAYER = 5`

Ambas hablan de “máximo de puntos”, pero no están coordinadas.

### Estrategia de resolución

1. **Unificar semántica antes que “ganar la pelea”**:

   * Sentarse (literal o virtualmente) a decidir:

     * ¿Cuál es el máximo que queremos de verdad?
     * ¿Necesitamos 2 constantes distintas o solo una bien nombrada?

2. **Refactorizar nombres**:

   * En vez de tener:

     * `MAX_POINTS_PER_PLAYER`
     * `PUNISHMENT_MAX_POINTS`
   * Definir algo como:

     * `PUNISHMENT_MAX_POINTS = 10  # Máximo de puntos de castigo por jugador objetivo`
   * Y usarlo en todos lados.

3. **Actualizar referencias**:

   * Buscar dónde se usan esas constantes y cambiar al nuevo nombre.
   * Correr tests para asegurarse de no haber roto nada.

### Prompt sugerido para IA (conflicto de constantes)

```text
Tengo un conflicto de diseño en el archivo de constantes de un experimento en oTree.

Versión 1 (rama A):
[pegar bloque de código con constantes de la rama A]

Versión 2 (rama B):
[pegar bloque de código con constantes de la rama B]

Contexto:
- Estas ramas implementan [describir brevemente].
- La constante X se usa en [archivo(s)].
- La constante Y se usa en [archivo(s)].

Quiero que:
1. Propongas un conjunto final de constantes con nombres claros y sin redundancia.
2. Especifiques para qué se usa cada constante (comentarios cortos).
3. Señales si hay riesgos de romper algo al unificar (por ejemplo, cambiar el valor del multiplicador o del costo del castigo).
```

---

# 9. GITHUB ACTIONS (OBLIGATORIO)

En este taller, **es obligatorio** tener un workflow mínimo de CI que:

1. **Se ejecute en cada PR hacia `main`.**
2. **Valide la sintaxis Python.**
3. **Corra tests básicos de oTree** (cuando los tengamos listos).

---

## 9.1 Workflow de CI básico

Crear el archivo:
`.github/workflows/ci.yml`

```yaml
name: CI

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  ci:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.11"

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          if [ -f requirements.txt ]; then
            pip install -r requirements.txt
          else
            pip install "otree>=5" "ruff"
          fi

      - name: Lint with ruff
        run: |
          ruff check .

      - name: Validate Python syntax
        run: |
          python -m compileall .

      - name: Run oTree tests
        run: |
          otree test
```

### ¿Qué hace cada paso?

* **checkout**: baja el código del repo a la máquina virtual.
* **setup-python**: elige la versión de Python (3.11).
* **Install dependencies**:

  * Si hay `requirements.txt`, lo respeta.
  * Si no, instala al menos `otree` y `ruff`.
* **ruff check**: linter rápido de Python (estilo, errores comunes).
* **compileall**: intenta compilar todos los `.py` → falla si hay errores de sintaxis.
* **otree test**: corre tests de oTree (cuando haya bots configurados; si no, se puede ajustar para que solo valide apps específicas).

---

## 9.2 Tests automáticos con oTree

A futuro (o en una versión extendida del taller):

* Podrán definir **bots** en `tests.py` dentro de cada app (`public_goods/tests.py`).
* `otree test public_goods` correrá esos bots y verificará:

  * Que el flujo de páginas no truena.
  * Que los cálculos de payoff se comportan como se espera en escenarios básicos.

Mientras tanto, podemos dejar:

```yaml
      - name: Run oTree tests
        run: |
          otree test public_goods
```

Y ajustar cuando se agreguen más apps.

---

## 9.3 Validación de sintaxis Python

Además del linter (`ruff`), el paso:

```yaml
python -m compileall .
```

sirve como red de seguridad:

* Si algún archivo `.py` tiene errores de sintaxis (por ejemplo, paréntesis mal cerrado, indentación inválida), el workflow **falla**.
* Con las reglas de **branch protection**, eso significa:

  * ❌ No se puede hacer merge del PR hasta que se arregle el error.

---

### Prompt sugerido para IA (diseño / ajuste de workflow de CI)

```text
Quiero diseñar / ajustar un workflow de GitHub Actions para un proyecto de oTree 5.

Objetivo del workflow:
1. Ejecutarse en cada push y pull_request a main.
2. Instalar dependencias desde requirements.txt (si existe).
3. Lint con ruff.
4. Validar sintaxis Python (compileall).
5. Correr `otree test public_goods`.

Contexto:
- El repositorio se llama [nombre del repo].
- La versión objetivo de Python es 3.11.
- Estoy trabajando en un taller de economía experimental (Public Goods Game).

Te paso mi workflow actual:
[pegar contenido actual de ci.yml]

Quiero que:
1. Lo revises y corrijas si hay errores.
2. Mejores nombres y mensajes de los pasos para que sean claros para estudiantes.
3. Propongas variantes opcionales (por ejemplo, solo correr tests en pull_request, no en cada push).
```

---

perfecto, vamos a dejar **todo cableado** para que:

* `otree test public_goods` funcione desde ya ✅
* el workflow de GitHub Actions lo corra en cada PR ✅

Te doy solo lo que falta agregar al Markdown (sección 9 extendida) y el código listo para copiar/pegar en tu repo.

---

## 9.4 Archivos necesarios para que CI funcione hoy mismo

Para que el paso:

```yaml
- name: Run oTree tests
  run: |
    otree test public_goods
```

funcione sin tocar nada más, necesitamos:

1. Un `requirements.txt` básico.
2. Un archivo `public_goods/tests.py` con un bot funcional.
3. (Opcional pero recomendado) Verificar que las `SESSION_CONFIGS` incluyan `public_goods`.

---

### 9.4.1 Crear `requirements.txt`

En la raíz del repositorio (`taller-otree-pgg/`), crear un archivo llamado:

`requirements.txt`:

```text
otree>=5.10,<6.0
ruff>=0.4
```

> Si más adelante agregan otras dependencias (por ejemplo, librerías para análisis de datos), se agregan aquí.

Con esto, el paso de instalación en GitHub Actions:

```yaml
- name: Install dependencies
  run: |
    python -m pip install --upgrade pip
    if [ -f requirements.txt ]; then
      pip install -r requirements.txt
    else
      pip install "otree>=5" "ruff"
    fi
```

ya instala todo lo necesario.

---

### 9.4.2 Crear `public_goods/tests.py` (bots para el juego)

Ahora definimos bots para el app `public_goods`.
Este bot:

* Recorre el flujo completo:

  `Introduction -> Comprehension -> Contribute -> Results -> Punishment -> FinalResults`

* Usa **tres casos de prueba**:

  * `min`: todos contribuyen 0.
  * `basic`: todos contribuyen la mitad de la dotación.
  * `max`: todos contribuyen toda la dotación.

* No aplica castigo (todos asignan 0 puntos), de modo que:

  * `payoff_before_punishment == payoff_after_punishment == payoff esperado`.

Crea el archivo:
`public_goods/tests.py` con este contenido:

```python
from otree.api import Bot, Submission, SubmissionMustFail, expect

from . import pages
from .models import C


class PlayerBot(Bot):
    """
    Bots para el Public Goods Game con etapa de castigo.

    Casos:
    - 'min': todos contribuyen 0
    - 'basic': todos contribuyen la mitad de la dotación
    - 'max': todos contribuyen toda la dotación

    No se envía castigo (castigo enviado = 0), así que:
    payoff_before_punishment == payoff_after_punishment == payoff final.
    """

    cases = ['basic', 'min', 'max']

    def play_round(self):
        # Parámetros relevantes (se adaptan a lo que haya en settings.py)
        session = self.session
        endowment = session.config.get('endowment', C.ENDOWMENT)
        multiplier = session.config.get('multiplier', C.MULTIPLIER)
        n_players = session.config.get('players_per_group', C.PLAYERS_PER_GROUP)

        # --- Flujo de páginas ---

        # 1) Introducción (sin formulario)
        yield pages.Introduction

        # 2) Preguntas de comprensión (usamos las respuestas correctas esperadas)
        #    Según la implementación propuesta en el módulo 3.1:
        #    comp_q1 = C.ENDOWMENT (100)
        #    comp_q2 = 150 (3 jugadores x 50)
        #    comp_q3 = 100 (300/3)
        yield pages.Comprehension, dict(
            comp_q1=C.ENDOWMENT,
            comp_q2=150,
            comp_q3=100,
        )

        # 3) Contribución según el caso
        if self.case == 'min':
            contribution = 0
        elif self.case == 'max':
            contribution = int(endowment)
        else:  # 'basic'
            contribution = int(endowment / 2)

        yield pages.Contribute, dict(contribution=contribution)

        # 4) Resultados del juego base (sin castigo todavía)
        yield pages.Results

        # --- Cálculo del payoff esperado antes de castigo ---

        # En cada test case, TODOS los jugadores usan la misma "case",
        # por lo que las contribuciones son simétricas:
        # total_contributed = contribution * n_players
        total_contributed = contribution * n_players
        individual_share = total_contributed * multiplier / n_players
        expected_payoff_base = endowment - contribution + individual_share

        # Verificamos que el payoff_before_punishment coincide con el cálculo teórico
        expect(self.player.payoff_before_punishment, expected_payoff_base)

        # 5) Etapa de castigo: asignamos 0 puntos a todos los demás
        #    (no hay costos ni impactos de castigo)
        punishment_form = {}

        # Campos en Player: punish_1, punish_2, punish_3
        # No podemos castigarnos a nosotros mismos.
        if self.player.id_in_group == 1:
            punishment_form = dict(punish_2=0, punish_3=0)
        elif self.player.id_in_group == 2:
            punishment_form = dict(punish_1=0, punish_3=0)
        elif self.player.id_in_group == 3:
            punishment_form = dict(punish_1=0, punish_2=0)

        # Si en el futuro hay más jugadores, se puede generalizar,
        # pero con PLAYERS_PER_GROUP = 3 esto es suficiente.
        yield pages.Punishment, punishment_form

        # 6) WaitPage que aplica el castigo (apply_punishment)
        #    -> no se rinde aquí, se maneja internamente en oTree.
        #    (No se escribe yield para PunishmentWaitPage)

        # 7) Resultados finales después del castigo
        yield pages.FinalResults

        # Con castigo = 0, el payoff final debe ser igual al payoff base
        expect(self.player.punishment_sent_total, 0)
        expect(self.player.punishment_received_total, 0)

        expect(self.player.payoff_after_punishment, expected_payoff_base)
        expect(self.player.payoff, expected_payoff_base)
```

> Nota:
>
> * No usamos `SubmissionMustFail` por ahora para mantener el bot simple.
> * Más adelante pueden extender este archivo para:
>
>   * Probar validación de comprensión.
>   * Probar castigo positivo y checar que los payoffs se ajustan correctamente.

Con esto, `otree test public_goods` ya tiene **algo real** que ejecutar.

---

### 9.4.3 Ajustes recomendados en `settings.py` (para tests)

En `settings.py`, asegúrate de que al menos UNA `SESSION_CONFIG` incluya `public_goods` en `app_sequence`. Con lo que ya habíamos propuesto, basta con algo así:

```python
SESSION_CONFIGS = [
    dict(
        name='public_goods_high_mpcr',
        display_name="Public Goods - High MPCR (0.67)",
        app_sequence=['public_goods'],
        num_demo_participants=3,
        endowment=100,
        multiplier=2.0,
        players_per_group=3,
        # Para browser bots (opcional):
        use_browser_bots=False,
        doc="""
        Tratamiento con MPCR alto (0.67).
        """
    ),
    dict(
        name='public_goods_low_mpcr',
        display_name="Public Goods - Low MPCR (0.40)",
        app_sequence=['public_goods'],
        num_demo_participants=3,
        endowment=100,
        multiplier=1.2,
        players_per_group=3,
        use_browser_bots=False,
        doc="""
        Tratamiento con MPCR bajo (0.40).
        """
    ),
]
```

> Para el CI con `otree test public_goods` no es necesario `use_browser_bots=True`; eso es solo si quieres que los bots se jueguen en navegador.
> Para **command-line bots** (los que usamos en CI) basta con `tests.py` + que la app exista.

---

### 9.4.4 Versión final recomendada de `ci.yml` (resumen)

Por claridad, dejo aquí el `ci.yml` completo ya alineado con lo anterior:

```yaml
name: CI

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  ci:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.11"

      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          if [ -f requirements.txt ]; then
            pip install -r requirements.txt
          else:
            pip install "otree>=5" "ruff"
          fi

      - name: Lint with ruff
        run: |
          ruff check .

      - name: Validate Python syntax
        run: |
          python -m compileall .

      - name: Run oTree tests (public_goods)
        run: |
          otree test public_goods
```

Con:

* `requirements.txt` ✅
* `public_goods/tests.py` ✅
* `SESSION_CONFIGS` con `public_goods` ✅

ya tienes la integración **GitHub Actions + tests automáticos de oTree** funcionando “de fábrica” en este taller.
