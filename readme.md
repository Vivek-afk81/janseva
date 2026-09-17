> **JANSEVA-INFRA**\
> Civic Issue Reporting and Management System

**Project Documentation**

**Team Members**

> Shaswat Mishra\
> Vivek Chauhan\
> Vansh Virmani\
> Vansh Gupta

Janseva-Infra \| Page 1

**1. Project Title**

**Janseva-Infra --- Civic Issue Reporting and Management System**\
A comprehensive web application that enables citizens to report civic
issues, track their status, and allows authorities to manage,
prioritize, and resolve reported issues.

**2. Team Members**

1\. **Shaswat Mishra**\
2. **Vivek Chauhan**\
3. **Vansh Virmani**\
4. **Vansh Gupta**

**3. Problem Background**

Civic issues such as damaged roads, potholes, garbage accumulation,
broken streetlights, drainage problems, and other infrastructure-related
concerns are often difficult for citizens to report and track
effectively.

Traditional complaint systems can lack real-time tracking, transparency,
proper prioritization, and efficient communication between citizens and
authorities. Janseva-Infra provides a centralized digital platform where
citizens can report problems with relevant details and images, while
authorities can view, prioritize, assign, and manage these issues
efficiently.

**4. Problem Statement**

There is a need for an efficient and transparent system that connects
citizens with civic authorities for reporting and resolving local
infrastructure issues.

• Lack of a centralized civic complaint platform.

• Difficulty in tracking the progress of complaints.

• Delayed identification of high-priority issues.

• Lack of visual and location-based information.

• Limited functionality when internet connectivity is unavailable.

• Difficulty for authorities in managing and assigning large numbers of
complaints.

**5. Impact**

Janseva-Infra can create a more transparent, responsive, and efficient
civic issue management process.

• **Citizens:** Easier reporting and real-time tracking of complaints.

• **Engineers:** Better visibility of assigned issues and their
locations.

• **Supervisors:** Improved monitoring and management of civic
complaints.

• **Authorities:** Faster identification and prioritization of important
issues.

• **Community:** Improved accountability and potentially faster
resolution of infrastructure problems.

Janseva-Infra \| Page 2

The system also supports offline reporting, allowing issues to be stored
locally and synchronized when connectivity becomes available.

**6. Proposed Software Solution**

We propose a web-based Civic Issue Reporting System with role-based
access for **Citizens, Engineers, and Supervisors**.

**Citizen Module:** Register/login, report civic issues, upload
photographs, provide issue details and location, and track reported
issues.

**Engineer Module:** View assigned issues, access issue details and
images, update progress/status, and use map-based information to locate
issues.

**Supervisor Module:** Monitor reported issues, manage and assign
issues, prioritize important complaints, and track overall issue
resolution.

**Smart Features:** Gemini AI for issue-priority prediction, Leaflet
maps, IndexedDB offline storage, automatic synchronization after
reconnecting, and real-time issue status tracking.

**7. Technical Feasibility**

The project is technically feasible because it uses established web
technologies and cloud services.

**8. Operational Feasibility**

The system is designed around the workflow of reporting, assigning,
monitoring, and resolving civic issues.

**Workflow:** Citizen reports issue → Issue is stored → Priority is
assessed → Authority/Engineer manages issue → Status is updated →
Citizen tracks progress.

The application supports both online and offline workflows. When
offline, an issue can be stored in an IndexedDB queue and synchronized
with the backend once connectivity is restored.

**9. Project Scope**

**In-Scope**\
• Citizen registration and authentication.

• Civic issue reporting and image attachment.

• Location-based issue visualization.

• Issue status tracking.

• Role-based access for Citizens, Engineers, and Supervisors.•
Assignment and management of issues.

• AI-based priority prediction.

• Offline issue reporting and automatic synchronization.

Janseva-Infra \| Page 3

• Interactive maps and dashboards.

**Out-of-Scope**\
• Direct integration with government municipal systems.

• Automated physical repair of civic infrastructure.

• IoT-based infrastructure monitoring.

• Large-scale deployment across multiple government departments.•
Advanced predictive infrastructure maintenance.

• Native Android/iOS applications.

**10. Expected Outcome**

The expected outcome of Janseva-Infra is a centralized civic issue
management platform that improves communication between citizens and
authorities.

1\. Make civic issue reporting simpler and more accessible.

2\. Improve transparency through issue status tracking.

3\. Help authorities prioritize issues using AI-assisted assessment. 4.
Improve issue management through role-based workflows.

5\. Provide geographical context through interactive maps.

6\. Allow reporting even without an active internet connection.

7\. Automatically synchronize offline reports once connectivity is
restored. 8. Improve accountability by maintaining issue information and
progress.

**Overall:** Janseva-Infra aims to transform civic complaint management
from a fragmented reporting process into a structured, trackable, and
technology-driven workflow.

**Technology Stack**

+-----------------------------------+-----------------------------------+
| > **Component**                   | > **Technology**                  |
+===================================+===================================+
| > Frontend                        | > React 18 + Vite                 |
+-----------------------------------+-----------------------------------+
| > Authentication                  | > Firebase Authentication         |
+-----------------------------------+-----------------------------------+
| > Database                        | > Firebase Firestore              |
+-----------------------------------+-----------------------------------+
| > Image Storage                   | > Supabase                        |
+-----------------------------------+-----------------------------------+
| > Offline Storage                 | > IndexedDB                       |
+-----------------------------------+-----------------------------------+
| > Maps                            | > Leaflet + React-Leaflet         |
+-----------------------------------+-----------------------------------+
| > Routing                         | > React Router                    |
+-----------------------------------+-----------------------------------+
| > State Management                | > React Context                   |
+-----------------------------------+-----------------------------------+
| > HTTP Client                     | > Axios                           |
+-----------------------------------+-----------------------------------+
| > AI                              | > Google Generative AI (Gemini)   |
+-----------------------------------+-----------------------------------+

Janseva-Infra \| Page 4

+-----------------------------------+-----------------------------------+
| > **Component**                   | > **Technology**                  |
+===================================+===================================+
| > Charts                          | > Recharts                        |
+-----------------------------------+-----------------------------------+
| > Forms                           | > React Hook Form                 |
+-----------------------------------+-----------------------------------+

Janseva-Infra \| Page 5

