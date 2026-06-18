# Как внести вклад

[English](CONTRIBUTING.md)

TrafficWrapper разделён на три публичных репозитория. Начинайте с
`orchestrator`, чтобы понять control plane, затем читайте `worker`, затем `app`.
Для owner-facing setup path используйте orchestrator
[сквозной сценарий](https://github.com/TrafficWrapper/orchestrator/blob/master/README.ru.md#начните-здесь-сквозной-сценарий).

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

## Local development stand

Это development stand на одной машине. Это не production deployment guide. Для
полного owner-facing setup path используйте orchestrator
[сквозной сценарий](https://github.com/TrafficWrapper/orchestrator/blob/master/README.ru.md#начните-здесь-сквозной-сценарий).
Команды тестов находятся ниже в [Тесты и стиль](#тесты-и-стиль).

Используйте parent workspace со всеми тремя public repositories:

```sh
mkdir -p tw-dev
cd tw-dev
git clone https://github.com/TrafficWrapper/orchestrator.git
git clone https://github.com/TrafficWrapper/worker.git
git clone https://github.com/TrafficWrapper/app.git
```

Запустите orchestrator:

```sh
cd orchestrator
cp .env.example .env
docker compose up -d --build signer orchestrator
docker compose logs orchestrator | grep -i 'initial admin password'
docker compose exec orchestrator orchestrator public-key
```

Дефолтный local stand использует self-signed TLS:

```sh
ORCH_TLS=1
ORCH_PUBLIC_URL=https://127.0.0.1:9091
```

Для local API calls к этому self-signed listener используйте `curl -k`.

Создайте worker token через admin API:

```sh
ORCH_URL=https://127.0.0.1:9091
INITIAL_PASSWORD='<password from docker compose logs orchestrator>'
NEW_PASSWORD='<new owner password>'

LOGIN_JSON=$(curl -ksS -H 'content-type: application/json' \
  --data "{\"secret\":\"$INITIAL_PASSWORD\"}" \
  "$ORCH_URL/admin/v1/login")
SESSION_TOKEN=$(printf '%s' "$LOGIN_JSON" | python3 -c 'import json,sys; print(json.load(sys.stdin)["session_token"])')

CHANGE_JSON=$(curl -ksS -H "authorization: Bearer $SESSION_TOKEN" \
  -H 'content-type: application/json' \
  --data "{\"current_secret\":\"$INITIAL_PASSWORD\",\"new_secret\":\"$NEW_PASSWORD\"}" \
  "$ORCH_URL/admin/v1/password/change")
SESSION_TOKEN=$(printf '%s' "$CHANGE_JSON" | python3 -c 'import json,sys; print(json.load(sys.stdin)["session_token"])')

WORKER_TOKEN=$(openssl rand -base64 24 | tr '+/' '-_' | tr -d '=')
curl -ksS -H "authorization: Bearer $SESSION_TOKEN" \
  -H 'content-type: application/json' \
  --data "{\"id\":\"worker-dev\",\"value\":\"$WORKER_TOKEN\",\"ttl\":\"48h\"}" \
  "$ORCH_URL/admin/v1/token/create"
```

Храните `WORKER_TOKEN` в shell history только для disposable local stands.

Запустите same-host worker:

```sh
cd ../worker
cp .env.example .env
```

Задайте минимум:

```sh
ORCH_URL=https://host.docker.internal:9091
ORCH_STATIC_PUBLIC_KEY=<output of orchestrator public-key>
ORCH_INSECURE_TLS=1
ENROLL_TOKEN=<WORKER_TOKEN>
PUBLIC_ADDRESS=127.0.0.1
CAMOUFLAGE_DOMAIN=<real TLS 1.3 domain for dev testing>
APPLY_NFT=0
```

Затем запустите:

```sh
docker compose up -d --build
```

Approve pending worker в orchestrator admin UI. Для local development держите
`APPLY_NFT=0`, если вы не проверили generated firewall/NAT rules.

Создайте и импортируйте device bootstrap через **Devices** -> **+ New device** в
admin UI или вызовите admin API:

```sh
EXPIRES=$(date -u -d '+24 hours' +%Y-%m-%dT%H:%M:%SZ)
curl -ksS -H "authorization: Bearer $SESSION_TOKEN" \
  -H 'content-type: application/json' \
  --data "{\"limits\":{},\"expires\":\"$EXPIRES\"}" \
  "$ORCH_URL/admin/v1/bootstrap-token/create"
```

Передайте QR, Base64 или JSON bootstrap в Android app и подтвердите
распарсенные `orchestrator_url` и `config_pubkey_pin`.

Соберите Android debug app:

```sh
cd ../app
./build/build-apk.sh
```

Установите debug APK на test device или emulator, импортируйте bootstrap и
проверьте request через local SOCKS front-end или app integration, который его
использует.

Development отличается от production:

- `ORCH_INSECURE_TLS=1` только для self-signed local TLS.
- `ORCH_PUBLIC_URL=https://127.0.0.1:9091` недоступен реальным remote devices.
- `PUBLIC_ADDRESS=127.0.0.1` полезен только для same-host experiments.
- `APPLY_NFT=0` избегает automatic firewall/NAT changes во время development.
- Production workers требуют реальные public ports, реальный
  `CAMOUFLAGE_DOMAIN` и deployment-specific keys.

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
