# Power Apps - NTF Canvas App Technical Specifications

**App Name:** NTF Submission Portal
**Version:** 1.0
**Created:** December 22, 2025

---

## GLOBAL SETTINGS

### Color Variables
Create these as app-level variables in the Power Apps studio:

```
ColorInk: RGBA(0, 30, 90, 1)           // #001E5A (Navy Blue)
ColorLinen: RGBA(240, 230, 180, 1)     // #F0E6B4 (Cream)
ColorText: RGBA(0, 30, 90, 1)           // Dark text
ColorWhite: RGBA(255, 255, 255, 1)     // White
ColorLight: RGBA(245, 245, 245, 1)     // Light gray for backgrounds
```

### App-Level Variables
Initialize these in the `OnStart` property:

```powerapps
Set(varCurrentScreen, "Home");
Set(varFormData, {
    Location: Blank(),
    LocationCompanyCode: Blank(),
    Submitter: Blank(),
    SubmitterDept: Blank(),
    SubmitterJob: Blank(),
    Supervisor1: Blank(),
    Supervisor2: Blank(),
    IncidentDate: Today(),
    IncidentSummary: "",
    TeamComments: "",
    CorrectiveAction: "",
    PolicyAcknowledgment: false,
    LeadershipPlan: "",
    AdditionalConversations: "",
    Participants: "",
    Confidential: false,
    Recipients: ""
});
Set(varRecipientsList, {});
```

---

## SCREEN SPECIFICATIONS

### Screen 1: Home (Welcome)
**Navigation:** Entry point

**Layout:**
- Top: Full-width header bar (ColorInk)
  - Logo/Company name: "CTR" or company logo
  - Text: "Corner Table Restaurants"

- Body (ColorLinen background):
  - Title: "Note to File Submission" (FontSize: 48, FontWeight: Bold, ColorInk)
  - Subtitle: "Submit a Note to File quickly and securely" (FontSize: 18, ColorInk)
  - Icon: Document/Form icon

- Button: "Start New NTF"
  - Style: ColorInk background, white text
  - OnSelect: `Navigate(Screen2_LocationSelect, ScreenTransition.Fade)`

---

### Screen 2: Location Selection
**Navigation:** After Start button

**Layout:**
- Header: "Step 1 of 7 - Select Location" (ColorInk background)

- Body:
  - Label: "Which location are you submitting from?"
  - Dropdown: `ddLocation`
    - Items: `Locations` list
    - Value: DBA Name
    - DisplayFields: "DBA Name", "Acronym"
  - Selected Value: `varFormData.Location`
  - OnChange:
    ```powerapps
    UpdateContext({varFormData: Patch(varFormData, {
        Location: Self.Selected.Value,
        LocationCompanyCode: Self.Selected.'Company Code'
    })})
    ```

- Display Selected Location Info:
  - Company: `ddLocation.Selected.'Company Name'`
  - DBA: `ddLocation.Selected.'DBA Name'`
  - Acronym: `ddLocation.Selected.Acronym`

- Navigation Buttons:
  - Cancel: `Navigate(Screen1_Home, ScreenTransition.Fade)`
  - Next:
    ```powerapps
    If(IsBlank(varFormData.Location),
        Notify("Please select a location", NotificationType.Error),
        Navigate(Screen3_EmployeeSelect, ScreenTransition.Fade)
    )
    ```

---

### Screen 3: Employee Selection
**Navigation:** After location selected

**Layout:**
- Header: "Step 2 of 7 - Select Employee" (ColorInk background)

- Body:
  - Label: "Who is submitting this NTF?"
  - Dropdown: `ddEmployee`
    - Items: `Filter(Employees, 'Company Code' = varFormData.LocationCompanyCode)`
    - Value: 'Employee Name'
    - DisplayFields: "Employee Name", "Job Title"
  - OnChange:
    ```powerapps
    UpdateContext({varFormData: Patch(varFormData, {
        Submitter: Self.Selected.'Employee Name',
        SubmitterJob: Self.Selected.'Job Title',
        SubmitterDept: If(
            Or(
                Self.Selected.'Org Level' = "GM",
                Self.Selected.'Org Level' = "AGM",
                Self.Selected.'Org Level' = "FOH Manager",
                Self.Selected.'Org Level' = "Office Manager",
                Self.Selected.'Org Level' = "Director of Ops"
            ),
            "FOH",
            "BOH"
        )
    })});

    // Get supervisor info
    Set(varSupervisorRecord, First(Filter(Supervisors, 'Team Member' = Self.Selected.'Employee Name')));
    UpdateContext({varFormData: Patch(varFormData, {
        Supervisor1: varSupervisorRecord.'Supervisor 1'.Value,
        Supervisor2: If(IsBlank(varSupervisorRecord.'Supervisor 2'), "", varSupervisorRecord.'Supervisor 2'.Value)
    })})
    ```

- Display Selected Employee:
  - Employee Name: `varFormData.Submitter`
  - Job Title: `varFormData.SubmitterJob`
  - Department: `varFormData.SubmitterDept` (FOH/BOH badge)
  - Supervisor 1: `varFormData.Supervisor1`
  - Supervisor 2: `varFormData.Supervisor2` (if exists)

- Navigation Buttons:
  - Back: `Navigate(Screen2_LocationSelect, ScreenTransition.Fade)`
  - Next:
    ```powerapps
    If(IsBlank(varFormData.Submitter),
        Notify("Please select an employee", NotificationType.Error),
        Navigate(Screen4_IncidentDetails, ScreenTransition.Fade)
    )
    ```

---

### Screen 4: Incident Details
**Navigation:** After employee selected

**Layout:**
- Header: "Step 3 of 7 - Incident Details" (ColorInk background)

- Body:
  - Label: "Date of Incident"
  - DatePicker: `dpIncidentDate`
    - Default: `Today()`
    - OnSelect: `UpdateContext({varFormData: Patch(varFormData, {IncidentDate: Self.SelectedDate})})`

  - Label: "Incident Summary" (Required *)
  - Hint Text: "Include witnesses, location, time, and dates"
  - Text Box: `txtIncidentSummary`
    - Multiline: true
    - Default: `varFormData.IncidentSummary`
    - OnChange: `UpdateContext({varFormData: Patch(varFormData, {IncidentSummary: Self.Value})})`

- Navigation Buttons:
  - Back: `Navigate(Screen3_EmployeeSelect, ScreenTransition.Fade)`
  - Next:
    ```powerapps
    If(IsBlank(varFormData.IncidentSummary),
        Notify("Incident Summary is required", NotificationType.Error),
        Navigate(Screen5_TeamComments, ScreenTransition.Fade)
    )
    ```

---

### Screen 5: Team Comments & Corrective Action
**Navigation:** After incident details

**Layout:**
- Header: "Step 4 of 7 - Comments & Corrective Action" (ColorInk background)

- Body:
  - Label: "Team Member Comments" (Optional)
  - Text Box: `txtTeamComments`
    - Multiline: true
    - Default: `varFormData.TeamComments`
    - OnChange: `UpdateContext({varFormData: Patch(varFormData, {TeamComments: Self.Value})})`

  - Label: "Corrective Action / Plan Moving Forward" (Required *)
  - Text Box: `txtCorrectiveAction`
    - Multiline: true
    - Default: `varFormData.CorrectiveAction`
    - OnChange: `UpdateContext({varFormData: Patch(varFormData, {CorrectiveAction: Self.Value})})`

  - Checkbox: `chkPolicyAcknowledgment`
    - Label: "I acknowledge understanding of the policy and suggested improvements"
    - Value: `varFormData.PolicyAcknowledgment`
    - OnChange: `UpdateContext({varFormData: Patch(varFormData, {PolicyAcknowledgment: Self.Value})})`

- Navigation Buttons:
  - Back: `Navigate(Screen4_IncidentDetails, ScreenTransition.Fade)`
  - Next:
    ```powerapps
    If(Or(IsBlank(varFormData.CorrectiveAction), Not(varFormData.PolicyAcknowledgment)),
        Notify("Please fill all required fields and acknowledge", NotificationType.Error),
        Navigate(Screen6_LeadershipInfo, ScreenTransition.Fade)
    )
    ```

---

### Screen 6: Leadership & Additional Info
**Navigation:** After team comments

**Layout:**
- Header: "Step 5 of 7 - Leadership Information" (ColorInk background)

- Body:
  - Label: "Leadership's Plan to Ensure Accountability" (Required *)
  - Text Box: `txtLeadershipPlan`
    - Multiline: true
    - Default: `varFormData.LeadershipPlan`
    - OnChange: `UpdateContext({varFormData: Patch(varFormData, {LeadershipPlan: Self.Value})})`

  - Label: "Additional Conversations" (Optional)
  - Hint: "If additional conversations will be had, please note when they will occur"
  - Text Box: `txtAdditionalConversations`
    - Multiline: true
    - Default: `varFormData.AdditionalConversations`
    - OnChange: `UpdateContext({varFormData: Patch(varFormData, {AdditionalConversations: Self.Value})})`

  - Label: "Who Participated in this Conversation?" (Required *)
  - Text Box: `txtParticipants`
    - Multiline: true
    - Placeholder: "List names of all participants"
    - Default: `varFormData.Participants`
    - OnChange: `UpdateContext({varFormData: Patch(varFormData, {Participants: Self.Value})})`

- Navigation Buttons:
  - Back: `Navigate(Screen5_TeamComments, ScreenTransition.Fade)`
  - Next:
    ```powerapps
    If(Or(IsBlank(varFormData.LeadershipPlan), IsBlank(varFormData.Participants)),
        Notify("Please fill all required fields", NotificationType.Error),
        Navigate(Screen7_ConfidentialityReview, ScreenTransition.Fade)
    )
    ```

---

### Screen 7: Confidentiality & Review
**Navigation:** After leadership info

**Layout:**
- Header: "Step 6 of 7 - Confidentiality & Review" (ColorInk background)

- Section 1: Confidentiality
  - Label: "Is this submission confidential?"
  - Toggle: `tglConfidential`
    - Value: `varFormData.Confidential`
    - OnChange: `UpdateContext({varFormData: Patch(varFormData, {Confidential: Self.Value})})`
  - Help Text: "If checked, only HR will receive this NTF. Otherwise, your supervisors and team will receive it."

- Section 2: Review Summary (Read-only display)
  - Location: `ddLocation.Selected.'DBA Name'`
  - Submitter: `varFormData.Submitter`
  - Department: `varFormData.SubmitterDept`
  - Supervisor 1: `varFormData.Supervisor1`
  - Supervisor 2: `varFormData.Supervisor2`
  - Date of Incident: `Text(varFormData.IncidentDate, "mm/dd/yyyy")`

- Section 3: Recipients Preview
  - Title: "This NTF will be sent to:"
  - Display recipient list (dynamically generated):
    ```powerapps
    If(varFormData.Confidential,
        "HR Department Only",
        Concatenate(
            "• " & varFormData.Supervisor1 & Char(10),
            If(IsBlank(varFormData.Supervisor2), "", "• " & varFormData.Supervisor2 & Char(10)),
            "• Director of Ops at " & varFormData.Location & Char(10),
            "• All " & varFormData.SubmitterDept & " team members" & Char(10),
            "• Corp Chef" & Char(10),
            "• HR Department"
        )
    )
    ```

- Navigation Buttons:
  - Back: `Navigate(Screen6_LeadershipInfo, ScreenTransition.Fade)`
  - Submit: Goes to Screen8_Confirmation (see next section)

---

### Screen 8: Confirmation
**Navigation:** After submission

**Layout:**
- Header: "Submission Received" (ColorInk background with success indicator)

- Body:
  - Success Icon: ✓ checkmark in green circle
  - Title: "Your NTF has been submitted successfully"
  - Message: "A PDF document has been generated and sent to all recipients. You will receive a confirmation email shortly."

  - Summary Box:
    - Submitted to: `varFormData.Location`
    - Submitted by: `varFormData.Submitter`
    - Submitted on: `Text(Now(), "mm/dd/yyyy hh:mm AM/PM")`
    - Recipients: Display count and list

  - Important Note (if Confidential):
    ```
    "⚠️ This was marked as confidential and has been sent to HR only."
    ```

- Navigation Buttons:
  - "Submit Another NTF":
    ```powerapps
    Set(varFormData, {
        Location: Blank(),
        LocationCompanyCode: Blank(),
        Submitter: Blank(),
        SubmitterDept: Blank(),
        SubmitterJob: Blank(),
        Supervisor1: Blank(),
        Supervisor2: Blank(),
        IncidentDate: Today(),
        IncidentSummary: "",
        TeamComments: "",
        CorrectiveAction: "",
        PolicyAcknowledgment: false,
        LeadershipPlan: "",
        AdditionalConversations: "",
        Participants: "",
        Confidential: false,
        Recipients: ""
    });
    Navigate(Screen2_LocationSelect, ScreenTransition.Fade)
    ```
  - "Return to Home":
    ```powerapps
    Set(varFormData, {});
    Navigate(Screen1_Home, ScreenTransition.Fade)
    ```

---

## FORM SUBMISSION LOGIC

**OnSelect for Submit button (Screen 7):**

```powerapps
Collect(
    'NTF Submissions',
    {
        Title: Concatenate("NTF - ", varFormData.Location, " - ", Text(Now(), "mm/dd/yyyy")),
        Submitter: {Value: varFormData.Submitter},
        Location: {Value: varFormData.Location},
        'Submitter Department': varFormData.SubmitterDept,
        'Supervisor 1': {Value: varFormData.Supervisor1},
        'Supervisor 2': {Value: varFormData.Supervisor2},
        'Date of Incident': varFormData.IncidentDate,
        'Incident Summary': varFormData.IncidentSummary,
        'Team Comments': varFormData.TeamComments,
        'Corrective Action': varFormData.CorrectiveAction,
        'Policy Acknowledgment': varFormData.PolicyAcknowledgment,
        'Leadership Plan': varFormData.LeadershipPlan,
        'Additional Conversations': varFormData.AdditionalConversations,
        Participants: varFormData.Participants,
        Confidential: varFormData.Confidential,
        Status: "Submitted"
    }
);
Notify("NTF submitted successfully. PDF is being generated...", NotificationType.Success);
Navigate(Screen8_Confirmation, ScreenTransition.Fade)
```

---

## STYLING GUIDELINES

### All Buttons
- Background: `ColorInk` (#001E5A)
- Text Color: `ColorWhite`
- Font Weight: Bold
- Padding: 12px 24px
- Border Radius: 4px
- Hover: Slightly darker shade

### All Headers/Titles
- Color: `ColorInk`
- Font: Segoe UI, Bold
- Size: 24-48px depending on hierarchy

- Background: `ColorInk` (#001E5A)
- Text on headers: `ColorWhite`

### Form Inputs
- Border: 1px `ColorInk`
- Border Radius: 4px
- Padding: 8px 12px
- Focus Border: 2px `ColorInk`

### Backgrounds
- Screens: `ColorLinen` (#F0E6B4)
- Cards/Sections: `ColorWhite`
- Header Bars: `ColorInk`

### Text
- Primary Text: `ColorInk` on light backgrounds
- Light Text: `ColorWhite` on dark backgrounds
- Helper/Hint: #666666 (gray)

---

## ACCESSIBILITY REQUIREMENTS

- All interactive elements have clear labels
- Color is not the only means of conveying information (use icons + text)
- Sufficient contrast ratios (Navy on Cream: 13.5:1 ✓)
- Tab order is logical and intuitive
- Form validation messages are clear
- Required fields marked with * and help text

---

## PERFORMANCE OPTIMIZATION

- Use `UpdateContext` for local variables instead of `Set` for better performance
- Cache supervisor and location data on app startup
- Minimize lookups in loops
- Use `Refresh` only when necessary

---

## TESTING CHECKLIST

- [ ] All navigation works smoothly
- [ ] Form validation prevents incomplete submissions
- [ ] Data persists across screen transitions
- [ ] Dropdown filters work correctly
- [ ] Supervisors auto-populate correctly
- [ ] Recipients list displays accurately
- [ ] Department classification (FOH/BOH) is correct
- [ ] Confidential toggle changes recipient list preview
- [ ] Submit button creates list item successfully
- [ ] Confirmation page displays all details
- [ ] UI matches color scheme and branding

---

**Last Updated:** December 22, 2025
**Version:** 1.0
