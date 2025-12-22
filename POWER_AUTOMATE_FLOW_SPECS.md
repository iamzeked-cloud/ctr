# Power Automate - NTF Routing & PDF Generation Flow

**Flow Name:** NTF_SubmissionProcessor
**Trigger:** When a new item is created in NTF Submissions list
**Version:** 1.0
**Created:** December 22, 2025

---

## FLOW OVERVIEW

```
Trigger: Item Created in NTF Submissions
    ↓
Get Submitter Details
    ↓
Get Supervisors (1 & 2)
    ↓
Determine Department (FOH/BOH)
    ↓
[Decision: Is Confidential?]
    ├─ YES → HR Only Recipients
    └─ NO → Build Full Recipient List
            ├─ Get Location Staff
                ├─ FOH: Get all FOH staff
                ├─ BOH: Get all BOH staff
                └─ Get Director of Ops + Corp Chef
            └─ Combine with Supervisors
    ↓
Get Email Addresses for All Recipients
    ↓
Generate PDF
    ↓
Save PDF to OneDrive
    ↓
Update List Item with PDF Path & Recipients
    ↓
Send Confirmation Email to Submitter
    ↓
Send NTF PDF to Recipients
```

---

## DETAILED FLOW STEPS

### Step 1: Trigger
**Trigger:** "When an item is created" - NTF Submissions list

**Trigger Condition:**
```
Status equals 'Submitted'
```

**Trigger Configuration:**
- List: NTF Submissions
- Item Properties to Include: All

---

### Step 2: Get Submitter Details
**Action:** Get item from Employees list

**Configuration:**
```
List: Employees
Id: trigger.Submitter (the ID value from the list item)
```

**Output Variables:**
- submitterEmail: outputs('Get_Submitter_Details')?['body/Email']
- submitterName: outputs('Get_Submitter_Details')?['body/Employee Name']
- submitterJobTitle: outputs('Get_Submitter_Details')?['body/Job Title']

---

### Step 3: Get Supervisor 1 Details
**Action:** Get item from Employees list

**Configuration:**
```
List: Employees
Id: trigger.'Supervisor 1' (the ID value)
```

**Output Variables:**
- supervisor1Email: outputs('Get_Supervisor_1')?['body/Email']
- supervisor1Name: outputs('Get_Supervisor_1')?['body/Employee Name']

---

### Step 4: Get Supervisor 2 Details (Conditional)
**Action:** Condition - Check if Supervisor 2 exists

**Condition:**
```
not(equals(trigger.'Supervisor 2', null))
```

**If True:** Get item from Employees list
```
List: Employees
Id: trigger.'Supervisor 2' (the ID value)
```

**If False:** Initialize null

**Output Variables:**
- supervisor2Email: (if exists) outputs('Get_Supervisor_2')?['body/Email']
- supervisor2Name: (if exists) outputs('Get_Supervisor_2')?['body/Employee Name']

---

### Step 5: Determine Department
**Action:** Initialize variable

**Variable Name:** departmentType
**Type:** String
**Value:**
```
trigger.'Submitter Department'
```

---

### Step 6: Build Recipients List - MAIN DECISION POINT
**Action:** Condition - Check if Confidential

**Condition:**
```
equals(trigger.Confidential, true)
```

---

## BRANCH A: CONFIDENTIAL SUBMISSION

### Step 6A.1: Get HR Email Addresses
**Action:** Get items from Employees list

**Filter:**
```
'Job Title' contains 'HR' OR 'Job Title' contains 'Human Resources'
```

**Alternative:** Manually set HR emails:
**Action:** Initialize variable

**Variable Name:** hrEmails
**Type:** String
**Value:**
```
"hr@cornertablerestaurants.com;director_hr@cornertablerestaurants.com"
```
*(Update with actual HR email addresses)*

### Step 6A.2: Set Recipients for Confidential
**Action:** Initialize variable

**Variable Name:** recipientList
**Type:** Array
**Value:**
```
split(variables('hrEmails'), ';')
```

### Step 6A.3: Create Recipients String
**Action:** Initialize variable

**Variable Name:** recipientEmails
**Type:** String
**Value:**
```
join(variables('recipientList'), ';')
```

---

## BRANCH B: NON-CONFIDENTIAL SUBMISSION

### Step 6B.1: Initialize Recipients Array
**Action:** Initialize variable

**Variable Name:** recipientArray
**Type:** Array
**Value:**
```
[]
```

### Step 6B.2: Add Supervisors to Recipients
**Action:** Append to array variable

**Variable Name:** recipientArray
**Value:**
```
createObject(
    'email', variables('supervisor1Email'),
    'name', variables('supervisor1Name')
)
```

**Then append Supervisor 2** (if exists):
```
If supervisor2 exists, append:
createObject(
    'email', variables('supervisor2Email'),
    'name', variables('supervisor2Name')
)
```

---

### Step 6B.3: Get Location All Staff
**Action:** Get items from Employees list

**Filter (based on Department):**

**If FOH:**
```
'Company Code' equals '[location_code]' AND
('Job Title' equals 'GM' OR 'Job Title' equals 'AGM' OR 'Job Title' equals 'FOH Manager' OR 'Job Title' equals 'Office Manager' OR 'Job Title' equals 'Director of Ops' OR 'Job Title' equals 'Corp Chef')
```

**If BOH:**
```
'Company Code' equals '[location_code]' AND
('Job Title' equals 'CDC' OR 'Job Title' equals 'EX Sous' OR 'Job Title' equals 'Sous' OR 'Job Title' equals 'Corp Chef' OR 'Job Title' equals 'Pastry' OR 'Job Title' equals 'Receiver')
```

**How to implement:** Use a condition to check department, then apply appropriate filter

---

### Step 6B.4: Get Director of Ops at Location
**Action:** Get items from Employees list

**Filter:**
```
'Company Code' equals '[location_code]' AND 'Job Title' equals 'Director of Ops'
```

---

### Step 6B.5: Get Corp Chef at Location
**Action:** Get items from Employees list

**Filter:**
```
'Company Code' equals '[location_code]' AND 'Job Title' equals 'Corp Chef'
```

---

### Step 6B.6: Append All Staff to Recipients
**Action:** Apply to each

**Loop through:** outputs of Step 6B.3

**In each loop:**
```
Append to array variable (recipientArray)
Value: createObject(
    'email', items('Apply_to_each')['Email'],
    'name', items('Apply_to_each')['Employee Name']
)
```

---

### Step 6B.7: Append Director of Ops
**Action:** Append to array variable

**Loop through:** outputs of Step 6B.4

**In each loop:**
```
Append to array variable (recipientArray)
Value: createObject(
    'email', items('Apply_to_each')['Email'],
    'name', items('Apply_to_each')['Employee Name']
)
```

---

### Step 6B.8: Append Corp Chef
**Action:** Append to array variable

**Loop through:** outputs of Step 6B.5

**In each loop:**
```
Append to array variable (recipientArray)
Value: createObject(
    'email', items('Apply_to_each')['Email'],
    'name', items('Apply_to_each')['Employee Name']
)
```

---

### Step 6B.9: Add HR Department
**Action:** Append to array variable

**Variable:** recipientArray
**Value:**
```
createObject(
    'email', variables('hrEmails'),
    'name', 'HR Department'
)
```

---

### Step 6B.10: Remove Duplicates and Build Email String
**Action:** Compose - Create unique recipient list

**Input:**
```
join(
    addProperty(
        outputs('recipientArray'),
        'Unique',
        true
    ),
    ';'
)
```

**Store in:** recipientEmails variable

---

## COMMON STEPS (Both Branches)

### Step 7: Generate PDF Document
**Action:** Word Online - Create document from template (or use HTML to PDF)

**Option A: Using Word Template**
- Create template in OneDrive with placeholders
- Use "Word Online" connector to populate template
- Fields to include:

```
[COMPANY_NAME] - Corner Table Restaurants
[LOCATION] - [DBA Name]
[SUBMIT_DATE] - [Current Date]

SUBMITTED BY: [Submitter Name]
DEPARTMENT: [FOH/BOH]
SUPERVISORS: [Supervisor 1], [Supervisor 2]

---

DATE OF INCIDENT:
[Incident Date]

INCIDENT SUMMARY:
[Incident Summary]

TEAM MEMBER COMMENTS:
[Team Comments]

CORRECTIVE ACTION / PLAN:
[Corrective Action]

POLICY ACKNOWLEDGMENT:
[Yes/No]

LEADERSHIP'S PLAN FOR ACCOUNTABILITY:
[Leadership Plan]

ADDITIONAL CONVERSATIONS:
[Additional Conversations]

WHO PARTICIPATED:
[Participants]

---
CONFIDENTIAL: [Yes/No]
RECIPIENTS: [List of recipient emails]
```

**Option B: Using HTML to PDF (Free alternative)**
- Create HTML string with NTF data
- Use "HTML to PDF" connector (if available)
- Or use "Send an email with attachment" with PDF rendering

---

### Step 8: Save PDF to OneDrive
**Action:** Create file in OneDrive

**Configuration:**
```
Location: OneDrive > Documents > NTF_Submissions (create folder if needed)
File Name:
    concat(
        'NTF_',
        triggerBody()['fields']['Location'],  // or use acronym
        '_',
        split(triggerBody()['fields']['Submitter Name'], ' ')[1],  // Last name
        '_',
        formatDateTime(utcNow(), 'yyyy-MM-dd'),
        '.pdf'
    )
File Content: [PDF from Step 7]
```

**Example:** `NTF_TSEV_Roesel_2025-12-22.pdf`

---

### Step 9: Update List Item with Metadata
**Action:** Update item - NTF Submissions list

**Configuration:**
```
Id: trigger.ID
Fields:
  - PDF File Path: [OneDrive URL from Step 8]
  - Recipients List: [recipientEmails variable]
  - Status: "In Review"
```

---

### Step 10: Send Confirmation Email to Submitter
**Action:** Send an email (V2)

**Configuration:**
```
To: variables('submitterEmail')
Subject: "Your NTF has been submitted"
Body:
    Hi [submitterName],

    Your Note to File has been successfully submitted and processed.

    Submission Details:
    • Location: [Location DBA Name]
    • Submitted: [Current Date/Time]
    • Submitted By: [Submitter Name]
    • Department: [FOH/BOH]

    Confidential: [Yes/No]

    A PDF copy has been generated and sent to all necessary recipients.

    If you have any questions, please contact HR.

    Best regards,
    Corner Table Restaurants
```

**Attachments:** [PDF from Step 8]

---

### Step 11: Send NTF to Recipients
**Action:** Send an email (V2)

**Configuration:**
```
To: [recipientEmails]
Subject: "Note to File Submission - [Location] - [Date]"
Body:
    Team,

    A Note to File has been submitted and requires your attention.

    SUBMITTER: [Submitter Name]
    LOCATION: [Location DBA Name]
    DEPARTMENT: [FOH/BOH]
    DATE SUBMITTED: [Current Date]

    INCIDENT DATE: [Incident Date]

    INCIDENT SUMMARY:
    [Incident Summary]

    Please see the attached PDF for complete details including team comments,
    corrective actions, and leadership's accountability plan.

    CONFIDENTIAL: [Yes/No]
    [If confidential: "This submission has been marked as confidential and
    is being sent to HR for review."]

    If you have questions, please reach out to the submitter or HR.

    Best regards,
    Corner Table Restaurants NTF System
```

**Attachments:** [PDF from Step 8]
**BCC:** (Optional - if HR should be BCC'd)

---

### Step 12: Set Status to Completed
**Action:** Update item - NTF Submissions list

**Configuration:**
```
Id: trigger.ID
Fields:
  - Status: "Closed"
```

---

## ERROR HANDLING

### Step 13: Error Handling (Apply to all critical steps)

**Actions to add error handling:**
- Step 7: PDF generation
- Step 8: OneDrive save
- Step 10 & 11: Email sending

**For each:** Add "Apply to each" or error path

**Error Email to HR:**
```
To: [hrEmails]
Subject: "ERROR - NTF Processing Failed"
Body:
    An error occurred processing the following NTF:

    Submitted By: [Submitter Name]
    Location: [Location]
    Error: [Flow error message]

    Manual intervention may be required.
```

---

## TROUBLESHOOTING GUIDE

### Issue: Recipients list is incomplete
- **Cause:** Employees missing email addresses or incorrect location code
- **Solution:** Verify Employees list has all staff with valid emails
- **Check:** Filter queries in Steps 6B.3-6B.5 are correct

### Issue: PDF not generating
- **Cause:** Template formatting issue or missing fields
- **Solution:** Check Word template has all placeholder fields
- **Alternative:** Use HTML to PDF method

### Issue: Emails not sending
- **Cause:** Invalid email addresses or distribution list issues
- **Solution:** Test email addresses in Employees list
- **Verify:** HR email addresses are correct

### Issue: Confidential NTFs going to wrong recipients
- **Cause:** Flow logic not properly checking Confidential flag
- **Solution:** Verify Condition in Step 6 is correctly evaluating
- **Test:** Submit test confidential NTF and check recipients

### Issue: Location code not matching
- **Cause:** Data inconsistency between lists
- **Solution:** Ensure Company Code in Employees matches Locations list
- **Sync:** All location references must be consistent

---

## TESTING CHECKLIST

- [ ] Flow triggers when item created in NTF Submissions
- [ ] Submitter details are correctly retrieved
- [ ] Supervisors are properly looked up
- [ ] Confidential toggle correctly routes to HR only
- [ ] Non-confidential NTFs include all required recipients
- [ ] FOH recipients correctly identified
- [ ] BOH recipients correctly identified
- [ ] Director of Ops included in both FOH and BOH
- [ ] Corp Chef included in both FOH and BOH
- [ ] Duplicate recipients are removed
- [ ] PDF generates with all form data
- [ ] PDF saves to OneDrive with correct naming
- [ ] List item updated with PDF path
- [ ] Status changes to "In Review" after processing
- [ ] Confirmation email sent to submitter
- [ ] NTF email sent to all recipients
- [ ] Emails include PDF attachment
- [ ] Error handling catches and notifies on failures
- [ ] Flow completes within reasonable time (< 2 minutes)

---

## CONFIGURATION VARIABLES

**Replace these with actual values:**

| Variable | Value | Notes |
|----------|-------|-------|
| hrEmails | TBD | Get HR email addresses |
| OneDrive Folder | NTF_Submissions | Folder path for PDF storage |
| PDF Template | TBD | If using Word template method |
| Email Template | (provided above) | Customize as needed |

---

## MONITORING & MAINTENANCE

### Review Regularly:
1. **Flow execution history** - Check for failures
2. **Recipient accuracy** - Verify correct people receiving NTFs
3. **Email delivery** - Confirm no NDRs (Non-Delivery Reports)
4. **PDF quality** - Ensure formatting looks professional
5. **OneDrive storage** - Monitor file growth

### Monthly Tasks:
- Archive old NTFs (30+ days) to archive folder
- Update employee email addresses if changed
- Review and update HR email addresses
- Check flow performance metrics

---

**Last Updated:** December 22, 2025
**Version:** 1.0
**Status:** Ready for Implementation
