# Intent: Results-ready notification

Sources: `inputs/01-approved-prd.md` (PRD N-17, approved for discovery and specification on 2026-09-18) and `inputs/02-current-meeting.md` (discovery notes, 2026-09-20). `inputs/03-superseded-request.md` was superseded by PRD N-17 and is history only: nothing in this intent relies on it.

## Problem

- **I-1** The clinic receives avoidable calls from patients checking whether their results are ready. (PRD: "receives avoidable calls from patients checking whether results are ready")
- **I-2** The problem is status-only calls. Clinical questions are a different kind of call and are counted separately. (Support lead: "count status-only calls separately from clinical questions")

## Proposed outcome

- **I-3** Once a doctor has reviewed and released a result, the patient is told that an update is available and is directed to the existing authenticated portal to view it. (PRD: "Notify the patient when a result is ready for their review in the existing authenticated portal")
- **I-4** In scope: the existing portal result route, preventing duplicate notifications for the same released result version, and handling delivery failures. (PRD Scope and constraints; engineering lead: "the existing portal path is adequate")
- **I-5** This intent covers discovery and specification only. PRD N-17 is approved for discovery and specification, not for build or release.

### How success will be observed

- **I-6** Status-only calls go down: calls that only ask whether results are ready, counted separately from clinical questions and compared against a measured baseline. (PRD: "Measure reduction in results-status calls"; support lead: "compare against a measured baseline")
- **I-7** Delivery failures are measured, and a failed delivery never leads to duplicate patient messages. (PRD: "Measure reduction in results-status calls and delivery failures")
- **I-8** No numeric target is approved. The baseline and target are agreed during discovery (see OPEN-3). Total inbound calls and clinical questions are not success measures.

## Affected users and systems

Users:

- **Patients:** receive the notification and view the result after logging in to the existing portal.
- **Reviewing doctors:** their review and release of a result is the gate for any notification.
- **Support staff:** handle results-status calls and count status-only calls separately from clinical questions.
- **Product, privacy and clinical owners:** own the open decisions below.
- **Engineering:** works within the existing portal result route, duplicate prevention and delivery-failure handling.

Systems:

- The existing authenticated portal result route, where the notification sends the patient.
- The doctor review and release step, which makes a result eligible for notification.
- Notification sending, delivery-failure handling, and the notification and operational logs.

## Out of scope

- **X-1** Portal redesign. (PRD Scope)
- **X-2** Appointment scheduling. (PRD Scope)
- **X-3** Clinical interpretation of results. (PRD Scope)
- **X-4** Notifying third parties. (PRD Scope) The referring doctor is decided separately as OPEN-2, not excluded here.
- **X-5** A new or rebuilt patient dashboard, which is a separate project. (Engineering lead: "A new patient dashboard is a separate project")
- **X-6** Everything that only the superseded early request asked for is excluded: notifying as soon as the lab upload finishes, putting result content in the message, copying the referring doctor, rebuilding the dashboard, and its call-reduction target.

## Constraints

- **C-1** A doctor must review and release the result before any notification is eligible. A finished laboratory upload alone does not make a result ready. (PRD: "A doctor must review and release the result before any notification is eligible"; product owner: "'Ready' must not mean that the laboratory upload merely finished")
- **C-2** The message says only that an update is available and directs the patient to the existing authenticated portal. It contains no diagnosis, test result, clinical measurement or treatment detail. (PRD approved constraints)
- **C-3** Messages, notification logs and operational logs contain no sensitive or clinical content. (PRD; privacy owner: "no sensitive content in messages or operational logs")
- **C-4** Access is authenticated in the existing portal. Results are never attached, and no public result link is introduced. (PRD: "Do not attach results or introduce a public result link")
- **C-5** There is no more than one notification per released result version, and delivery failures are handled without duplicate patient messages. (PRD: "Avoid duplicate notifications for the same released result version")
- **C-6** The patient is the only approved recipient. No other recipient is added until its owners decide (OPEN-2).

## Open questions and owners

- **OPEN-1 Channel:** should the notification go by SMS, email or portal push? **Owners: product owner and privacy owner**, after the consent and delivery review. No channel is assumed until then. (PRD Unresolved decisions; privacy owner: "Consent and the actual channel are still under review")
- **OPEN-2 Referring-doctor notification:** may the referring doctor be notified, and how is that recipient authorized? **Owners: clinical owner and privacy owner.** PRD N-17 does not approve it, and approval must not be inferred from the superseded request. (Clinical owner: "do not infer approval for referring-doctor notifications from the old request")
- **OPEN-3 Baseline and numeric target:** what is the measured baseline of status-only calls, and what target applies? PRD N-17 says both are agreed during discovery. **No source names an owner yet**, so one must be identified.

## Decision record

```json
{
  "notifyWhen": ["doctor-reviewed", "released"],
  "messageMayInclude": ["update-available"],
  "inScope": ["existing-portal-route", "duplicate-prevention", "delivery-failure-handling"],
  "openDecisions": [
    {"topic": "channel", "owners": ["product", "privacy"]},
    {"topic": "referring-doctor-notification", "owners": ["clinical", "privacy"]}
  ],
  "successMeasures": ["status-only-calls", "delivery-failures"],
  "numericTarget": null
}
```
