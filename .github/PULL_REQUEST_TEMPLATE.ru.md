[English](PULL_REQUEST_TEMPLATE.md)

## Краткое описание

## Тесты

## Checklist

- [ ] Required tests прошли для каждого затронутого repository.
- [ ] Commits включают DCO sign-off (`git commit -s`).
- [ ] Не закоммичены `.env`, private keys, tokens, bootstrap payloads, APKs, `worker-state/` или `orch-state/`.
- [ ] Public PR text не включает реальные domains, IPs, SNI values, tokens, keys или deployment infrastructure details.
- [ ] Example AWG dialects или `CAMOUFLAGE_DOMAIN` values не представлены как production defaults.
- [ ] Security-sensitive changes проверены на minisign, APK signature, Noise pinning, enroll-token и docker.sock impacts там, где это применимо.
