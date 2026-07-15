# ACU-Student-Retention-System

Enterprise Student At-Risk Retention & Intervention System 
 
Project Scope & Objectives

The Project is a multi-channel case management solution for a higher education organisation (simulated for Australian Catholic University) to proactively track academic drop-out risks across multiple physical campuses. The objective of this initiative is to eliminate manual email-driven interventions for at-risk university students. The system must centralize academic metrics, automate risk identification, provide mobile operational tools for field advisors, and enforce strict student data privacy laws. 

Functional Requirements

FR-1 [Data Centralization]: The system must support relational integrity linking a student profile to multiple concurrent course enrolments and historical support tickets.

FR-2 [Automated Triage]: The system must actively monitor grade inputs. If a student's mid-semester mark registers below 50%, a high-priority intervention ticket must automatically generate within 30 seconds.

FR-3 [Advisory Workspace]: Academic Staff require a desktop portal to view a student's administrative file alongside an embedded grid of their live academic metrics.

FR-4 [Mobile Field Operations]: Academic Advisors requiring mobility across campuses must be able to search the directory via a smartphone app, view real-time student grades, and append student meeting/advisory notes to existing case files.

FR-5 [Audit Trail]: All mobile data entry entries must automatically calculate and append an immutable timestamp along with the full name of the Academic advisor (field user) to the record string.

Non-Functional Requirements

NFR-1 [Data Security & Privacy]: In compliance with university privacy policies, teaching staff (Professors) must be completely restricted from viewing or discovering student intervention case logs.

NFR-2 [Scalability & Delegation]: Mobile directory filtering operations must leverage fully delegable Power Fx queries to ensure maximum processing performance on the Dataverse cloud server as student rows grow.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++


Enterprise Student At-Risk Retention & Intervention System
 

An end-to-end, multi-channel business applications platform engineered inside the Microsoft Power Platform ecosystem. Designed around a simulated higher education framework for an Australian institution, this solution addresses data fragmentation, automated case management, and mobile field operations.

🚀 System Architecture
The solution maps across three distinct platform layers to deliver automated data insights:


[Dataverse Database] ──> [Model-Driven App] ──> [Power Automate Flow] ──> [Canvas Mobile App]
 (Relational Tables)     (Advisor Portal)    (Automated Risk Check)     (Mobile Advisor Hub)

 
📊 1. Relational Data Layer (Microsoft Dataverse)
The database architecture shifts away from flat files, deploying a relational star-schema utilizing unmanaged solution containers for structured lifecycle management.

Entity Relationship Model
ACU Student (Master Hub Table)

Course Enrolment (Many-to-One [N:1] Lookup to Student)

Intervention Case (Many-to-One [N:1] Lookup to Student)

📸 [ ERD CANVAS DIAGRAM SCREENSHOT]

<img width="1440" height="900" alt="ERD Diagram" src="https://github.com/user-attachments/assets/2f5349e3-6430-4c90-9fff-c0e081b55be1" />






💻 2. Back-Office Desktop Portal (Model-Driven Power App)
Designed as a high-density administrative desk for university Academic Advisors to review profiles, manage operations, and analyze student health records.

Key Technical Implementations:
Custom public layout view filtering (Active ACU Students View) with integrated system sequencing column logic.

Relational Subgrids: Form layout cards embedding dynamic grids that isolate and display course performance data contextually on individual student forms.

📸 [DESKTOP ADVISOR PORTAL SCREENSHOT]

<img width="1440" height="861" alt="Desktop Advisor Portal App" src="https://github.com/user-attachments/assets/705ae705-3985-4778-9443-419457cde450" />
Portal App


<img width="1440" height="860" alt="Desktop Advisor Portal App 2" src="https://github.com/user-attachments/assets/6456558a-4cf6-4257-aff4-dbc9233ffd8c" />
Students Records


<img width="1440" height="861" alt="Desktop Advisor Portal App 3" src="https://github.com/user-attachments/assets/2895e7d5-e8bd-418f-a795-33e8cd5e5de2" />
Course Enrollment Record


<img width="1440" height="858" alt="Desktop Advisor Portal App 4" src="https://github.com/user-attachments/assets/6bb15392-ad1f-4f7b-85bb-3e1f03f9af46" />
Intervention Case Ticket automatically being created for a student who have received less than 50 mid-sem marks. 




🤖 3. Risk Triage Automation (Power Automate Cloud Flow)
An asynchronous background cloud flow constructed to handle automated system evaluations.

Trigger: Dataverse row update payload monitoring Course Enrolments.

Condition Logic: Evaluates Mid-Sem Mark against an integer parameter < 50.

Action Logic: Dynamically creates a new record inside the Intervention Cases table using manual OData relational path parsing:

/acu_acustudents(@{triggerOutputs()?['body/_acu_student_value']})


📸 [POWER AUTOMATE LOGIC FLOW DIAGRAM SCREENSHOT]

<img width="1440" height="857" alt="Power Automate Cloud Flow Diagram" src="https://github.com/user-attachments/assets/aeb2bf49-a998-4769-b453-a45d45318b6d" />




📱 4. Mobile Field Activity App (Canvas Power App)
A pixel-perfect smartphone application engineered using Phone Format layouts, empowering advisors to remain mobile while navigating campuses.

Key Power Fx Implementation Highlights:
Delegable Search Architecture: Optimized database queries utilizing server-side filtering logic:


Search('ACU Students', TextInput1.Text, acu_fullname)
Optimized State Navigation: Leveraged global state parameters (Set) on gallery transitions to eliminate client-side parsing bottlenecks:


Set(SelectedStudentID, ThisItem.acu_acustudentid); Navigate('Detailed Screen', ScreenTransition.Cover)
Advanced Database Patching & Auditing: Custom database entry command utilizing the Patch function combined with system environment variables to create an incremental audit log file without field bloat:


Patch(
    'Intervention Cases',
    LookUp('Intervention Cases', Student.acu_acustudentid = SelectedStudentID && 'Case Status' = 'Case Status (Intervention Cases)'.New),
    {
        'Staff Notes': LookUp('Intervention Cases', Student.acu_acustudentid = SelectedStudentID).'Staff Notes' & 
                       Char(10) & Char(10) & 
                       "[" & Text(Now(), "dd/mm/yyyy hh:mm AM/PM") & " - " & User().FullName & "]: " & 
                       TextInput2.Text
    }
);
Notify("Notes successfully appended with timestamp!", NotificationType.Success);
Reset(TextInput2)



📸 [MOBILE ADVISOR APP SEARCH & DETAILS SCREEN SCREENSHOTS]


<img width="1440" height="858" alt="Mobile Advisor App 1" src="https://github.com/user-attachments/assets/a916e8b7-814e-4860-90d8-cba1fcb5c1af" />

Students with marks below 50 are highlighted as ‘AT RISK’. For large number of records and faster retrieval, user can search via student name or student id in the search box.

 

<img width="1440" height="855" alt="Mobile Advisor App 2" src="https://github.com/user-attachments/assets/d617f4c4-d902-4f7c-9cc4-182ba6771177" />

Student details along with enrolled units and mid-sem marks are displayed on this screen. Field advisors can put in their notes when meeting with the students on campus. 
 


<img width="1440" height="855" alt="Mobile Advisor App 4" src="https://github.com/user-attachments/assets/36857673-1862-483a-8b05-c32fc89f67ff" />

Meeting notes successfully saved in intervention case ticket in backend.



<img width="1440" height="858" alt="Mobile Advisor App 5" src="https://github.com/user-attachments/assets/9fedf15d-1322-4d5a-8266-f5f7e3175e58" />

Advisory notes saved in intervention case ticket in backend portal with audit trail.



🔐 5. Governance & Data Security
Role-based access controls (RBAC) configured via the Power Platform security matrix interface to handle information filtering:

ACU Professor Role: Read permissions assigned to student rows, full write rights allocated to grade columns, complete masking (none) applied to the intervention system.

ACU Academic Advisor Role: Global organization privileges (Create, Read, Update, Delete) configured across all relational system entities.

 
📸 [SECURITY ROLES FOR BACKEND PROFESSOR USERS AND FIELD ADVISOR USERS]

<img width="1440" height="860" alt="Security Roles" src="https://github.com/user-attachments/assets/fa96aca8-f631-414e-bf06-9a653acc3ccc" />

