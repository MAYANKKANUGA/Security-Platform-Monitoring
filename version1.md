# Phase 1: Automated Security Incident Enrichment Pipeline

This repository documents the current (Phase 1) architecture of my cloud-native security operations pipeline. The goal of this phase is to eliminate manual IOC (Indicator of Compromise) lookups by automating threat intelligence enrichment and case creation using SIEM and SOAR.

## 🚀 Current Architecture Workflow

```text
                               ┌───────────────────────────────┐
                               │       AWS Cloud Environment   │
                               │-------------------------------│
                               │ EC2 Instances                 │
                               │ Linux/Windows Endpoints       │
                               └──────────────┬────────────────┘
                                              │
                                              ▼
                               ┌─────────────────────────────────┐
                               │         Wazuh SIEM              │
                               │---------------------------------│
                               │ 1. Ingests Raw Logs             │
                               │ 2. Detects Anomalies            │
                               │ 3. Generates Security Alerts    │
                               └──────────────┬──────────────────┘
                                              │
                                       Webhook Trigger
                                              │
                                              ▼
                                ┌───────────────────────────────────┐
                                │         Shuffle SOAR              │
                                │-----------------------------------│
                                │ 1. Receives Wazuh Alert           │
                                │ 2. Parses JSON Payload            │
                                │ 3. Extracts IOCs (IPs, Hashes)    │
                                └──────────────┬────────────────────┘
                                               │
                                               ▼
                                ┌───────────────────────────────────┐
                                │   Threat Intelligence API         │
                                │-----------------------------------│
                                │ VirusTotal API                    │
                                │ (Fetches Reputation & Context)    │
                                └──────────────┬────────────────────┘
                                               │
                                               ▼
                                ┌───────────────────────────────────┐
                                │   Custom Python Automation        │
                                │   (Inside Shuffle SOAR)           │
                                │-----------------------------------│
                                │ Evaluates VirusTotal Score        │
                                │ Maps Alert to Severity Levels     │
                                └──────────────┬────────────────────┘
                                               │
                                               ▼
                        ┌─────────────────────────────────────┐
                        │         TheHive Case Manager        │
                        │-------------------------------------│
                        │ 1. Creates Incident Case            │
                        │ 2. Attaches Parsed IOCs             │
                        │ 3. Adds VirusTotal Report           │
                        │ 4. Sets Dynamic Severity            │
                        └─────────────────────────────────────┘
