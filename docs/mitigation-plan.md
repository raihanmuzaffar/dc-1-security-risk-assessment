# Mitigation Plan

| ID | Risk scenario | Recommended mitigation | Priority |
|---|---|---|---|
| R01 | Outdated web components | Upgrade or replace unsupported components; validate compatibility and patch regularly. | High |
| R02 | Unauthorized SSH access | Restrict source IPs, use SSH keys, disable unnecessary password authentication, and monitor login attempts. | High |
| R03 | RPC service exposure | Disable RPC if unnecessary or restrict it to trusted interfaces and networks. | Medium |
| R04 | Service/version disclosure | Reduce unnecessary banners and detailed version exposure; keep software patched. | Medium |
| R05 | Service availability disruption | Maintain backups, monitoring, recovery procedures, and resource limits. | Medium |
| R06 | Administrative page/account misuse | Enforce least privilege, strong authentication, and review administrative access. | Medium |

Priorities are estimates for this coursework and should be validated against the actual environment.
