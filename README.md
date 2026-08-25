# common

Modulo compartido para concentrar piezas reutilizables entre servicios Go de DLabsPeru.

```bash
go get github.com/DLabsPeru/common@latest
```

Incluye:

- helpers de configuracion y entorno
- errores genericos
- middlewares HTTP
- logger con `zap`
- envelope de respuesta HTTP
- health HTTP reutilizable

## Estructura

- `config`
- `errors`
- `http/health`
- `http/middleware`
- `logger`
- `response`

## Enfoque

Este modulo no contiene reglas de negocio ni configuracion especifica de un servicio.
La idea es que cada proyecto defina su propia estructura de configuracion y reutilice aqui:

- tipos base (`App`, `CORS`, `Database`, `HTTPServer`)
- helpers para `.env`
- logger
- middlewares HTTP
- response envelope
- health handler generico basado en `Checker`

## Versionado automatico

El repositorio queda preparado con GitHub Actions para CI y autoversionado.

- `CI`: verifica formato, ejecuta `go vet`, pruebas con detector de carreras y compila todos los paquetes en cada `pull_request` hacia `main`
- `Release Please`: calcula la siguiente version, crea o actualiza el PR de release y publica el tag/release cuando ese PR se fusiona

El modulo se publica desde `github.com/DLabsPeru/common`. Los tags anteriores a la migracion de namespace conservan el historial, pero los consumidores deben usar una version que ya declare este nuevo module path.

### Migracion desde Destiny-Peru

Desde la primera version publicada bajo el nuevo module path, reemplaza los imports `github.com/Destiny-Peru/common/...` por `github.com/DLabsPeru/common/...` y actualiza la dependencia:

```bash
go get github.com/DLabsPeru/common@latest
go mod tidy
```

No mantengas ambos module paths dentro del mismo servicio, porque Go los considera dependencias diferentes aunque provengan del mismo código fuente.

### Convencion de commits

Usa Conventional Commits para que el versionado sea automatico:

- `fix: corrige bug en health handler` -> sube `patch` (`v1.0.1`)
- `feat: agrega middleware reutilizable` -> sube `minor` (`v1.1.0`)
- `feat!: cambia contrato de response` o `BREAKING CHANGE:` -> sube `major` (`v2.0.0`)

Si el commit no sigue esta convencion, Release Please no podra inferir correctamente el cambio de version.
