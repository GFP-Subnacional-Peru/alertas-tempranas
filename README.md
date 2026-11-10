# Alertas tempranas de deforestación

Visor para consultar, clasificar y reportar alertas tempranas dentro de las Áreas de Conservación Regional.
Es un servicio propio: Java 21, Spring Boot y PostGIS. No forma parte del dashboard de línea base.

En línea: https://acr-atd.onrender.com/
Salud: `GET /api/atd/salud`

El dashboard de deforestación y de acuerdos está en
[acr-dashboard](https://github.com/GFP-Subnacional-Peru/acr-dashboard).

## Carpetas

| Carpeta | Para qué está |
| --- | --- |
| `src/main/java` | API, sesión, clasificación, fotos y PDF. |
| `src/main/resources/static` | Visor (HTML, `visor.js`, límites). |
| `src/main/resources/db/migration` | Esquema Flyway, esquema `atd`. |
| `db/carga` | Carga inicial desde las GDB. No entra en la imagen Docker. |
| `Dockerfile` | Imagen que publica el servicio. El contexto es esta raíz. |

## Correr en local

Hace falta Docker Desktop y un archivo `.env` (copiar `.env.example` y completar los usuarios).

```
docker compose up -d --build
```

El visor queda en http://localhost:8081/
En Windows también abre `ABRIR_ATD_LOCAL.bat`.

La base local escucha en el puerto 5433. Usuario `atd`, base `atd`. La clave es `ATD_DB_PASSWORD` del `.env`; si no se define, docker compose usa la clave local de desarrollo.

La carga de límites lee `acr_all.geojson` y `zi_all.geojson` desde `ATD_LIMITES_DIR`
(por defecto `db/limites/`, carpeta que no se versiona). Esos dos archivos son el caché
`data/cache/geojson` del dashboard.

## Publicación

El servicio que ya está en línea se construye, por ahora, desde la carpeta `atd-service/`
del repositorio `acr-dashboard`. Este repositorio es la raíz limpia: el `Dockerfile` está aquí.

Para publicar desde aquí, en Render:

1. Abrir el servicio de alertas (el de https://acr-atd.onrender.com).
2. Cambiar el repositorio a `GFP-Subnacional-Peru/alertas-tempranas`, rama `main`.
3. Directorio raíz vacío. Dockerfile: `Dockerfile`.
4. Comprobar `GET /api/atd/salud`.

No hace falta crear un servicio nuevo. Un segundo servicio web compartiría el cupo de horas
y a mitad de mes se apagarían los dos.

De lunes a viernes, de 8:00 a 18:00 (hora de Lima), el workflow `.github/workflows/keep-render-awake.yml`
llama a `/api/atd/salud` para que el servicio no se duerma en el horario de trabajo.
El mismo horario también lo llama el repositorio del dashboard.
