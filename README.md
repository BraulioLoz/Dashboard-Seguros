# Dashboard de Seguros

Sistema de visualización y gestión de datos de seguros con análisis de fraude, desarrollado como proyecto para ITAM.

## Descripción

Dashboard interactivo para la visualización y análisis de datos de seguros de automóviles. El sistema permite consultar información sobre asegurados, pólizas, vehículos, incidentes y reclamos, con capacidades especiales de análisis de fraude mediante gráficos interactivos, tablas de datos y visualización geográfica.

## Características

- **Dashboard Interactivo**: Visualización de métricas clave con KPIs en tiempo real
- **Análisis de Fraude**: Gráficos y estadísticas detalladas sobre reclamos fraudulentos
- **Visualización Geográfica**: Mapa interactivo con ubicaciones de incidentes usando Leaflet
- **Tablas de Datos**: Visualización paginada de reclamos e incidentes con búsqueda
- **Series Temporales**: Análisis de tendencias de fraude a lo largo del tiempo
- **API RESTful**: Backend completo con FastAPI y documentación automática (Swagger)
- **Modo Oscuro/Claro**: Tema personalizable para mejor experiencia de usuario

## Tecnologías Utilizadas

### Backend

- **FastAPI** (0.115.2) - Framework web moderno y rápido
- **SQLModel** (0.0.22) - ORM basado en SQLAlchemy y Pydantic
- **SQLite** - Base de datos relacional
- **Pandas** (2.3.3) - Procesamiento de datos CSV
- **Uvicorn** (0.30.6) - Servidor ASGI

### Frontend

- **HTML5/CSS3** - Estructura y estilos
- **JavaScript (ES6+)** - Lógica de la aplicación con módulos ES6
- **Bootstrap 5.3.8** - Framework CSS responsive
- **Chart.js 4.4.0** - Gráficos interactivos
- **Leaflet 1.9.4** - Mapas interactivos

### Despliegue

- **Render** - Hosting del backend (FastAPI)
- **GitHub Pages** - Hosting del frontend estático

## Estructura del Proyecto

```
Dashboard-Seguros/
│
├── client/                 # Frontend (HTML/CSS/JS)
│   ├── css/
│   │   └── custom.css      # Estilos personalizados
│   ├── img/
│   │   └── logo.png         # Logo del dashboard
│   ├── js/
│   │   ├── api.js          # Cliente API con detección de ambiente
│   │   ├── app.js          # Lógica principal de la aplicación
│   │   ├── charts.js       # Configuración de gráficos Chart.js
│   │   ├── maps.js         # Configuración de mapas Leaflet
│   │   ├── storage.js      # Gestión de almacenamiento local
│   │   ├── tables.js       # Manejo de tablas de datos
│   │   └── theme.js        # Gestión de temas (oscuro/claro)
│   └── index.html          # Página principal
│
├── server/                 # Backend (FastAPI)
│   ├── main.py             # Aplicación FastAPI principal
│   ├── models.py           # Modelos SQLModel (Insured, Policy, Vehicle, etc.)
│   ├── db.py               # Configuración de base de datos
│   ├── add_data.py         # Script para cargar datos desde CSV
│   ├── requirements.txt    # Dependencias Python
│   └── render.yaml         # Configuración para Render
│
├── data/                   # Datos del proyecto
│   ├── insurance_claims_clean.csv  # Dataset principal limpio
│   ├── insurance_claims.csv        # Dataset original
│   ├── check_duplicates.ipynb      # Notebooks de análisis
│   └── load_data.ipynb             # Notebook de carga de datos
│
├── .gitignore             # Archivos ignorados por git
└── README.md              # Este archivo
```

## Modelos de Datos

El sistema utiliza seis entidades principales relacionadas:

1. **Insured** - Información del asegurado (edad, sexo, educación, ocupación, etc.)
2. **Policy** - Detalles de la póliza (número, estado, deducible, prima anual, etc.)
3. **Vehicle** - Información del vehículo (marca, modelo, año)
4. **Incident** - Detalles del incidente (fecha, tipo, severidad, ubicación, etc.)
5. **Claim** - Información del reclamo (montos, fraude reportado)
6. **Case** - Caso completo que relaciona todas las entidades anteriores

## Instalación y Configuración Local

### Requisitos Previos

- Python 3.11 o superior
- Git
- Navegador web moderno

### Pasos de Instalación

1. **Clonar el repositorio**

   ```bash
   git clone https://github.com/BraulioLoz/Dashboard-Seguros.git
   cd Dashboard-Seguros
   ```
2. **Crear entorno virtual**

   ```bash
   python -m venv venv
   ```
3. **Activar entorno virtual**

   **Windows (PowerShell):**

   ```powershell
   .\venv\Scripts\Activate.ps1
   ```

   **Windows (CMD):**

   ```cmd
   venv\Scripts\activate.bat
   ```

   **Linux/Mac:**

   ```bash
   source venv/bin/activate
   ```
4. **Instalar dependencias**

   ```bash
   pip install -r server/requirements.txt
   ```
5. **Configurar variables de entorno (opcional)**

   Crear archivo `.env` en la raíz del proyecto:

   ```env
   DATABASE_URL=sqlite:///./database.db
   ```
6. **Inicializar base de datos y cargar datos**

   ```bash
   cd server
   python add_data.py
   ```
7. **Iniciar servidor backend**

   ```bash
   cd server
   uvicorn main:app --reload
   ```

   El servidor estará disponible en `http://127.0.0.1:8000`

   - API: `http://127.0.0.1:8000`
   - Documentación Swagger: `http://127.0.0.1:8000/docs`
   - Documentación ReDoc: `http://127.0.0.1:8000/redoc`
8. **Abrir frontend**

   Abrir `client/index.html` en un navegador o usar un servidor local:

   **Con Python:**

   ```bash
   cd client
   python -m http.server 5500
   ```

   Luego abrir `http://localhost:5500` en el navegador

## API Endpoints

### Información General

- `GET /` - Información de la API
- `GET /health` - Estado de salud del servicio
- `GET /docs` - Documentación Swagger UI
- `GET /redoc` - Documentación ReDoc

### Estadísticas

- `GET /stats` - Estadísticas generales (totales, fraude, montos)
- `GET /stats/fraud-analysis` - Análisis detallado de fraude por severidad, estado y tipo
- `GET /stats/time-series` - Series temporales de incidentes y fraude

### Entidades Principales

Todas las entidades soportan paginación con parámetros `page` y `per_page` (máximo 100).

#### Insured (Asegurados)

- `GET /insureds` - Listar asegurados
- `GET /insureds/{insured_id}` - Obtener asegurado por ID

#### Policy (Pólizas)

- `GET /policies` - Listar pólizas (con filtro opcional `policy_state`)
- `GET /policies/{policy_id}` - Obtener póliza por ID
- `GET /policies/{policy_id}/coverage-level` - Nivel de cobertura de la póliza

#### Vehicle (Vehículos)

- `GET /vehicles` - Listar vehículos
- `GET /vehicles/{vehicle_id}` - Obtener vehículo por ID

#### Incident (Incidentes)

- `GET /incidents` - Listar incidentes
- `GET /incidents/{incident_id}` - Obtener incidente por ID
- `GET /incidents/map-data` - Datos de incidentes para visualización en mapa

#### Claim (Reclamos)

- `GET /claims` - Listar reclamos
- `GET /claims/{claim_id}` - Obtener reclamo por ID
- `GET /claims/{claim_id}/fraud-check` - Verificar fraude de un reclamo

#### Case (Casos)

- `GET /cases` - Listar casos completos
- `GET /cases/{case_id}` - Obtener caso completo con todas las relaciones

### Ejemplo de Respuesta

```json
{
  "data": [...],
  "page": {
    "page": 1,
    "per_page": 10,
    "total": 1000
  }
}
```

## Despliegue

### Backend en Render

1. Crear cuenta en [Render](https://render.com)
2. Conectar repositorio de GitHub
3. Crear nuevo **Web Service**
4. Configuración:
   - **Name**: `insurance-api` (o el nombre deseado)
   - **Runtime**: `Python 3`
   - **Build Command**: `pip install -r server/requirements.txt`
   - **Start Command**: `cd server && uvicorn main:app --host 0.0.0.0 --port $PORT`
   - **Environment Variables**:
     - `DATABASE_URL`: `sqlite:///./database.db`
5. Desplegar y obtener la URL del servicio (ej: `https://dashboard-seguros-895z.onrender.com`)

### Frontend en GitHub Pages

1. Ir a Settings del repositorio en GitHub
2. Seleccionar **Pages** en el menú lateral
3. Configurar:
   - **Source**: `Deploy from a branch`
   - **Branch**: `main`
   - **Folder**: `/client`
4. Guardar y esperar el despliegue
5. La URL será: `https://braulioloz.github.io/Dashboard-Seguros/`

### Actualizar URL de API en Frontend

Después de obtener la URL de Render, actualizar `client/js/api.js`:

```javascript
const API_BASE = isLocal 
    ? 'http://127.0.0.1:8000'
    : 'https://TU-SERVICIO.onrender.com';  // Reemplazar con tu URL de Render
```

### Carga Automática de Datos

El sistema carga automáticamente los datos desde `data/insurance_claims_clean.csv` cuando detecta que la base de datos está vacía. Esto ocurre automáticamente al desplegar en Render.

Para cargar datos manualmente:

```bash
cd server
python add_data.py
```

## 🔧 Desarrollo

### Estructura de Archivos Frontend

- **api.js**: Cliente API con detección automática de ambiente (local/producción)
- **app.js**: Coordinador principal, manejo de navegación y secciones
- **charts.js**: Configuración y renderizado de gráficos Chart.js
- **maps.js**: Inicialización y gestión del mapa Leaflet
- **tables.js**: Lógica de tablas con paginación y búsqueda
- **theme.js**: Gestión de tema oscuro/claro con persistencia
- **storage.js**: Utilidades para almacenamiento local

### Estructura de Archivos Backend

- **main.py**: Aplicación FastAPI con todos los endpoints y configuración CORS
- **models.py**: Modelos SQLModel con relaciones y métodos de negocio
- **db.py**: Configuración de motor de base de datos y sesiones
- **add_data.py**: Script de carga de datos desde CSV con manejo de duplicados

### Variables de Entorno

- `DATABASE_URL`: URL de conexión a la base de datos (por defecto: `sqlite:///./database.db`)

## Funcionalidades del Dashboard

### Sección Resumen (Overview)

- **KPIs**: Total de casos, reclamos, tasa de fraude, importe total
- **Gráficos**:
  - Distribución de reclamos de fraude (histograma)
  - Desglose de reclamos por tipo (gráfico apilado)
  - Tendencias de fraude a lo largo del tiempo (línea)
  - Distribución de severidad de incidentes (gráfico circular)

### Sección Data Tables

- Tablas paginadas para Reclamos e Incidentes
- Búsqueda en tiempo real
- Detalles completos de casos en modales

### Sección Mapa

- Visualización geográfica de incidentes
- Filtro para mostrar solo incidentes con fraude
- Marcadores interactivos con información detallada

## Troubleshooting

### Error: "Module not found"

- Asegúrate de tener activado el entorno virtual
- Verifica que todas las dependencias estén instaladas: `pip install -r server/requirements.txt`

### Error: "Database is locked"

- Cierra otras conexiones a la base de datos
- En desarrollo, reinicia el servidor

### Frontend no conecta con el backend

- Verifica que el backend esté corriendo en `http://127.0.0.1:8000`
- Revisa la consola del navegador (F12) para ver errores
- Verifica la configuración de CORS en `server/main.py`

### Datos no se cargan en Render

- Revisa los logs en Render para ver errores
- Verifica que el archivo CSV esté en el repositorio
- El archivo debe estar en `data/insurance_claims_clean.csv`

## Notas Importantes

- **Base de datos SQLite**: En Render con el plan gratuito, la base de datos SQLite es efímera. Los datos pueden perderse si el servicio se reinicia. Para producción, considera usar PostgreSQL.
- **Sueño de Render**: El plan gratuito pone el servicio en "sleep" después de 15 minutos de inactividad. La primera petición puede tardar ~30 segundos en despertar el servicio.
- **Detección de ambiente**: El frontend detecta automáticamente si está en local o producción y usa la URL correspondiente.

## Equipo

**Equipo 4 - ITAM - 2025**

Proyecto desarrollado como parte del curso de Introducción a Desarrollo Web.

## Licencia

Este proyecto es de carácter educativo y académico.

## Enlaces Útiles

- **Documentación FastAPI**: https://fastapi.tiangolo.com/
- **Documentación SQLModel**: https://sqlmodel.tiangolo.com/
- **Documentación Chart.js**: https://www.chartjs.org/docs/
- **Documentación Leaflet**: https://leafletjs.com/
- **Render Documentation**: https://render.com/docs
- **GitHub Pages**: https://pages.github.com/

## Contacto

Para preguntas o sugerencias sobre este proyecto, puedes abrir un issue en el repositorio de GitHub.

---

**Dashboard de Seguros** - Visualización y análisis de datos de seguros con detección de fraude.
