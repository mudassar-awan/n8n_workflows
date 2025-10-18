# Payment Status Email Notifier  (Google Sheets & Gmail)

This n8n workflow automates the accounts receivable (A/R) process by reading invoice data from a Google Sheet, filtering for outstanding payments, and sending customized email reminders via Gmail based on the invoice's payment status.

<img width="1360" height="606" alt="image" src="https://github.com/user-attachments/assets/7f8206e5-31de-476e-818d-fbfd704dba7c" />

## What it Does

* Fetches all invoice records from a designated Google Sheet.
* Filters out all invoices that are already marked as "Paid."
* Sends different email reminders based on whether an invoice is "Unpaid" or "Overdue."
* Flags any invoices with an unusual status (e.g., "Pending," "Partial," or error) by sending an internal email for manual review.

## How it Works

The workflow executes in the following sequence:

1.  **Trigger:** The workflow is initiated **manually** when a user clicks "Execute workflow."
2.  **Get Data:** Connects to Google Sheets and retrieves all rows from the specified sheet.
3.  **Filter:** It filters this data, allowing only the rows where the `Payment Status` column **does not equal** "Paid" to proceed.
4.  **Branch (Switch):** The workflow then uses a Switch node to route the outstanding invoices into one of three paths based on their `Payment Status`:

    * **Path 1: "Unpaid"**
        * **Condition:** `Payment Status` equals "Unpaid".
        * **Action:** Sends a "due soon" reminder email to the `Customer Email`.
        * **Subject:** `Your invoice is due`

    * **Path 2: "Overdue"**
        * **Condition:** `Payment Status` equals "Overdue".
        * **Action:** Sends a "past due" warning email to the `Customer Email`.
        * **Subject:** `Hello Important! Your inovice is past due`

    * **Path 3: Fallback (Other)**
        * **Condition:** Catches any status that is not "Paid," "Unpaid," or "Overdue."
        * **Action:** Sends an internal notification email (to `aiml@`) for manual review.
        * **Subject:** `accont verification`

## Dependencies & Setup

To use this workflow, you will need:

1.  **n8n:** A running n8n instance.
2.  **Credentials:**
    * **Google Sheets OAuth2:** Credentials to read the spreadsheet.
    * **Gmail OAuth2:** Credentials to send emails.
3.  **Google Sheet Structure:** The source sheet *must* contain the following columns (names are case-sensitive):
    * `Payment Status`
    * `Customer Email`
    * `Customer Name`
    * `Invoice Due`

## How to Use

1.  Download the `workflow.json` file from this repository.
2.  Import the workflow into your n8n instance.
3.  Configure the **"Get row(s) in sheet"** node by selecting your Google Sheets credential and specifying your Document ID and Sheet Name.
4.  Configure the three **"Send a message" (Gmail)** nodes by selecting your Gmail credential.
5.  Activate the workflow.

**Recommendation:** For full automation, replace the **"Manual Trigger"** node with a **"Schedule"** node (e.g., set to run once every day).

Usage:
git clone https://github.com/mudassar-awan/n8n_workflows.git

The workflow contains a few typos in the email subject lines which you may want to correct:
* **Overdue Subject:** `Your inovice is past due` -> `Your invoice is past due`
* **Fallback Subject:** `accont verification` -> `account verification`
