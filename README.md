# DBMS Lab — ER Forensics: Find the Hidden Modeling Flaws

Solutions to the 5-question ER modeling assignment (Dr. Vikas Srivastava), sourced from `er_forensics_dbms_lab_assignment.pdf`.

## Assignment Recap

Each question presents an ER diagram that is *syntactically plausible but semantically flawed*, plus a set of business rules the diagram fails to support. For each question the task is to:

1. Find exactly **4** significant modeling issues.
2. For each issue, state what information is lost or misrepresented.
3. Redraw **only** the part of the ER model that must change — not the whole diagram.

Cardinality labels were intentionally omitted from the original diagrams and are **not** counted as a flaw.

## How to read the answers

Each question has its own file with: the original flawed model recreated, the business rules, the 4 issues (flaw → information lost → minimal correction), and a corrected ER diagram fragment in [Mermaid](https://mermaid.js.org/syntax/entityRelationshipDiagram.html) `erDiagram` syntax.

The Mermaid diagrams render automatically in GitHub, most modern Markdown viewers (VS Code preview, Obsidian, Typora, etc.), and Notion. If your viewer doesn't support Mermaid, paste the code block into [mermaid.live](https://mermaid.live) to see it rendered.

## Files

| Question | Topic | Root cause of the flaws |
|---|---|---|
| [Q1_University_Course_Registration.md](Q1_University_Course_Registration.md) | University Course Registration | Missing `Section` entity — semester/year, instructor, classroom, and enrollment are all wired to the course catalog level instead of a specific offering |
| [Q2_Hospital_Prescription_System.md](Q2_Hospital_Prescription_System.md) | Hospital Prescription System | Missing `Consultation`/`Prescription` entities — visits and prescriptions can't repeat or carry their own date/dosage |
| [Q3_Ecommerce_Order_Fulfilment.md](Q3_Ecommerce_Order_Fulfilment.md) | E-Commerce Order Fulfilment | Missing `OrderLine`/`ShipmentLine` entities — quantity, address, and shipment splitting have no line-item granularity to attach to |
| [Q4_Project_Staffing_and_Roles.md](Q4_Project_Staffing_and_Roles.md) | Project Staffing and Roles | Missing `Assignment`/`ManagerHistory` entities — role, hours, and management are all time-dependent facts stored as static attributes/edges |
| [Q5_Airline_Booking_and_Seat_Assignment.md](Q5_Airline_Booking_and_Seat_Assignment.md) | Airline Booking and Seat Assignment | `Flight` conflates flight-number with flight-occurrence, no `PNR` entity, and seating is wrongly modeled as ternary |

## Common Pattern Across All 5 Questions

Every flawed diagram makes some version of the same mistake: **a fact that is actually time-dependent, event-specific, or per-instance is modeled as a static attribute (or a plain, unattributed relationship) on a coarser-grained entity.** The fix is consistently the same shape — introduce an associative ("junction") entity at the correct grain (section, consultation/prescription, order line/shipment line, assignment/management period, flight occurrence/PNR segment) and move the misplaced attribute or relationship down onto it. Recognizing this pattern is the fastest way to spot the 4 issues in each question.
