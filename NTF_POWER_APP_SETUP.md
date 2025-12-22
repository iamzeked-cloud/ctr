# NTF Power App - Complete Setup Guide
**Corner Table Restaurants - Note to File Submission System**

---

## 1. SHAREPOINT LIST SCHEMAS

### 1.1 Locations List
**List Name:** `Locations`

| Column | Type | Description |
|--------|------|-------------|
| Company Code | Single Line Text | Required. e.g., 101 |
| Company Name | Single Line Text | e.g., 3rd Avenue Hospitality, LLC |
| DBA Name | Single Line Text | e.g., The Smith East Village |
| Acronym | Single Line Text | e.g., TSEV |

**Locations Data:**
```
101 | 3rd Avenue Hospitality, LLC | The Smith East Village | TSEV
102 | TS2 Hospitality, LLC | The Smith Midtown | TSMT
103 | TS3 Hospitality, LLC | The Smith Lincoln Square | TSLS
104 | TS4 Hospitality, LLC | The Smith Nomad | TSNM
105 | TS5 Hospitality, LLC | The Smith Penn Quarter | TSPQ
106 | TS6 Hospitality, LLC | The Smith U Street | TSUS
107 | TS7 Hospitality, LLC | The Smith River North | TSRN
202 | Columbus Avenue Hospitality, LLC | Parla | PUWS
901 | Corner Table Restaurants, LLC | Corner Table Restaurants | CTR
```

---

### 1.2 Employees List
**List Name:** `Employees`

| Column | Type | Description |
|--------|------|-------------|
| Employee Name | Single Line Text | Required. Full name |
| ID# | Single Line Text | Required. Employee ID |
| Job Title | Single Line Text | Required. e.g., "Sous Chef", "FOH Manager" |
| Location | Lookup | Lookup to Locations list (DBA Name) |
| Org Level | Single Line Text | Department category (FOH or BOH) |
| Company Code | Single Line Text | Reference to location |

**Org Level Classification:**
- **FOH:** GM, AGM, FOH Manager, Office Manager, Director of Ops
- **BOH:** CDC, EX Sous, Sous, Corp Chef, Pastry, Receiver

---

### 1.3 Supervisors List
**List Name:** `Supervisors`

| Column | Type | Description |
|--------|------|-------------|
| Team Member | Single Line Text | Required. Employee being supervised |
| Employee Name | Single Line Text | Employee name (duplicate for easy lookup) |
| Supervisor 1 | Lookup | Lookup to Employees list |
| Supervisor 2 | Lookup | Lookup to Employees list (nullable) |
| Location | Lookup | Automatically populated based on Team Member |

---

### 1.4 Management by Location
**This determines who receives NTFs based on location and department**

#### The Smith East Village (101)
| Role | Name | Department | Reports To |
|------|------|-----------|-----------|
| GM | JOEY ROESEL | FOH | — |
| AGM | MARC LICO | FOH | — |
| FOH Manager | LISA LOUKAS | FOH | — |
| Office Manager | JENNIFER BEISHEIM | FOH | — |
| Director of Ops | JENNIFER WOODHULL | FOH | — |
| Corp Chef | CHUCK SCHMIER | BOH | — |
| EX Sous | KARIM BINTOU TOURAY | BOH | — |
| EX Sous | VICTOR CALDERON | BOH | — |
| Sous | GUILERMO MARTINEZ | BOH | — |
| Sous | ROBERTO MORALES | BOH | — |

#### The Smith Midtown (102)
| Role | Name | Department | Reports To |
|------|------|-----------|-----------|
| GM | JON KARSKY | FOH | — |
| AGM | KERRY-ANN MADDEN | FOH | — |
| FOH Manager | MARSHALL MADSEN | FOH | — |
| FOH Manager | KAYA HUMPHREYS | FOH | — |
| Office Manager | JENNIFER BEISHEIM | FOH | — |
| Director of Ops | JENNIFER WOODHULL | FOH | — |
| Corp Chef | CHUCK SCHMIER | BOH | — |
| CDC | JUAN LUCERO | BOH | — |
| EX Sous | LE PROPHETE AULELEY | BOH | — |
| EX Sous | FERNANDO BAUTISTA | BOH | — |
| Sous | JUAN RODRIGUEZ | BOH | — |
| Sous | FERNANDO SERRANO-GUERRERO | BOH | — |
| Receiver | JEAN CARLOS BAUTISTA | BOH | — |

#### The Smith Lincoln Square (103)
| Role | Name | Department | Reports To |
|------|------|-----------|-----------|
| GM | MONICA MANERI | FOH | — |
| AGM | QUINN WOLFENDON | FOH | — |
| FOH Manager | TIM HERNANDEZ | FOH | — |
| FOH Manager | PETER ROMANIELLO | FOH | — |
| FOH Manager | ARNAUD SPANOS | FOH | — |
| FOH Manager | KEVIN O'MALLEY | FOH | — |
| FOH Manager | MICHAEL CHAN | FOH | — |
| Office Manager | CHARESSE CONYERS | FOH | — |
| Director of Ops | JENNIFER WOODHULL | FOH | — |
| Corp Chef | CHUCK SCHMIER | BOH | — |
| CDC | PATRICK TRACEY | BOH | — |
| EX Sous | CRISTIAN DOMINGUEZ | BOH | — |
| EX Sous | MOHAMMED AYISHAO | BOH | — |
| Sous | JOHANA MARQUEZ | BOH | — |
| Sous | ROBERTO MORALES | BOH | — |
| Pastry | CESAR AREVALO | BOH | — |
| Receiver | DEVIN BROWN | BOH | — |

#### The Smith Nomad (104)
| Role | Name | Department | Reports To |
|------|------|-----------|-----------|
| GM | AUGUSTO GALINDO | FOH | — |
| AGM | GERARDO SOLIS | FOH | — |
| FOH Manager | STEVEN RIGGLE | FOH | — |
| FOH Manager | JESSICA SCHUFELD | FOH | — |
| FOH Manager | ULI HERRERA | FOH | — |
| FOH Manager | ALEX ESTRELLA | FOH | — |
| Office Manager | CHARESSE CONYERS | FOH | — |
| Director of Ops | JOSH BIDWELL | FOH | — |
| Corp Chef | CHUCK SCHMIER | BOH | — |
| EX Sous | DANIEL MORENO | BOH | — |
| EX Sous | ANTHONY PEREA | BOH | — |
| Sous | ERICK PEREZ LUNA | BOH | — |
| Sous | WILLIE CLEMENTE | BOH | — |
| Sous | ROBERTO RAMOS | BOH | — |

#### The Smith Penn Quarter (105)
| Role | Name | Department | Reports To |
|------|------|-----------|-----------|
| GM | RASHEEN GEORGE | FOH | — |
| AGM | DOUGLAS BERRYHILL | FOH | — |
| FOH Manager | ASHANTI BOWENS | FOH | — |
| FOH Manager | CHRISTOPHER BEST | FOH | — |
| Office Manager | JENNIFER BEISHEIM | FOH | — |
| Director of Ops | REBECCA SMITH | FOH | — |
| Corp Chef | AMANDO AULELEY | BOH | — |
| CDC | AHMED IBRAHIM | BOH | — |
| Sous | DAVID CANIZALEZ | BOH | — |
| Sous | MARTIR GUEVARA | BOH | — |
| Sous | DANILO MOTTA | BOH | — |

#### The Smith River North (107)
| Role | Name | Department | Reports To |
|------|------|-----------|-----------|
| GM | ERIC BRYDA | FOH | — |
| AGM | ELDRIDGE WILLIAMS | FOH | — |
| FOH Manager | ASHLEY DEROUSSEAU | FOH | — |
| FOH Manager | AAKIFA PATEL | FOH | — |
| Office Manager | CHARESSE CONYERS | FOH | — |
| Director of Ops | REBECCA SMITH | FOH | — |
| Corp Chef | AMANDO AULELEY | BOH | — |
| CDC | ANDREW ACKER | BOH | — |
| EX Sous | CORTNEY PIERECE | BOH | — |
| Sous | JUSTIN GAMINO | BOH | — |
| Sous | CARLOS MORENO | BOH | — |
| Sous | CODY LUTZ | BOH | — |

#### Parla (202)
| Role | Name | Department | Reports To |
|------|------|-----------|-----------|
| GM | PETER STALEY | FOH | — |
| FOH Manager | ZACH WHITMAN | FOH | — |
| FOH Manager | SAMANTHA WAGNER | FOH | — |
| Office Manager | CHARESSE CONYERS | FOH | — |
| Corp Chef | MICHAEL KOLLARIK | BOH | — |
| EX Sous | GIOVANNI CASTILLA | BOH | — |
| Sous | SEAN MCNORTON | BOH | — |

---

### 1.5 NTF Submissions List
**List Name:** `NTF Submissions`

| Column | Type | Description |
|--------|------|-------------|
| Title | Single Line Text | Auto-generated: "NTF - [Location] - [Date]" |
| Submitter | Lookup | Lookup to Employees list |
| Location | Lookup | Lookup to Locations list |
| Submitter Department | Single Line Text | FOH or BOH (auto-populated) |
| Supervisor 1 | Lookup | From Supervisors list |
| Supervisor 2 | Lookup | From Supervisors list (nullable) |
| Date of Incident | Date | Required |
| Incident Summary | Multiple Lines Text | Required. Include witnesses, location, time, dates |
| Team Comments | Multiple Lines Text | Comments from team member(s) |
| Corrective Action | Multiple Lines Text | Corrective action/plan moving forward |
| Policy Acknowledgment | Yes/No | Team member acknowledges understanding |
| Leadership Plan | Multiple Lines Text | Leadership's plan to ensure accountability |
| Additional Conversations | Multiple Lines Text | If more conversations needed, include when they'll occur |
| Participants | Multiple Lines Text | Who participated in the conversation |
| Confidential | Yes/No | If Yes, only HR receives (default: No) |
| Recipients List | Multiple Lines Text | Auto-generated list of email recipients |
| Status | Choice | Draft, Submitted, In Review, Closed (default: Submitted) |
| PDF File Path | Single Line Text | Path to OneDrive PDF |
| Created | Date | Auto |
| Created By | Person | Auto |
| Modified | Date | Auto |
| Modified By | Person | Auto |

---

## 2. RECIPIENT ROUTING LOGIC

### Non-Confidential NTF
**Recipients always include:**
1. Submitter's Supervisor 1
2. Submitter's Supervisor 2 (if exists)
3. Director of Ops at their location
4. HR (emails TBD)

**Additional recipients based on department:**

**If FOH submitter adds:**
- All FOH team members at that location
- Corp Chef at that location

**If BOH submitter adds:**
- All BOH team members at that location
- Corp Chef at that location

### Confidential NTF
**Recipients:**
- HR only (emails TBD)
- Submitter's Supervisor 1 and 2 are EXCLUDED

---

## 3. POWER APPS CANVAS APP DESIGN

### Color Scheme (Corner Table Restaurants Branding)
- **Primary (Ink):** #001E5A (Navy Blue)
- **Secondary (Linen):** #F0E6B4 (Cream)
- **Text:** #001E5A on Linen, #F0E6B4 on Ink
- **Accent:** White for highlights

### App Structure

#### Screen 1: Welcome/Home
- Company logo/branding header
- Title: "Note to File Submission"
- Subtitle: "Submit an NTF quickly and easily"
- Button: "Start New NTF"

#### Screen 2: Location Selection
- Dropdown: Select Location (from Locations list)
- Displays: DBA Name and location details
- Navigation: Next/Back buttons

#### Screen 3: Employee Selection
- Dropdown: Select Employee (filtered by selected location)
- Displays: Employee name, Job title, Department (auto-populated)
- Auto-populates: Supervisor 1, Supervisor 2, Department (FOH/BOH)
- Navigation: Next/Back buttons

#### Screen 4: Incident Details
- **Date of Incident** (Date picker) - Required
- **Incident Summary** (Large text box) - Required
  - Placeholder: "Include witnesses, location, time, and dates"
- Navigation: Next/Back buttons

#### Screen 5: Team Comments & Corrective Action
- **Team Member Comments** (Large text box) - Optional
- **Corrective Action/Plan** (Large text box) - Required
- **Policy Acknowledgment** (Checkbox) - Required
  - "I acknowledge understanding of the policy and suggested improvements"
- Navigation: Next/Back buttons

#### Screen 6: Leadership & Additional Info
- **Leadership's Plan** (Large text box) - Required
- **Additional Conversations** (Large text box) - Optional
  - Placeholder: "If additional conversations will be had, please note when they will occur"
- **Participants** (Large text box) - Required
  - Placeholder: "Who participated in this conversation?"
- Navigation: Next/Back buttons

#### Screen 7: Confidentiality & Review
- **Confidential Submission** (Toggle/Checkbox)
  - Help text: "If checked, only HR will receive this NTF. Otherwise, your supervisors and team will receive it."
- **Review Summary:**
  - Display all filled information in read-only format
  - Submitter name, location, supervisors
  - Show who will receive this NTF (dynamically generated)
- Navigation: Submit/Back buttons

#### Screen 8: Confirmation
- Success message
- Displays: Submission received
- Summary of recipients
- Message: "A PDF has been generated and sent to all recipients. You will receive a confirmation email."
- Button: "Submit Another NTF" (returns to Screen 1)
- Button: "Return to Home"

---

## 4. POWER AUTOMATE FLOW

### Trigger
**When a new item is created in NTF Submissions list**

### Flow Steps

1. **Get Submitter Details**
   - Get the full employee record from Employees list

2. **Determine Department**
   - Use Job Title to classify FOH or BOH
   - Store in a variable

3. **Get Location Details**
   - Get all management from that location

4. **Build Recipient List**
   - IF Confidential = TRUE
     - Recipients = HR only
   - IF Confidential = FALSE
     - Always include: Supervisor 1, Supervisor 2, Director of Ops
     - IF FOH: Add all FOH staff + Corp Chef
     - IF BOH: Add all BOH staff + Corp Chef
     - Always add: HR

5. **Get Supervisor Email Addresses**
   - Lookup Supervisor 1 email from Employees list
   - Lookup Supervisor 2 email from Employees list (if exists)

6. **Get All Staff Emails for Department**
   - Query Employees list for all staff at location + same department
   - Get their email addresses

7. **Build Email String**
   - Combine all unique email addresses with semicolons

8. **Update NTF List Item**
   - Set Recipients List field with email string
   - Set Status = "Submitted"

9. **Generate PDF**
   - Use Word Online connector or HTML to PDF
   - Format submission data into professional PDF
   - Include: All form fields, location, submitter, date, recipients
   - Use CTR branding colors in header

10. **Save PDF to OneDrive**
    - Save with naming convention: `NTF_[Location]_[Submitter]_[Date].pdf`
    - Example: `NTF_TSEV_JoeyRoesel_2025-12-22.pdf`

11. **Send Confirmation Email to Submitter**
    - Subject: "Your NTF has been submitted"
    - Body: Confirmation message with recipient list

12. **Send NTF PDF to Recipients**
    - Subject: "Note to File - [Location] - [Date]"
    - Body: Professional message with PDF attachment
    - BCC: HR if not already included
    - Recipients: Email string from step 7

---

## 5. IMPLEMENTATION CHECKLIST

### Phase 1: SharePoint Lists
- [ ] Create Locations list with all 9 locations
- [ ] Populate Employees list (import from existing list if available)
- [ ] Create Supervisors list with manager relationships
- [ ] Create NTF Submissions list with all fields
- [ ] Set up lookup relationships between lists
- [ ] Test: Verify lookups work correctly

### Phase 2: Power Apps Canvas App
- [ ] Create new Canvas App in Power Apps Studio
- [ ] Add branding/colors to all screens
- [ ] Build Screen 1: Welcome
- [ ] Build Screen 2: Location Selection
- [ ] Build Screen 3: Employee Selection with auto-population
- [ ] Build Screen 4: Incident Details
- [ ] Build Screen 5: Team Comments & Corrective Action
- [ ] Build Screen 6: Leadership & Additional Info
- [ ] Build Screen 7: Confidentiality & Review
- [ ] Build Screen 8: Confirmation
- [ ] Add navigation logic between screens
- [ ] Test: Form submission to SharePoint list
- [ ] Test: Data validation and required fields

### Phase 3: Power Automate
- [ ] Create flow: Trigger on NTF Submissions item created
- [ ] Add recipient logic (Confidential vs. Normal)
- [ ] Add PDF generation step
- [ ] Add OneDrive save step
- [ ] Add email routing logic
- [ ] Test: Submit sample NTF and verify recipients
- [ ] Test: PDF generation and formatting
- [ ] Test: Email delivery

### Phase 4: Testing & Refinement
- [ ] Test: FOH submission routing
- [ ] Test: BOH submission routing
- [ ] Test: Confidential submission (HR only)
- [ ] Test: Multi-supervisor routing
- [ ] Test: PDF formatting with data
- [ ] Test: Email attachments
- [ ] Refine: UI/UX based on testing

### Phase 5: Deployment
- [ ] Share Power App with all 30 users
- [ ] Share NTF Submissions list
- [ ] Provide user documentation
- [ ] Train key users
- [ ] Monitor initial submissions
- [ ] Address any issues

---

## 6. ADDITIONAL NOTES

- **HR Email Addresses:** TBD - Add HR email addresses to a configuration list or Power Automate variable
- **Director of Ops:** Each location has a Director of Ops who should always receive non-confidential NTFs
- **Corp Chef:** Each location has a Corp Chef who should always receive non-confidential NTFs
- **Office Manager:** JENNIFER BEISHEIM and CHARESSE CONYERS work at multiple locations - ensure they're correctly assigned
- **PDF Naming:** Use format `NTF_[ACRONYM]_[Submitter]_[YYYY-MM-DD].pdf` for easier organization
- **Version History:** No versioning needed - only latest submission is stored
- **Draft Saves:** Consider adding auto-save to draft functionality if needed

---

## 7. NEXT STEPS

1. **Verify data accuracy** in the management roster (ensure all employees are in the Employees list)
2. **Get HR email addresses** for confidential routing
3. **Set up SharePoint lists** with provided schema
4. **Build Power Apps** canvas app with provided specifications
5. **Create Power Automate flow** with recipient routing logic
6. **Test with sample submissions** from different departments/locations
7. **Refine UI and routing** based on testing feedback
8. **Deploy to users** with training documentation

---

**Last Updated:** December 22, 2025
**Version:** 1.0
**Status:** Ready for Implementation
