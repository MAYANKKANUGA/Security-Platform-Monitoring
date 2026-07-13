# AI-Driven Cloud Security Observability & Automated Triage Platform

An enterprise-grade, cloud-native Security Operations platform deployed on AWS that integrates SIEM, SOAR, and Generative AI to automate threat detection, enrich alerts with real-time threat intelligence, and act as an AI Copilot for SOC analysts.

## 🎯 Executive Summary
Traditional SOC Tier-1 operations suffer from alert fatigue and high Mean Time to Respond (MTTR). This project shifts the paradigm by treating security operations as a unified platform. It leverages **Wazuh** for observability, **Shuffle SOAR** for automated enrichment, and **OpenAI (GenAI)** to generate instant incident summaries. 

**Core Philosophy:** *AI augments the analyst instead of replacing them.* All response actions remain analyst-approved (Human-in-the-Loop), ensuring complete oversight while drastically improving investigation efficiency.

---

## 🏗️ Architecture Blueprint

```text
                                ┌───────────────────────────────┐
                                │       Endpoints & Cloud       │
                                │-------------------------------│
                                │ Windows                       │
                                │ Linux                         │
                                │ AWS CloudTrail                │
                                │ AWS VPC Flow Logs             │
                                │ Sysmon                        │
                                │ Applications                  │
                                │ Firewalls                     │
                                └──────────────┬────────────────┘
                                               │
                                               ▼
                               ┌─────────────────────────────────┐
                               │         Wazuh SIEM              │
                               │---------------------------------│
                               │ Log Collection                  │
                               │ Detection Rules                 │
                               │ Correlation                     │
                               │ Alert Generation                │
                               └──────────────┬──────────────────┘
                                              │
                                           Webhook/API
                                              │
                                              ▼
                                ┌───────────────────────────────────┐
                                │         Shuffle SOAR              │
                                │-----------------------------------│
                                │ Parse Alert                       │
                                │ Extract IOC                       │
                                │ Extract User                      │
                                │ Extract Host                      │
                                │ Extract Hash                      │
                                │ Build Investigation Context       │
                                └──────────────┬────────────────────┘
                                               │
                           ┌───────────────────┴────────────────────┐
                           ▼                                        ▼
             Threat Intelligence Layer                    Internal Context
             ------------------------                     -----------------
             VirusTotal                                   Asset Information
             AbuseIPDB                                    Host Details
             GeoIP                                        User Details
             MITRE ATT&CK                                 Previous Alerts
             Shodan (Future)                              Severity History

                           └───────────────────┬────────────────────┘
                                               ▼
                              AI SOC Copilot (LLM Agent)
        ┌──────────────────────────────────────────────────────────────────────┐
        │                                                                      │
        │ 1. Explain the alert in plain English                                │
        │ 2. Correlate all Threat Intel                                        │
        │ 3. Determine likely attack stage                                     │
        │ 4. Map MITRE ATT&CK                                                  │
        │ 5. Generate Risk Score                                               │
        │ 6. Generate Confidence Score                                         │
        │ 7. Create Executive Summary                                          │
        │ 8. Create Technical Summary                                          │
        │ 9. Recommend Next Actions                                            │
        │10. Recommend Containment Strategy                                    │
        │11. Decide Suggested Severity                                         │
        │12. Produce JSON Output for Automation                                │
        └──────────────────────────────┬───────────────────────────────────────┘
                                       │
                                       ▼
                        ┌─────────────────────────────────────┐
                        │         TheHive Case Manager        │
                        │-------------------------------------│
                        │ Create Case                         │
                        │ Attach IOC                          │
                        │ Attach AI Report                    │
                        │ Attach Threat Intel                 │
                        │ Severity                            │
                        │ MITRE Mapping                       │
                        │ Timeline                            │
                        └──────────────────┬──────────────────┘
                                           │
                                           ▼
                             Analyst Dashboard (SOC Portal)
        ┌────────────────────────────────────────────────────────────────────┐
        │                                                                    │
        │ New Cases                                                          │
        │ AI Investigation Report                                            │
        │ IOC Details                                                        │
        │ Threat Intel                                                       │
        │ MITRE ATT&CK                                                       │
        │ Confidence Score                                                   │
        │ Risk Score                                                         │
        │ Recommended Actions                                                │
        │                                                                    │
        │              [Approve]      [Reject]      [Needs Investigation]    │
        │                                                                    │
        └─────────────────────────────┬──────────────────────────────────────┘
                                      │
                           Human Approval Required
                                      │
                     ┌────────────────┴────────────────┐
                     ▼                                 ▼
                 Approved                           Rejected
                     │                                 │
                     ▼                                 ▼
             Automated Response                  Close / Reopen
                     │
                     ▼
        ┌───────────────────────────────────────────────────────┐
        │ Firewall Block (Palo Alto / Fortinet)                 │
        │ Disable User                                          │
        │ Isolate Endpoint                                      │
        │ Update AWS Security Group                             │
        │ Create Jira Ticket                                    │
        │ Notify Microsoft Teams                                │
        │ Send Email                                            │
        │ Store Incident Report                                 │
        └───────────────────────────────────────────────────────┘
