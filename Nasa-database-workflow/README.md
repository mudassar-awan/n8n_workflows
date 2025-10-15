# NASA Solar Flare Monitoring Workflow

**Description:**
This workflow provides an automated solution for monitoring **NASA's solar flare data** using **n8n**. It periodically fetches recent solar flare events from the NASA DONKI API, filters them based on their classification, and can be configured to send notifications to a specified endpoint.

---

## 🧠 Workflow Overview

**Workflow Name:** `Nasa-database-workflow`
**Platform:** [n8n.io](https://n8n.io)
**Data Source API:** [NASA DONKI API](https://api.nasa.gov/)

This workflow acts as an **automated data pipeline** between the NASA API and a notification service. It runs on a predefined schedule, pulls the latest solar flare data, and uses conditional logic to decide whether an event is significant enough to trigger an alert. It’s a practical example of using n8n for real-time data monitoring and event-driven automation.

---

## ⚙️ Key Features

- 🤖 **Automated Monitoring:** Runs on a schedule to automatically check for new solar flare data.
- ☀️ **Real-time NASA Data:** Directly integrates with the official NASA DONKI (Database of Notifications, Knowledge, Information) API.
- 🔄 **Conditional Filtering:** Processes and filters events based on specific criteria (e.g., flare class).
- ⚙️ **Low Code:** Built entirely with n8n’s visual automation platform.
- 🧩 **Extensible:** Easily connect the output to any service like Slack, Discord, email, or a custom API for notifications.

---

## 🧩 Workflow Steps (Simplified)

1.  **Schedule Trigger**
    -   Initiates the workflow automatically on a recurring schedule (e.g., hourly).

2.  **Get a DONKI solar flare (NASA Node)**
    -   Fetches solar flare data from the last 20 days using the NASA API.
    -   Requires NASA API credentials to authenticate.

3.  **If Condition**
    -   Checks the `classType` of each flare to see if it meets a specific condition (e.g., contains 'c').

4.  **Send Request (PostBin Node)**
    -   Sends a formatted message to a webhook endpoint based on the result of the `If` condition.
    -   This node acts as a placeholder for a real notification service.

---

## 🧰 Prerequisites

| Requirement | Description |
| :--- | :--- |
| n8n Account | An active instance of n8n (Cloud or self-hosted). [https://n8n.io](https://n8n.io) |
| NASA API Key | A free API key from NASA for accessing their data. [https://api.nasa.gov/](https://api.nasa.gov/) |

---

## 🔐 Credentials

This workflow uses n8n's built-in credential management system instead of environment variables for the API key.

| Credential | Description |
| :--- | :--- |
| `NASA API Credential` | Stored within n8n to authenticate with the NASA API. You will need to add your NASA API key here. |

---

## 🚀 Usage

1.  Clone the repository:
    ```bash
    git clone [https://github.com/mudassar-awan/n8n_workflows.git](https://github.com/mudassar-awan/n8n_workflows.git)
    ```

2.  **Import Workflow**: In your n8n canvas, import the `Nasa-database-workflow.json` file.

3.  **Configure Credentials**: The `Get a DONKI solar flare` node requires a NASA API Key. Create a new `NASA` credential in your n8n instance and select it in the node.

4.  **Customize Endpoint**: Replace the `PostBin` nodes with your desired notification service (e.g., Slack, Email) and configure the message.

5.  **Activate Workflow**: Save and activate the workflow to start monitoring.
