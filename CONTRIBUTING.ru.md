# Как внести вклад

TrafficWrapper разделён на три публичных репозитория. Начинайте с
`orchestrator`, чтобы понять control plane, затем читайте `worker`, затем `app`.

## Локальная сборка

Orchestrator:

```sh
git clone https://github.com/TrafficWrapper/orchestrator.git
cd orchestrator
docker compose up -d --build signer orchestrator
go test ./...
go vet ./...
```

Worker:

```sh
git clone https://github.com/TrafficWrapper/worker.git
cd worker
docker compose up -d --build
(cd agent && go test ./...)
(cd awg-gw && go test ./...)
(cd awg-smoke && go test ./...)
(cd core && go test ./...)
```

App:

```sh
git clone https://github.com/TrafficWrapper/app.git
cd app
(cd core && go test ./...)
(cd client && ./gradlew :app:testPublicDebugUnitTest)
./build/build-apk.sh
```

Полная APK/AAR-сборка требует Android SDK, NDK, JDK 17, gomobile, Xray assets,
release keystore и заметного места на диске. Не кладите release keys или private
minisign update keys в репозиторий.

## Тесты и стиль

Обязательные команды перед pull request:

```sh
# orchestrator
go test ./...

# worker
(cd agent && go test ./...)
(cd awg-gw && go test ./...)
(cd awg-smoke && go test ./...)
(cd core && go test ./...)

# app
(cd core && go test ./...)
(cd client && ./gradlew :app:testPublicDebugUnitTest)
```

Для Go используйте `gofmt`; по возможности запускайте `go vet ./...`. Android
изменения должны проходить Kotlin/Android lint для затронутого модуля.

Worker и app используют локальные `replace ../core`, поэтому Go modules
собираются против публичного `core` в этом репозитории, а не внешнего монорепо.

## DCO

Проект использует Developer Certificate of Origin sign-off, не CLA. Подписывайте
коммиты так:

```sh
git commit -s
```

## Чеклист PR

- Тесты выше проходят для затронутых репозиториев.
- Коммиты подписаны DCO.
- В коммитах нет `.env`, private keys, tokens, bootstrap payloads, APK,
  `worker-state/` или `orch-state/`.
- В публичном issue/PR нет реальных domains, IP, SNI, tokens, keys или деталей
  вашей инфраструктуры.
- Примерные AWG dialects и `CAMOUFLAGE_DOMAIN` не подаются как production-safe
  defaults.
- Release keystore и private minisign update keys остаются offline или в
  приватном secret storage владельца.
