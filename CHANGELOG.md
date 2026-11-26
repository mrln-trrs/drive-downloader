# Changelog

Todos los cambios relevantes de este proyecto se documentan en este archivo.

El formato sigue [Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/)
y el proyecto usa [Versionado Semántico](https://semver.org/lang/es/).

## [1.0.0] - 2026-11-26

### Añadido
- Opción `--host` para el modo servidor.
- `.python-version`, `SECURITY.md`, `.github/dependabot.yml` y CI mínima (instalación + `compileall`).
- README completo y este CHANGELOG.

### Cambiado
- El modo servidor escucha en `127.0.0.1` por defecto (antes `0.0.0.0`, sin autenticación).
- `requirements.txt` completo: se añaden google-api-python-client, google-auth, google-auth-oauthlib,
  fastapi, pydantic, uvicorn y psutil, que el código importaba sin declararlas.
- Python mínimo: 3.11.5.

### Seguridad
- requests 2.34.2, urllib3 2.8.0, certifi 2026.11.20, idna 3.20, beautifulsoup4 4.15.0 y soupsieve 4.1.3
  (cierra las alertas de Dependabot abiertas).

[1.0.0]: https://github.com/mrln-trrs/drive-downloader/releases/tag/v1.0.0
