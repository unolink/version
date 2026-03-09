# version

Информация о версии сборки для Go-бинарников. Ноль зависимостей.

Внедряйте версию, коммит, дату и номер сборки через `ldflags` на этапе компиляции. Получайте чистый, читаемый или semver-совместимый вывод без шаблонного кода.

## Установка

```bash
go get github.com/unolink/version
```

## Использование

### Внедрение при сборке

```bash
go build -ldflags "\
  -X github.com/unolink/version.Version=1.2.0 \
  -X github.com/unolink/version.Commit=$(git rev-parse HEAD) \
  -X github.com/unolink/version.Date=$(date -u +%Y-%m-%dT%H:%M:%SZ) \
  -X github.com/unolink/version.Build=42"
```

### Чтение в коде

```go
package main

import (
    "fmt"

    "github.com/unolink/version"
)

func main() {
    fmt.Println(version.Human())     // 1.2.0-a1b2c3d (#42, 2026-03-09T12:00:00Z)
    fmt.Println(version.Technical()) // 1.2.0+42.20260309T12:00:00Z.a1b2c3d
}
```

## API

| Функция | Формат вывода | Пример |
|---------|--------------|--------|
| `Human()` | `{version}-{short_commit} (#{build}, {date})` | `1.2.0-a1b2c3d (#42, 2026-03-09)` |
| `Technical()` | `{version}+{build}.{date}.{short_commit}` | `1.2.0+42.20260309.a1b2c3d` |

## Переменные

| Переменная | По умолчанию | Описание |
|------------|-------------|----------|
| `Version` | `"dev"` | Семантическая версия |
| `Commit` | `"none"` | Полный хеш git-коммита (обрезается до 7 символов в выводе) |
| `Date` | `"unknown"` | Дата сборки (рекомендуется RFC3339) |
| `Build` | `"0"` | Номер сборки CI |

## Зачем этот пакет?

Каждому Go-бинарнику нужна информация о версии. Паттерн `ldflags` стандартен, но требует повторяющегося кода. Этот пакет даёт две готовые функции форматирования и разумные значения по умолчанию — без зависимостей и настройки.

## Лицензия

[MIT](LICENSE)
