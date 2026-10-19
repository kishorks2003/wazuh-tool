# Wazuh-based Host Monitoring and Incident Detection

Project result

- Agents: 1 app host successfully onboarded and reporting.
- Alerts: Several alerts surfaced; I validated true positive and tuned rules to reduce noise.
- FIM: Detected an unexpected config change which was reverted and documented.
- Vulnerability detection: Identified outdated packages with actionable CVEs; prioritized patching.
- Network correlation: Wireshark capture validated suspicious connection attempts flagged by Wazuh.

What I did

1. Set up the Wazuh environment
   - Deployed a Wazuh Manager (single-node for POC) and connected it to an indexer/dashboard (Elasticsearch or OpenSearch + Kibana/OpenSearch Dashboards).
   - Enabled TLS for secure agent-manager communications.

2. Created and installed a Wazuh agent on the target application host
   - Registered a new agent in the manager and installed the agent package on the application server.
   - Verified agent status (connected, last keepalive) in the Wazuh dashboard.

3. Configured log collection and modules
   - Enabled collection of system logs, application logs, and Windows Event Logs (if applicable).
   - Activated modules used: file integrity monitoring (FIM), rootcheck, vulnerability-detector, and syscollector.

4. Tuned detection rules and custom decoders
   - Imported or created custom rules to reduce false positives for the application.
   - Adjusted rule levels and added exceptions where known benign activity occurred.

5. Verified alerts and investigated notable detections
   - Reviewed recent alerts in the Wazuh/Kibana dashboard (alert list, severity, rule id, agent, timestamp).
   - For each alert: inspected raw event details (full log message, fields like src_ip/dst_ip/user/command) for triage.

6. Performed file integrity monitoring (FIM) checks
   - Configured paths to monitor (app config files, webroot, /etc, critical binaries).
   - Investigated FIM alerts for unauthorized changes and restored or quarantined suspicious files.

7. Ran vulnerability detection
   - Enabled vulnerability-detector to scan installed packages and report CVEs and missing patches.
   - Prioritized critical/high CVEs and documented remediation steps.

8. Used active-response and automated actions (if configured)
   - Created active-response scripts to block suspicious IPs (e.g., via firewall) or quarantine hosts.
   - Tested active-response behavior from a simulated alert.

9. Captured network traffic and correlated with alerts (Wireshark)
   - Ran Wireshark captures on the monitored host or network segment to validate suspicious traffic flagged by Wazuh.
   - Correlated packet-level data (flows, protocols, suspicious payloads) with Wazuh alerts to confirm incidents.

10. Exported and documented evidence
    - Exported alert lists, raw event JSON, and PCAPs for forensic documentation.
    - Saved screenshots of dashboards, alert details, and Wireshark captures for the project report.
