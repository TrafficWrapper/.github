# TrafficWrapper

[English](README.md)

TrafficWrapper — публичная self-hosted anti-censorship tunnel platform. Это не
Android VPN: Android client открывает local SOCKS front-end и использует
user-space transports, выбранные из signed deployment config.

Читайте repositories в таком порядке:

1. [orchestrator](https://github.com/TrafficWrapper/orchestrator) - control
   plane, admin UI, worker approval, device bootstrap, config signing, APK
   publication metadata.
2. [worker](https://github.com/TrafficWrapper/worker) - data plane node с Xray
   REALITY, AmneziaWG, in-tunnel distributor и worker agent.
3. [app](https://github.com/TrafficWrapper/app) - Android public client и Go
   transport core.

Self-hosters отвечают за собственные deployment choices, local law, hosting
provider terms и operational security. Не публикуйте реальные domains, IP
addresses, SNI values, tokens, device credentials или private keys в public
issues или pull requests.
