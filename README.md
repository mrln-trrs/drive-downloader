# Google Drive Downloader

Script en Python para descargar en lote archivos públicos de Google Drive a partir de una lista de enlaces.

![Estado](https://img.shields.io/badge/estado-estable-brightgreen)
![Licencia](https://img.shields.io/badge/licencia-MIT-blue)
![Python](https://img.shields.io/badge/python-3.11.5%2B-3776AB)
[![CI](https://github.com/mrln-trrs/drive-downloader/actions/workflows/ci.yml/badge.svg)](https://github.com/mrln-trrs/drive-downloader/actions/workflows/ci.yml)

## Estado del proyecto

**Estable.** Herramienta personal de un solo archivo (`drive_downloader.py`). Nació para evitar descargar a mano,
uno por uno, decenas de fragmentos de un archivo grande.

## Características

- Descarga paralela con hilos (`--workers`) y tamaño de bloque configurable (`--chunk-size`).
- Reintentos con espera progresiva (`--retries`) y detección de transferencias demasiado lentas.
- Manejo del formulario de confirmación de Google Drive para archivos grandes y de los avisos de cuota excedida.
- Modo de validación sin descargar (`--dry-run`).
- Descarga opcional mediante la API oficial de Google Drive con OAuth y alcance `drive.readonly` (`--use-api`).
- Hash SHA-256 de cada archivo descargado.
- Resumen en `download_summary.csv` y `download_summary.json`, lista de enlaces fallidos en `failed_downloads.txt`
  y registro rotativo en `downloads.log`.
- Modo servidor FastAPI (`--serve`) con los puntos `POST /download`, `GET /status` y `GET /summary`.

## Stack

| Componente | Versión |
|---|---|
| Python | 3.11.5 o superior (probado con 3.12) |
| requests / urllib3 | 2.34.2 / 2.8.0 |
| beautifulsoup4 / soupsieve | 4.15.0 / 4.1.3 |
| google-api-python-client / google-auth-oauthlib | 2.202.0 / 1.5.0 |
| FastAPI / pydantic / uvicorn | 0.165.0 / 2.14.4 / 0.54.0 |
| tqdm, colorama, psutil (opcional) | ver `requirements.txt` |

## Requisitos

- Python 3.11.5 o superior.
- Conexión a internet (al arrancar se comprueba el acceso a `https://drive.google.com`).
- Solo para `--use-api`: un `credentials.json` de cliente OAuth de escritorio creado en Google Cloud Console
  con la API de Google Drive habilitada.

## Instalación

```bash
git clone https://github.com/mrln-trrs/drive-downloader.git
cd drive-downloader
python3 -m venv .venv
source .venv/bin/activate        # En Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

## Uso

Desde un archivo con un enlace por línea:

```bash
python drive_downloader.py --input-file list.txt --workers 4
```

Sin `--input-file`, el script pide los enlaces por consola; se termina con una línea vacía.

Formatos de enlace aceptados:

```text
https://drive.google.com/file/d/<ID>/view
https://drive.google.com/open?id=<ID>
https://drive.google.com/uc?export=download&id=<ID>
```

Opciones:

| Opción | Descripción | Valor por defecto |
|---|---|---|
| `--input-file` | Archivo de texto con los enlaces | (entrada por consola) |
| `--workers` | Descargas en paralelo | 4 |
| `--chunk-size` | Tamaño de bloque en bytes | 32768 |
| `--retries` | Reintentos por archivo | 3 |
| `--dry-run` | Solo valida los enlaces | desactivado |
| `--use-api` | Usa la API oficial de Google Drive | desactivado |
| `--serve` | Arranca el servidor FastAPI | desactivado |
| `--host` | Dirección del servidor | `127.0.0.1` |
| `--port` | Puerto del servidor | 8000 |

Los archivos se guardan en `drive_downloads/` dentro del directorio de trabajo.

### API oficial de Google Drive

Coloca `credentials.json` en el directorio de trabajo y ejecuta con `--use-api`. La primera vez se abre el navegador
para autorizar el acceso y se guarda `token.json`. Ambos archivos están en `.gitignore` y no deben versionarse.

### Modo servidor

```bash
python drive_downloader.py --serve --port 8000
curl -X POST http://127.0.0.1:8000/download -H "Content-Type: application/json" \
     -d '{"link": "https://drive.google.com/file/d/<ID>/view"}'
curl http://127.0.0.1:8000/status
```

## Estructura del proyecto

```text
drive_downloader.py   Script principal (CLI y servidor FastAPI)
requirements.txt      Dependencias con versiones fijadas
.python-version       Versión de Python de referencia
.github/              Dependabot y CI (instalación + compilación)
```

## Limitaciones conocidas

- Solo descarga archivos públicos, salvo con `--use-api`. Carpetas de Drive no están soportadas.
- El modo servidor **no tiene autenticación**: cualquiera que alcance el puerto puede lanzar descargas al disco.
  Por eso escucha en `127.0.0.1` por defecto; usar `--host 0.0.0.0` solo en una red de confianza.
- El estado del servidor (`/status`) vive en memoria y se pierde al reiniciar.
- Google puede cambiar el formulario de confirmación de descarga; si cambia, el modo sin API puede dejar de funcionar.
- No hay pruebas automatizadas; la CI solo instala dependencias y compila.

## Autor

Marlon Torres Espinoza ([@mrln-trrs](https://github.com/mrln-trrs)).

## Licencia

[MIT](LICENSE).
