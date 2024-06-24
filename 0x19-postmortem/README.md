 Postmortem Report: Web Application Outage on June 22, 2024

## Issue Summary

- Duration of the Outage: June 22, 2024, from 14:00 UTC to 16:30 UTC (2 hours and 30 minutes)
- Impact:Our primary web application was down. Users experienced 500 Internal Server Errors and could not access the service. Approximately 85% of the users were affected.
- Root Cause: The outage was caused by an unintentional deployment of a misconfigured database connection pool setting during a routine update, leading to database connection exhaustion.

Timeline

- 14:00 UTC: Issue detected via monitoring alert indicating a spike in 500 Internal Server Errors.
- 14:05 UTC: On-call engineer notified and began investigation.
- 14:10 UTC: Initial assumption was a temporary database outage or network issue. Database and network health checks performed.
- 14:25 UTC: Database and network checks returned normal results. Issue escalated to the DevOps team.
- 14:40 UTC: Detailed log analysis started; several potential root causes considered, including application server misconfiguration and potential recent deployment issues.
- 15:00 UTC: Misleading path taken assuming a network firewall issue; network team involved for checks.
- 15:20 UTC: Rollback of recent deployments initiated to test if recent changes were the cause.
- 15:45 UTC: Rollback completed, service began recovering. Confirmed that the recent deployment caused the issue.
- 16:00 UTC: Detailed comparison of configuration settings identified the misconfigured database connection pool as the root cause.
- 16:30 UTC: Correct configuration deployed, and all services confirmed back to normal. Incident resolved.

 Root Cause and Resolution

- Root Cause: During a routine update, a configuration change was introduced to the database connection pool settings. The new configuration set the maximum number of database connections to a value lower than required, causing the application servers to exhaust available connections rapidly. This led to the application being unable to process requests, resulting in 500 Internal Server Errors.
- Resolution: The deployment was rolled back to the previous stable configuration. Once the issue was identified, the correct database connection pool settings were redeployed. Post-redeployment, the system was closely monitored to ensure stability.

 Corrective and Preventative Measures

 Improvements and Fixes

- Enhance pre-deployment testing to include configuration changes.
- Implement automated checks for critical configuration settings before deployment.
- Improve monitoring to detect database connection pool exhaustion more quickly.

 Task List

- Review Deployment Process: Ensure a thorough review process for all configuration changes.
- Automate Configuration Checks: Develop scripts to verify key configuration parameters before allowing a deployment.
- Increase Monitoring Coverage: Add specific alerts for database connection pool usage and thresholds.
- Documentation Update: Update the deployment and rollback procedure documentation to include specific steps for verifying configuration settings.
- Training: Conduct training sessions for the engineering team on the importance of configuration management and monitoring.

By addressing these areas, we aim to prevent similar incidents in the future and enhance the reliability of our deployment processes.i
