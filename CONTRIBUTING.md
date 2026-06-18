# Contributing

[Русский](CONTRIBUTING.ru.md)

TrafficWrapper is split across three public repositories. Start with
`orchestrator` to understand the control plane, then read `worker`, then `app`.

## Local Build

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

The full APK/AAR build needs Android SDK, NDK, JDK 17, gomobile, Xray assets, a
release keystore, and significant disk space. Do not put release keys or update
minisign private keys in the repository.

## Tests and Style

Required test commands before opening a pull request:

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

Use `gofmt` for Go changes. Run `go vet ./...` where practical. Android changes
should pass Kotlin/Android lint for the affected module.

The worker and app repositories use local `replace ../core` directives so their
Go modules build against the checked-out public `core` directory, not an
external monorepo.

## DCO

This project uses Developer Certificate of Origin sign-off, not a CLA. Sign your
commits with:

```sh
git commit -s
```

## Pull Request Checklist

- Tests above pass for affected repositories.
- Commits are signed off with DCO.
- No `.env`, private keys, tokens, bootstrap payloads, APKs, `worker-state/`, or
  `orch-state/` are committed.
- Public issue or PR text does not include real domains, IPs, SNI values,
  tokens, keys, or deployment-specific infrastructure.
- Example AWG dialects and `CAMOUFLAGE_DOMAIN` values are not presented as
  production-safe defaults.
- Release keystore and minisign update private keys stay offline or in private
  owner-controlled secret storage.
