# NTF Power App Project - Corner Table Restaurants

**Project Status:** Documentation Complete - Ready for Implementation
**Created:** December 22, 2025
**Organization:** Corner Table Restaurants (CTR)

---

## PROJECT OVERVIEW

This project provides a complete solution for submitting **Notes to File (NTF)** using Microsoft Power Apps, SharePoint, and Power Automate. The solution provides a user-friendly, branded interface for CTR management to submit confidential and non-confidential NTFs with automatic routing to appropriate recipients.

### Key Features
- ✅ **Professional Form Interface** with CTR branding (Navy #001E5A, Cream #F0E6B4)
- ✅ **Smart Recipient Routing** based on location, department (FOH/BOH), and supervisors
- ✅ **Confidentiality Handling** - Confidential NTFs route only to HR
- ✅ **Automatic PDF Generation** with professional formatting
- ✅ **OneDrive Storage** for document archival
- ✅ **Email Notifications** to all recipients
- ✅ **Comprehensive Data Validation** and error handling

---

## DOCUMENTATION FILES

### 1. **NTF_POWER_APP_SETUP.md**
Complete reference for SharePoint list architecture and data structure.

**Includes:**
- SharePoint list schemas (Locations, Employees, Supervisors, NTF Submissions)
- Full management roster organized by location (9 locations)
- Complete recipient routing logic
- Power Apps canvas app screen specifications
- Power Automate flow overview

**Use when:** Setting up SharePoint lists and understanding data structure

---

### 2. **POWER_APPS_TECHNICAL_SPECS.md**
Detailed technical specifications for building the Power Apps canvas app.

**Includes:**
- Color scheme and branding configuration
- Global variables and formulas
- 8 complete screen designs with detailed layouts
- Form field specifications and validation
- Navigation logic and formulas
- Styling guidelines and accessibility requirements
- Performance optimization tips

**Use when:** Building the Power Apps canvas application

---

### 3. **POWER_AUTOMATE_FLOW_SPECS.md**
Step-by-step specifications for creating the Power Automate flow.

**Includes:**
- Flow diagram showing logic branches
- 12+ detailed flow steps with configurations
- Recipient determination logic (Confidential vs. Normal)
- PDF generation instructions (Word template or HTML)
- OneDrive storage configuration
- Email templates (Submitter confirmation, Recipient notification, Error alerts)
- Error handling strategies
- Troubleshooting guide

**Use when:** Building the Power Automate workflow

---

### 4. **IMPLEMENTATION_GUIDE.md**
Complete phased implementation plan with step-by-step instructions.

**Includes:**
- Pre-implementation checklist
- 7 implementation phases:
  1. Data Preparation
  2. SharePoint List Setup
  3. Power Apps Canvas App
  4. Power Automate Flow
  5. Testing (UAT)
  6. Deployment & Training
  7. Post-Implementation Support
- Testing scenarios and validation checklists
- User training guidelines
- Go-live preparation
- Monitoring and maintenance schedule
- Rollback plan
- Success metrics

**Use when:** Planning and executing the implementation

---

## QUICK START

### For Project Managers
1. Start with **IMPLEMENTATION_GUIDE.md** - Section "Pre-Implementation Checklist"
2. Plan resources and timeline using the 7 phases
3. Assign responsibilities from each task section

### For IT Administrators
1. Review **IMPLEMENTATION_GUIDE.md** - Phase 1 (Data Preparation)
2. Prepare data in Employees list
3. Follow Phase 2 (SharePoint List Setup) step-by-step

### For Power Apps Developer
1. Review **POWER_APPS_TECHNICAL_SPECS.md** - Sections 1-2 (Settings and Screens)
2. Create canvas app following detailed screen specifications
3. Test using Phase 5 testing scenarios from Implementation Guide

### For Power Automate Developer
1. Review **POWER_AUTOMATE_FLOW_SPECS.md** - Overview and Steps 1-12
2. Create flow trigger on NTF Submissions list
3. Implement steps following detailed configurations
4. Test with sample submissions

---

## LOCATIONS & MANAGEMENT

The solution supports **9 locations** with automatic recipient routing:

| Location | DBA Name | Acronym | Company Code |
|----------|----------|---------|--------------|
| The Smith East Village | TSEV | 101 |
| The Smith Midtown | TSMT | 102 |
| The Smith Lincoln Square | TSLS | 103 |
| The Smith Nomad | TSNM | 104 |
| The Smith Penn Quarter | TSPQ | 105 |
| The Smith U Street | TSUS | 106 |
| The Smith River North | TSRN | 107 |
| Parla | PUWS | 202 |
| Corner Table Restaurants | CTR | 901 |

Each location has:
- General Manager (GM)
- Assistant General Manager (AGM)
- FOH Managers (1-5 per location)
- Office Manager
- Director of Operations
- Corporate Chef
- BOH staff (CDC, Sous Chefs, Pastry, etc.)

---

## RECIPIENT ROUTING LOGIC

### Non-Confidential Submissions
Recipients always include:
- **Submitter's Supervisor 1 & 2**
- **Director of Ops** at their location
- **HR Department**

Plus, based on department:
- **If FOH:** All FOH team members + Corp Chef
- **If BOH:** All BOH team members + Corp Chef

### Confidential Submissions
- **HR Department ONLY**
- Supervisors and team members explicitly EXCLUDED

---

## NTF FORM SECTIONS

1. **Location Selection** - Choose from 9 locations
2. **Employee Selection** - Select submitter (auto-populates supervisors and department)
3. **Incident Details** - Date of incident and summary
4. **Team Comments & Corrective Action** - Comments and action plan
5. **Leadership Information** - Leadership's accountability plan and participants
6. **Confidentiality & Review** - Toggle confidential flag and review all data
7. **Confirmation** - Success message and recipient list

---

## COLOR SCHEME & BRANDING

**Primary Color (Ink):** #001E5A (Navy Blue)
**Secondary Color (Linen):** #F0E6B4 (Cream)
**Text:** Dark navy on cream, white on navy backgrounds

All colors specified in POWER_APPS_TECHNICAL_SPECS.md with RGB/CMYK/Pantone values.

---

## TECHNICAL STACK

- **Frontend:** Power Apps Canvas App (Tablet format)
- **Backend:** SharePoint Lists (data storage)
- **Automation:** Power Automate Cloud Flow
- **Document Generation:** Word Online template + HTML to PDF
- **File Storage:** OneDrive
- **Email:** Outlook
- **Users:** ~30 NTF submitters

---

## DATA REQUIREMENTS

Before implementation, ensure you have:
- ✅ Complete Employees list (name, email, job title, location, org level)
- ✅ Supervisors list with manager relationships
- ✅ HR email addresses
- ✅ Director of Ops for each location
- ✅ Corp Chef for each location
- ✅ CTR logo (optional but recommended)

---

## IMPLEMENTATION TIMELINE

**Typical timeline (can vary based on organization):**

- **Phase 1 (Data Prep):** 2-4 hours
- **Phase 2 (SharePoint Setup):** 3-5 hours
- **Phase 3 (Power Apps):** 6-8 hours
- **Phase 4 (Power Automate):** 3-5 hours
- **Phase 5 (Testing):** 3-4 hours
- **Phase 6 (Deployment):** 1-2 hours
- **Phase 7 (Support):** Ongoing

**Total:** 2-4 weeks depending on team size and complexity

---

## KEY DECISION POINTS

1. **PDF Template:** Use Word template or HTML-to-PDF? (see POWER_AUTOMATE_FLOW_SPECS.md Step 7)
2. **OneDrive Organization:** Flat structure or by month/year folders? (see IMPLEMENTATION_GUIDE.md Phase 4)
3. **Email Delivery:** Individual emails to each recipient or one email to all? (see POWER_AUTOMATE_FLOW_SPECS.md Step 11)
4. **Soft Launch:** Deploy to all users at once or gradual rollout? (see IMPLEMENTATION_GUIDE.md Phase 6)

---

## SUPPORT & MAINTENANCE

### Post-Go-Live Support
- **Week 1:** Daily monitoring and user support
- **Weeks 2-4:** Weekly check-ins and issue resolution
- **Ongoing:** Monthly maintenance and quarterly updates

### Regular Maintenance Tasks
- Monitor Power Automate flow execution
- Update Employees and Supervisors lists when staff changes
- Archive old PDFs to reduce OneDrive storage
- Review and optimize flow performance

---

## TROUBLESHOOTING

For common issues and solutions, refer to:
- **Power Apps Issues:** POWER_APPS_TECHNICAL_SPECS.md - Accessibility & Performance sections
- **Flow Issues:** POWER_AUTOMATE_FLOW_SPECS.md - Troubleshooting Guide
- **Implementation Issues:** IMPLEMENTATION_GUIDE.md - Phase 5 Testing

---

## NEXT STEPS

1. **Review** all documentation and clarify any questions
2. **Gather** data: Employees list, Supervisors list, HR emails
3. **Assign** team members to each implementation phase
4. **Start Phase 1:** Data Preparation (see IMPLEMENTATION_GUIDE.md)
5. **Execute** phases sequentially, using provided checklists
6. **Test** thoroughly with power users before go-live
7. **Train** all users on new NTF submission process
8. **Deploy** and monitor closely first week

---

## CONTACT & QUESTIONS

For questions about:
- **Architecture/Design:** See NTF_POWER_APP_SETUP.md
- **Power Apps specifics:** See POWER_APPS_TECHNICAL_SPECS.md
- **Power Automate specifics:** See POWER_AUTOMATE_FLOW_SPECS.md
- **Implementation process:** See IMPLEMENTATION_GUIDE.md

---

## VERSION HISTORY

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 12/22/2025 | Initial documentation - Ready for implementation |

---

**Last Updated:** December 22, 2025
**Project Status:** ✅ Documentation Complete
**Next Phase:** Begin Phase 1 - Data Preparation (IMPLEMENTATION_GUIDE.md)

---

*This project is built with Corner Table Restaurants' branding and specific requirements for 9 locations with ~30 users across FOH (Front of House) and BOH (Back of House) departments.*
