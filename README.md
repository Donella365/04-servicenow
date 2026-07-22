# 04 ServiceNow ITSM
ServiceNow PDI · Incidents · Service Catalog · Change Management · Reporting

---

## [▶️ Lab Walkthrough Video] (https://www.loom.com/share/29dab3fa6a624e9cae4dc0e6da4dc945)

## What This Lab Covers

ServiceNow is an IT Service Management platform. It's the system most enterprise IT organizations use to track, route, prioritize, and resolve IT work.

In this lab I set up a ServiceNow Personal Developer Instance and touched the three core ITIL record types. I created users, walked an incident through its lifecycle, built a service catalog item with mandatory intake fields, pushed a change request through approval into a scheduled maintenance window, and built a report on incident volume by priority.

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

## What I Built

**Users first.** Tickets need callers. You can't log an incident against nobody, so I created three users with titles so every record traces to a person.

**An incident.** A user couldn't connect to Outlook. I created the ticket and set priority off impact: one user down, webmail as a workaround, so a P3. Moved it to In Progress, assigned it to myself, and logged work notes as I went. Rebuilt the profile, wrote the resolution note, closed it. One thing I kept deliberate: the caller's description stays in the caller's plain words. The technical detail goes in the work notes. Tickets come in vague, the analyst makes them precise.

**A service catalog item.** Different thing from an incident. Incident means broken, request means someone wants something. I built a laptop request form with four fields: requester, business justification, date needed, model preference. Three are mandatory, so no request arrives half-empty. Data quality gets enforced at intake instead of chased after.

**A change request.** Monthly security patching for Windows servers. That's planned work on production, so it goes through change management. I documented risk, impact, a test plan, and a backout plan, then scheduled it into a Saturday 2-6 AM maintenance window. Approval ran in two rounds: a change manager signed off on the assessment, then a CAB (Change Advisory Board) authorized it. The change now sits in Scheduled until the window opens.

**A report.** Incident volume by priority over the last 30 days. Reports are how IT ops sees where the volume is, what's aging, and who's overloaded.

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
