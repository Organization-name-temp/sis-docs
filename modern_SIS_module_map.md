# Modern SIS module map

This is the backend domain/service module map. The [whole-system PRD](prd/README.md) has one product requirements document per business module; role work is derived from each PRD later. Work proceeds Backend + AI + UI/UX → Frontend → QA, with security after each phase; UI/UX has no security assessment. Mobile is removed from current delivery. This backend tree is a reference, not the product hierarchy or frontend navigation.

```text
MODERN STUDENT INFORMATION SYSTEM
│
├── A. Platform & Foundation
│   ├── 01. Identity & Access Management
│   │   ├── Person registry
│   │   ├── User accounts
│   │   ├── Authentication
│   │   ├── Role assignments
│   │   ├── Permission policies
│   │   ├── Scoped memberships
│   │   └── Security history
│   ├── 02. Institution Management
│   │   ├── Institution profiles
│   │   ├── Campus structure
│   │   ├── Academic units
│   │   ├── Unit responsibilities
│   │   └── Structure history
│   └── 03. Configuration & Policy Management
│       ├── Configuration registry
│       ├── Localized settings
│       ├── Policy publication
│       ├── Template configuration
│       ├── Change control
│       └── Configuration preview
│
├── B. Academic Foundation
│   ├── 04. Academic Structure & Calendar
│   │   ├── Academic careers
│   │   ├── Academic years
│   │   ├── Terms and sessions
│   │   ├── Academic deadlines
│   │   ├── Calendar publication
│   │   └── Calendar presentation
│   ├── 05. Program & Curriculum Management
│   │   ├── Program definitions
│   │   ├── Program versions
│   │   ├── Curriculum versions
│   │   ├── Requirement groups
│   │   ├── Cohort applicability
│   │   └── Version comparison
│   ├── 06. Course Catalog Management
│   │   ├── Course definitions
│   │   ├── Course versions
│   │   ├── Career applicability
│   │   ├── Prerequisite declarations
│   │   ├── Catalog publication
│   │   └── Equivalence references
│   └── 07. Academic Rules Engine
│       ├── Rule definitions
│       ├── Eligibility evaluation
│       ├── Grading policies
│       ├── Progression rules
│       ├── Evaluation traces
│       ├── Exception application
│       └── Policy simulation
│
├── C. Student Lifecycle
│   ├── 08. Admissions Management
│   │   ├── Intakes and requirements
│   │   ├── Application schemas
│   │   ├── Draft applications
│   │   ├── Supporting evidence
│   │   ├── Submission
│   │   ├── Review queues
│   │   ├── Decisions and offers
│   │   └── Matriculation handoff
│   ├── 09. Student Lifecycle Management
│   │   ├── Matriculation
│   │   ├── Student profiles
│   │   ├── Career memberships
│   │   ├── Program enrollments
│   │   ├── Status transitions
│   │   ├── Student context
│   │   ├── Student requests
│   │   └── Advisor context
│   ├── 10. Course & Section Management
│   │   ├── Course offerings
│   │   ├── Sections
│   │   ├── Instructor assignments
│   │   ├── Meeting schedules
│   │   ├── Capacity configuration
│   │   ├── Rosters
│   │   └── Scheduling checks
│   ├── 11. Course Registration
│   │   ├── Section search
│   │   ├── Eligibility checks
│   │   ├── Seat allocation
│   │   ├── Duplicate protection
│   │   ├── Add and drop
│   │   ├── Registration exceptions
│   │   ├── Schedule and history
│   │   └── External roster projection
│   ├── 12. Academic Records & Grading
│   │   ├── Grade entry
│   │   ├── Grade publication
│   │   ├── Grade corrections
│   │   ├── Academic attempts
│   │   ├── GPA calculation
│   │   ├── Academic standing
│   │   ├── Transcript views
│   │   └── Record requests
│   ├── 13. Degree Audit & Graduation
│   │   ├── Requirement evaluation
│   │   ├── Degree progress
│   │   ├── Eligibility review
│   │   ├── Audit snapshots
│   │   ├── Graduation review
│   │   └── Academic awards
│   └── 14. Postgraduate Research
│       ├── Research candidature
│       ├── Research topics
│       ├── Supervisor assignments
│       ├── Milestone templates
│       ├── Progress reviews
│       ├── Thesis submissions
│       ├── Research requests
│       └── Examination processes
│
├── D. Student Services
│   ├── 15. Student Finance & Holds
│   │   ├── Student accounts
│   │   ├── Charge policies
│   │   ├── Ledger entries
│   │   ├── Balance views
│   │   ├── Holds
│   │   ├── Scholarship credits
│   │   ├── Reconciliation
│   │   └── Payment adapters
│   ├── 16. Scholarships & Financial Aid
│   │   ├── Scholarship programs
│   │   ├── Eligibility policies
│   │   ├── Aid applications
│   │   ├── Evaluation and review
│   │   ├── Award decisions
│   │   ├── Budget allocation
│   │   ├── Disbursement instructions
│   │   ├── Renewals
│   │   ├── Suspension and termination
│   │   └── Aid reporting
│   └── 17. Notifications & Communication
│       ├── In-app inbox
│       ├── Email delivery
│       ├── Student push
│       ├── Recipient resolution
│       ├── Template management
│       ├── Delivery operations
│       └── Preferences
│
├── E. System Capabilities
│   ├── 18. Reporting & Analytics
│   │   ├── Operational dashboards
│   │   ├── Report definitions
│   │   ├── Safe exports
│   │   ├── Reconciliation reports
│   │   ├── Cohort analytics
│   │   ├── Data feeds
│   │   └── Scholarship analytics
│   └── 19. AI Assistant
│       ├── Instituational Policy Q&A
│       ├── Academic Advisor
│       ├── Natural Language Reporting (Staff)
│       ├── Automation
│       └── Auditing & Decision Making
│
└── F. SaaS Platform
    └── 20. Multi-Tenancy Management
        ├── Tenant provisioning
        ├── Tenant lifecycle
        ├── Isolation controls
        ├── Tenant settings
        ├── Feature entitlements
        ├── Platform operations
        ├── Support access
        ├── Commercial plans
        └── Deployment options
```
