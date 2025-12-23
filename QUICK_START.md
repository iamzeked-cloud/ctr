# NTF Power App - Quick Start Guide

**Get your Power App running in 1-2 weeks**

---

## STEP 0: Prerequisites (Check These First)

Before you start, confirm you have:

- ✅ **SharePoint site** where you can create lists
- ✅ **Power Apps environment** access (powerapps.microsoft.com)
- ✅ **Power Automate** access (make.powerautomate.com)
- ✅ **OneDrive** for PDF storage
- ✅ **Employees list** with names, emails, job titles (even basic version is fine)

**Don't have an Employees list yet?** Start with Task 1 below.

---

## TASK 1: Prepare Your Employee Data (2-3 hours)

### 1a. Find or Create Your Employees List

**Option A: You already have an Employees list**
- [ ] Open SharePoint
- [ ] Go to your Employees list
- [ ] Check it has these columns:
  - Employee Name
  - Email
  - Job Title
  - Company Code (location identifier: 101, 102, 103, etc.)

**Option B: You need to create one**
- [ ] Go to SharePoint
- [ ] Click "+ New" → "List"
- [ ] Name it: `Employees`
- [ ] Click "Create"
- [ ] Add these columns:
  - "Employee Name" (Single line text)
  - "Email" (Email column)
  - "Job Title" (Single line text)
  - "Company Code" (Single line text)

### 1b. Add Employee Data

**Quick way:** Import from CSV
- Open Excel
- Create columns: Employee Name | Email | Job Title | Company Code
- Add your ~30 team members
- Save as CSV
- In SharePoint list → "Data" tab → "Get data" → "Upload CSV"

**Or:** Enter manually (list view)

**Important:** Use these company codes for locations:
```
101 = The Smith East Village
102 = The Smith Midtown
103 = The Smith Lincoln Square
104 = The Smith Nomad
105 = The Smith Penn Quarter
106 = The Smith U Street
107 = The Smith River North
202 = Parla
901 = Corner Table Restaurants
```

### 1c. Verify Your Data

- [ ] All employees have an email
- [ ] All have a job title
- [ ] All have correct company code
- [ ] No duplicates

**Example:**
```
JOEY ROESEL | joey.roesel@cornertablerestaurants.com | GM | 101
MARC LICO | marc.lico@cornertablerestaurants.com | AGM | 101
LISA LOUKAS | lisa.loukas@cornertablerestaurants.com | FOH Manager | 101
```

---

## TASK 2: Create SharePoint Lists (2 hours)

### 2a. Create Locations List

1. Go to SharePoint
2. Click "+ New" → "List"
3. Name: `Locations`
4. Create

5. Add these columns:
   - Title (rename to "Company Code")
   - Add: "Company Name" (Text)
   - Add: "DBA Name" (Text)
   - Add: "Acronym" (Text)

6. Add all 9 locations (copy from README.md or use below):

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

✅ **Done:** Locations list created

---

### 2b. Create Supervisors List

1. Go to SharePoint
2. Click "+ New" → "List"
3. Name: `Supervisors`
4. Create

5. Add these columns:
   - Title (rename to "Team Member")
   - Add: "Employee Name" (Text)
   - Add: "Supervisor 1" (Lookup → Employees → Employee Name)
   - Add: "Supervisor 2" (Lookup → Employees → Employee Name, allow blank)

6. For each manager in your organization, add who they report to
   - Example: JOEY ROESEL → Supervisor 1: [CEO/Director Name]
   - Or leave blank if they're at top level

✅ **Done:** Supervisors list created

---

### 2c. Create NTF Submissions List

1. Go to SharePoint
2. Click "+ New" → "List"
3. Name: `NTF Submissions`
4. Create

5. Add these columns: (copy from IMPLEMENTATION_GUIDE.md Phase 2, Task 2.4)

**Quick method - Copy/paste these:**

```
Submitter (Lookup - Employees - Employee Name)
Location (Lookup - Locations - DBA Name)
Submitter Department (Single line text)
Supervisor 1 (Lookup - Employees - Employee Name)
Supervisor 2 (Lookup - Employees - Employee Name)
Date of Incident (Date)
Incident Summary (Multiple lines of text)
Team Comments (Multiple lines of text)
Corrective Action (Multiple lines of text)
Policy Acknowledgment (Yes/No)
Leadership Plan (Multiple lines of text)
Additional Conversations (Multiple lines of text)
Participants (Multiple lines of text)
Confidential (Yes/No)
Recipients List (Multiple lines of text)
Status (Choice: Draft, Submitted, In Review, Closed)
PDF File Path (Single line text)
```

✅ **Done:** NTF Submissions list created

---

## TASK 3: Create Your Power Apps Canvas App (4-6 hours)

### 3a. Create a New Canvas App

1. Go to **Power Apps** (powerapps.microsoft.com)
2. Click **"+ Create"**
3. Select **"Canvas app from blank"**
4. **Name:** `NTF Submission Portal`
5. **Format:** Select **Tablet** (horizontal - better for forms)
6. Click **"Create"**

✅ **App created** - It will open in edit mode

---

### 3b. Connect Your Data Sources

In Power Apps Studio:

1. Left sidebar → **"Data"** tab
2. Click **"Add data"**
3. Search for your SharePoint site
4. Add these lists:
   - [ ] Locations
   - [ ] Employees
   - [ ] Supervisors
   - [ ] NTF Submissions

✅ **Data connected**

---

### 3c. Design Screen 1: Welcome Home

1. **Delete** the default blank screen
2. **Insert** a new blank screen
3. On this screen, add:

**Header (top section):**
- Rectangle shape
- Fill color: `#001E5A` (Navy blue)
- Width: Full width
- Height: 80

**Text in header:**
- Text: "CTR Note to File"
- Color: White
- Font size: 32, Bold

**Body:**
- Background: `#F0E6B4` (Cream)
- Title text: "Note to File Submission" (size 48, navy)
- Subtitle: "Submit an NTF quickly and securely" (size 18, navy)

**Button:**
- Text: "Start New NTF"
- Fill: `#001E5A` (Navy)
- Text color: White
- OnSelect: `Navigate(Screen2, ScreenTransition.Fade)`

✅ **Screen 1 complete**

---

### 3d. Design Screen 2: Location Selection

1. **Insert** new blank screen
2. Rename to `Screen2`

**Header:**
- Rectangle `#001E5A`, full width
- Text: "Step 1 of 7 - Select Location" (white, size 24)

**Body:**
- Label: "Which location are you submitting from?"
- Dropdown control
  - Name: `ddLocation`
  - Items: `Locations`
  - Value: `DBA Name`
  - OnChange: `Set(varLocation, Self.Selected)`

**Buttons:**
- "Back" → `Navigate(Screen1, ScreenTransition.Fade)`
- "Next" → `If(IsBlank(ddLocation.Selected), Notify("Select a location"), Navigate(Screen3, ScreenTransition.Fade))`

✅ **Screen 2 complete**

---

### 3e. Design Screen 3: Employee Selection

1. **Insert** new blank screen
2. Rename to `Screen3`

**Header:**
- Text: "Step 2 of 7 - Select Employee"

**Body:**
- Label: "Who is submitting this NTF?"
- Dropdown: `ddEmployee`
  - Items: `Filter(Employees, 'Company Code' = varLocation.'Company Code')`
  - Value: `'Employee Name'`
  - OnChange:
    ```
    Set(varSubmitter, Self.Selected);
    Set(varSupervisors, First(Filter(Supervisors, 'Team Member' = Self.Selected.'Employee Name')))
    ```

**Display selected info:**
- Show: `varSubmitter.'Employee Name'`
- Show: `varSubmitter.'Job Title'`
- Show: Supervisors: `varSupervisors.'Supervisor 1'.'Employee Name'`

**Buttons:**
- "Back" → `Navigate(Screen2)`
- "Next" → `If(IsBlank(ddEmployee.Selected), Notify("Select employee"), Navigate(Screen4))`

✅ **Screen 3 complete**

---

### 3f. Design Screen 4: Incident Details

1. **Insert** new blank screen
2. Rename to `Screen4`

**Header:** "Step 3 of 7 - Incident Details"

**Body:**
- Label: "Date of Incident *"
- DatePicker control
  - Default: `Today()`
  - OnSelect: `Set(varIncidentDate, Self.SelectedDate)`

- Label: "Incident Summary *"
- TextInput (multiline)
  - Mode: Multiline
  - OnChange: `Set(varIncidentSummary, Self.Value)`

**Buttons:**
- "Back" → `Navigate(Screen3)`
- "Next" → `If(IsBlank(varIncidentSummary), Notify("Required field"), Navigate(Screen5))`

✅ **Screen 4 complete**

---

### 3g. Design Screen 5: Comments & Action

1. **Insert** new blank screen
2. Rename to `Screen5`

**Header:** "Step 4 of 7 - Comments & Corrective Action"

**Body:**
- Label: "Team Comments (Optional)"
- TextInput multiline: `varTeamComments`

- Label: "Corrective Action *"
- TextInput multiline: `varCorrectiveAction`

- Checkbox: "I acknowledge understanding"
- OnChange: `Set(varAcknowledgement, Self.Value)`

**Buttons:**
- "Back" → `Navigate(Screen4)`
- "Next" → `If(Or(IsBlank(varCorrectiveAction), Not(varAcknowledgement)), Notify("Please complete all required fields"), Navigate(Screen6))`

✅ **Screen 5 complete**

---

### 3h. Design Screen 6: Leadership Info

1. **Insert** new blank screen
2. Rename to `Screen6`

**Header:** "Step 5 of 7 - Leadership Information"

**Body:**
- Label: "Leadership's Plan for Accountability *"
- TextInput multiline: `varLeadershipPlan`

- Label: "Additional Conversations (Optional)"
- TextInput multiline: `varAdditionalConversations`

- Label: "Who Participated? *"
- TextInput multiline: `varParticipants`

**Buttons:**
- "Back" → `Navigate(Screen5)`
- "Next" → `If(Or(IsBlank(varLeadershipPlan), IsBlank(varParticipants)), Notify("Complete required fields"), Navigate(Screen7))`

✅ **Screen 6 complete**

---

### 3i. Design Screen 7: Review & Confidential

1. **Insert** new blank screen
2. Rename to `Screen7`

**Header:** "Step 6 of 7 - Review & Confidentiality"

**Body - Confidentiality section:**
- Label: "Is this submission confidential?"
- Toggle control: `tglConfidential`
- OnChange: `Set(varConfidential, Self.Value)`
- Help text: "If checked, only HR receives this. Otherwise, your supervisors and team receive it."

**Body - Review section:**
- Display read-only:
  - Location: `varLocation.'DBA Name'`
  - Submitter: `varSubmitter.'Employee Name'`
  - Supervisors: `varSupervisors.'Supervisor 1'.'Employee Name'`
  - Date: `Text(varIncidentDate, "mm/dd/yyyy")`

**Body - Recipients section:**
- Label: "This NTF will be sent to:"
- If Confidential: "HR Department Only"
- If Not Confidential: Show list of supervisors + team + HR

**Buttons:**
- "Back" → `Navigate(Screen6)`
- "Submit" → (see next section)

✅ **Screen 7 complete**

---

### 3j. Design Screen 8: Confirmation

1. **Insert** new blank screen
2. Rename to `Screen8`

**Header:** "Submission Received ✓" (with green accent)

**Body:**
- Success icon
- Title: "Your NTF has been submitted successfully"
- Message: "A PDF is being generated and sent to recipients. Check your email for confirmation."
- Summary box showing:
  - Location: `varLocation.'DBA Name'`
  - Submitted by: `varSubmitter.'Employee Name'`
  - Submitted on: `Text(Now(), "mm/dd/yyyy hh:mm AM/PM")`

**Buttons:**
- "Submit Another NTF" → Reset all variables and go to Screen2
- "Return Home" → Go to Screen1

✅ **Screen 8 complete**

---

### 3k. Add Submit Logic to Screen 7

**On the "Submit" button, add this OnSelect formula:**

```powerapps
Collect(
    'NTF Submissions',
    {
        Submitter: {Value: varSubmitter.ID},
        Location: {Value: varLocation.ID},
        'Submitter Department': If(
            Or(varSubmitter.'Job Title' = "GM", varSubmitter.'Job Title' = "AGM", varSubmitter.'Job Title' = "FOH Manager", varSubmitter.'Job Title' = "Office Manager", varSubmitter.'Job Title' = "Director of Ops"),
            "FOH",
            "BOH"
        ),
        'Supervisor 1': {Value: varSupervisors.'Supervisor 1'.ID},
        'Supervisor 2': If(IsBlank(varSupervisors.'Supervisor 2'), Blank(), {Value: varSupervisors.'Supervisor 2'.ID}),
        'Date of Incident': varIncidentDate,
        'Incident Summary': varIncidentSummary,
        'Team Comments': varTeamComments,
        'Corrective Action': varCorrectiveAction,
        'Policy Acknowledgment': varAcknowledgement,
        'Leadership Plan': varLeadershipPlan,
        'Additional Conversations': varAdditionalConversations,
        Participants: varParticipants,
        Confidential: varConfidential,
        Status: "Submitted"
    }
);
Notify("NTF submitted! PDF is being generated...", NotificationType.Success);
Navigate(Screen8, ScreenTransition.Fade)
```

✅ **Submit logic complete**

---

### 3l. Save and Test Your App

1. Click **"Save"** (top right)
2. Name your app: `NTF Submission Portal`
3. Click **"Publish"**
4. Test by clicking through all screens
5. Submit a test form

✅ **Power Apps Canvas App Complete!**

---

## TASK 4: Test Your App (30 minutes)

### Test checklist:
- [ ] All screens load without errors
- [ ] Location dropdown filters correctly
- [ ] Employee dropdown shows employees from selected location
- [ ] Form validates required fields
- [ ] Submit button creates item in NTF Submissions list
- [ ] Confirmation screen displays
- [ ] Check SharePoint list - can you see the submitted item?

**Check the list:**
1. Go to SharePoint
2. Open "NTF Submissions" list
3. Look for your test submission
4. All fields populated? ✅

---

## WHAT'S NEXT (After Power App Works)

Once your Power App is submitting data successfully, you'll build the **Power Automate Flow** to:

1. ✅ Automatically route to correct recipients (based on location/department)
2. ✅ Generate PDF
3. ✅ Save PDF to OneDrive
4. ✅ Send emails to recipients

**But first:** Make sure your Power App is working and creating list items!

---

## QUICK REFERENCE: Color Codes

Copy/paste these into your designs:

| Element | Color | Hex Code |
|---------|-------|----------|
| Headers/Buttons | Navy Blue | `#001E5A` |
| Backgrounds | Cream | `#F0E6B4` |
| Text (on cream) | Navy Blue | `#001E5A` |
| Text (on navy) | White | `#FFFFFF` |

---

## TROUBLESHOOTING

**"Dropdown doesn't show any items"**
- Check: Is your list connected in Data tab?
- Check: Is the column name correct?

**"Submit button doesn't work"**
- Check: Are you referencing correct list name?
- Check: All variable names spelled correctly?

**"Navigation isn't working"**
- Make sure screen names match (Screen1, Screen2, etc.)
- Check: Button OnSelect formula is correct

---

## WHERE TO GET HELP

- **Power Apps docs:** https://docs.microsoft.com/powerapps
- **Formulas reference:** https://docs.microsoft.com/powerapps/maker/canvas-apps/formula-reference
- **Your documentation:** See POWER_APPS_TECHNICAL_SPECS.md for detailed specs

---

**Ready? Start with TASK 1: Prepare Employee Data**

Once complete, move to TASK 2: Create SharePoint Lists, then TASK 3: Build Power App.

Good luck! 🚀
