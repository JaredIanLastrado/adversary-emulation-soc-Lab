# SOAR Automation

This section contains three Shuffle playbooks implementing the AESOC alert, case, and ticket lifecycle.

| Playbook | Purpose |
|---|---|
| [01 – Wazuh Alert Intake](01-Wazuh-Alert-Intake/) | Routes Wazuh alerts into TheHive and Slack |
| [02 – Case Updates and Ticket Routing](02-Case-Updates-and-Ticket-Routing/) | Processes TheHive events and creates Zammad tickets |
| [03 – Ticket Closure Handback](03-Ticket-Closure-Handback/) | Synchronizes completed Zammad work back to TheHive |

Each playbook includes implementation details and validation evidence collected in the AESOC home lab.
