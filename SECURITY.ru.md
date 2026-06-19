# Политика безопасности

[English](SECURITY.md)

Не открывайте public issue для уязвимостей или отчётов, содержащих реальные
deployment domains, IP addresses, SNI values, tokens, keys, bootstrap payloads,
device credentials или worker state.

Используйте GitHub Private Vulnerability Reporting для затронутого репозитория:

- orchestrator: <https://github.com/TrafficWrapper/orchestrator/security/advisories/new>
- worker: <https://github.com/TrafficWrapper/worker/security/advisories/new>
- app: <https://github.com/TrafficWrapper/app/security/advisories/new>

Если вы не уверены, какой repository затронут, выберите repository, ближайший к
vulnerable code path, и отметьте остальные components в private report.

## Scope

In scope:

- обходы или надёжное ослабление REALITY или AmneziaWG obfuscation;
- deanonymization self-hoster's dialect, domains, SNI values, update keys,
  device credentials или worker identity;
- leaks или misuse device bootstrap data, per-device credentials, AWG keys или
  REALITY UUIDs;
- обход minisign verification для client config, discovery endpoints или APK
  update manifests;
- обход Android APK signing-certificate pinning;
- compromise или replay worker enroll tokens или device bootstrap tokens;
- RCE, container escape или privilege escalation в worker agent, особенно paths,
  involving `docker.sock`;
- unauthorized admin access, worker approval, device enrollment, config signing
  или APK publication.

Out of scope:

- reports, требующие compromised owner machines, private keys или unlocked admin
  sessions без дополнительной vulnerability;
- generic best-practice findings без exploitability;
- scanner output, не указывающий concrete vulnerable code path;
- disclosure деталей вашего собственного deployment после того, как вы
  намеренно их опубликовали.

## Response Targets

- Initial acknowledgement: в течение 3 business days.
- Triage and severity estimate: в течение 10 business days.
- Remediation plan или status update: в течение 20 business days для accepted
  reports.

Timelines могут отличаться для сложных cross-repository issues, но maintainers
будут обновлять private report.

## Safe Harbor

Good-faith research приветствуется, когда оно ограничено systems, которыми вы
владеете или имеете permission to test, избегает service disruption, persistence,
data exfiltration сверх минимального proof, и reported privately. Не тестируйте
third-party self-hosters или public infrastructure без explicit authorization.

## OpSec operators и maintainers

Донат-адреса, domains, build machines, package signing identities и другая
побочная infrastructure могут связать отдельный deployment с реальным человеком
или организацией. Self-hosters и maintainers должны использовать изолированные
addresses, accounts, domains и build identities, когда им нужна separation от
personal infrastructure. Не включайте реальные donation addresses, payment
metadata, domains, IP addresses или account handles в public issues, если вы не
хотите сделать их public.
