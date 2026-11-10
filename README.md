# Alertas tempranas de deforestación

Visor de las alertas tempranas de deforestación en las Áreas de Conservación Regional. Lo usa el programa GFP Subnacional para mirar el aviso, clasificarlo y dejar el reporte.

En línea: https://acr-atd.onrender.com/
Salud del servicio: `GET /api/atd/salud`

## Qué hace

El visor abre el territorio por región, ámbito, ACR y periodo. Cada alerta se ve sobre el mapa, con su código y su fecha.

Quien consulta recorre las alertas. Quien entra con usuario las clasifica, adjunta la foto de campo y cierra el reporte. La foto guarda la coordenada en UTM: si el celular trae ubicación, el visor la lee y la escribe. El par de imágenes Sentinel, una de antes y una de después, ayuda a leer el cambio. El reporte en PDF lo arma el especialista. La propuesta automática no reemplaza esa decisión ni se guarda sola.

## Cómo está hecho

El servicio es Java 21 con Spring Boot. La base es PostGIS 16, esquema `atd`, y el esquema entra por migraciones Flyway. El visor es HTML, CSS y JavaScript con Leaflet. Las claves de los usuarios viven en variables de entorno, no en el código.

La imagen se construye con el `Dockerfile` de esta raíz: Maven compila dentro de la imagen y el servicio arranca con el puerto que le asigna la publicación. Un push a `main` actualiza el visor cuando el servicio apunta a este repositorio.

## Carpetas

| Carpeta | Para qué está |
| --- | --- |
| `src/main/java` | API, sesión, clasificación, fotos y PDF. |
| `src/main/resources/static` | Visor: `index.html`, `visor.js` y los límites del mapa. |
| `src/main/resources/db/migration` | Migraciones Flyway del esquema `atd`. |
| `db/carga` | Carga inicial desde las GDB. No entra en la imagen. |
| `Dockerfile` | Imagen del servicio. El contexto de construcción es esta raíz. |
| `.github/workflows/` | Mantiene el servicio despierto en el horario de trabajo. |

## Horario

De lunes a viernes, de 8:00 a 18:00, hora de Lima. GitHub Actions llama a `GET /api/atd/salud` desde las 7:50, para que a las 8 el visor ya responda, y deja de llamar a las 18. A esa hora el servicio se apaga solo. La computadora de quien desarrolla no tiene que estar prendida: el aviso corre en GitHub.

Hay dos workflows, `keep-render-awake.yml` y `keep-render-awake-offset.yml`. El segundo va desfasado unos minutos por si el primero se atrasa. Los dos cubren el mismo horario.

## Correr en local

Hace falta Docker Desktop y un archivo `.env`. Se copia `.env.example` y se completan los usuarios. Sin ese archivo nadie puede entrar.

```
docker compose up -d --build
```

El visor queda en http://localhost:8081/
En Windows también abre `ABRIR_ATD_LOCAL.bat`.

La base local escucha en el puerto 5433, base `atd`, usuario `atd`. La clave es `ATD_DB_PASSWORD` del `.env`.

La carga de límites lee `acr_all.geojson` y `zi_all.geojson` desde `ATD_LIMITES_DIR` (por defecto `db/limites/`, carpeta que no se versiona).
