# TrafficWrapper

[Русский](README.ru.md)

TrafficWrapper is an open-source self-hosted private transport platform for
small operator deployments and anti-censorship transport research. It separates
control plane, worker data plane, and Android client code so the operator owns
deployment keys, worker endpoints, bootstrap payloads, and update policy.

It is not an Android VPN: the Android client exposes a local SOCKS front-end and
uses user-space transports selected from signed deployment config. The platform
uses minisign-signed config/update artifacts, per-device enrollment, REALITY and
AmneziaWG routes, and reproducible build verification for the public APK.

Read the repositories in this order:

1. [orchestrator](https://github.com/TrafficWrapper/orchestrator) - control
   plane, admin UI, worker approval, device bootstrap, config signing, APK
   publication metadata.
2. [worker](https://github.com/TrafficWrapper/worker) - data plane node with
   Xray REALITY, AmneziaWG, in-tunnel distributor, and worker agent.
3. [app](https://github.com/TrafficWrapper/app) - Android public client and Go
   transport core.

## Operator Comparison

This is an orientation aid, not a ranking. Other projects change over time; for
their details, check upstream documentation.

| Project | Model | Transport / mimicry | Client mode | Key ownership | Distribution | Build verification | License |
| --- | --- | --- | --- | --- | --- | --- | --- |
| TrafficWrapper | Self-hosted control + worker data plane | REALITY + AmneziaWG routes from signed config | Local SOCKS; not Android `VpnService` | Operator-owned keys | Config/APK through in-tunnel `/tw/` | Documented source build + verifier script | MIT |
| Amnezia | Self-hosted VPN/client tooling | Varies by deployment | Usually VPN-style clients | Operator/server owner, depending on setup | Varies | See upstream | See upstream |
| Marzban | Self-hosted proxy panel | Varies; commonly Xray-profile based | External clients vary | Operator controls panel/server keys | Varies | See upstream | See upstream |
| 3x-ui / Hiddify | Self-hosted proxy panels | Varies by panel/profile | External clients vary | Operator controls panel/server keys | Varies | See upstream | See upstream |
| Outline | Self-hosted access server + clients | Shadowsocks-oriented model | Usually VPN-style clients | Operator controls server access keys | Varies | See upstream | See upstream |

Self-hosters are responsible for their own deployment choices, local law,
hosting provider terms, and operational security. Do not publish real domains,
IP addresses, SNI values, tokens, device credentials, or private keys in public
issues or pull requests.
