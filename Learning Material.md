KQL Learning Material (Beginner to Intermediate)
What is KQL?
Kusto Query Language (KQL) is used to query large datasets (e.g., in Azure Data Explorer, Log Analytics, and Microsoft Sentinel).
1. KQL Operators (Tabular Operators)
These work on tablesand define the flow of data.
✅1.1 Filtering Operators
where
Filters rows based on conditions.
Logs
| where Status == "Failed"
Returns only failed records.
take / limit
Returns a fixed number of rows.
Logs
| take 5
distinct
Removes duplicate values.
Logs
| distinct UserId
✅1.2 Column Operations
project
Select specific columns.
Logs
| project TimeGenerated, UserId
extend
Create new calculated columns.
Logs
| extend UserName = tostring(UserId)
Structured Learning Material
Tuesday, May 12, 2026
9:20 AM
Learn Page 1
project-away
Remove columns.
Logs
| project-away Password
project-rename
Rename columns.
Logs
| project-rename User = UserId
✅1.3 Aggregation Operators
summarize
Group and aggregate data.
Logs
| summarize Total = count() by Status
✅1.4 Sorting Operators
order by / sort
Sort results.
Logs
| order by TimeGenerated desc
top
Get top N values.
Logs
| top 5 by ResponseTime desc
✅1.5 Join & Combine Operators
join
Combine two tables.
Table1
| join Table2 on UserId
union
Combine multiple datasets.
Table1
| union Table2
✅1.6 Data Expansion
mv-expand
Learn Page 2
mv-expand
Expand array values.
Logs
| mv-expand Tags
2. KQL Keywords
Keywords control query structure and logic.
✅Common Keywords
let (Variable Definition)
let recentLogs = Logs | where TimeGenerated > ago(1d);
recentLogs
by (Grouping in summarize)
Logs
| summarize count() by UserId
on (Join condition)
Table1
| join Table2 on Id
as (Alias)
Logs
| summarize Total=count() by UserId
| project UserId as User
✅Logical Keywords
Logs
| where Status == "Failed" and Severity > 3
•
and, or, not
✅Filtering Keywords
Logs
| where Message contains "error"
•
contains
•
has
•
startswith
•
endswith
•
in
•
between
Learn Page 3
3. KQL Functions
Functions work on values or expressions.
✅3.1 Type Conversion Functions
extend User = tostring(UserId)
extend Count = toint("100")
extend Date = todatetime("2024-01-01")
✅3.2 String Functions
extend Length = strlen(Message)
extend Lower = tolower(Message)
extend Part = substring(Message, 0, 5)
✅3.3 Date & Time Functions
Logs
| where TimeGenerated > ago(1d)
Other examples:
now()
startofday(now())
endofday(now())
✅3.4 Conditional Functions
iff()
Logs
| extend StatusType = iff(Status == "Failed", "Error", "OK")
case()
Logs
| extend SeverityLevel = case(
Severity >= 5, "High",
Severity >= 3, "Medium",
"Low"
)
✅3.5 Aggregation Functions
Used with summarize.
Logs
| summarize
Count = count(),
AvgTime = avg(ResponseTime),
MaxTime = max(ResponseTime)
Learn Page 4
MaxTime = max(ResponseTime)
4. End-to-End Example (Realistic Scenario)
let recentLogs = Logs
| where TimeGenerated > ago(1d)
| extend User = tostring(UserId);
recentLogs
| summarize TotalRequests = count() by User
| order by TotalRequests desc
| top 10 by TotalRequests
✅What it does:
•
Filters last 1 day data
•
Converts UserId to string
•
Aggregates requests per user
•
Sorts and returns top 10 users
Quick Revision Cheat Sheet
Category
Examples
Operators
where, summarize, project, join, order by
Keywords
let, by, on, and, or
Functions
tostring(), ago(), count(), iff()
Tips to Learn Faster
•
Think in pipes (|) = step-by-step processing
•
Start with:
○
where → filter
○
project → shape output
○
summarize → analyze
•
Use extend for calculations
=================================================
Here are hands-on KQL exercises with answersdesigned to help you practice step by step—from beginner to intermediate level.
KQL Hands-On Exercises (with Answers)
We’ll assume a sample table called Logswith columns:
•
TimeGenerated (datetime)
•
UserId (string)
•
Status (string: Success/Failed)
•
ResponseTime (int)
•
Severity (int)
•
Message (string)
✅Exercise 1: Basic Filtering
Task
Learn Page 5
Task
Retrieve all logs where Status is "Failed"
✅Answer
Logs
| where Status == "Failed"
✅Exercise 2: Select Columns
Task
Display only TimeGenerated, UserId, and Status
✅Answer
Logs
| project TimeGenerated, UserId, Status
✅Exercise 3: Multiple Conditions
Task
Find failed logs with Severity greater than 3
✅Answer
Logs
| where Status == "Failed" and Severity > 3
✅Exercise 4: Top Records
Task
Get top 5 records with highest ResponseTime
✅Answer
Logs
| top 5 by ResponseTime desc
✅Exercise 5: Sorting
Task
Sort logs by TimeGenerated (latest first)
✅Answer
Logs
| order by TimeGenerated desc
✅Exercise 6: Create New Column
Task
Create a new column StatusType:
•
"Error" if Status = Failed
•
"OK" otherwise
Learn Page 6
•
"OK" otherwise
✅Answer
Logs
| extend StatusType = iff(Status == "Failed", "Error", "OK")
✅Exercise 7: Aggregation
Task
Count number of logs per Status
✅Answer
Logs
| summarize Count = count() by Status
✅Exercise 8: Distinct Values
Task
Get unique users
✅Answer
Logs
| distinct UserId
✅Exercise 9: Time Filtering
Task
Get logs from the last 24 hours
✅Answer
Logs
| where TimeGenerated > ago(1d)
✅Exercise 10: Using let (Variable)
Task
Define a variable for last day logs and display them
✅Answer
let recentLogs = Logs
| where TimeGenerated > ago(1d);
recentLogs
✅Exercise 11: Summarize with Multiple Metrics
Task
For each user:
•
Count logs
•
Average response time
✅Answer
Learn Page 7
✅Answer
Logs
| summarize
Total = count(),
AvgResponse = avg(ResponseTime)
by UserId
✅Exercise 12: String Filtering
Task
Find logs where message contains "error"
✅Answer
Logs
| where Message contains "error"
✅Exercise 13: Case Statement
Task
Create severity labels:
○
=5 → High
○
=3 → Medium
•
Else → Low
✅Answer
Logs
| extend SeverityLabel = case(
Severity >= 5, "High",
Severity >= 3, "Medium",
"Low"
)
✅Exercise 14: Join Example
Assume another table Users(UserId, Department)
Task
Join Logs with Users to show department info
✅Answer
Logs
| join Users on UserId
✅Exercise 15: Combine Tables
Task
Combine Logs from two tables
✅Answer
Logs
Learn Page 8
Logs
| union LogsArchive
✅Exercise 16: Real-World Scenario (SIEM Style)
Task
Find top 5 users with most failed logins in last 24 hours
✅Answer
Logs
| where TimeGenerated > ago(1d)
| where Status == "Failed"
| summarize FailedAttempts = count() by UserId
| order by FailedAttempts desc
| top 5 by FailedAttempts
✅Exercise 17: Data Transformation
Task
Convert UserId to string and show only 10 records
✅Answer
Logs
| extend User = tostring(UserId)
| take 10
✅Exercise 18: Advanced Filter
Task
Find logs where:
•
Severity is between 3 and 5
•
Message starts with "Error"
✅Answer
Logs
| where Severity between (3 .. 5)
| where Message startswith "Error"
Bonus Challenge (Try Yourself)
Task
Find:
•
Last 7 days logs
•
Only failed events
•
Group by UserId
•
Show count
•
Sort descending
✅Try before checking:
✅Answer
Logs
Learn Page 9
Logs
| where TimeGenerated > ago(7d)
| where Status == "Failed"
| summarize Count = count() by UserId
| order by Count desc
Practice Tips
•
Always think in pipeline steps (|)
•
Break queries into:
1.
Filter (where)
2.
Transform (extend)
3.
Aggregate (summarize)
4.
Sort (order by)
===================================================
Mini Project: Log Analysis using KQL
Objective
Analyze application logs to identify:
•
Failed login patterns
•
Slow responses
•
High-risk users
•
Trends over time
Sample Dataset (You can imagine this in a table called AppLogs)
TimeGenerated
UserId
Status
ResponseTime
Severity
Message
2026-05-10 10:00:00
user1
Success
120
1
Login successful
2026-05-10 10:05:00
user2
Failed
200
4
Invalid password
2026-05-10 10:10:00
user1
Failed
350
5
Account locked
2026-05-11 11:00:00
user3
Success
95
1
Login successful
2026-05-11 11:05:00
user2
Failed
250
4
Invalid password
2026-05-11 11:10:00
user2
Failed
300
5
Multiple failed attempts
2026-05-12 09:00:00
user4
Success
110
2
Login successful
Tasks + Solutions
✅Task 1: Identify Failed Logins
Question
Find all failed login attempts.
✅Query
Learn Page 10
AppLogs
| where Status == "Failed"
✅Task 2: Top Users with Failed Logins
Question
Which users have the highest number of failed logins?
✅Query
AppLogs
| where Status == "Failed"
| summarize FailedCount = count() by UserId
| order by FailedCount desc
✅Task 3: Detect Suspicious Users
Question
List users with more than 2 failed attempts
✅Query
AppLogs
| where Status == "Failed"
| summarize FailedAttempts = count() by UserId
| where FailedAttempts > 2
✅Task 4: Slow Performance Detection
Question
Find requests with response time greater than 250 ms
✅Query
AppLogs
| where ResponseTime > 250
✅Task 5: Average Response Time per User
Question
Calculate average response time per user
✅Query
AppLogs
| summarize AvgResponse = avg(ResponseTime) by UserId
✅Task 6: High Severity Events
Question
Find all high severity events (Severity ≥4)
✅Query
AppLogs
Learn Page 11
AppLogs
| where Severity >= 4
✅Task 7: Daily Login Trend
Question
Count number of logins per day
✅Query
AppLogs
| summarize Count = count() by bin(TimeGenerated, 1d)
✅Task 8: Failed Login Trend Over Time
Question
Track failed login attempts per day
✅Query
AppLogs
| where Status == "Failed"
| summarize FailedCount = count() by bin(TimeGenerated, 1d)
✅Task 9: Categorize Logs
Question
Create a column categorizing logs:
•
ResponseTime > 250 → "Slow"
•
Else → "Normal"
✅Query
AppLogs
| extend Performance = iff(ResponseTime > 250, "Slow", "Normal")
✅Task 10: Most Critical Errors
Question
Find top 3 highest severity events
✅Query
AppLogs
| top 3 by Severity desc
✅Task 11: Detect Account Lock Scenario
Question
Find users whose message contains "locked"
✅Query
AppLogs
| where Message contains "locked"
Learn Page 12
✅Task 12: Combine Multiple Conditions
Question
Find failed logins with high severity
✅Query
AppLogs
| where Status == "Failed" and Severity >= 4
✅Final Real-World Detection Query
Scenario
Detect users who:
•
Failed login more than 2 times
•
Within last 2 days
✅Query
AppLogs
| where TimeGenerated > ago(2d)
| where Status == "Failed"
| summarize FailedCount = count() by UserId
| where FailedCount > 2
| order by FailedCount desc
Optional Advanced Challenge
Task
Create a query that:
•
Shows top 3 users
•
With highest avg response time
•
Only for failed requests
✅Answer
AppLogs
| where Status == "Failed"
| summarize AvgResponse = avg(ResponseTime) by UserId
| top 3 by AvgResponse desc
What You Learned in This Project
✅Filtering (where)
✅Aggregation (summarize)
✅Sorting (order by, top)
✅Time analysis (bin(), ago())
✅Calculations (extend, iff)
✅Security detection scenarios
==========================================================================================
SOC Use Cases with KQL (Microsoft Sentinel)
Learn Page 13
SOC Use Cases with KQL (Microsoft Sentinel)
These are mapped to real attack patterns (MITRE-style thinking)and include ready-to-use queries.
1. Brute Force Attack Detection
Scenario
An attacker tries multiple passwords to gain access.
✅Detection Logic
•
Multiple failed logins
•
Same user or IP
•
Within short time window
✅KQL Query
SigninLogs
| where TimeGenerated > ago(1h)
| where ResultType != 0 // Failed logins
| summarize FailedAttempts = count() by UserPrincipalName, IPAddress
| where FailedAttempts > 10
| order by FailedAttempts desc
✅What it detects:
High number of failed login attempts → possible brute force
2. Password Spray Attack
Scenario
One attacker tries same passwordacross many accounts.
✅Detection Logic
•
Same IP
•
Multiple users
•
Failed logins
✅KQL Query
SigninLogs
| where TimeGenerated > ago(1h)
| where ResultType != 0
| summarize UserCount = dcount(UserPrincipalName) by IPAddress
| where UserCount > 20
| order by UserCount desc
✅What it detects:
Single IP targeting many accounts → password spray
3. Impossible Travel (Suspicious Login Locations)
Scenario
User logs in from two distant locations in short time.
✅Detection Logic
•
Same user
•
Different countries
Learn Page 14
•
Different countries
•
Short time gap
✅KQL Query
SigninLogs
| where TimeGenerated > ago(1d)
| summarize Locations = make_set(Location), Times = make_set(TimeGenerated) by UserPrincipalName
| where array_length(Locations) > 1
✅What it detects:
Login location anomalies (geo-based attacks)
4. Privilege Escalation Detection
Scenario
User gains elevated privileges (e.g., Global Admin)
✅Detection Logic
•
Role assignment events
✅KQL Query
AuditLogs
| where OperationName contains "Add member to role"
| where TargetResources contains "Admin"
| project TimeGenerated, InitiatedBy, TargetResources
✅What it detects:
Unauthorized admin role assignment
5. Suspicious PowerShell Activity
Scenario
Attackers execute malicious scripts.
✅Detection Logic
•
Encoded commands
•
Suspicious arguments
✅KQL Query
DeviceProcessEvents
| where ProcessCommandLine contains "-enc"
or ProcessCommandLine contains "Invoke-Expression"
| project TimeGenerated, DeviceName, ProcessCommandLine
✅What it detects:
Fileless malware / script-based attacks
6. Data Exfiltration Detection
Scenario
Large data transfers from a system.
✅Detection Logic
•
Abnormally high outbound traffic
Learn Page 15
•
Abnormally high outbound traffic
✅KQL Query
DeviceNetworkEvents
| summarize TotalBytes = sum(SentBytes) by DeviceName
| where TotalBytes > 1000000000
| order by TotalBytes desc
✅What it detects:
Potential data theft
7. Multiple Account Lockouts
Scenario
Attack causing multiple accounts to lock.
✅Detection Logic
•
Many lockouts in short time
✅KQL Query
SigninLogs
| where ResultDescription contains "locked"
| summarize LockoutCount = count() by UserPrincipalName
| where LockoutCount > 5
✅What it detects:
Active attack or password spray causing lockouts
8. Suspicious File Execution
Scenario
Execution of unusual or malicious files.
✅Detection Logic
•
Unknown file names
•
Temp folders
✅KQL Query
DeviceProcessEvents
| where FolderPath contains "Temp"
| project TimeGenerated, DeviceName, FileName, FolderPath
✅What it detects:
Malware execution from temp directories
9. Lateral Movement Detection
Scenario
Attacker moves across systems.
✅Detection Logic
•
Same account accessing many devices
✅KQL Query
Learn Page 16
DeviceLogonEvents
| summarize DeviceCount = dcount(DeviceName) by AccountName
| where DeviceCount > 5
✅What it detects:
Potential lateral movement
10. Unusual Admin Activity Time
Scenario
Admins logging in at unusual hours.
✅Detection Logic
•
Admin activity outside working hours
✅KQL Query
SigninLogs
| where UserPrincipalName contains "admin"
| extend Hour = datetime_part("hour", TimeGenerated)
| where Hour < 6 or Hour > 20
✅What it detects:
Suspicious admin activity
11. Malware Indicator (Known Bad IP)
Scenario
Traffic to malicious IPs.
✅Detection Logic
•
Match threat intel feed
✅KQL Query
let ThreatIPs = datatable(IP:string)
[
"192.168.1.100",
"10.0.0.5"
];
DeviceNetworkEvents
| where RemoteIP in (ThreatIPs)
✅What it detects:
Communication with known bad actors
12. Ransomware Behavior Detection
Scenario
Rapid file modifications.
✅Detection Logic
•
High file change rate
✅KQL Query
Learn Page 17
DeviceFileEvents
| summarize FileChanges = count() by DeviceName
| where FileChanges > 1000
✅What it detects:
Potential ransomware activity
How SOC Analysts Use These
In real SOC workflow:
1.
Alerts are created from queries
2.
Alerts mapped to MITRE ATT&CK techniques
3.
Analysts investigate:
○
User behavior
○
IP reputation
○
Geo anomalies
4.
Response actions:
○
Block IP
○
Disable account
○
Isolate device
Pro Tips for Real Use
✅Always add:
| where TimeGenerated > ago(1h)
✅Tune thresholds:
•
Reduce false positives
•
Customize per organization
✅Combine signals:
•
Failed logins + geo anomaly = stronger detection
=========================================================================
SOC Use Cases with KQL (Microsoft Sentinel)
These are mapped to real attack patterns (MITRE-style thinking)and include ready-to-use queries.
1. Brute Force Attack Detection
Scenario
An attacker tries multiple passwords to gain access.
✅Detection Logic
•
Multiple failed logins
•
Same user or IP
•
Within short time window
✅KQL Query
SigninLogs
| where TimeGenerated > ago(1h)
| where ResultType != 0 // Failed logins
Learn Page 18
| where ResultType != 0 // Failed logins
| summarize FailedAttempts = count() by UserPrincipalName, IPAddress
| where FailedAttempts > 10
| order by FailedAttempts desc
✅What it detects:
High number of failed login attempts → possible brute force
2. Password Spray Attack
Scenario
One attacker tries same passwordacross many accounts.
✅Detection Logic
•
Same IP
•
Multiple users
•
Failed logins
✅KQL Query
SigninLogs
| where TimeGenerated > ago(1h)
| where ResultType != 0
| summarize UserCount = dcount(UserPrincipalName) by IPAddress
| where UserCount > 20
| order by UserCount desc
✅What it detects:
Single IP targeting many accounts → password spray
3. Impossible Travel (Suspicious Login Locations)
Scenario
User logs in from two distant locations in short time.
✅Detection Logic
•
Same user
•
Different countries
•
Short time gap
✅KQL Query
SigninLogs
| where TimeGenerated > ago(1d)
| summarize Locations = make_set(Location), Times = make_set(TimeGenerated) by UserPrincipalName
| where array_length(Locations) > 1
✅What it detects:
Login location anomalies (geo-based attacks)
4. Privilege Escalation Detection
Scenario
User gains elevated privileges (e.g., Global Admin)
✅Detection Logic
•
Role assignment events
Learn Page 19
•
Role assignment events
✅KQL Query
AuditLogs
| where OperationName contains "Add member to role"
| where TargetResources contains "Admin"
| project TimeGenerated, InitiatedBy, TargetResources
✅What it detects:
Unauthorized admin role assignment
5. Suspicious PowerShell Activity
Scenario
Attackers execute malicious scripts.
✅Detection Logic
•
Encoded commands
•
Suspicious arguments
✅KQL Query
DeviceProcessEvents
| where ProcessCommandLine contains "-enc"
or ProcessCommandLine contains "Invoke-Expression"
| project TimeGenerated, DeviceName, ProcessCommandLine
✅What it detects:
Fileless malware / script-based attacks
6. Data Exfiltration Detection
Scenario
Large data transfers from a system.
✅Detection Logic
•
Abnormally high outbound traffic
✅KQL Query
DeviceNetworkEvents
| summarize TotalBytes = sum(SentBytes) by DeviceName
| where TotalBytes > 1000000000
| order by TotalBytes desc
✅What it detects:
Potential data theft
7. Multiple Account Lockouts
Scenario
Attack causing multiple accounts to lock.
✅Detection Logic
•
Many lockouts in short time
✅KQL Query
Learn Page 20
✅KQL Query
SigninLogs
| where ResultDescription contains "locked"
| summarize LockoutCount = count() by UserPrincipalName
| where LockoutCount > 5
✅What it detects:
Active attack or password spray causing lockouts
8. Suspicious File Execution
Scenario
Execution of unusual or malicious files.
✅Detection Logic
•
Unknown file names
•
Temp folders
✅KQL Query
DeviceProcessEvents
| where FolderPath contains "Temp"
| project TimeGenerated, DeviceName, FileName, FolderPath
✅What it detects:
Malware execution from temp directories
9. Lateral Movement Detection
Scenario
Attacker moves across systems.
✅Detection Logic
•
Same account accessing many devices
✅KQL Query
DeviceLogonEvents
| summarize DeviceCount = dcount(DeviceName) by AccountName
| where DeviceCount > 5
✅What it detects:
Potential lateral movement
10. Unusual Admin Activity Time
Scenario
Admins logging in at unusual hours.
✅Detection Logic
•
Admin activity outside working hours
✅KQL Query
SigninLogs
| where UserPrincipalName contains "admin"
Learn Page 21
| where UserPrincipalName contains "admin"
| extend Hour = datetime_part("hour", TimeGenerated)
| where Hour < 6 or Hour > 20
✅What it detects:
Suspicious admin activity
11. Malware Indicator (Known Bad IP)
Scenario
Traffic to malicious IPs.
✅Detection Logic
•
Match threat intel feed
✅KQL Query
let ThreatIPs = datatable(IP:string)
[
"192.168.1.100",
"10.0.0.5"
];
DeviceNetworkEvents
| where RemoteIP in (ThreatIPs)
✅What it detects:
Communication with known bad actors
12. Ransomware Behavior Detection
Scenario
Rapid file modifications.
✅Detection Logic
•
High file change rate
✅KQL Query
DeviceFileEvents
| summarize FileChanges = count() by DeviceName
| where FileChanges > 1000
✅What it detects:
Potential ransomware activity
How SOC Analysts Use These
In real SOC workflow:
1.
Alerts are created from queries
2.
Alerts mapped to MITRE ATT&CK techniques
3.
Analysts investigate:
○
User behavior
○
IP reputation
○
Geo anomalies
4.
Response actions:
○
Block IP
Disable account
Learn Page 22
○
Disable account
○
Isolate device
Pro Tips for Real Use
✅Always add:
| where TimeGenerated > ago(1h)
✅Tune thresholds:
•
Reduce false positives
•
Customize per organization
✅Combine signals:
•
Failed logins + geo anomaly = stronger detection
Learn Page 23
