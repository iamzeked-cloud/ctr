# NTF Power App - Implementation Guide

**Project:** Note to File Submission System
**Organization:** Corner Table Restaurants
**Timeline:** Phased Implementation
**Version:** 1.0
**Created:** December 22, 2025

---

## TABLE OF CONTENTS

1. [Pre-Implementation Checklist](#pre-implementation-checklist)
2. [Phase 1: Data Preparation](#phase-1-data-preparation)
3. [Phase 2: SharePoint List Setup](#phase-2-sharepoint-list-setup)
4. [Phase 3: Power Apps Canvas App](#phase-3-power-apps-canvas-app)
5. [Phase 4: Power Automate Flow](#phase-4-power-automate-flow)
6. [Phase 5: Testing](#phase-5-testing)
7. [Phase 6: Deployment & User Training](#phase-6-deployment--user-training)
8. [Post-Implementation](#post-implementation)

---

## PRE-IMPLEMENTATION CHECKLIST

Before starting, ensure you have:

### Access & Permissions
- [ ] Admin access to SharePoint site
- [ ] Admin access to Power Apps environment
- [ ] Power Automate premium licenses or cloud flows enabled
- [ ] OneDrive access for PDF storage
- [ ] Access to Outlook/Microsoft Teams for communications

### Data Requirements
- [ ] Complete Employees list with emails
- [ ] All 9 locations mapped in Locations list
- [ ] Complete Supervisors list with manager relationships
- [ ] HR email addresses identified
- [ ] Director of Ops for each location identified
- [ ] Corp Chef for each location identified

### Design Assets
- [ ] CTR company logo (optional but recommended)
- [ ] Color palette confirmed (#001E5A Navy, #F0E6B4 Cream)
- [ ] Company branding guidelines

### Communication
- [ ] Identify 2-3 power users for initial testing
- [ ] Schedule user training date
- [ ] Prepare user documentation
- [ ] Notify management of rollout plan

---

## PHASE 1: DATA PREPARATION

### Task 1.1: Audit Existing Employees List
**Responsibility:** IT/Administrator
**Time:** 2-4 hours
**Steps:**

1. Open your existing Employees list in SharePoint
2. Verify it contains:
   - Employee Name (exact spelling)
   - ID# (employee ID)
   - Job Title (must match management roster)
   - Company Code (location identifier)
   - Email address (required for routing)
   - Org Level (FOH/BOH classification)

3. **Clean up data:**
   - Remove duplicates
   - Standardize job titles (see list below)
   - Ensure all emails are valid
   - Add missing employees from management roster

4. **Job Title Standardization:**

   **FOH Roles:**
   - GM (General Manager)
   - AGM (Assistant General Manager)
   - FOH Manager (Front of House Manager)
   - Office Manager
   - Director of Ops (Director of Operations)

   **BOH Roles:**
   - CDC (Chef de Cuisine)
   - EX Sous (Executive Sous Chef)
   - Sous (Sous Chef)
   - Corp Chef (Corporate Chef)
   - Pastry (Pastry Chef)
   - Receiver (Receiving)

5. **Verify email format:** firstname.lastname@cornertablerestaurants.com or your domain

---

### Task 1.2: Create Locations List Data
**Responsibility:** IT/Administrator
**Time:** 30 minutes
**Steps:**

1. Create a spreadsheet with location data:

```
Company Code | Company Name | DBA Name | Acronym
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

2. Save as CSV file for bulk import

---

### Task 1.3: Create Supervisors Mapping
**Responsibility:** HR/Management
**Time:** 2-3 hours
**Steps:**

1. Create a spreadsheet with supervisor relationships:

```
Team Member | Employee Name | Supervisor 1 | Supervisor 2
[Employee] | [Full Name] | [Manager 1] | [Manager 2]
```

2. For each employee in the management roster, identify:
   - Their supervisor(s) from the org chart
   - If they have multiple supervisors (e.g., shared reporting)

3. Ensure all supervisors exist in the Employees list

4. Save as CSV for bulk import

---

### Task 1.4: Identify HR Recipients
**Responsibility:** HR
**Time:** 30 minutes
**Steps:**

1. Identify HR team members who should receive NTFs:
   - HR Director/Manager
   - HR Generalists
   - Compliance Officer (if applicable)

2. Collect their email addresses

3. Store in a variable for the Power Automate flow:
   ```
   hr@cornertablerestaurants.com;hrmanager@cornertablerestaurants.com
   ```

---

## PHASE 2: SHAREPOINT LIST SETUP

### Task 2.1: Create Locations List
**Responsibility:** IT/Administrator
**Time:** 30 minutes
**Steps:**

1. **Create new list in SharePoint:**
   - Go to your SharePoint site
   - Click "+ New" → List
   - Name: `Locations`
   - Choose "Blank list"

2. **Add columns:**
   - Title: (auto-created, rename to "Company Code")
   - Add column: "Company Name" (Single line text)
   - Add column: "DBA Name" (Single line text)
   - Add column: "Acronym" (Single line text)

3. **Import data:**
   - Click on list → "Data" → "Get data" → "Upload from CSV"
   - Or manually enter 9 locations from Task 1.2

4. **Verify:**
   - All 9 locations present
   - No duplicate entries
   - All fields populated

---

### Task 2.2: Update Employees List
**Responsibility:** IT/Administrator
**Time:** 1-2 hours
**Steps:**

1. **Add missing columns to Employees list:**
   - Verify: Employee Name, ID#, Job Title, Company Code, Email, Org Level

2. **Add new columns if missing:**
   - Add: "Company Code" (Single line text)
   - Add: "Email" (Email) if not present
   - Add: "Org Level" (Choice - FOH or BOH)

3. **Bulk update Org Level:**
   - For each employee, set Org Level to FOH or BOH based on job title
   - Use job title standardization from Task 1.1

4. **Verify all employees have:**
   - Valid email address
   - Correct company code
   - Correct org level
   - Correct job title (standardized)

---

### Task 2.3: Create Supervisors List
**Responsibility:** IT/Administrator
**Time:** 1 hour
**Steps:**

1. **Create new list:**
   - Go to SharePoint site
   - Click "+ New" → List
   - Name: `Supervisors`
   - Choose "Blank list"

2. **Add columns:**
   - Title: (rename to "Team Member") - Single line text
   - Add: "Employee Name" - Single line text
   - Add: "Supervisor 1" - Lookup (to Employees list, "Employee Name")
   - Add: "Supervisor 2" - Lookup (to Employees list, "Employee Name") - Allow multiple selections: NO
   - Add: "Location" - Lookup (to Locations list, "DBA Name")

3. **Import data:**
   - Bulk import from CSV created in Task 1.3
   - Or manually enter for each employee

4. **Verify:**
   - All employees have at least one supervisor
   - Supervisors reference existing employees
   - No circular references (employee can't be own supervisor)

---

### Task 2.4: Create NTF Submissions List
**Responsibility:** IT/Administrator
**Time:** 1.5 hours
**Steps:**

1. **Create new list:**
   - Name: `NTF Submissions`
   - Choose "Blank list"

2. **Add all required columns:**

   **Basic Info:**
   - Title (auto-created, leave as default)
   - Created (auto)
   - Created By (auto)
   - Modified (auto)
   - Modified By (auto)

   **Submission Info:**
   - Add: "Submitter" - Lookup (to Employees list, "Employee Name")
   - Add: "Location" - Lookup (to Locations list, "DBA Name")
   - Add: "Submitter Department" - Single line text (will be auto-set to FOH or BOH)
   - Add: "Supervisor 1" - Lookup (to Employees list, "Employee Name")
   - Add: "Supervisor 2" - Lookup (to Employees list, "Employee Name")

   **Incident Details:**
   - Add: "Date of Incident" - Date
   - Add: "Incident Summary" - Multiple lines text (Rich text)
   - Add: "Team Comments" - Multiple lines text
   - Add: "Corrective Action" - Multiple lines text
   - Add: "Policy Acknowledgment" - Yes/No

   **Leadership Info:**
   - Add: "Leadership Plan" - Multiple lines text
   - Add: "Additional Conversations" - Multiple lines text
   - Add: "Participants" - Multiple lines text

   **Processing:**
   - Add: "Confidential" - Yes/No (default: No)
   - Add: "Recipients List" - Multiple lines text
   - Add: "Status" - Choice: Draft, Submitted, In Review, Closed (default: Submitted)
   - Add: "PDF File Path" - Hyperlink or URL

3. **Set required fields:**
   - Submitter (required)
   - Location (required)
   - Date of Incident (required)
   - Incident Summary (required)
   - Corrective Action (required)
   - Policy Acknowledgment (required)
   - Leadership Plan (required)
   - Participants (required)

4. **Configure list settings:**
   - Versioning: Not needed (no version tracking required)
   - Audience targeting: Off
   - Draft item security: All users can create

---

## PHASE 3: POWER APPS CANVAS APP

### Task 3.1: Create Canvas App
**Responsibility:** Power Apps Developer
**Time:** 2 hours
**Steps:**

1. **Create new Canvas App:**
   - Go to Power Apps (powerapps.microsoft.com)
   - Click "+ Create"
   - Select "Canvas app from blank"
   - Name: `NTF Submission Portal`
   - Format: Tablet (horizontal) - better for form layout
   - Create

2. **Connect to data sources:**
   - Click "Data" in left panel
   - Add: Locations list
   - Add: Employees list
   - Add: Supervisors list
   - Add: NTF Submissions list

3. **Configure formula for department lookup:**
   - This will be used in Power Apps to determine FOH vs BOH

---

### Task 3.2: Build App Screens
**Responsibility:** Power Apps Developer
**Time:** 6-8 hours
**Steps:**

Refer to `POWER_APPS_TECHNICAL_SPECS.md` for detailed screen specifications.

**Screens to create:**
1. Screen 1: Welcome/Home
2. Screen 2: Location Selection
3. Screen 3: Employee Selection
4. Screen 4: Incident Details
5. Screen 5: Team Comments & Corrective Action
6. Screen 6: Leadership & Additional Info
7. Screen 7: Confidentiality & Review
8. Screen 8: Confirmation

**For each screen:**
- Follow color scheme (Navy #001E5A, Cream #F0E6B4)
- Add navigation buttons with proper formulas
- Add form validation
- Test data flow between screens
- Implement auto-population of supervisors and department

---

### Task 3.3: Configure Form Submission
**Responsibility:** Power Apps Developer
**Time:** 1-2 hours
**Steps:**

1. **Add Submit button logic:**
   - Collects form data to NTF Submissions list
   - Includes all form fields
   - Sets Status to "Submitted"
   - Clears form after submission
   - Navigates to confirmation screen

2. **Test submission:**
   - Submit test form
   - Verify item appears in NTF Submissions list
   - Verify all fields populated correctly

---

### Task 3.4: Test Power Apps
**Responsibility:** Power Apps Developer + Power Users
**Time:** 2-3 hours
**Steps:**

1. **Functional testing:**
   - [ ] All screens load without errors
   - [ ] Location dropdown filters correctly
   - [ ] Employee dropdown filters by location
   - [ ] Supervisors auto-populate correctly
   - [ ] Department (FOH/BOH) auto-sets correctly
   - [ ] All form fields accept input
   - [ ] Date picker works
   - [ ] Form validation prevents incomplete submission
   - [ ] Submit button creates list item
   - [ ] Confirmation screen displays

2. **Data accuracy:**
   - [ ] Supervisors match management roster
   - [ ] Department classification is correct
   - [ ] Location mapping is accurate

3. **User experience:**
   - [ ] Buttons are clearly clickable
   - [ ] Colors match branding
   - [ ] Text is readable
   - [ ] Navigation is intuitive
   - [ ] Error messages are helpful

4. **Document issues:**
   - Create list of bugs/improvements
   - Prioritize and fix before moving to Phase 4

---

## PHASE 4: POWER AUTOMATE FLOW

### Task 4.1: Create Main Flow
**Responsibility:** Power Automate Developer
**Time:** 3-4 hours
**Steps:**

1. **Create cloud flow:**
   - Go to Power Automate (make.powerautomate.com)
   - Click "+ Create"
   - Select "Cloud flow" → "Automated cloud flow"
   - Name: `NTF_SubmissionProcessor`
   - Trigger: "When an item is created" (SharePoint - NTF Submissions)

2. **Build flow steps:**
   Refer to `POWER_AUTOMATE_FLOW_SPECS.md` for detailed steps:
   - Get submitter details
   - Get supervisor details
   - Build recipient list (with confidentiality logic)
   - Generate PDF
   - Save PDF to OneDrive
   - Update list item
   - Send confirmation email
   - Send NTF email to recipients

3. **Configure variables:**
   - HR emails: [from Task 1.4]
   - Location code mapping: [from Locations list]
   - Email templates: [customize as needed]

4. **Test flow:**
   - Submit test NTF from Power App
   - Verify flow triggers
   - Check list item is updated
   - Verify emails are sent
   - Verify PDF is created

---

### Task 4.2: Create PDF Template (if using Word)
**Responsibility:** Power Automate Developer
**Time:** 1-2 hours
**Steps:**

1. **Create Word document template:**
   - Go to OneDrive
   - Create new Word document: `NTF_Template.docx`

2. **Add CTR branding:**
   - Header with company colors (Navy/Cream)
   - Logo (if available)
   - Title: "Note to File"

3. **Add placeholder fields:**
   Use Word placeholders for:
   - Location name
   - Submission date
   - Submitter name/department
   - Supervisors
   - Incident date
   - Incident summary
   - Team comments
   - Corrective action
   - Policy acknowledgment
   - Leadership plan
   - Additional conversations
   - Participants
   - Confidential flag
   - Recipients list

4. **Save template location:**
   - Document: `/NTF Templates/NTF_Template.docx`
   - Reference this path in Power Automate

---

### Task 4.3: Configure PDF Storage
**Responsibility:** IT/Administrator
**Time:** 30 minutes
**Steps:**

1. **Create OneDrive folder structure:**
   - OneDrive → Documents
   - Create folder: `NTF_Submissions`
   - Create subfolder: `2025` (year-based organization)
   - Create subfolders by month: `January`, `February`, etc. (or keep flat)

2. **Set folder permissions:**
   - Owner: System account/Power Automate
   - Read access: HR
   - Read access: Relevant managers (optional)

3. **Document folder path:**
   - Path for Power Automate: `/Documents/NTF_Submissions/`
   - PDF naming: `NTF_[Location]_[Submitter]_[YYYY-MM-DD].pdf`

---

### Task 4.4: Configure Email Templates
**Responsibility:** HR/Communications
**Time:** 1 hour
**Steps:**

1. **Confirmation email to submitter:**
   - From: donotreply@cornertablerestaurants.com
   - Subject: "Your NTF has been submitted"
   - Template: See POWER_AUTOMATE_FLOW_SPECS.md

2. **NTF email to recipients:**
   - From: donotreply@cornertablerestaurants.com
   - Subject: "Note to File Submission - [Location] - [Date]"
   - Template: See POWER_AUTOMATE_FLOW_SPECS.md

3. **Error notification to HR:**
   - From: donotreply@cornertablerestaurants.com
   - Subject: "ERROR - NTF Processing Failed"
   - Include: Submitter name, location, error details

---

## PHASE 5: TESTING

### Task 5.1: Unit Testing
**Responsibility:** Developers
**Time:** 1-2 hours
**Steps:**

1. **Power Apps Testing:**
   - Test each screen independently
   - Verify all dropdowns and filters
   - Test form validation
   - Verify submission creates list item

2. **Power Automate Testing:**
   - Trigger flow manually
   - Check each step completes successfully
   - Verify output variables contain correct data
   - No errors in flow execution

3. **Integration Testing:**
   - End-to-end: Power App submission → Flow execution → Email receipt

---

### Task 5.2: User Acceptance Testing (UAT)
**Responsibility:** Power Users + HR
**Time:** 2-3 hours
**Steps:**

1. **Select 2-3 power users** from different departments (FOH and BOH)

2. **Test scenarios:**

   **Test Case 1: FOH Employee Submits Non-Confidential NTF**
   - Power user selects FOH location
   - Selects themselves as employee
   - Fills all form fields
   - Toggles confidential: OFF
   - Submits
   - **Verify:**
     - Supervisors 1 & 2 receive email
     - All FOH staff at location receive email
     - Director of Ops receives email
     - Corp Chef receives email
     - HR receives email
     - PDF is properly formatted
     - List item created with correct status

   **Test Case 2: BOH Employee Submits Confidential NTF**
   - Power user selects BOH location
   - Selects themselves as employee
   - Fills all form fields
   - Toggles confidential: ON
   - Submits
   - **Verify:**
     - ONLY HR receives email
     - Supervisors do NOT receive email
     - Other staff do NOT receive email
     - List item marked as confidential
     - PDF generated correctly

   **Test Case 3: Non-Confidential with Multiple Supervisors**
   - Use employee with 2 supervisors
   - Submits non-confidential NTF
   - **Verify:**
     - Both supervisors receive email
     - Recipient list shows both supervisor names

   **Test Case 4: Form Validation**
   - Try to submit without required fields
   - **Verify:**
     - Submit button disabled or shows error
     - Error message is clear

3. **Document feedback:**
   - UI/UX improvements
   - Functional issues
   - Email template improvements
   - PDF formatting issues

4. **Fix issues:**
   - High priority (blocking): Fix immediately
   - Medium priority: Schedule for next iteration
   - Low priority (cosmetic): Consider for future

---

### Task 5.3: Data Accuracy Validation
**Responsibility:** HR/Management
**Time:** 1-2 hours
**Steps:**

1. **Verify recipient routing:**
   - Check several real NTFs
   - Confirm correct people received them
   - Verify no incorrect recipients

2. **Verify department classification:**
   - FOH roles: GM, AGM, FOH Manager, Office Manager, Director of Ops
   - BOH roles: CDC, EX Sous, Sous, Corp Chef, Pastry, Receiver
   - Spot-check several employees

3. **Verify supervisor relationships:**
   - Check Supervisors list matches org chart
   - Confirm no circular references

4. **Verify location mapping:**
   - Confirm all 9 locations appear in dropdown
   - Verify employees filter correctly by location

---

## PHASE 6: DEPLOYMENT & USER TRAINING

### Task 6.1: Prepare Deployment
**Responsibility:** IT/Administrator
**Time:** 1-2 hours
**Steps:**

1. **Copy Power Apps to production** (if separate environments)
   - Or mark current app as "Published"
   - Set permissions: Specific users or entire organization

2. **Verify all lists and flows are live**
   - Test in production environment
   - Confirm Power Automate flow is active

3. **Create user documentation:**
   - Step-by-step guide for submitting NTF
   - Screenshots of each screen
   - Common troubleshooting
   - Who to contact for help

4. **Schedule user training:**
   - Live demo for all users
   - Q&A session
   - Recorded demo for reference

---

### Task 6.2: Share Power App with Users
**Responsibility:** IT/Administrator
**Time:** 30 minutes - 1 hour
**Steps:**

1. **In Power Apps Studio:**
   - Click "Share" button
   - Add users: All 30 NTF submitters
   - Or add specific security group
   - Permission: Can Use (read-only execution)

2. **Create app link:**
   - Get link: [Power App URL]
   - Share in email or Teams
   - Add to company intranet/portal if available

3. **Confirm access:**
   - Have users test they can open app
   - Confirm they see form screens

---

### Task 6.3: User Training
**Responsibility:** HR/Communications
**Time:** 1-2 hours (live session)
**Steps:**

1. **Conduct training session:**
   - Live demonstration of form
   - Walk through each screen
   - Explain what happens after submission
   - Show confirmation email

2. **Q&A:**
   - Answer user questions
   - Address concerns about confidentiality
   - Clarify how recipients are determined

3. **Provide resources:**
   - User guide document
   - Recorded demo video
   - Contact info for support

4. **Soft launch (optional):**
   - Deploy to 10% of users first
   - Monitor for issues
   - Scale to 100% after 1 week

---

### Task 6.4: Go-Live
**Responsibility:** IT/Administrator
**Time:** 1 hour
**Steps:**

1. **Verify all systems:**
   - [ ] SharePoint lists active and populated
   - [ ] Power Apps published and shared
   - [ ] Power Automate flow active
   - [ ] OneDrive folder created
   - [ ] HR email addresses configured

2. **Send announcement:**
   - Email to all employees
   - Explain new NTF submission process
   - Provide Power App link
   - Link to user guide

3. **Monitor initial submissions:**
   - Watch for flow errors
   - Check email delivery
   - Verify PDF generation
   - Monitor list item creation

4. **Be ready to support:**
   - Have support person available first day
   - Monitor for user issues
   - Address problems quickly

---

## PHASE 7: POST-IMPLEMENTATION

### Task 7.1: Monitor & Support
**Responsibility:** IT/HR
**Time:** Ongoing
**Steps:**

1. **Week 1:**
   - Monitor every submission
   - Check flow execution history for errors
   - Verify emails are being sent
   - Collect user feedback

2. **Week 2-4:**
   - Continue monitoring
   - Address any recurring issues
   - Refine email templates if needed
   - Document any needed changes

3. **Ongoing:**
   - Monthly review of submissions
   - Check for failed flows
   - Monitor OneDrive storage
   - Update employee/supervisor data as needed
   - Refresh Supervisors list quarterly with org changes

---

### Task 7.2: Optimization
**Responsibility:** Power Apps/Automate Developer
**Time:** As needed
**Steps:**

1. **Performance improvements:**
   - Monitor flow execution time (target: < 2 minutes)
   - Optimize lookups if slow
   - Cache frequently accessed data

2. **User experience improvements:**
   - Implement suggested changes from feedback
   - Refine error messages
   - Add additional features if requested

3. **Reporting:**
   - Create dashboard showing NTF metrics:
     - Submissions by location
     - Submissions by department
     - Average resolution time
     - Submission trends

---

### Task 7.3: Maintenance Schedule
**Responsibility:** IT/Administrator
**Time:** Monthly
**Steps:**

**Monthly:**
- Review Power Automate flow execution logs
- Check for failed flows or email delivery issues
- Archive old PDFs to OneDrive archive folder
- Verify all employees still have correct email addresses

**Quarterly:**
- Update Supervisors list with org changes
- Update Employees list with new hires/terminations
- Review and update HR email addresses if changed
- Test flow with sample submissions

**Annually:**
- Review system usage and performance
- Plan any improvements or enhancements
- Update user documentation
- Conduct refresher training if needed

---

## ROLLBACK PLAN (If needed)

If critical issues are discovered:

1. **Disable Power Apps sharing** - Users can't access app
2. **Deactivate Power Automate flow** - No automated processing
3. **Revert to manual NTF process** - Existing process
4. **Address issues** in development environment
5. **Redeploy** after verification

---

## SUCCESS METRICS

Track these to measure implementation success:

1. **User adoption:**
   - % of users who have submitted at least one NTF
   - Target: 80%+ by end of Month 1

2. **System reliability:**
   - % of submissions processed successfully
   - Target: 99%+

3. **Email delivery:**
   - % of emails delivered to recipients
   - Target: 99%+

4. **Time to process:**
   - Average time from submission to PDF delivery
   - Target: < 2 minutes

5. **User satisfaction:**
   - Survey users on ease of use
   - Target: 4/5 stars or higher

---

## SUPPORT CONTACTS

**For issues, contact:**

| Issue | Contact | Email |
|-------|---------|-------|
| Power Apps/Automate errors | IT Developer | [email] |
| SharePoint list issues | IT Administrator | [email] |
| Recipient routing questions | HR Manager | [email] |
| User access issues | IT Help Desk | [email] |
| PDF formatting issues | IT Developer | [email] |

---

## APPENDIX: Quick Links

- **Power Apps Studio:** https://make.powerapps.com
- **Power Automate:** https://make.powerautomate.com
- **SharePoint Site:** [Your SharePoint URL]
- **OneDrive NTF Folder:** [Your OneDrive URL]/NTF_Submissions
- **Power Apps Documentation:** https://docs.microsoft.com/en-us/powerapps/

---

**Last Updated:** December 22, 2025
**Version:** 1.0
**Status:** Ready for Implementation

**Next Step:** Begin Phase 1 - Data Preparation
