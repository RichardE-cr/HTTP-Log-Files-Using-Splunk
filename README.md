# Using Splunk SIEM to Inspect HTTP Logs

## Introduction
This lab explores the application of Splunk Security Information and Event Management (SIEM) in analyzing HTTP log files to gain insights into web traffic, detect unusual activity, and identify security threats. Participants will create a controlled environment to ingest, query, and interpret log data, simulating realistic cybersecurity scenarios. The lab emphasizes analyzing HTTP traffic patterns and utilizing Splunk's Search Processing Language (SPL) for comprehensive log analysis.

## Objectives
The aim of this lab is to provide hands-on experience in:
- Ingesting and analyzing HTTP logs using Splunk SIEM.
- Detecting anomalies and potential threats in web traffic.
- Utilizing SPL queries to extract, correlate, and visualize meaningful data.
- Developing skills essential for SOC Tier 1 analysts, including detecting Indicators of Compromise (IoCs) and presenting findings.

## Skills Developed
- Parsing and analyzing HTTP log files for cybersecurity insights.
- Writing and executing SPL queries to filter and correlate log data.
- Identifying and interpreting common attack patterns, such as SQL injection and cross-site scripting (XSS).
- Detecting IoCs and reporting actionable intelligence.
- Creating dashboards and reports to visualize security events.

## Tools and Resources
- **Splunk Enterprise**: SIEM tool for log management and security analytics.
- **HTTP log samples**: Pre-collected datasets for analysis.
- **Web Browser**: Access Splunk's web-based interface.
- **Text Editor**: Review and understand raw log data.

## Lab Workflow

### 1. Uploading HTTP Log Files to Splunk
- **Logged into Splunk** and navigated to **Settings** > **Add Data**.
- Selected **Upload** as the input method and chose the sample log file.
- Specified the source type (`access_combined`) and verified other settings (index, host, sourcetype).
- Uploaded and confirmed that HTTP events were visible using a search query:
    ```spl
    index=http_logs sourcetype=access_combined
    ```

### 2. Analyzing HTTP Events
- **Search Queries for Analysis**:
  - Filtered HTTP GET requests:
    ```spl
    index=http_logs method=GET
    ```
  - Identified potential SQL injection attempts:
    ```spl
    index=http_logs uri_query="*' OR '1'='1" | stats count by src_ip
    ```
  - Detected unusual HTTP status codes (e.g., 500 errors):
    ```spl
    index=http_logs status>=500 | stats count by status, uri_path
    ```
  - Tracked top 10 requested URLs:
    ```spl
    index=http_logs | stats count by uri_path | sort -count limit=10
    ```

### 3. Detecting Anomalies
- Searched for unusual file transfer activity:
  ```spl
  index=http_logs | timechart span=1h count by uri_path
  ```
- Investigated high volumes of error responses:
  ```spl
  index=http_logs | stats count by status | where status >= 400
  ```
- Monitored traffic from suspicious IPs:
  ```spl
  index=http_logs | search src_ip="192.168.1.100"
  ```

### 4. Visualizing Findings
- **Dashboards Created**:
  - Distribution of HTTP status codes.
  - Top accessed endpoints by users.

### 5. Reporting and Use Cases
- Generating detailed reports on:
  - Potential attack patterns detected (e.g., brute force attempts, SQL injections).
  - Traffic anomalies and response trends.
  - Recommendations for mitigating risks.

- **Real-World Application Scenarios**:
  - Analyzing a spike in 500 error codes indicating possible server misconfigurations or DDoS attacks.
  - Investigating repeated failed login attempts to detect brute-force attacks.
  - Identifying malicious traffic sources for IP blocklisting.

## Conclusion
By completing this lab, I acquired the skills to effectively analyze HTTP logs, detect anomalies, and identify IoCs using Splunk SIEM. These competencies align with the responsibilities of a SOC Tier 1 analyst, including log monitoring, threat detection, and reporting. The ability to create meaningful dashboards and actionable reports demonstrates a readiness to contribute to cybersecurity operations in real-world scenarios



