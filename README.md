# ArenaGG · Frontend

Interfaz web de ArenaGG, la plataforma de torneos de esports. SPA en **JavaScript vanilla** con módulos ES nativos, Bootstrap por CDN y navegación por rutas hash (`#/...`). No requiere compilación ni frameworks.

Consume la API del repositorio [PawsAndCommits/ArenaGG-Backend](https://github.com/PawsAndCommits/ArenaGG-Backend).

## Requisitos

- Un navegador moderno con soporte de módulos ES.
- Python 3 (para el servidor estático) o cualquier servidor HTTP.

## Configuración de la API

La URL del backend se define en `assets/js/config.js`:

```js
window.ARENAGG_API_URL = "http://127.0.0.1:8000";
```

Cambiala por la URL del backend desplegado si corresponde. Si el valor queda vacío, las peticiones se hacen contra el mismo origen que sirve el frontend.

El backend debe permitir el origen del frontend por CORS. Por defecto acepta `http://localhost:5173` y `http://127.0.0.1:5173` (variable `CORS_ORIGINS`).

## Ejecución

Primero arrancá el backend en el puerto `8000` (ver su README).

Después, desde la raíz de este repositorio:

```powershell
python -m http.server 5173
```

Abrí <http://localhost:5173>.

Alternativas:

```powershell
npx serve -l 5173 .
```

O la extensión **Live Server** de VS Code.

> No abras `index.html` con doble clic (`file://`): los módulos ES y las llamadas a la API requieren un servidor HTTP.

## Cuentas de prueba

Los usuarios se crean en el backend con `seed_demo.py`:

| Usuario | Contraseña |
|---|---|
| `admin` | `Admin1234` |
| `ShadowFox`, `NeoBlaze`, `PixelQueen`, `Ragnar`, `LunaByte`, `Kairo`, `Nova`, `Drift`, `Vex`, `Onyx` | `Jugador1234` |

También podés registrar una cuenta nueva desde `#/registro`; el registro público crea jugadores.

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
| `#/admin` | Panel de administración (solo admin) |
| `#/login` y `#/registro` | Autenticación |

Como se usan rutas hash, no hace falta configurar reescrituras en el servidor estático.

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
        ├── layout.js         # Navbar, notificaciones y toasts
        ├── components.js     # Componentes reutilizables
        ├── utils.js          # Utilidades varias
        └── pages/            # Una vista por ruta
```

## Despliegue

Al ser 100% estático, se puede publicar en GitHub Pages, Netlify, Vercel o cualquier hosting de archivos.

1. Configurá `assets/js/config.js` con la URL pública del backend.
2. Asegurate de que el backend incluya ese origen en `CORS_ORIGINS`.
3. Si el frontend se sirve por HTTPS, el backend también debe usar HTTPS para evitar contenido mixto.

## Notas

- Las peticiones `DELETE` exitosas responden `204 No Content`; el cliente no espera cuerpo en esos casos.
- El inicio de un torneo usa `PATCH /torneos/{torneo_id}/iniciar`.
- El contador de notificaciones se actualiza por polling cada 20 segundos.
