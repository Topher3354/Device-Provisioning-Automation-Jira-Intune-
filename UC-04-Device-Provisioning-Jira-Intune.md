# Use Case 04 — Device Provisioning Automation (Jira → Intune)

**Category:** IT Operations & ITSM
**Author:** Christopher Ayodeji Ojo — [Trublshut](https://whop.com/trublshut)
**Tools:** Jira REST API · Microsoft Intune API · Microsoft Power Automate · Microsoft Entra ID · SharePoint

---

## 🔴 The Problem

Device provisioning requires IT to manually monitor Jira for approved requests, then separately open Intune, configure the device profile, assign policies, enrol the device, update the Jira ticket, and notify the user.

Each of those steps happens in a different system. Each requires a human to remember to do it. The result is delays, configuration drift, missing policy assignments, and frustrated employees waiting on hardware they need to work.

---

## ✅ The Solution

An end-to-end integration that reads approved device requests from Jira and automatically triggers device provisioning in Microsoft Intune — assigning configuration profiles, compliance policies, and application packages — then updates the ticket and notifies the user.

Approved to ready. No manual steps in between.

---

## ⚙️ How It Works

```
Jira device request ticket created + approved
        ↓
Power Automate trigger fires on Jira status change → "Approved"
        ↓
Read ticket fields: device type, user, department, start date
        ↓
Call Intune API: create device configuration profile
        ↓
Assign correct compliance policies based on department
        ↓
Assign application packages (standard + role-specific apps)
        ↓
Enrol device under the correct Entra ID user object
        ↓
Update Jira ticket: status → "Device Ready" + add comment
        ↓
Notify user via Teams / Email with setup instructions
        ↓
Log provisioning record to asset tracker (SharePoint)
```

---

## 🛠️ Tech Stack

| Tool | Role |
|------|------|
| Power Automate | Trigger on Jira status change + orchestration |
| Jira REST API | Ticket reading + status update + comment |
| Microsoft Intune API | Device profile creation + policy + app assignment |
| Microsoft Entra ID | Device enrolment under correct user |
| SharePoint | Asset lifecycle tracking log |

---

## 🔧 Key Logic

- **Trigger condition:** Power Automate watches for Jira tickets with `issue_type = Device Request` and `status = Approved`
- **Device type mapping:** ticket field `device_type` (Laptop / Mobile / Tablet) maps to the correct Intune configuration profile template
- **Department policy mapping:** department field maps to the correct compliance policy set (Finance gets stricter encryption requirements than general staff, etc.)
- **App assignment:** role field maps to a pre-defined app package list in Intune — standard apps + role-specific tools
- **Enrolment:** device is enrolled under the requesting user's Entra ID object using the user's UPN from the ticket
- **Asset tracking:** provisioning record written to SharePoint list with: device serial, user, department, policies applied, provisioning date

---

## 📊 Outcomes

- ✅ Device provisioning time cut from hours to minutes
- ✅ Consistent policy assignment — no manual configuration drift
- ✅ Every device enrolled correctly under the right user from day one
- ✅ Full device lifecycle audit trail from request through to decommission
- ✅ IT team focused on exceptions, not routine provisioning tasks

---

## 📁 How to Use This

1. In Power Automate, create a flow triggered by a Jira webhook on ticket status change to "Approved"
2. Add condition: `issue_type = Device Request`
3. Read ticket fields: device type, department, requesting user UPN
4. Call Microsoft Intune API to create a configuration profile from the correct template
5. Assign the department's compliance policy and app packages via Intune API
6. Enrol the device under the user's Entra ID object
7. Call Jira REST API to update ticket status and add a provisioning summary comment
8. Send Teams/email notification to user with device collection or setup instructions
9. Write a row to SharePoint asset tracker: device, user, date, policies applied

---

## 🔗 Related Use Cases

- [Use Case 02 — Automated Employee Onboarding Pipeline](./UC-02-Automated-Employee-Onboarding-Pipeline.md)
- [Use Case 03 — Automated Employee Offboarding & Access Revocation](./UC-03-Employee-Offboarding-Access-Revocation.md)

---

*© 2026 Christopher Ayodeji Ojo · Trublshut AI Automation · christopherayodeji131@gmail.com*
