# TrafficWrapper

TrafficWrapper is a public, self-hosted anti-censorship tunnel platform. It is
not an Android VPN: the Android client exposes a local SOCKS front-end and uses
user-space transports selected from signed deployment config.

Read the repositories in this order:

1. [orchestrator](https://github.com/TrafficWrapper/orchestrator) - control
   plane, admin UI, worker approval, device bootstrap, config signing, APK
   publication metadata.
2. [worker](https://github.com/TrafficWrapper/worker) - data plane node with
   Xray REALITY, AmneziaWG, in-tunnel distributor, and worker agent.
3. [app](https://github.com/TrafficWrapper/app) - Android public client and Go
   transport core.

Self-hosters are responsible for their own deployment choices, local law,
hosting provider terms, and operational security. Do not publish real domains,
IP addresses, SNI values, tokens, device credentials, or private keys in public
issues or pull requests.
