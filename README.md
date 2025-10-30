Automating Oracle Data Safe Report Delivery Using OCI Object Storage and Email Service

This bash script automates the generation, download, and distribution of Oracle Cloud Infrastructure (OCI) Data Safe reports — including Security Assessment, User Assessment, and Audit Logs.
It uploads them to Object Storage, creates Pre-Authenticated Requests (PARs) for easy access, and sends an email notification via OCI Notifications Service.

Features:

Generates Data Safe Security Assessment and User Assessment reports in PDF format

Extracts and cleans Audit Log data in CSV format

Uploads all reports to OCI Object Storage

Creates Pre-Authenticated Requests (PARs) valid for 7 days

Sends a consolidated email notification with all three download links

Fully automated — can be scheduled via cron job

Prerequisites:

OCI CLI installed and configured
oci --version

If not configured:

oci setup config

Make sure your config file (~/.oci/config) contains:

[DEFAULT]
user=<user_ocid>
fingerprint=<fingerprint>
tenancy=<tenancy_ocid>
region=us-ashburn-1
key_file=/home/opc/.oci/oci_api_key.pem
jq and libreoffice installed:
sudo yum install -y jq libreoffice

IAM Permissions for the CLI user to:
Manage Data Safe Assessments and Reports

Read Compartments

Write to Object Storage

Publish messages to Notifications Service

Existing Resources:
Data Safe Assessments (Security & User)

Object Storage bucket

Notifications topic

Configuration:

Update the following variables in the script before use:

REGION="us-ashburn-1"
SECURITY_ASSESSMENT_OCID="ocid1.datasafesecurityassessment..."
USER_ASSESSMENT_OCID="ocid1.datasafeuserassessment..."
COMPARTMENT_OCID="ocid1.compartment..."
BUCKET_NAME="DataSafe-SecurityAssessment"
EXISTING_TOPIC_OCID="ocid1.onstopic..."
Usage:

Make the script executable:
chmod +x datasafe_daily.sh

Run manually:
./datasafe_daily.sh

Output:
PDFs and CSV uploaded to Object Storage

7-day public PAR links generated

Email notification sent with all links

Schedule Daily Execution (Optional)

To run it daily at 6:45 PM IST, add this cron job:

crontab -e

Add the line:

15 13 * * * /home/opc/datasafe_daily.sh >> ~/datasafe_daily.log 2>&1

Note: 13:15 UTC = 18:45 IST

Output Example:

At the end of execution, you’ll see:

✅ Uploaded to Object Storage: datasafe-assessment-2025-10-30-184500.pdf
✅ Uploaded to Object Storage: datasafe-user-assessment-2025-10-30-184505.pdf
✅ Uploaded to Object Storage: datasafe-audit-logs-2025-10-30-184510.csv
✅ Notification with all reports sent successfully!


