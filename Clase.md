# Taller Interactivo: Colaboración con Git/GitHub en Proyectos oTree

## Información del Taller

| Campo | Valor |
|-------|-------|
| **Duración estimada** | 3-4 horas |
| **Participantes** | Mauricio, José Miguel, Sergio, Donovan |
| **Nivel** | Intermedio (conocimiento básico de Git requerido) agregar 5 líneas con los comandos de git|
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
Hi [tu-usuario]! You've successfully authenticated, but GitHub does not provide shell access.
```

---

## 1.2 Creación del Repositorio (Solo el Facilitador). quitar esta sección. Solamente José Miguel

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
# Agregar remoto (reemplazar [USUARIO] con el usuario de GitHub)
git remote add origin git@github.com:[USUARIO]/taller-otree-pgg.git

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
git clone git@github.com:[USUARIO]/taller-otree-pgg.git

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
- [ ] Se visualiza la página "no more pages left" una vez que respondió el cuestionario sin errores. 

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

---

## 3.4 MÓDULO 4: Donovan - Sistema de Castigo (Punishment)

### Objetivo
Implementar una etapa de castigo después de ver los resultados iniciales, donde los participantes pueden pagar para reducir el payoff de otros jugadores, siguiendo el diseño de Fehr & Gächter (2000).

### Flujo de trabajo Git

```bash
# 1. Asegurarse de estar en main actualizado
git checkout main
git pull origin main

# 2. Crear rama de feature
git checkout -b feature/sistema-castigo

# 3. Verificar que estás en la rama correcta
git branch
# Debe mostrar: * feature/sistema-castigo
```

### Prompt sugerido para IA

> **Modelo recomendado:** Claude Opus 4.5  
> **Justificación:** Esta es la tarea más compleja del taller: requiere crear nuevas páginas, modificar la lógica de payoffs, manejar interacciones entre jugadores, y mantener coherencia con el diseño experimental de Fehr & Gächter. Opus 4.5 es superior para tareas multi-archivo con lógica compleja.

```
Eres un economista experimental y desarrollador oTree experto. Tu tarea es implementar el mecanismo de castigo del paper de Fehr & Gächter (2000).

CONTEXTO:
Tengo un Public Goods Game funcionando en oTree 5 con:
- 3 jugadores por grupo
- Dotación de 100 puntos
- Multiplicador configurable (1.2 o 2.0)
- Páginas: Contribute -> ResultsWaitPage -> Results

OBJETIVO:
Agregar una etapa de castigo entre los resultados iniciales y los resultados finales.

DISEÑO DEL CASTIGO (Fehr & Gächter 2000):
1. Después de ver las contribuciones de todos, cada jugador puede asignar "puntos de castigo" a otros jugadores
2. COSTO: Cada punto de castigo cuesta 1 unidad al que castiga
3. EFECTO: Cada punto de castigo reduce 3 unidades al castigado
4. ANONIMATO: Los jugadores no saben quién los castigó
5. LÍMITE: Máximo 10 puntos de castigo por jugador castigado

FLUJO NUEVO:
Contribute -> ResultsWaitPage -> IntermediateResults -> Punishment -> PunishmentWaitPage -> FinalResults

REQUISITOS TÉCNICOS:
1. En IntermediateResults: mostrar contribuciones (sin payoff final aún)
2. En Punishment: interfaz para asignar puntos de castigo a cada otro jugador
3. Necesito campos para:
   - punishment_sent_to_player_X (cuánto castigué a cada uno)
   - punishment_received (total que me castigaron)
   - cost_of_punishment (cuánto gasté castigando)
4. En FinalResults: mostrar payoff final = payoff_inicial - costo_castigo - castigo_recibido*3

CONSIDERACIONES:
- El castigo debe ser a jugadores identificados por número, no por nombre real
- Debo poder identificar a cada jugador sin revelar identidades
- Usar player.id_in_group para identificar jugadores (1, 2, 3)

OUTPUT ESPERADO:
1. Campos nuevos para Player
2. Código completo de las nuevas páginas
3. Templates para IntermediateResults, Punishment, y FinalResults
4. Función para calcular payoffs finales con castigo
5. page_sequence actualizado

Incluye comentarios que expliquen la lógica económica del mecanismo.
```

### Descripción de la tarea

**Archivos a crear/modificar:**
- `public_goods/__init__.py` - Agregar campos, páginas y lógica de castigo
- `public_goods/templates/public_goods/IntermediateResults.html` - Nuevo
- `public_goods/templates/public_goods/Punishment.html` - Nuevo
- `public_goods/templates/public_goods/FinalResults.html` - Nuevo

**Especificaciones:**
1. Ratio de castigo: 1:3 (cuesta 1, reduce 3)
2. Máximo 10 puntos de castigo por jugador
3. El castigo es anónimo
4. Mostrar claramente el impacto del castigo en el payoff final

---

### 💡 HINT (leer solo si llevas más de 15 minutos atascado)

<details>
<summary>Click para ver el hint</summary>

**Para manejar castigo entre jugadores en oTree 5:**

1. **Para el castigo enviado**, necesitas campos dinámicos. Una forma es usar campos separados:
```python
class Player(BasePlayer):
    # Castigo enviado a cada posición (no a IDs específicos)
    punishment_sent_1 = models.IntegerField(min=0, max=10, initial=0)
    punishment_sent_2 = models.IntegerField(min=0, max=10, initial=0)
    # (Para 3 jugadores, solo necesitas castigar a 2 otros)
```

2. **Para identificar a quién castigar**, mapea posiciones:
```python
@staticmethod
def vars_for_template(player):
    others = player.get_others_in_group()
    other_players_info = []
    for p in others:
        other_players_info.append({
            'id_in_group': p.id_in_group,
            'contribution': p.contribution,
        })
    return dict(others=other_players_info)
```

3. **Para calcular castigo recibido**, itera sobre el grupo:
```python
def calculate_punishment(group):
    players = group.get_players()
    for p in players:
        received = 0
        for other in p.get_others_in_group():
            # Obtener cuánto 'other' castigó a 'p'
            field_name = f'punishment_to_{p.id_in_group}'
            received += getattr(other, field_name, 0)
        p.punishment_received = received
```

4. **Alternativa más limpia**: Usa un campo JSON o ExtraModel para almacenar la matriz de castigo.

</details>

---

### ✅ SOLUCIÓN COMPLETA

<details>
<summary>Click para ver la solución completa</summary>

#### Modificaciones completas a `public_goods/__init__.py`

```python
from otree.api import *
import json

doc = """
Public Goods Game con castigo (punishment).
Basado en Fehr & Gächter (2000): "Cooperation and Punishment in Public Goods Experiments"

Diseño del castigo:
- Costo: 1 punto por cada punto de castigo asignado
- Efecto: 3 puntos de reducción por cada punto recibido
- Ratio 1:3 es estándar en la literatura experimental
"""


class C(BaseConstants):
    NAME_IN_URL = 'public_goods'
    PLAYERS_PER_GROUP = 3
    NUM_ROUNDS = 1
    ENDOWMENT = cu(100)
    MULTIPLIER = 2
    
    # Parámetros del castigo
    PUNISHMENT_COST = 1      # Costo por punto de castigo enviado
    PUNISHMENT_EFFECT = 3    # Reducción por punto de castigo recibido
    MAX_PUNISHMENT = 10      # Máximo de puntos de castigo por jugador


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
    
    # Campos de comprensión (del módulo de Mauricio)
    comp_q1 = models.IntegerField(
        label="¿Cuántos puntos recibe cada jugador al inicio?"
    )
    comp_q2 = models.IntegerField(
        label="Si todos contribuyen 50 puntos, ¿cuánto habrá en el fondo antes de multiplicar?",
        choices=[[50, '50'], [100, '100'], [150, '150'], [200, '200']]
    )
    comp_q3 = models.IntegerField(
        label="Si el fondo multiplicado tiene 300 puntos, ¿cuánto recibe cada jugador?",
        choices=[[50, '50'], [100, '100'], [150, '150'], [300, '300']]
    )
    
    # Payoff intermedio (antes del castigo)
    intermediate_payoff = models.CurrencyField()
    
    # Castigo enviado a cada otro jugador
    # Usamos campos explícitos para los 2 otros jugadores posibles
    punishment_to_1 = models.IntegerField(
        min=0, max=C.MAX_PUNISHMENT, initial=0,
        label="Puntos de castigo para Jugador 1"
    )
    punishment_to_2 = models.IntegerField(
        min=0, max=C.MAX_PUNISHMENT, initial=0,
        label="Puntos de castigo para Jugador 2"
    )
    punishment_to_3 = models.IntegerField(
        min=0, max=C.MAX_PUNISHMENT, initial=0,
        label="Puntos de castigo para Jugador 3"
    )
    
    # Totales de castigo
    total_punishment_sent = models.IntegerField(initial=0)
    total_punishment_received = models.IntegerField(initial=0)
    cost_of_punishment = models.CurrencyField(initial=0)
    punishment_deduction = models.CurrencyField(initial=0)


# FUNCIONES AUXILIARES

def get_punishment_field_name(target_id):
    """Retorna el nombre del campo de castigo para un jugador específico."""
    return f'punishment_to_{target_id}'


def get_punishment_to(player, target_id):
    """Obtiene cuánto castigo envió player al jugador con target_id."""
    field_name = get_punishment_field_name(target_id)
    return getattr(player, field_name, 0)


def set_punishment_to(player, target_id, value):
    """Establece el castigo de player hacia target_id."""
    field_name = get_punishment_field_name(target_id)
    setattr(player, field_name, value)


# FUNCIONES DE GRUPO

def set_payoffs(group: Group):
    """
    Calcula payoffs intermedios (antes del castigo).
    Payoff = Endowment - Contribution + Share
    """
    session = group.session
    endowment = session.config.get('endowment', C.ENDOWMENT)
    multiplier = session.config.get('multiplier', C.MULTIPLIER)
    n_players = session.config.get('players_per_group', C.PLAYERS_PER_GROUP)
    
    players = group.get_players()
    contributions = [p.contribution for p in players]
    group.total_contribution = sum(contributions)
    group.individual_share = (group.total_contribution * multiplier) / n_players
    
    for p in players:
        p.intermediate_payoff = endowment - p.contribution + group.individual_share


def calculate_final_payoffs(group: Group):
    """
    Calcula payoffs finales después del castigo.
    
    Final Payoff = Intermediate Payoff - Cost of Punishment Sent - (Punishment Received × Effect)
    
    Donde:
    - Cost of Punishment Sent = Total puntos enviados × PUNISHMENT_COST
    - Punishment Received × Effect = Total puntos recibidos × PUNISHMENT_EFFECT
    """
    players = group.get_players()
    
    # Primero, calcular castigo enviado y recibido para cada jugador
    for player in players:
        # Calcular total de castigo enviado
        sent = 0
        for other in player.get_others_in_group():
            sent += get_punishment_to(player, other.id_in_group)
        player.total_punishment_sent = sent
        player.cost_of_punishment = sent * C.PUNISHMENT_COST
        
        # Calcular total de castigo recibido
        received = 0
        for other in player.get_others_in_group():
            received += get_punishment_to(other, player.id_in_group)
        player.total_punishment_received = received
        player.punishment_deduction = received * C.PUNISHMENT_EFFECT
    
    # Calcular payoff final
    for player in players:
        player.payoff = (
            player.intermediate_payoff 
            - player.cost_of_punishment 
            - player.punishment_deduction
        )
        # Asegurar que el payoff no sea negativo
        if player.payoff < 0:
            player.payoff = cu(0)


# PAGES

class Introduction(Page):
    """Página de instrucciones."""
    pass


class Comprehension(Page):
    """Preguntas de comprensión."""
    form_model = 'player'
    form_fields = ['comp_q1', 'comp_q2', 'comp_q3']
    
    @staticmethod
    def error_message(player, values):
        solutions = {'comp_q1': 100, 'comp_q2': 150, 'comp_q3': 100}
        errors = []
        for field, correct in solutions.items():
            if values[field] != correct:
                errors.append(f"Revisa tu respuesta a la pregunta sobre {field}.")
        if errors:
            return ' '.join(errors)


class Contribute(Page):
    """Página de contribución."""
    form_model = 'player'
    form_fields = ['contribution']
    
    @staticmethod
    def vars_for_template(player):
        session = player.session
        return dict(
            endowment=session.config.get('endowment', C.ENDOWMENT),
            multiplier=session.config.get('multiplier', C.MULTIPLIER),
        )


class ResultsWaitPage(WaitPage):
    """Espera a que todos contribuyan."""
    after_all_players_arrive = set_payoffs


class IntermediateResults(Page):
    """
    Muestra resultados iniciales antes del castigo.
    Los jugadores ven las contribuciones de todos pero no el payoff final.
    """
    @staticmethod
    def vars_for_template(player):
        group = player.group
        session = player.session
        
        endowment = session.config.get('endowment', C.ENDOWMENT)
        multiplier = session.config.get('multiplier', C.MULTIPLIER)
        
        # Información de otros jugadores
        players_info = []
        for p in group.get_players():
            players_info.append({
                'id': p.id_in_group,
                'contribution': float(p.contribution),
                'is_self': p.id == player.id,
                'label': 'Tú' if p.id == player.id else f'Jugador {p.id_in_group}',
            })
        
        return dict(
            players_info=players_info,
            total_contribution=group.total_contribution,
            multiplied_fund=float(group.total_contribution) * multiplier,
            individual_share=group.individual_share,
            intermediate_payoff=player.intermediate_payoff,
            endowment=endowment,
            multiplier=multiplier,
            punishment_cost=C.PUNISHMENT_COST,
            punishment_effect=C.PUNISHMENT_EFFECT,
            max_punishment=C.MAX_PUNISHMENT,
        )


class Punishment(Page):
    """
    Página de castigo.
    Cada jugador puede asignar puntos de castigo a los otros jugadores.
    """
    form_model = 'player'
    
    @staticmethod
    def get_form_fields(player):
        """Genera dinámicamente los campos según los otros jugadores."""
        others = player.get_others_in_group()
        return [f'punishment_to_{p.id_in_group}' for p in others]
    
    @staticmethod
    def vars_for_template(player):
        others = player.get_others_in_group()
        others_info = []
        for p in others:
            others_info.append({
                'id': p.id_in_group,
                'contribution': float(p.contribution),
                'field_name': f'punishment_to_{p.id_in_group}',
            })
        
        return dict(
            others_info=others_info,
            my_intermediate_payoff=player.intermediate_payoff,
            punishment_cost=C.PUNISHMENT_COST,
            punishment_effect=C.PUNISHMENT_EFFECT,
            max_punishment=C.MAX_PUNISHMENT,
        )
    
    @staticmethod
    def error_message(player, values):
        """Valida que el jugador tenga suficientes puntos para castigar."""
        total_punishment = sum(values.values())
        cost = total_punishment * C.PUNISHMENT_COST
        
        if cost > player.intermediate_payoff:
            return f'No tienes suficientes puntos. El costo total del castigo ({cost}) excede tu payoff intermedio ({player.intermediate_payoff}).'


class PunishmentWaitPage(WaitPage):
    """Espera a que todos decidan su castigo."""
    after_all_players_arrive = calculate_final_payoffs


class FinalResults(Page):
    """
    Muestra resultados finales después del castigo.
    Incluye desglose completo del cálculo.
    """
    @staticmethod
    def vars_for_template(player):
        group = player.group
        session = player.session
        
        # Info de todos los jugadores para la tabla
        players_final = []
        for p in group.get_players():
            players_final.append({
                'id': p.id_in_group,
                'contribution': float(p.contribution),
                'intermediate_payoff': float(p.intermediate_payoff),
                'punishment_sent': p.total_punishment_sent,
                'punishment_received': p.total_punishment_received,
                'cost': float(p.cost_of_punishment),
                'deduction': float(p.punishment_deduction),
                'final_payoff': float(p.payoff),
                'is_self': p.id == player.id,
            })
        
        return dict(
            players_final=players_final,
            my_contribution=player.contribution,
            intermediate_payoff=player.intermediate_payoff,
            punishment_sent=player.total_punishment_sent,
            punishment_received=player.total_punishment_received,
            cost_of_punishment=player.cost_of_punishment,
            punishment_deduction=player.punishment_deduction,
            final_payoff=player.payoff,
            punishment_cost_ratio=C.PUNISHMENT_COST,
            punishment_effect_ratio=C.PUNISHMENT_EFFECT,
        )


# Legacy Results page (puede eliminarse o mantener para compatibilidad)
class Results(Page):
    """Página de resultados legacy - redirige a FinalResults si hay castigo."""
    @staticmethod
    def is_displayed(player):
        return False  # No mostrar, usamos FinalResults


page_sequence = [
    Introduction,
    Comprehension,
    Contribute,
    ResultsWaitPage,
    IntermediateResults,
    Punishment,
    PunishmentWaitPage,
    FinalResults,
]
```

#### Template: `IntermediateResults.html`

Crear `public_goods/templates/public_goods/IntermediateResults.html`:

```html
{{ block title }}
    Resultados de Contribuciones
{{ endblock }}

{{ block styles }}
<style>
    .results-box {
        background: #f8f9fa;
        border-radius: 8px;
        padding: 20px;
        margin: 15px 0;
    }
    .highlight {
        background: #e3f2fd;
        border-left: 4px solid #2196f3;
        padding: 15px;
        margin: 15px 0;
    }
    .warning-box {
        background: #fff3e0;
        border-left: 4px solid #ff9800;
        padding: 15px;
        margin: 15px 0;
    }
    table {
        width: 100%;
        border-collapse: collapse;
        margin: 15px 0;
    }
    th, td {
        padding: 12px;
        text-align: center;
        border: 1px solid #ddd;
    }
    th {
        background: #4caf50;
        color: white;
    }
    tr.is-self {
        background: #e8f5e9;
        font-weight: bold;
    }
</style>
{{ endblock }}

{{ block content }}
<div class="results-box">
    <h3>📊 Contribuciones del Grupo</h3>
    
    <table>
        <thead>
            <tr>
                <th>Jugador</th>
                <th>Contribución</th>
            </tr>
        </thead>
        <tbody>
            {{ for p in players_info }}
            <tr class="{{ if p.is_self }}is-self{{ endif }}">
                <td>{{ p.label }}</td>
                <td>{{ p.contribution }} puntos</td>
            </tr>
            {{ endfor }}
        </tbody>
    </table>
    
    <div class="highlight">
        <strong>Total contribuido:</strong> {{ total_contribution }}<br>
        <strong>Fondo multiplicado (×{{ multiplier }}):</strong> {{ multiplied_fund }} puntos<br>
        <strong>Tu parte del fondo:</strong> {{ individual_share }}
    </div>
    
    <p><strong>Tu payoff intermedio:</strong> {{ intermediate_payoff }}</p>
</div>

<div class="warning-box">
    <h4>⚠️ Etapa de Castigo</h4>
    <p>A continuación, tendrás la oportunidad de <strong>castigar</strong> a otros jugadores si lo deseas.</p>
    <ul>
        <li><strong>Costo:</strong> Cada punto de castigo te cuesta {{ punishment_cost }} punto</li>
        <li><strong>Efecto:</strong> Cada punto de castigo reduce {{ punishment_effect }} puntos al jugador castigado</li>
        <li><strong>Máximo:</strong> Puedes asignar hasta {{ max_punishment }} puntos de castigo por jugador</li>
        <li><strong>Anonimato:</strong> Los jugadores no sabrán quién los castigó</li>
    </ul>
</div>

{{ next_button }}
{{ endblock }}
```

#### Template: `Punishment.html`

Crear `public_goods/templates/public_goods/Punishment.html`:

```html
{{ block title }}
    Etapa de Castigo
{{ endblock }}

{{ block styles }}
<style>
    .punishment-container {
        max-width: 600px;
        margin: 0 auto;
    }
    .player-card {
        background: #f8f9fa;
        border-radius: 8px;
        padding: 20px;
        margin: 15px 0;
        border-left: 4px solid #2196f3;
    }
    .info-box {
        background: #e3f2fd;
        padding: 15px;
        border-radius: 5px;
        margin-bottom: 20px;
    }
    .cost-warning {
        color: #d32f2f;
        font-size: 0.9em;
        margin-top: 5px;
    }
    input[type="number"] {
        width: 80px;
        padding: 8px;
        font-size: 16px;
        text-align: center;
    }
    .slider-container {
        display: flex;
        align-items: center;
        gap: 10px;
    }
</style>
{{ endblock }}

{{ block content }}
<div class="punishment-container">
    <div class="info-box">
        <p><strong>Tu payoff actual:</strong> {{ my_intermediate_payoff }}</p>
        <p>
            Recuerda: Cada punto de castigo te cuesta <strong>{{ punishment_cost }}</strong> punto 
            y reduce <strong>{{ punishment_effect }}</strong> puntos al jugador castigado.
        </p>
    </div>
    
    <h3>¿Deseas castigar a algún jugador?</h3>
    <p><em>Puedes dejar en 0 si no deseas castigar.</em></p>
    
    {{ for other in others_info }}
    <div class="player-card">
        <h4>Jugador {{ other.id }}</h4>
        <p>Contribuyó: <strong>{{ other.contribution }} puntos</strong></p>
        
        <label>Puntos de castigo (0-{{ max_punishment }}):</label>
        <div class="slider-container">
            {{ formfield other.field_name }}
        </div>
        <p class="cost-warning">
            Costo para ti: <span id="cost_{{ other.id }}">0</span> puntos
        </p>
    </div>
    {{ endfor }}
    
    <div style="background: #ffebee; padding: 15px; border-radius: 5px; margin-top: 20px;">
        <strong>Costo total de castigo:</strong> <span id="total_cost">0</span> puntos
    </div>
    
    {{ next_button }}
</div>
{{ endblock }}

{{ block scripts }}
<script>
    // Actualizar costos en tiempo real
    const costPerPoint = {{ punishment_cost }};
    const inputs = document.querySelectorAll('input[type="number"]');
    
    function updateCosts() {
        let total = 0;
        inputs.forEach(input => {
            const value = parseInt(input.value) || 0;
            const playerId = input.name.split('_').pop();
            const costSpan = document.getElementById('cost_' + playerId);
            if (costSpan) {
                costSpan.textContent = value * costPerPoint;
            }
            total += value * costPerPoint;
        });
        document.getElementById('total_cost').textContent = total;
    }
    
    inputs.forEach(input => {
        input.addEventListener('input', updateCosts);
    });
    
    updateCosts();
</script>
{{ endblock }}
```

#### Template: `FinalResults.html`

Crear `public_goods/templates/public_goods/FinalResults.html`:

```html
{{ block title }}
    Resultados Finales
{{ endblock }}

{{ block styles }}
<style>
    .final-container {
        max-width: 800px;
        margin: 0 auto;
    }
    .section {
        background: #f8f9fa;
        border-radius: 8px;
        padding: 20px;
        margin: 20px 0;
    }
    .section h3 {
        margin-top: 0;
        border-bottom: 2px solid #4caf50;
        padding-bottom: 10px;
    }
    table {
        width: 100%;
        border-collapse: collapse;
        margin: 15px 0;
        font-size: 14px;
    }
    th, td {
        padding: 10px;
        text-align: center;
        border: 1px solid #ddd;
    }
    th {
        background: #4caf50;
        color: white;
    }
    tr.is-self {
        background: #e8f5e9;
        font-weight: bold;
    }
    .calculation {
        background: #fff;
        border: 1px solid #ddd;
        border-radius: 5px;
        padding: 15px;
        margin: 15px 0;
    }
    .calc-row {
        display: flex;
        justify-content: space-between;
        padding: 8px 0;
        border-bottom: 1px dashed #eee;
    }
    .calc-row:last-child {
        border-bottom: none;
        font-weight: bold;
        font-size: 1.2em;
        color: #4caf50;
    }
    .calc-row.negative {
        color: #d32f2f;
    }
    .final-payoff-box {
        background: linear-gradient(135deg, #4caf50, #8bc34a);
        color: white;
        padding: 30px;
        border-radius: 10px;
        text-align: center;
        margin-top: 20px;
    }
    .final-payoff-box h2 {
        margin: 0;
        font-size: 2em;
    }
</style>
{{ endblock }}

{{ block content }}
<div class="final-container">
    
    <!-- Tabla resumen de todos los jugadores -->
    <div class="section">
        <h3>📊 Resumen del Grupo</h3>
        <table>
            <thead>
                <tr>
                    <th>Jugador</th>
                    <th>Contribución</th>
                    <th>Payoff Intermedio</th>
                    <th>Castigo Enviado</th>
                    <th>Castigo Recibido</th>
                    <th>Payoff Final</th>
                </tr>
            </thead>
            <tbody>
                {{ for p in players_final }}
                <tr class="{{ if p.is_self }}is-self{{ endif }}">
                    <td>{{ if p.is_self }}Tú{{ else }}Jugador {{ p.id }}{{ endif }}</td>
                    <td>{{ p.contribution }}</td>
                    <td>{{ p.intermediate_payoff }}</td>
                    <td>{{ p.punishment_sent }} pts (costo: {{ p.cost }})</td>
                    <td>{{ p.punishment_received }} pts (deducción: {{ p.deduction }})</td>
                    <td><strong>{{ p.final_payoff }}</strong></td>
                </tr>
                {{ endfor }}
            </tbody>
        </table>
    </div>
    
    <!-- Desglose de tu cálculo -->
    <div class="section">
        <h3>🧮 Tu Cálculo Detallado</h3>
        
        <div class="calculation">
            <div class="calc-row">
                <span>Payoff intermedio (antes del castigo):</span>
                <span>{{ intermediate_payoff }}</span>
            </div>
            
            <div class="calc-row negative">
                <span>Costo de castigo enviado ({{ punishment_sent }} × {{ punishment_cost_ratio }}):</span>
                <span>- {{ cost_of_punishment }}</span>
            </div>
            
            <div class="calc-row negative">
                <span>Deducción por castigo recibido ({{ punishment_received }} × {{ punishment_effect_ratio }}):</span>
                <span>- {{ punishment_deduction }}</span>
            </div>
            
            <div class="calc-row">
                <span>TU PAYOFF FINAL:</span>
                <span>{{ final_payoff }}</span>
            </div>
        </div>
    </div>
    
    <!-- Información sobre el castigo -->
    <div class="section">
        <h3>📝 Información sobre el Castigo</h3>
        <ul>
            <li><strong>Castigo que enviaste:</strong> {{ punishment_sent }} puntos (te costó {{ cost_of_punishment }})</li>
            <li><strong>Castigo que recibiste:</strong> {{ punishment_received }} puntos (te dedujeron {{ punishment_deduction }})</li>
        </ul>
        <p><em>Nota: El castigo es anónimo. No puedes saber quién te castigó.</em></p>
    </div>
    
    <!-- Payoff final destacado -->
    <div class="final-payoff-box">
        <p>Tu ganancia final en esta ronda:</p>
        <h2>{{ player.payoff }}</h2>
    </div>
    
</div>

{{ next_button }}
{{ endblock }}
```

#### Commits sugeridos

```bash
# Después de agregar campos y lógica
git add public_goods/__init__.py
git commit -m "feat(public_goods): implementa sistema de castigo Fehr-Gächter

- Agrega campos de castigo enviado/recibido
- Implementa cálculo de payoffs con castigo (ratio 1:3)
- Agrega páginas IntermediateResults, Punishment, FinalResults
- Documenta mecanismo económico en comentarios"

# Templates
git add public_goods/templates/public_goods/IntermediateResults.html
git commit -m "feat(public_goods): crea template IntermediateResults"

git add public_goods/templates/public_goods/Punishment.html
git commit -m "feat(public_goods): crea template Punishment con cálculo dinámico de costos"

git add public_goods/templates/public_goods/FinalResults.html
git commit -m "feat(public_goods): crea template FinalResults con desglose completo"

# Push
git push -u origin feature/sistema-castigo
```

</details>

---

### Verificación local

```bash
# Iniciar servidor
otree devserver

# Probar el flujo completo con 3 navegadores/pestañas
# 1. Cada jugador contribuye diferentes cantidades
# 2. Ver que IntermediateResults muestra contribuciones correctas
# 3. Asignar diferentes cantidades de castigo
# 4. Verificar que FinalResults muestra cálculos correctos

# Casos de prueba:
# - Jugador que no castiga (costo = 0)
# - Jugador que castiga al máximo (verificar límite de puntos)
# - Jugador que recibe castigo de múltiples jugadores
```

### Crear Pull Request

**Title:**
```
feat(public_goods): Implementa sistema de castigo basado en Fehr & Gächter (2000)
```

**Body:**
```markdown
## Descripción
Implementa la etapa de castigo del Public Goods Game siguiendo el diseño de Fehr & Gächter (2000).

## Diseño del mecanismo
| Parámetro | Valor |
|-----------|-------|
| Costo por punto | 1 |
| Efecto por punto | 3 |
| Ratio | 1:3 |
| Máximo por jugador | 10 puntos |

## Nuevo flujo
```
Contribute → ResultsWaitPage → IntermediateResults → Punishment → PunishmentWaitPage → FinalResults
```

## Cambios
- Nuevos campos: `punishment_to_X`, `total_punishment_sent/received`, `cost_of_punishment`, etc.
- Función `calculate_final_payoffs` con lógica de castigo
- 3 nuevos templates con UI interactiva
- JavaScript para cálculo dinámico de costos en Punishment

## Testing
- [x] Cálculos de payoff correctos
- [x] Límite de castigo funciona
- [x] Validación de puntos suficientes
- [x] UI responsiva

## Referencias
- Fehr, E., & Gächter, S. (2000). Cooperation and punishment in public goods experiments. American Economic Review.

Closes #4
```

---

# PARTE 4: INTEGRACIÓN - PULL REQUESTS Y CODE REVIEW

## 4.1 Flujo de Pull Requests

### Anatomía de un buen Pull Request

```
┌─────────────────────────────────────────────────────────────┐
│  TÍTULO: tipo(scope): descripción concisa                   │
│  Ejemplo: feat(public_goods): agrega sistema de castigo     │
├─────────────────────────────────────────────────────────────┤
│  DESCRIPCIÓN:                                               │
│  ## ¿Qué hace este PR?                                      │
│  - Resumen de cambios                                       │
│                                                             │
│  ## ¿Por qué?                                               │
│  - Contexto y motivación                                    │
│                                                             │
│  ## ¿Cómo probarlo?                                         │
│  - Pasos para verificar                                     │
│                                                             │
│  ## Screenshots (si aplica)                                 │
│  - Capturas de UI                                           │
│                                                             │
│  Closes #N                                                  │
├─────────────────────────────────────────────────────────────┤
│  LABELS: feature, priority: high, assigned: nombre          │
│  REVIEWERS: @colaborador1, @colaborador2                    │
│  MILESTONE: v1.0 - MVP Public Goods Game                    │
└─────────────────────────────────────────────────────────────┘
```

### Proceso paso a paso

1. **Antes de crear el PR:**
   ```bash
   # Asegurarse de que la rama está actualizada
   git checkout main
   git pull origin main
   git checkout feature/mi-rama
   git rebase main  # O merge, según preferencia del equipo
   ```

2. **Resolver conflictos si los hay:**
   ```bash
   # Si hay conflictos durante rebase
   # Editar archivos conflictivos
   git add .
   git rebase --continue
   ```

3. **Push y crear PR:**
   ```bash
   git push -u origin feature/mi-rama
   # Ir a GitHub y crear PR
   ```

---

## 4.2 Checklist de Code Review

### Para el Reviewer

Usar esta checklist al revisar PRs:

#### Funcionalidad
- [ ] ¿El código hace lo que dice el issue?
- [ ] ¿Funciona correctamente? (probarlo localmente)
- [ ] ¿Maneja casos edge correctamente?
- [ ] ¿Los mensajes de error son claros?

#### Código
- [ ] ¿El código es legible y bien organizado?
- [ ] ¿Los nombres de variables/funciones son descriptivos?
- [ ] ¿Hay código duplicado que debería refactorizarse?
- [ ] ¿Los comentarios son útiles (no obvios)?

#### oTree específico
- [ ] ¿Se usa la sintaxis de oTree 5 (no oTree 3)?
- [ ] ¿Los campos del modelo tienen validación apropiada?
- [ ] ¿Los templates usan la sintaxis correcta de Django/Jinja2?
- [ ] ¿El `page_sequence` está en orden lógico?

#### Testing
- [ ] ¿Se puede probar el flujo completo sin errores?
- [ ] ¿El servidor inicia correctamente (`otree devserver`)?
- [ ] ¿Los datos se guardan correctamente?

### Cómo dejar feedback

**Comentarios constructivos:**
```
✅ BIEN: "Este cálculo podría dar error si contribution es None. 
         ¿Qué tal agregar validación?"

❌ MAL:  "Esto está mal."
```

**Tipos de comentarios:**
- 🔴 **Blocker:** Debe arreglarse antes de merge
- 🟡 **Sugerencia:** Mejoraría el código pero no es crítico
- 🟢 **Nitpick:** Estilo o preferencia personal
- 💬 **Pregunta:** Aclaración o duda

---

## 4.3 Proceso de Merge

### Antes del merge (checklist)

- [ ] Al menos 1 aprobación de reviewer
- [ ] Todos los comentarios resueltos
- [ ] CI/CD pasa (cuando esté configurado)
- [ ] Rama actualizada con main

### Tipos de merge

| Tipo | Cuándo usar | Comando |
|------|-------------|---------|
| **Merge commit** | PRs con múltiples commits importantes | Botón "Merge" en GitHub |
| **Squash and merge** | PRs con muchos commits pequeños | Botón "Squash and merge" |
| **Rebase and merge** | Mantener historial lineal | Botón "Rebase and merge" |

**Recomendación para este taller:** Usar **Squash and merge** para mantener historial limpio.

### Después del merge

```bash
# Localmente, actualizar main
git checkout main
git pull origin main

# Eliminar rama local (opcional)
git branch -d feature/mi-rama

# Eliminar rama remota (si no se borró automáticamente)
git push origin --delete feature/mi-rama
```

---

## 4.4 Ejercicio Práctico: Review Cruzado

### Asignación de reviews

| PR de | Lo revisa |
|-------|-----------|
| Mauricio (Instrucciones) | José Miguel |
| José Miguel (Parámetros) | Sergio |
| Sergio (Resultados) | Donovan |
| Donovan (Castigo) | Mauricio |

### Pasos del ejercicio

1. **Cada persona crea su PR** (siguiendo el módulo asignado)

2. **Ir al PR asignado para review:**
   - GitHub → Pull requests → Seleccionar el PR asignado
   - Click en "Files changed"

3. **Hacer checkout de la rama para probar localmente:**
   ```bash
   git fetch origin
   git checkout -b review/nombre-feature origin/feature/nombre-feature
   otree devserver
   # Probar la funcionalidad
   ```

4. **Dejar al menos 3 comentarios:**
   - 1 comentario positivo (algo que está bien hecho)
   - 1 pregunta o sugerencia
   - 1 comentario sobre algo que mejorar (si aplica)

5. **Aprobar o solicitar cambios:**
   - "Files changed" → "Review changes"
   - Escribir resumen
   - Seleccionar: Approve / Request changes / Comment
   - Submit review

6. **El autor del PR:**
   - Responde a comentarios
   - Hace cambios si es necesario
   - Push de cambios adicionales
   - Re-solicita review si hubo "Request changes"

7. **Merge cuando esté aprobado**

---

# PARTE 5: RESOLUCIÓN DE CONFLICTOS

## 5.1 ¿Cuándo ocurren conflictos?

Los conflictos ocurren cuando:
- Dos personas modifican las mismas líneas del mismo archivo
- Una persona elimina un archivo que otra modificó
- Cambios en archivos binarios

### Escenarios de conflicto en este taller

| Escenario | Probabilidad | Archivos |
|-----------|--------------|----------|
| Modificación de `page_sequence` | Alta | `__init__.py` |
| Cambios en clase `Player` | Media | `__init__.py` |
| Imports duplicados | Baja | `__init__.py` |
| Cambios en `settings.py` | Media | `settings.py` |

---

## 5.2 Ejercicio: Conflicto Simulado

### Setup del conflicto

**Facilitador:** Crear un commit en main que modifique `page_sequence`:

```bash
git checkout main
# Editar __init__.py para agregar una página dummy
git add .
git commit -m "chore: agrega página temporal para simular conflicto"
git push origin main
```

**Participante (ej. Mauricio):**
```bash
git checkout feature/instrucciones-comprension
git fetch origin
git rebase origin/main
# Conflicto aparecerá
```

### Anatomía de un conflicto

```python
page_sequence = [
<<<<<<< HEAD
    Introduction,
    Comprehension,
    Contribute,
=======
    DummyPage,  # Página del facilitador
    Contribute,
>>>>>>> origin/main
    ResultsWaitPage,
    Results,
]
```

**Explicación:**
- `<<<<<<< HEAD`: Tu versión (la rama actual)
- `=======`: Separador
- `>>>>>>> origin/main`: Versión de main

### Resolución paso a paso

1. **Identificar el conflicto:**
   ```bash
   git status
   # Muestra: "both modified: public_goods/__init__.py"
   ```

2. **Abrir el archivo en VS Code:**
   - VS Code resalta los conflictos
   - Opciones: "Accept Current", "Accept Incoming", "Accept Both"

3. **Resolver manualmente (combinar ambos cambios):**
   ```python
   page_sequence = [
       Introduction,      # De tu rama
       Comprehension,     # De tu rama
       DummyPage,         # De main (si queremos mantenerlo)
       Contribute,
       ResultsWaitPage,
       Results,
   ]
   ```

4. **Marcar como resuelto:**
   ```bash
   git add public_goods/__init__.py
   git rebase --continue
   # O si era merge: git commit
   ```

5. **Push forzado (si rebaseaste):**
   ```bash
   git push --force-with-lease origin feature/instrucciones-comprension
   ```

---

## 5.3 Escenarios de Conflicto Adicionales

### Escenario A: Conflicto en constants.py

**Situación:** Mauricio y José Miguel ambos agregan constantes.

```python
# Mauricio agrega:
COMPREHENSION_REQUIRED = True

# José Miguel agrega:
DEFAULT_MULTIPLIER = 2.0
```

**Resolución:** Mantener ambas constantes, ordenar alfabéticamente.

### Escenario B: Conflicto en imports

**Situación:** Sergio y Donovan ambos agregan imports.

```python
<<<<<<< HEAD
import json
from otree.api import *
=======
from otree.api import *
import math
>>>>>>> origin/main
```

**Resolución:**
```python
import json
import math
from otree.api import *
```

### Escenario C: Conflicto en settings.py SESSION_CONFIGS

**Situación:** Múltiples tratamientos agregados por diferentes personas.

```python
<<<<<<< HEAD
SESSION_CONFIGS = [
    dict(name='high_mpcr', ...),
    dict(name='low_mpcr', ...),
]
=======
SESSION_CONFIGS = [
    dict(name='with_punishment', ...),
]
>>>>>>> origin/main
```

**Resolución:** Combinar todos los tratamientos:
```python
SESSION_CONFIGS = [
    dict(name='high_mpcr', ...),
    dict(name='low_mpcr', ...),
    dict(name='with_punishment', ...),
]
```

---

## 5.4 Mejores Prácticas para Evitar Conflictos

1. **Comunicación:** Avisar cuando trabajas en un archivo compartido
2. **Pull frecuente:** `git pull origin main` antes de empezar a trabajar
3. **Commits pequeños:** Más fáciles de mergear
4. **Archivos separados:** Cuando sea posible, trabajar en archivos diferentes
5. **Merge/rebase frecuente:** No dejar que las ramas diverjan mucho

---

# PARTE 6: GITHUB ACTIONS (CI/CD)

## 6.1 Introducción a GitHub Actions

GitHub Actions permite automatizar tareas cuando ocurren eventos en el repositorio.

### Conceptos clave

| Concepto | Descripción |
|----------|-------------|
| **Workflow** | Proceso automatizado definido en YAML |
| **Job** | Conjunto de steps que se ejecutan en el mismo runner |
| **Step** | Tarea individual (ejecutar comando, usar action) |
| **Action** | Componente reutilizable (ej. checkout, setup-python) |
| **Runner** | Servidor que ejecuta los workflows |
| **Event** | Disparador del workflow (push, PR, etc.) |

---

## 6.2 Crear el Workflow CI

### Paso 1: Crear estructura de directorios

```bash
mkdir -p .github/workflows
```

### Paso 2: Crear archivo de workflow

Crear `.github/workflows/ci.yml`:

```yaml
# .github/workflows/ci.yml
name: CI - oTree Public Goods

# Cuándo ejecutar el workflow
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

# Trabajos a ejecutar
jobs:
  lint-and-test:
    name: Lint & Test
    runs-on: ubuntu-latest
    
    steps:
      # 1. Checkout del código
      - name: Checkout repository
        uses: actions/checkout@v4
      
      # 2. Configurar Python
      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.11'
      
      # 3. Instalar dependencias
      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install otree
          pip install ruff  # Linter moderno y rápido
      
      # 4. Verificar sintaxis Python con Ruff
      - name: Lint with Ruff
        run: |
          ruff check . --select=E,F --ignore=E501
      
      # 5. Verificar que oTree puede leer la configuración
      - name: Validate oTree config
        run: |
          otree check
      
      # 6. Intentar iniciar el servidor (test básico)
      - name: Test server startup
        run: |
          timeout 10 otree devserver || code=$?
          if [ $code -eq 124 ]; then
            echo "Server started successfully (timed out as expected)"
            exit 0
          else
            echo "Server failed to start"
            exit 1
          fi
```

### Prompt sugerido para personalizar el workflow

> **Modelo recomendado:** Claude Sonnet 4.5  
> **Justificación:** Configuración de CI/CD es una tarea bien definida con patrones establecidos. Sonnet es suficiente y más rápido.

```
Eres un experto en GitHub Actions y CI/CD para proyectos Python/oTree.

CONTEXTO:
Tengo un workflow básico de CI para oTree que hace:
- Lint con Ruff
- Validación de config con `otree check`
- Test de inicio de servidor

NECESITO AGREGAR:
1. Cache de pip para acelerar builds
2. Ejecutar `otree test` si hay bots definidos
3. Notificación de Slack cuando falla (opcional)
4. Badge de status para el README

RESTRICCIONES:
- Usar acciones oficiales de GitHub cuando sea posible
- Mantener el workflow simple y legible
- El workflow debe completar en menos de 3 minutos

OUTPUT:
Archivo ci.yml completo con las mejoras solicitadas.
```

---

## 6.3 Configuración Adicional: Ruff

Crear archivo de configuración `ruff.toml` en la raíz:

```toml
# ruff.toml
# Configuración de Ruff para el proyecto oTree

[lint]
# Reglas a verificar
select = [
    "E",    # pycodestyle errors
    "F",    # pyflakes
    "I",    # isort (imports)
    "B",    # flake8-bugbear
]

# Reglas a ignorar
ignore = [
    "E501",  # line too long (oTree a veces tiene líneas largas)
    "E402",  # module level import not at top (común en oTree)
]

# Archivos a excluir
exclude = [
    "__pycache__",
    ".git",
    "db.sqlite3",
    "_static_root",
]

[lint.per-file-ignores]
# Ignorar import order en __init__.py de oTree
"*/__init__.py" = ["I001"]
```

---

## 6.4 Agregar Badge al README

Crear o actualizar `README.md`:

```markdown
# Taller Git/GitHub - Public Goods Game

![CI Status](https://github.com/[USUARIO]/taller-otree-pgg/actions/workflows/ci.yml/badge.svg)

## Descripción

Implementación del Public Goods Game con sistema de castigo para el taller de Git/GitHub.

## Instalación

```bash
git clone git@github.com:[USUARIO]/taller-otree-pgg.git
cd taller-otree-pgg
pip install otree
otree devserver
```

## Tratamientos

| Nombre | MPCR | Multiplicador |
|--------|------|---------------|
| high_mpcr | 0.67 | 2.0 |
| low_mpcr | 0.40 | 1.2 |

## Estructura del juego

1. Instrucciones
2. Preguntas de comprensión
3. Contribución
4. Resultados intermedios
5. Etapa de castigo
6. Resultados finales

## Referencia

Fehr, E., & Gächter, S. (2000). Cooperation and punishment in public goods experiments. American Economic Review, 90(4), 980-994.
```

---

## 6.5 Verificar GitHub Actions

### En GitHub

1. Ir a **Actions** tab en el repositorio
2. Ver el workflow "CI - oTree Public Goods"
3. Click en un run para ver detalles

### Verificar status checks en PRs

1. Crear un PR
2. En la parte inferior, debería aparecer:
   - ✅ `lint-and-test` — Successful
   - O ❌ con detalles del error

### Troubleshooting común

| Error | Causa | Solución |
|-------|-------|----------|
| `otree: command not found` | pip install falló | Verificar `requirements.txt` |
| Lint errors | Código no sigue estilo | Ejecutar `ruff check --fix .` localmente |
| `otree check` falla | Error en settings.py | Verificar SESSION_CONFIGS |
| Timeout en server test | Servidor no inicia | Revisar imports y errores de sintaxis |

---

## 6.6 Commits para GitHub Actions

```bash
# Crear estructura
mkdir -p .github/workflows

# Agregar workflow
git add .github/workflows/ci.yml
git commit -m "ci: agrega workflow de CI con lint y validación oTree"

# Agregar configuración de Ruff
git add ruff.toml
git commit -m "chore: agrega configuración de Ruff linter"

# Actualizar README
git add README.md
git commit -m "docs: agrega README con badge de CI y documentación"

# Push
git push origin main
```

---

# PARTE 7: EJERCICIO FINAL DE INTEGRACIÓN

## 7.1 Objetivo

Integrar todos los módulos en una versión funcional del Public Goods Game con:
- ✅ Instrucciones y comprensión
- ✅ Parámetros configurables
- ✅ Visualización de resultados
- ✅ Sistema de castigo
- ✅ CI/CD funcionando

## 7.2 Orden de merge recomendado

Para minimizar conflictos, seguir este orden:

1. **Primero:** José Miguel - Parámetros y Tratamientos
   - Base para los demás módulos
   - Modifica principalmente `settings.py` y constantes

2. **Segundo:** Mauricio - Instrucciones y Comprensión
   - Agrega páginas al inicio del flujo
   - Modifica `page_sequence`

3. **Tercero:** Sergio - Resultados con Visualización
   - Mejora página existente
   - Puede necesitar ajustes por cambios anteriores

4. **Cuarto:** Donovan - Sistema de Castigo
   - Más cambios en `page_sequence`
   - Mayor probabilidad de conflictos (resolver con cuidado)

5. **Último:** Workflow de CI/CD
   - Una vez que el código está estable

## 7.3 Checklist de integración final

Después de mergear todos los PRs:

```bash
# 1. Actualizar local
git checkout main
git pull origin main

# 2. Verificar que todo funciona
otree devserver

# 3. Probar flujo completo
# - Abrir 3 navegadores/pestañas
# - Crear sesión de demo
# - Completar todas las etapas
# - Verificar resultados finales
```

### Verificación funcional

- [ ] Página de instrucciones se muestra correctamente
- [ ] Preguntas de comprensión validan respuestas
- [ ] Contribución acepta valores válidos
- [ ] Resultados intermedios muestran contribuciones
- [ ] Castigo funciona con límites correctos
- [ ] Resultados finales calculan payoffs correctamente
- [ ] Gráfico de Chart.js renderiza
- [ ] Ambos tratamientos (high/low MPCR) funcionan
- [ ] CI/CD pasa en GitHub Actions

## 7.4 Demostración final

### Setup para demostración

1. **Crear sesión de laboratorio:**
   ```
   http://localhost:8000/room/lab_session
   ```

2. **Compartir links con participantes:**
   - Cada participante abre el link en su navegador
   - Esperar a que todos estén conectados

3. **Ejecutar el experimento:**
   - Facilitador puede monitorear en Admin
   - Ver resultados en tiempo real

### Métricas a observar

- Tiempo promedio por página
- Distribución de contribuciones
- Uso del sistema de castigo
- Diferencias entre tratamientos

---

# APÉNDICES

## A. Comandos Git de referencia rápida

```bash
# Configuración
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"

# Básicos
git status                    # Ver estado
git add .                     # Agregar todos los cambios
git commit -m "mensaje"       # Commit con mensaje
git push                      # Subir cambios
git pull                      # Bajar cambios

# Ramas
git branch                    # Listar ramas
git checkout -b nueva-rama    # Crear y cambiar a rama
git checkout main             # Cambiar a main
git merge otra-rama           # Mergear rama

# Resolución de conflictos
git fetch origin              # Traer cambios sin mergear
git rebase main               # Rebasar sobre main
git rebase --continue         # Continuar después de resolver
git rebase --abort            # Cancelar rebase

# Histórico
git log --oneline             # Ver commits resumidos
git log --graph               # Ver con gráfico de ramas
git diff                      # Ver cambios no committeados
```

## B. Estructura final del proyecto

```
taller-otree-pgg/
├── .github/
│   └── workflows/
│       └── ci.yml
├── public_goods/
│   ├── __init__.py
│   └── templates/
│       └── public_goods/
│           ├── Introduction.html
│           ├── Comprehension.html
│           ├── Contribute.html
│           ├── IntermediateResults.html
│           ├── Punishment.html
│           ├── FinalResults.html
│           └── Results.html
├── settings.py
├── ruff.toml
├── README.md
├── .gitignore
└── requirements.txt
```

## C. Recursos adicionales

### Documentación oficial
- [oTree 5 Documentation](https://otree.readthedocs.io/en/latest/)
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Git Book](https://git-scm.com/book/en/v2)

### Papers de referencia
- Fehr, E., & Gächter, S. (2000). Cooperation and punishment in public goods experiments. *American Economic Review*, 90(4), 980-994.
- Fehr, E., & Gächter, S. (2002). Altruistic punishment in humans. *Nature*, 415(6868), 137-140.

### Herramientas recomendadas
- [VS Code](https://code.visualstudio.com/) con extensiones:
  - Python
  - GitLens
  - GitHub Pull Requests
- [Sourcetree](https://www.sourcetreeapp.com/) - GUI para Git
- [Chart.js](https://www.chartjs.org/) - Documentación de gráficos

---

## D. Solución de problemas comunes

| Problema | Causa probable | Solución |
|----------|----------------|----------|
| `ModuleNotFoundError: otree` | oTree no instalado | `pip install otree` |
| Template not found | Ruta incorrecta | Verificar estructura de carpetas |
| CSRF error | Falta {% csrf_token %} | Agregar en formularios (aunque oTree lo maneja automático) |
| Payoff es None | set_payoffs no ejecutado | Verificar WaitPage con `after_all_players_arrive` |
| Chart.js no renderiza | CDN bloqueado | Verificar conexión a internet |
| GitHub Actions falla | Error de sintaxis YAML | Validar indentación |
| Push rechazado | Branch protection | Crear PR en lugar de push directo |
| Merge conflict | Cambios paralelos | Resolver conflictos manualmente |

---

**¡Fin del taller! 🎉**

*Documento generado para el taller interactivo de Git/GitHub con oTree*
*Versión: 1.0*
*Fecha: [Fecha del taller]*
