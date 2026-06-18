# Security Policy

[Русский](SECURITY.ru.md)

Do not open a public issue for vulnerabilities or for reports containing real
deployment domains, IP addresses, SNI values, tokens, keys, bootstrap payloads,
device credentials, or worker state.

Use GitHub Private Vulnerability Reporting for the affected repository:

- orchestrator: <https://github.com/TrafficWrapper/orchestrator/security/advisories/new>
- worker: <https://github.com/TrafficWrapper/worker/security/advisories/new>
- app: <https://github.com/TrafficWrapper/app/security/advisories/new>

If you are unsure which repository is affected, choose the repository closest to
the vulnerable code path and note the other components in the private report.

## Scope

In scope:

- bypasses or reliable weakening of REALITY or AmneziaWG obfuscation;
- deanonymization of a self-hoster's dialect, domains, SNI values, update keys,
  device credentials, or worker identity;
- leaks or misuse of device bootstrap data, per-device credentials, AWG keys, or
  REALITY UUIDs;
- bypass of minisign verification for client config, discovery endpoints, or APK
  update manifests;
- bypass of Android APK signing-certificate pinning;
- compromise or replay of worker enroll tokens or device bootstrap tokens;
- RCE, container escape, or privilege escalation in the worker agent, especially
  paths involving `docker.sock`;
- unauthorized admin access, worker approval, device enrollment, config signing,
  or APK publication.

Out of scope:

- reports requiring compromised owner machines, private keys, or unlocked admin
  sessions without an additional vulnerability;
- generic best-practice findings without exploitability;
- scanner output that does not identify a concrete vulnerable code path;
- disclosure of your own deployment details after you intentionally published
  them.

## Response Targets

- Initial acknowledgement: within 3 business days.
- Triage and severity estimate: within 10 business days.
- Remediation plan or status update: within 20 business days for accepted
  reports.

Timelines can vary for complex cross-repository issues, but maintainers will keep
the private report updated.

## Safe Harbor

Good-faith research is welcome when it is limited to systems you own or have
permission to test, avoids service disruption, avoids persistence, avoids data
exfiltration beyond the minimum proof needed, and is reported privately. Do not
test against third-party self-hosters or public infrastructure without explicit
authorization.
