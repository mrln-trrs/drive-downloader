# Política de seguridad

## Versiones con soporte

| Versión | Soporte |
|---|---|
| 1.0.x | Sí |
| < 1.0 | No |

## Cómo reportar una vulnerabilidad

No abras una issue pública. Usa el aviso privado de GitHub
(**Security → Report a vulnerability**) en este repositorio. Se responde en un plazo razonable
y, si se confirma, se publica una versión corregida.

## Notas

- El modo servidor (`--serve`) no tiene autenticación. Escucha en `127.0.0.1` por defecto;
  no lo expongas en `0.0.0.0` fuera de una red de confianza.
- `credentials.json` y `token.json` (API de Google Drive) nunca deben versionarse.
