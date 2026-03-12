# Suricata detection as code

Example demonstrating how custom Suricata rules can be distributed and dynamically loaded across environments using configuration templating and rule reloads.

## Define custom rule files

```yaml
suricata_custom_rule_files:
    - {{ suricata_custom_rules_everyone | default([]) }}
    - reconnaissance.rules
    - defense_evasion.rules
    - exfiltration.rules
    - ... additional rule files
```

## Deploy rules

Copy the rules to: 

```bash
/etc/suricata/suricata_custom_rules/
```

## Update suricata.yaml

```yaml
rule-files:
 - suricata.rules
{% for suricata_custom_rule_file in suricata_custom_rule_files %}
 - /etc/suricata/suricata_custom_rules/{{ suricata_custom_rule_file }}
{% endfor %}
```

## Reload suricata rules
Trigger rule reload via:
```bash
suricatasc -c ruleset-reload
```

or

```bash
suricatasc -c ruleset-reload-nonblocking
```

This can be executed via Ansible `notify` handler.

Ready to push out explicit rules to multiple environments! =)

