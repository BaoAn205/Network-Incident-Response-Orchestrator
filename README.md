# Network Incident Response Orchestrator

**An AI-assisted Multi-Agent framework for automated cybersecurity incident investigation and reporting.**

Network Incident Response Orchestrator is a cybersecurity project designed to automate the incident response (IR) workflow using specialized AI agents and Python-based analysis skills. It processes security alerts, Windows Sysmon logs, and network flow data to correlate evidence, enrich indicators of compromise (IOCs), and generate structured incident reports.

The project is evaluated using the **Splunk Boss of the SOC v1 (BOTSv1)** dataset and focuses on improving investigation efficiency while keeping findings grounded in collected evidence.

## Key Features

- **Automated Alert Triage:** Identifies attacker and victim IP addresses, establishes an investigation time window, and prepares the initial incident context.
- **Threat Intelligence Enrichment:** Investigates IP addresses, domains, and file hashes using OSINT sources such as VirusTotal, AbuseIPDB, and ThreatFox.
- **Host Forensics:** Analyzes Sysmon events to identify suspicious process execution, network connections, file creation, and registry or WMI persistence activity.
- **Network Traffic Analysis:** Examines network flows, HTTP requests, DNS queries, traffic volume, protocol distribution, and suspicious communication patterns.
- **Cross-Evidence Correlation:** Connects host and network artifacts to identify related indicators and strengthen investigation findings.
- **Automatic IOC Pivoting:** Extracts additional IP addresses, domains, and file hashes to expand the investigation.
- **Incident Reporting:** Consolidates evidence, builds an event timeline, maps observed behavior to MITRE ATT&CK, and generates a Vietnamese incident response report.

## Architecture and Investigation Workflow

The system follows a six-phase incident response pipeline orchestrated by specialized agents.

| Phase | Component | Main Responsibility |
|---|---|---|
| 0 | Alert Triage | Identify incident context, attacker, victim, and timeline |
| 1 | Initial Recon | Enrich initial IOCs using threat intelligence |
| 2 | Deep Evidence Collection | Analyze Sysmon logs and network flows in parallel |
| 3 | Correlation | Correlate host and network evidence |
| 4 | Pivot Recon | Investigate newly discovered IOCs |
| 5 | Synthesis & Reporting | Generate the final incident response report |

The workflow combines parallel evidence collection with sequential correlation and reporting to organize the investigation process.

## Technology Stack

- **Language:** Python
- **AI Agent Framework:** Pi CLI / configured agent workflows
- **Agent Configuration:** Markdown prompts and YAML workflow configuration
- **Data Formats:** JSON, Markdown
- **Security Analysis:** Sysmon event analysis, network flow analysis, IOC extraction, statistical anomaly detection
- **Threat Intelligence:** VirusTotal, AbuseIPDB, ThreatFox
- **Threat Mapping:** MITRE ATT&CK
- **Dataset:** Splunk BOTSv1

## Project Structure

```text
.
├── .pi/
│   ├── agents/          # Specialized agent definitions
│   ├── chains/          # Workflow configuration
│   ├── prompts/         # System prompts and investigation rules
│   ├── scripts/         # Supporting scripts
│   └── skills/          # Python analysis skills
├── data/                # Input alerts and investigation datasets
├── logs/                # Local logs and runtime artifacts
├── reports/             # Analysis results and incident reports
├── triage/              # Triage-related resources
├── config.json          # System configuration
├── .env.example         # Environment variable template
├── .gitignore           # Excluded files and secrets
└── README.md
```

Large raw datasets and local log files are excluded from version control. Their paths and availability may be required for the corresponding analysis skills to run successfully.

## Requirements

- Python 3.8 or later
- Pi CLI (`pi`) or a compatible environment configured for the project's agent workflows
- Internet access for external threat intelligence queries
- Valid API credentials for the OSINT providers used by the enabled analysis skills
- The required BOTSv1 input files for host and network analysis

Install the Python dependency used by the relevant scripts:

```bash
pip install requests
```

Review the project's scripts and configuration for any additional dependencies before execution.

## Configuration

1. Clone the repository.
2. Create a local `.env` file based on `.env.example`.
3. Add the required API keys to `.env`.
4. Prepare the required input datasets under `data/`.
5. Review `config.json` and the agent instructions under `.pi/`.

**Security note:** Never commit `.env`, API keys, access tokens, or other credentials to GitHub.

## Usage

From the project root, start an investigation through the configured Pi CLI workflow.

Example: investigate a suspicious IP address.

```bash
pi "Based on .pi/AGENT.md, investigate IP 40.80.148.42 using the configured sub-agents and save the report in reports."
```

Example: investigate a suspected brute-force attack.

```bash
pi "Based on .pi/AGENT.md, investigate a suspected brute-force attack against the Joomla administration portal. Determine whether a login succeeded and identify subsequent attacker activity. Save the findings in reports."
```

The exact commands and supported prompts depend on the installed CLI version and the project's agent configuration.

## Output Artifacts

Intermediate results and reports are stored in `reports/`, including:

- `triage_context.json` — initial incident context and timeline
- `recon_result.json` — initial threat intelligence findings
- `log_collector_result.json` — host forensic findings
- `network_analyzer_result.json` — network analysis results
- `pivot_manifest.json` — correlated artifacts and candidate IOCs
- `pivot_summary.json` — expanded IOC investigation results
- `BAO_CAO_IR.md` — final Vietnamese incident response report

Some filenames may vary depending on the current workflow configuration.

## Security Analysis Capabilities

The project is designed to help investigate behaviors such as:

- Suspicious PowerShell execution
- Process and file activity associated with malware
- Registry and WMI persistence
- Command-and-control (C2) beaconing
- Port and protocol anomalies
- Suspicious HTTP URIs and DNS queries
- Potential data exfiltration
- Related infrastructure discovered through IOC pivoting

Findings should be validated against the underlying logs and network evidence. Threat intelligence results are enrichment signals and should not be treated as definitive proof by themselves.

## Limitations

- The current workflow focuses on offline dataset analysis rather than continuous real-time monitoring.
- Results depend on input data quality, configured detection logic, and the availability of external threat intelligence services.
- Automated findings require analyst review before operational response actions are taken.
- The project does not replace a production SIEM or a fully integrated security operations platform.

## Team Project

Developed as a networking and cybersecurity course project.

**Contributors**
- Trịnh Bảo Toàn — Network Analyzer, evidence correlation, and IOC pivoting
- Phạm Bảo An — Sysmon Log Collector and alert triage
- Trương Quốc Thịnh — Initial reconnaissance and incident reporting

## References

- [Splunk BOTSv1 Dataset](https://github.com/splunk/botsv1)
- [MITRE ATT&CK](https://attack.mitre.org/)
- [VirusTotal Documentation](https://docs.virustotal.com/)
- [AbuseIPDB](https://www.abuseipdb.com/)
- [ThreatFox](https://threatfox.abuse.ch/)

---

*This project demonstrates the application of AI-assisted workflows, host and network forensics, threat intelligence, and evidence correlation to cybersecurity incident response.*
