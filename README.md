# DC-1 Security Risk Assessment

Security risk assessment documentation for the DC-1 virtual machine in an isolated VirtualBox lab.

## Scope

- Attacker/testing machine: Kali Linux
- Target: DC-1
- Lab network: `192.168.52.0/24`
- Kali host-only address: `192.168.52.10`
- DC-1 address: `192.168.52.11`

All testing is intended for the isolated lab environment.

## Observed Services

| Port | Service | Observation |
|---|---|---|
| 22/tcp | SSH | OpenSSH 6.0p1 |
| 80/tcp | HTTP | Apache httpd 2.2.22; Drupal 7 |
| 111/tcp | RPC | rpcbind |

These observations describe the lab scan results. They do not, by themselves, prove successful exploitation.

## Repository Structure

- `docs/` — final report, executive summary, risk matrix, and presentation outline
- `evidence/` — scan outputs and HTTP/RPC evidence
- `risk-assessment/` — risk register
- `diagrams/` — lab topology source
- `presentation/` — presentation files, if available
- `reports/` — exported report files, if available

## Risk Methodology

Risk scores are estimates based on the documented likelihood and impact ratings. They are not proof of exploitability or measured real-world loss. Any quantitative financial figures are illustrative assumptions for coursework.

## Limitations

The findings are based on the evidence collected in this lab. No successful exploitation is claimed unless explicitly supported by evidence in the report.

## Recommended Mitigations

- Upgrade unsupported operating systems and web components.
- Restrict SSH access and enforce strong authentication.
- Limit RPC exposure to trusted networks.
- Reduce unnecessary service and version disclosure.
- Apply monitoring, backups, and recovery procedures.
- Review administrative access controls.

## Ethics

Only test systems you own or have explicit permission to assess. Keep the DC-1 target isolated from public networks.
