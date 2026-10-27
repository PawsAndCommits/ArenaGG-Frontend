# ArenaGG — Frontend

Interfaz web de ArenaGG, plataforma de torneos de esports. SPA en JavaScript vanilla con módulos ES, Bootstrap por CDN y navegación por rutas hash (`#/...`). No requiere compilación ni frameworks.

Backend: <https://github.com/PawsAndCommits/ArenaGG-Backend>

## Funcionalidades

- Registro, login y perfil de usuario.
- Listado y detalle de torneos, con inscripción individual o por equipo.
- Sala de partida con marcador, rosters y carga de resultados.
- Gestión de equipos, roster, invitaciones y capitanía.
- Panel de administración de usuarios, torneos e inscripciones.
- Bandeja de notificaciones con contador actualizado por polling.
- Tabla de posiciones, historial y estadísticas.

## Requisitos

- Navegador con soporte de módulos ES.
- Python 3 o cualquier servidor HTTP estático.

## Configuración

La URL del backend se define en `assets/js/config.js`:

```js
window.ARENAGG_API_URL = "http://127.0.0.1:8000";
```

Si el valor queda vacío, las peticiones se realizan contra el mismo origen que sirve el frontend. El backend debe incluir el origen del frontend en `CORS_ORIGINS`; por defecto acepta `http://localhost:5173` y `http://127.0.0.1:5173`.

## Ejecución

El backend debe estar en ejecución en el puerto `8000`. Desde la raíz de este repositorio:

```powershell
python -m http.server 5173
```

La aplicación queda disponible en <http://localhost:5173>.

Alternativas:

```powershell
npx serve -l 5173 .
```

El archivo `index.html` no debe abrirse mediante `file://`, porque los módulos ES y las llamadas a la API requieren un servidor HTTP.

## Cuentas de prueba

Los usuarios se generan con `seed_demo.py` del backend:

| Usuario | Contraseña |
|---|---|
| `admin` | `Admin1234` |
| `ShadowFox`, `NeoBlaze`, `PixelQueen`, `Ragnar`, `LunaByte`, `Kairo`, `Nova`, `Drift`, `Vex`, `Onyx` | `Jugador1234` |

El registro público crea únicamente jugadores.

## Rutas

| Ruta | Pantalla |
|---|---|
| `#/` | Inicio |
| `#/torneos` | Listado de torneos |
| `#/torneos/:id` | Detalle e inscripción |
| `#/torneos/:id/sala/:partidaId` | Sala de partida |
| `#/equipos` | Listado de equipos |
| `#/equipos/:id` | Detalle del equipo |
| `#/mis-torneos` | Torneos del usuario |
| `#/invitaciones` | Invitaciones a equipos |
| `#/notificaciones` | Bandeja de avisos |
| `#/perfil` | Perfil propio |
| `#/admin` | Panel de administración |
| `#/login` y `#/registro` | Autenticación |

Las rutas hash no requieren configuración de reescritura en el servidor estático.

## Estructura

```
ArenaGG-Frontend/
├── index.html
└── assets/
    ├── css/estilos.css       # Tema oscuro con acentos rojos
    ├── audio/faceit-ready.mp3
    └── js/
        ├── config.js         # URL del backend
        ├── app.js            # Punto de entrada
        ├── api.js            # Cliente HTTP y manejo de errores
        ├── auth.js           # Sesión y token JWT
        ├── router.js         # Router por hash
        ├── layout.js         # Barra de navegación, notificaciones y avisos
        ├── components.js     # Componentes reutilizables
        ├── utils.js          # Utilidades
        └── pages/            # Una vista por ruta
```

## Despliegue

El proyecto es estático y puede publicarse en GitHub Pages, Netlify, Vercel o cualquier hosting de archivos.

1. Configurar `assets/js/config.js` con la URL pública del backend.
2. Agregar ese origen a `CORS_ORIGINS` en el backend.
3. Si el frontend se sirve por HTTPS, el backend también debe usar HTTPS para evitar contenido mixto.
