# Política de seguridad

## Cómo reportar una vulnerabilidad

Reporta las vulnerabilidades de forma privada con el reporte privado de vulnerabilidades de GitHub:

1. Entra en <https://github.com/Cordelia242/cuenca-selfhosted/security>.
2. Pulsa **Report a vulnerability** y completa el formulario.

Nunca reportes una vulnerabilidad en un issue público, en una discusión ni en un comentario: cualquiera podría aprovecharla
antes de que exista una corrección.

## Qué incluir

- La versión del paquete (`cuenca-selfhosted-<versión>`) y de la imagen `ghcr.io/cordelia242/cuenca-gateway:<versión>`.
- El componente afectado (gateway, PowerSync, Postgres, Caddy, los scripts del paquete) y el impacto que esperas.
- Los pasos para reproducirla, con datos de prueba inventados.
- Si ya sabes cómo mitigarla, tu propuesta.

## Qué no incluir

- Datos reales de personas ni de sus espacios (movimientos, saldos, nombres, correos).
- Tokens, contraseñas, frases de paso de respaldos ni el contenido de `.env`. Si un secreto quedó expuesto, cámbialo en tu
  servidor; no hace falta enviarlo para demostrar el problema.

## Qué versiones se corrigen

El proyecto se ofrece sin soporte y sin SLA: no hay plazos comprometidos para responder ni para corregir, y los reportes se
atienden cuando se puede. Si llega una corrección de seguridad, sale solo en una versión nueva, después de la última
publicada en [Releases](https://github.com/Cordelia242/cuenca-selfhosted/releases); antes de reportar, comprueba si el
problema sigue en ella.
