# TrafficWrapper

[English](README.md)

TrafficWrapper — open-source self-hosted платформа private transport для
небольших operator deployments и transport-obfuscation research. Она
разделяет control plane, worker data plane и Android client code, чтобы operator
владел deployment keys, worker endpoints, bootstrap payloads и update policy.

Это не Android VPN: Android client открывает local SOCKS front-end и использует
user-space transports, выбранные из signed deployment config. Платформа
использует minisign-signed config/update artifacts, per-device enrollment,
REALITY и AmneziaWG routes, а также reproducible build verification для
публичного APK.

Читайте repositories в таком порядке:

1. [orchestrator](https://github.com/TrafficWrapper/orchestrator) - control
   plane, admin UI, worker approval, device bootstrap, config signing, APK
   publication metadata.
2. [worker](https://github.com/TrafficWrapper/worker) - data plane node с Xray
   REALITY, AmneziaWG, in-tunnel distributor и worker agent.
3. [app](https://github.com/TrafficWrapper/app) - Android public client и Go
   transport core.

## Сравнение для operators

Это ориентир, а не ranking. Другие проекты меняются со временем; детали
проверяйте в их upstream документации.

| Проект | Модель | Transport / mimicry | Client mode | Key ownership | Distribution | Build verification | License |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TrafficWrapper | Self-hosted control + worker data plane | REALITY + AmneziaWG routes из signed config | Local SOCKS; не Android `VpnService` | Operator-owned keys | Config/APK через in-tunnel `/tw/` | Документированная source build + verifier script | MIT |
| Amnezia | Self-hosted VPN/client tooling | Varies by deployment | Обычно VPN-style clients | Operator/server owner, зависит от setup | Varies | См. upstream | См. upstream |
| Marzban | Self-hosted proxy panel | Varies; часто Xray-profile based | External clients vary | Operator controls panel/server keys | Varies | См. upstream | См. upstream |
| 3x-ui / Hiddify | Self-hosted proxy panels | Varies by panel/profile | External clients vary | Operator controls panel/server keys | Varies | См. upstream | См. upstream |
| Outline | Self-hosted access server + clients | Shadowsocks-oriented model | Обычно VPN-style clients | Operator controls server access keys | Varies | См. upstream | См. upstream |

Self-hosters отвечают за собственные deployment choices, local law, hosting
provider terms и operational security. Не публикуйте реальные domains, IP
addresses, SNI values, tokens, device credentials или private keys в public
issues или pull requests.
