# 04 ServiceNow ITSM
ServiceNow PDI · Incidents · Service Catalog · Change Management · Reporting

---

## [▶️ Lab Walkthrough Video](https://www.loom.com/share/29dab3fa6a624e9cae4dc0e6da4dc945)

## What This Lab Covers

ServiceNow is an IT Service Management platform. It's the system most enterprise IT organizations use to track, route, prioritize, and resolve IT work.

In this lab I set up a ServiceNow Personal Developer Instance and touched the three core ITIL record types. I created users, walked an incident through its lifecycle, built a service catalog item with mandatory intake fields, pushed a change request through approval into a scheduled maintenance window, and built reports.

## Focus Areas

| Area | What I did |
| --- | --- |
| Incident management | Created a ticket, set priority, added work notes, resolved and closed it |
| Service catalog | Built a request form with variables, mandatory fields, and a choice list |
| Change management | Normal change with test and backout plans, approved through a CAB |
| Reporting | Bar report grouped by priority with a relative date condition |

## The Operational Model

Work enters the system as one of three record types. The state model enforces the process: a change can't reach Implement without passing Authorize.

```
            ┌──────────────────────────────────────────────┐
            │            ServiceNow ITSM                    │
            │                                              │
  broken? ──►  INCIDENT                                     │
            │  New → In Progress → Resolved → Closed        │
            │  (work notes + resolution notes)              │
            │                                              │
  want    ──►  SERVICE REQUEST                              │
  something?│  Catalog form → mandatory fields →            │
            │  fulfillment                                  │
            │                                              │
  planned ──►  CHANGE                                       │
  work?     │  New → Assess → Authorize → Scheduled →       │
            │  Implement → Review → Closed                  │
            │  (approval gates: Change Mgmt → CAB)          │
            └──────────────────────────────────────────────┘
```

## Lab Details

| Variable | Value |
| --- | --- |
| Platform | ServiceNow Personal Developer Instance (free) |
| Test users | rachel.kim · tom.davis · sarah.jones |
| Incident | INC0010001, Outlook connectivity, P3 |
| Catalog item | New Laptop Request, 4 variables, 3 mandatory |
| Change | CHG0030001, monthly Windows patching, approved to Scheduled |
| Report | Incident Volume by Priority, Last 30 Days |

## Step-by-Step
 
### Incident
 
An incident is an unplanned interruption to a service. This part of the lab walks a single ticket through its full lifecycle, from creation to close, using a common scenario: a user who can't connect to Outlook.
 
**1.** Went to Service Desk → Incidents → New. This opens a blank incident form.
 
**2.** Filled in Caller, Category, Subcategory, Short description, and Description. The caller is who reported the problem. The short description is a one-line summary. The description holds the full detail, written in the user's own words.
 
**3.** Set Priority to 3, Moderate. Priority is a judgment call based on impact, not a default value. One user was affected and a workaround existed, webmail, so this didn't rise to a higher priority.
 
**4.** Set Assignment group to Service Desk. This routes the ticket into the right team's queue.
 
**5.** Submitted the form. ServiceNow generated the ticket number, INC0010001.
 
**6.** Opened the ticket, set State to In Progress, and assigned it to myself. This marks the ticket as actively being worked instead of sitting untouched in the queue.
 
**7.** Added a work note. Work notes are internal, visible to IT staff only. Logged the diagnosis: confirmed the error with the user, found the Outlook profile was corrupted, started a repair, and told the user to use webmail in the meantime.
 
**8.** Added a resolution note. This is a separate field from the work note, and it documents what actually fixed the problem, not the steps taken to get there. Rebuilt the profile, root cause was a corrupted OST file, user confirmed Outlook was working again.
 
**9.** Set State to Resolved, then Closed. Closing only happens after the user confirms the fix worked.

<img width="1526" height="732" alt="incientresolved" src="https://github.com/user-attachments/assets/dadcda53-93b3-4327-878f-b2aad53cd806" />

---
 
### Service Catalog Item
 
A service catalog item is different from an incident. An incident is an unplanned interruption to a service. A catalog item is a request for something new, in this case a laptop. Catalog items let users self-serve routine requests without calling the help desk for every one.
 
**1.** Went to Service Catalog → Catalog Definitions → Maintain Items → New.
 
**2.** Set Name to New Laptop Request and Category to Hardware.
 
**3.** Opened the Variables tab and added four fields: Requester Name, Business Justification, Required By Date, Laptop Model Preference. Variables are the form fields a requester fills out when submitting the request.
 
**4.** Set Type first on each variable, before touching anything else. The form redraws itself based on Type, so setting it first avoids losing other settings on the field.
 
**5.** Made three of the four variables mandatory: Requester Name, Business Justification, Required By Date. Mandatory fields block submission until they're filled in, which keeps incomplete requests from ever reaching the fulfillment team.
 
**6.** Set Order numbers on each variable: 100, 200, 300, 400. Order controls the sequence fields appear in on the form. Without explicit numbers, fields can render out of sequence.
 
**7.** On the Select Box variable, set both a Text value and a Value for each choice: Standard, Developer, Executive. Text is what the requester sees in the dropdown. Value is what actually gets stored in the database. Leaving Value blank is a common reason a choice won't save correctly.
 
**8.** Saved the item and used Try It to preview the form exactly as a requester would see it in the portal.

<img width="1915" height="848" alt="laptoprequest" src="https://github.com/user-attachments/assets/02df7007-2b37-4688-9116-b614b17ba8a3" />

---
### Change Request
 
A change request is planned work on production infrastructure, in this case monthly security patching. Because it touches production, it requires approval before anything happens.
 
**1.** Navigated directly to the change request list by URL, since the module wasn't easy to find through this release's navigation menu.
 
**2.** Started a new change and set Model to Normal. Normal changes run a full approval flow. Standard changes are pre-approved by definition and would have skipped that flow entirely, so Normal was the right choice here.
 
**3.** Filled in Category, Risk (Low), Impact (2, Medium), Short description, and Description.
 
**4.** Under the Planning tab, wrote a test plan and a backout plan. The test plan says how success gets verified after the patch goes out. The backout plan says what happens if the change needs to be undone.
 
**5.** Under the Schedule tab, set the maintenance window: Saturday, 2 AM to 6 AM. This is the block of time the change is actually allowed to happen in.
 
**6.** Requested approval. This generated two approval records for the Change Management group. Only one approval was needed to move forward.
 
**7.** Approved one of those records as the admin. The second automatically flipped to No Longer Required, and the change moved to the Authorize stage.
 
**8.** Reaching Authorize triggered six more approval records, this time for the CAB, the Change Advisory Board.
 
**9.** Approved one CAB record. The remaining five flipped to No Longer Required, and the change moved to Scheduled.
 
**10.** Left the change sitting in Scheduled. That's where a real change sits until its maintenance window actually opens. I did not click Implement, since the window hadn't arrived yet.

<img width="1528" height="696" alt="changerequest" src="https://github.com/user-attachments/assets/713a67b5-d69e-4482-aaf4-57b856a8357a" />

---

### Reporting
 
Reports convert raw ticket data into a form managers can read: volume, workload, and resolution time. All three reports below use Source type Table, not Data source, pointed at the Incident [incident] table.
 
**Report 1: Incident Volume by Priority**
 
This report answers a basic question: how many tickets came in, broken down by priority.
 
**1.** Went to Reports → Create New and named it Incident Volume by Priority, Last 30 Days.
 
**2.** Set Type to Bar.
 
**3.** Set Group by to Priority.
 
**4.** Added a condition: Created, on, Last 30 days. This limits the report to recent activity instead of every incident ever logged in the instance.
 
**5.** Saved and ran the report.

<img width="1568" height="747" alt="report1" src="https://github.com/user-attachments/assets/7d977d92-f73c-44ce-9f8a-3ad4721ce844" />

<br><br>
 
**Report 2: Open Incidents by Assigned Agent**
 
This report shows workload: how many open tickets each agent is currently carrying.
 
**1.** Went to Reports → Create New and named it Open Incidents by Assigned Agent.
 
**2.** Set Type to Bar.
 
**3.** Set Group by to Assigned to.
 
**4.** Added a condition: Active, is, true. This filters the report down to tickets that are still open, excluding anything already resolved or closed.
 
**5.** Saved and ran the report.

<img width="1568" height="745" alt="report2" src="https://github.com/user-attachments/assets/2495d565-8308-4868-a357-46dd9bc62ff4" />

<br><br>
 
**Report 3: MTTR by Assignment Group**
 
MTTR stands for Mean Time to Resolution. Unlike the first two reports, which count tickets, this one measures how long tickets take to close, grouped by team.
 
**1.** Went to Reports → Create New and named it Mean Time to Resolution by Assignment Group.
 
**2.** Set Type to Bar.
 
**3.** Set Group by to Assignment group.
 
**4.** Changed the Aggregation setting from Count to Average, and selected the Resolve time field. This is the setting that turns the report from answering "how many" into answering "how long."
 
**5.** Added a condition: State, is, Closed. Only closed tickets have a resolve time, since resolution isn't calculated until a ticket is actually done.
 
**6.** Saved and ran the report.

<img width="1568" height="749" alt="report3" src="https://github.com/user-attachments/assets/514b09b7-4936-44ad-8bd5-62664f190f37" />

<br><br>

## Verification

| Check | Result |
| --- | --- |
| Users created and active | ✅ rachel.kim, tom.davis, sarah.jones in sys_user |
| Incident closed | ✅ INC0010001 with work notes and resolution notes |
| Catalog item live | ✅ Renders via Try It, 4 variables in order, mandatory enforced |
| Choice list working | ✅ Standard / Developer / Executive selectable |
| Change approved | ✅ CHG0030001 through New, Assess, Authorize; state = Scheduled |
| Approval trail intact | ✅ Change Manager approval + CAB authorization on the record |
| Report runs | ✅ Bar chart grouped by priority, Created on Last 30 days |

## Troubleshooting

| Problem | Fix |
| --- | --- |
| "Invalid update / Match not found" creating a user | Department is a reference field. It links to existing records and won't accept typed text. Pick from the lookup or leave it blank. |
| Form closed unexpectedly after saving | Update saves and closes the record. Right-click the header bar and choose Save to stay on it. |
| Catalog item list shows "No records to display" | The search box inside the list searches item names. Clear it and navigate with the All menu filter instead. |
| Select Box choice won't submit | Value is required alongside Text. Text is what users see, Value is what gets stored. |
| Catalog form fields display in the wrong order | Variables render by their Order number. Set explicit values (100, 200, 300) or the form scrambles. |
| Change has no approval step | Standard changes are pre-approved by definition. Use a Normal change to see the Assess and Authorize gates. |
| Change module missing from navigation | Go direct: `<instance>.service-now.com/change_request_list.do` |
| Report groups by the wrong field | The main Group by dropdown drives the chart. "Additional group by" is a different setting. Also: Source type is Table, not Data source. |
