# 📦 Guía de Instalación y Setup

## Requisitos Previos

- Python 3.10+
- Git
- pip o conda

## Instalación Rápida

### 1. Clonar el repositorio

```bash
git clone <repository-url>
cd iajordy2
```

### 2. Crear ambiente virtual

```bash
# Windows
python -m venv .venv
.venv\Scripts\Activate.ps1

# Linux/Mac
python3 -m venv .venv
source .venv/bin/activate
```

### 3. Instalar dependencias

```bash
pip install -r requirements.txt
```

### 4. Descargar modelos entrenados

Los siguientes archivos deben descargarse por separado (son demasiado grandes para Git):

- `models/voc_multilabel_final.h5` (25MB) - Modelo principal entrenado
- `data/voc2007/voc2007_multilabel.npz` (292MB) - Dataset VOC 2007

**Opción A: Si tienes acceso a Google Drive o servidor compartido:**
```bash
# Descargar y colocar en:
# - models/voc_multilabel_final.h5
# - data/voc2007/voc2007_multilabel.npz
```

**Opción B: Entrenar desde cero (recomendado para pruebas):**
```bash
# Ejecutar notebook
jupyter notebook notebooks/03_training_real_images.ipynb
```

### 5. Ejecutar la aplicación

```bash
# Opción 1: Flask directo
python run_server.py

# Opción 2: Con módulo
python -m app.api

# Luego accede a:
# - http://127.0.0.1:5000/simple (versión simplificada)
# - http://127.0.0.1:5000 (versión completa)
```

---

## Estructura del Proyecto

```
iajordy2/
├── app/
│   ├── api.py                 # API Flask principal
│   ├── utils.py               # Funciones de utilidad
│   ├── static/                # Archivos estáticos (CSS, JS)
│   │   ├── script.js
│   │   ├── style.css
│   │   └── favicon.ico
│   └── templates/             # Plantillas HTML
│       ├── index.html         # Interfaz completa
│       └── simple.html        # Interfaz simplificada
│
├── data/
│   ├── voc2007/              # Dataset VOC 2007
│   │   ├── classes.json      # Clases VOC (20 objetos)
│   │   └── voc2007_multilabel.npz (*)
│   ├── test_images/          # Imágenes para pruebas
│   ├── corrections/          # Correcciones guardadas
│   └── uploads/              # Imágenes subidas
│
├── models/
│   └── voc_multilabel_final.h5 (*)  # Modelo entrenado
│
├── notebooks/                # Jupyter notebooks
│   ├── 01_data_analysis.ipynb
│   ├── 02_modeling.ipynb
│   ├── 03_training_real_images.ipynb
│   └── 04_prediction.ipynb
│
├── requirements.txt          # Dependencias
├── .gitignore               # Archivos ignorados por Git
├── run_server.py            # Script para iniciar servidor
└── README.md                # Este archivo

(*) = Descargables por separado (archivos grandes)
```

---

## Archivos Ignorados por Git

Estos archivos no están versionados porque son muy grandes:

- `.venv/` - Ambiente virtual (>500MB)
- `models/*.h5` - Modelos entrenados (25MB+)
- `models/*.weights.h5` - Pesos del reentrenamiento
- `data/voc2007/voc2007_multilabel.npz` - Dataset (292MB)
- `data/food101/` - Dataset Food101
- `data/open_images/` - Dataset Open Images
- `notebooks/.ipynb_checkpoints/` - Cache Jupyter

---

## Configuración Rápida

### Variables de Entorno (opcional)

Crear archivo `.env` en la raíz:

```
FLASK_ENV=production
DEBUG=False
PORT=5000
```

### Puertos Disponibles

Si el puerto 5000 está en uso, modificar en `run_server.py`:

```python
app.run(host='127.0.0.1', port=5001, debug=False)
```

---

## Solución de Problemas

### Error: "No module named 'tensorflow'"

```bash
pip install tensorflow==2.20.0
```

### Error: "Model file not found"

```bash
# Opción 1: Entrenar nuevo modelo
jupyter notebook notebooks/03_training_real_images.ipynb

# Opción 2: Usar modelo de prueba
# Ejecutar: python test_retraining_system.py
```

### Puerto 5000 en uso

```bash
# Windows: encontrar proceso
netstat -ano | findstr :5000

# Matar proceso
taskkill /PID <pid> /F

# O cambiar puerto en run_server.py
```

---

## Desarrollo

### Estructura de Código

- **Backend (Flask)**: `app/api.py`
  - Endpoints: `/predict`, `/save_correction`, `/retrain`, etc.
  - Procesamiento: imágenes, modelos, almacenamiento

- **Frontend (HTML/CSS/JS)**: `app/static/` y `app/templates/`
  - Interfaz web responsiva
  - Comunicación con API vía fetch
  - Manejo de estado del cliente

- **Modelos ML**: `app/utils.py`
  - Predicción multilabel
  - Reentrenamiento incremental
  - Preprocessing de imágenes

### Testing

```bash
# Prueba de sistema de reentrenamiento
python test_retraining_system.py

# Prueba de ciclo completo
python test_full_cycle.py

# Prueba de API endpoints
python test_api.py
```

---

## Mejoras Futuras

- [ ] Soporte para más datasets
- [ ] Dashboard de métricas
- [ ] Export de modelos
- [ ] API REST completa
- [ ] Deploy en cloud (AWS, GCP, Heroku)
- [ ] Tests unitarios automatizados
- [ ] Validación de entrada mejorada

---

## Licencia

Ver archivo [LICENSE](LICENSE)

---

## Contacto

Para preguntas o problemas, crear un issue en GitHub.
