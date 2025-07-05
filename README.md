# Autonomous SIEM Alert Enrichment and Ticketing

This workflow provides a powerful automation for Security Operations Centers (SOCs) and IT teams. It takes raw security alerts from a SIEM (Security Information and Event Management) system, enriches them with threat intelligence from the MITRE ATT&CK framework, and automatically creates detailed, context-rich tickets in Zendesk.

## Overview

Security analysts are often overwhelmed by a high volume of alerts, many of which lack sufficient context for immediate action. This workflow solves that problem by programmatically enriching each alert with relevant tactics, techniques, and procedures (TTPs) from the MITRE ATT&CK knowledge base. By using a Qdrant vector database to store and search the ATT&CK data, the system can quickly find the most relevant context for any given alert, allowing analysts to prioritize and respond to threats more effectively.

## Features

* **Automated Enrichment:** Triggers on new security alerts from any SIEM system capable of sending a webhook.
* **Vectorized Threat Intelligence:** Uses a pre-populated Qdrant vector database containing the entire MITRE ATT&CK framework for fast, semantic searching.
* **Contextual Analysis:** For each alert, it searches the Qdrant database to find related threat actor groups, techniques, and mitigation strategies.
* **Ticket Creation:** Automatically creates a new, detailed ticket in Zendesk for each enriched alert.
* **Structured Ticket Information:** The Zendesk ticket is populated with the original alert data plus all the enriched context from MITRE ATT&CK, giving analysts a comprehensive view of the threat.

## How It Works

1.  **Ingest ATT&CK Data (Prerequisite):** A separate workflow (not included) is used to process the MITRE ATT&CK STIX data, convert it to vector embeddings, and store it in a Qdrant collection.
2.  **Receive Alert:** The workflow is triggered by a webhook from a SIEM system, which sends the raw alert data (e.g., IP addresses, event descriptions).
3.  **Query Qdrant:** The core information from the alert is used to query the Qdrant vector store. This similarity search finds the most relevant entries from the MITRE ATT&CK framework.
4.  **Aggregate Context:** The workflow aggregates the top results from the Qdrant search to build a rich context block.
5.  **Create Zendesk Ticket:** A new ticket is created in Zendesk. The ticket description includes the original alert details along with the enriched information, such as:
    * Suspected Threat Actor Groups
    * Relevant ATT&CK Techniques (e.g., T1059.001 - PowerShell)
    * Recommended Mitigations
6.  **Assign and Notify:** The ticket can be automatically assigned to the appropriate SOC team or analyst.

## Technologies Used

* n8n
* Qdrant (Vector Database)
* Zendesk
* MITRE ATT&CK Framework
* An embedding model (e.g., from OpenAI or a self-hosted alternative)

## Setup & Configuration

1.  **Populate Qdrant:** You must first populate your Qdrant collection with the MITRE ATT&CK data. This typically involves a separate data ingestion workflow.
2.  **Qdrant Credentials:** Add your Qdrant API key and URL to your n8n credentials.
3.  **Zendesk Credentials:** Add your Zendesk API credentials to n8n.
4.  **SIEM Webhook:** Configure your SIEM tool to send alert notifications to the n8n webhook URL. You will need to adjust the initial node to correctly parse the data format from your specific SIEM.

## How to Use

1.  Ensure your Qdrant database is populated with the ATT&CK data.
2.  Activate the n8n workflow.
3.  Trigger an alert from your SIEM system.
4.  Check Zendesk for a new, enriched ticket containing both the original alert and the contextual information from MITRE ATT&CK.

This automation transforms raw alerts into actionable intelligence, significantly reducing mean time to respond (MTTR) for security incidents. [book a chat here😁](https://cal.com/closegem/coffee-chat)
