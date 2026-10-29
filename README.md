# Cuenca autohospedado

Este repositorio publica el paquete para alojar en tu propio servidor los datos de tus espacios de
[Cuenca](https://cuenca.app). La nube de Cuenca sigue a cargo de las cuentas, los miembros de cada espacio y sus permisos,
pero los datos de los espacios que asignes a tu servidor se guardan en él y los dispositivos se sincronizan directamente con
él: no pasan por la nube.

El paquete levanta cuatro servicios con Docker Compose:

| Servicio | Qué hace |
|---|---|
| `postgres` | Guarda los datos de los espacios alojados. No publica puertos. |
| `powersync` | Envía a cada dispositivo los espacios que su token le permite leer. |
| `gateway` | Recibe los cambios de los dispositivos, verifica sus tokens y los escribe en Postgres. |
| `caddy` | Sirve todo por HTTPS en tu dominio y obtiene el certificado automáticamente. |

La imagen del gateway se publica en `ghcr.io/cordelia242/cuenca-gateway:<versión>`. Las demás son imágenes oficiales fijadas
por digest.

## Antes de empezar: lo que debes saber

- **Quien opera el servidor puede leer los datos que aloja.** Los datos de cada espacio quedan legibles en tu Postgres, y el
  operador también ve los tokens de los miembros de esos espacios. Aloja solo espacios cuyos miembros confíen en quien opera
  el servidor.
- **No hay cifrado de extremo a extremo.** La conexión va por HTTPS, pero el servidor guarda y procesa los datos tal cual.
- Eres responsable de los respaldos, las actualizaciones y la seguridad del equipo donde corre.

## Requisitos

- Linux con Docker y el plugin Docker Compose v2, con acceso a su demonio.
- 2 GB de RAM o más y 5 GiB libres en el disco como mínimo.
- `bash` 4.3 o posterior, `openssl` y `curl`.
- Un dominio propio que apunte al equipo, con los puertos 80 y 443 accesibles desde Internet. Para probar en tu propio equipo,
  sin dominio ni TLS, existe el modo local (`./instalar.sh --local`).
- Salida a Internet: el gateway y PowerSync descargan las claves públicas de la nube de Cuenca.

## Instalación rápida

1. Descarga `cuenca-selfhosted-<versión>.tar.gz` y `cuenca-selfhosted-<versión>.tar.gz.sha256` de la última versión en
   [Releases](https://github.com/Cordelia242/cuenca-selfhosted/releases).
2. Comprueba la descarga y descomprímela:

   ```sh
   sha256sum -c cuenca-selfhosted-<versión>.tar.gz.sha256
   tar -xzf cuenca-selfhosted-<versión>.tar.gz
   cd cuenca-selfhosted-<versión>
   ```

3. Ejecuta el instalador guiado. Pregunta lo necesario (dominio, correo para el certificado, el `servidorId` que te da la app),
   genera las contraseñas, descarga las imágenes y espera a que todo esté sano:

   ```sh
   ./instalar.sh
   ```

La guía completa, con la configuración manual, el registro del servidor en la app y la solución de problemas, está en
<https://cuenca.app/guia/autohospedaje/>.

## Mantenimiento

Desde la carpeta del paquete:

```sh
./respaldar.sh      # respaldo cifrado de la base de datos con tu frase de paso
./restaurar.sh      # restaura un respaldo y comprueba que el servidor vuelve a estar sano
./actualizar.sh     # respalda, descarga las imágenes nuevas y recrea los servicios
```

Guarda la frase de paso de los respaldos fuera del servidor: sin ella no se pueden restaurar. Los detalles de cada script
están en la [guía](https://cuenca.app/guia/autohospedaje/).

## Problemas y propuestas

Este proyecto se ofrece sin soporte y sin SLA (ningún compromiso de tiempos de respuesta ni de corrección): los issues se
leen y se responden cuando se puede.

- **Algo no funciona**: abre un [issue](https://github.com/Cordelia242/cuenca-selfhosted/issues/new/choose) con la plantilla
  de error e incluye la salida de `./diagnostico.sh`. El informe oculta contraseñas y tokens, pero revísalo antes de pegarlo.
- **Una idea o mejora**: usa la plantilla de propuesta.
- **Una vulnerabilidad**: nunca en un issue público. Sigue [SECURITY.md](SECURITY.md).

Nunca pegues tokens, frases de paso, el contenido de `.env` ni datos financieros de personas en un issue.

Más detalles sobre cómo participar en [CONTRIBUTING.md](CONTRIBUTING.md).
