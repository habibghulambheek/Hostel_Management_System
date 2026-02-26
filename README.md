#  Hostel Management Database System

A relational database system built to manage the day-to-day operations of a university hostel — covering residents, rooms, employees, inventory, mess services, fees, and departments.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Database Schema](#database-schema)
- [Normalization](#normalization)
- [Views](#views)
- [Reports](#reports)
- [PL/SQL Components](#plsql-components)
- [Tech Stack](#tech-stack)
- [Team](#team)

---

## Overview

This system provides a structured and normalized database to handle all hostel-related data in one place. It ensures data integrity, avoids redundancy, and supports common operations like room allocation, fee tracking, mess enrollment, and inventory management.

The schema was designed using both **top-down** and **bottom-up** normalization approaches and satisfies up to **4NF**.

---

## Features

- Room and block management with occupancy tracking
- Resident (allottee) registration with personal and contact details
- Fee tracking with paid/unpaid status
- Mess enrollment and mess fee linking
- Inventory tracking per room with condition monitoring
- Employee management with role hierarchy
- Department linkage for academic context
- Automated capacity validation via triggers
- Reporting queries for occupancy, salary, and inventory insights

---

## Database Schema

The system consists of **13 tables**:

| Table | Description |
|---|---|
| `ALLOTEE` | Stores resident personal and room assignment details |
| `ALLOTEE_EMAIL` | Multi-valued emails for residents |
| `ALLOTEE_PHONENO` | Multi-valued phone numbers for residents |
| `ROOM` | Room details including capacity and occupancy status |
| `BLOCK` | Block details and assigned block manager |
| `MESS_ENROLLMENT` | Tracks which residents are enrolled in the mess |
| `MESS_FEE` | Links mess enrollments to fee records |
| `FEE` | All fee records with payment status |
| `INVENTORY_ITEM` | Items assigned to rooms with condition tracking |
| `DEPARTMENT` | Academic departments linked to residents |
| `HOSTEL_EMPLOYEE` | Employee records with roles and salary |
| `EMPLOYEE_EMAIL` | Multi-valued emails for employees |
| `EMPLOYEE_PHONENO` | Multi-valued phone numbers for employees |

### Key Design Decisions

- Each hostel is divided into **blocks**, and each block contains multiple **rooms**
- Each room has a **maximum student limit** enforced at the database level via triggers
- **Cascading deletes** ensure that removing a resident automatically cleans up their mess and fee records
- Fees default to `Unpaid` status upon creation
- Inventory items track condition as `Good`, `Needs repairment`, or `Unrepairable`

---

## Normalization

The schema was validated through full normalization from **1NF to 4NF**:

- **1NF** — All multi-valued attributes (emails, phone numbers) were separated into their own tables
- **2NF** — All partial dependencies were removed by decomposing relations
- **3NF** — All transitive dependencies were eliminated (e.g., department info separated from allottee, block info separated from room)
- **BCNF** — All determinants are candidate keys
- **4NF** — No multi-valued dependencies exist

Both top-down (starting from entities) and bottom-up (starting from a single flat relation) approaches were applied and cross-validated against each other.

---

## Views

Two views were created for the most common operational needs:

**1. `OUTSTANDING_MESS_FEES`**
Lists all mess enrollments that have unpaid fees — useful for hostel administration to follow up on pending payments.

**2. `DEPARTMENT_ALLOTEES`**
Shows a count of residents per academic department along with department details — useful for reporting and resource planning.

---

## Reports

Five SQL queries are included for generating common operational reports:

1. **Inventory Summary by Block** — Total cost and item condition breakdown per block
2. **Room Occupancy Report** — Current occupants vs. max capacity per room
3. **Block Manager Contact Details** — Lists managers and their contact info per block
4. **Blood Group Distribution** — Count of residents grouped by blood group and block
5. **Salary Expenditure by Job Role** — Total and average salary per employee role

---

## PL/SQL Components

### Triggers
| Trigger | Purpose |
|---|---|
| `INSERT_ROOM_CAPACITY` | Prevents inserting a new resident if the room is already at full capacity |
| `UPDATE_ROOM_CAPACITY` | Prevents updating a resident's room if the target room is already full |

### Stored Procedures
| Procedure | Purpose |
|---|---|
| `UPDATE_ITEM_CONDITION` | Updates the condition of an inventory item by ID |
| `UPDATE_FEE_STATUS` | Updates the payment status of a fee record |

### Functions
| Function | Purpose |
|---|---|
| `GET_AVAILABILITY_STATUS` | Returns `AVAILABLE`, `UNAVAILABLE`, or `NO SUCH ROOM EXIST` for a given room and block |

---

## Tech Stack

- **Database:** Oracle SQL
- **Procedural Extension:** PL/SQL
- **Normalization:** BCNF / 4NF
- **Design Approach:** Top-down + Bottom-up ERD modeling

---

## Team

Developed as a Database Systems course project at the **University of Punjab**.

| Name | Student ID |
|---|---|
| Habib Ghulam Bheek | BITF23M020 |
| Asim Iftikhar | BITF23M029 |
| Mohsin Ali | BITF23M031 |
