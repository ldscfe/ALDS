# UI Design Skill: Modern Enterprise SaaS Interface

## Version

```
UI Design System v1.0
Style:
Linear + Ant Design Pro + Apple Human Interface Guidelines
Target:
Enterprise SaaS / Internal Management System
```

---

# 1. Design Philosophy

## Core Principle

All UI must embody:

> Professional, Minimal, Elegant, Efficient.

Avoid:

* Traditional ERP style
* Bootstrap default style
* Dense table stacking
* Cheap gradients
* Large-area borders
* Excessive decoration

Design Goals:

Similar to:

* Linear's minimalist efficiency
* Ant Design Pro's enterprise-grade specifications
* Apple's visual refinement

---

# 2. Overall Visual Language

## Layout

Adopt:

```
Content First
```

Principles:

* Page whitespace first
* Information hierarchy first
* Reduce visual noise

Prohibited:

```
┌────────────────────┐
│████████████████████│
│ field field field  │
│ field field field  │
│ field field field  │
└────────────────────┘
```

Recommended:

```
┌────────────────────────┐

 User Profile

 Basic Information

 Name
 ┌──────────────────┐
 │ Adam             │
 └──────────────────┘


 Permission

 Role
 ┌──────────────────┐
 │ Admin       ▼    │
 └──────────────────┘

└────────────────────────┘
```

---

# 3. Color System

## Primary

Use brand color:

```
Primary:
#1677FF
```

For:

* Primary Button
* Link
* Focus State
* Active Navigation

---

## Neutral

Backgrounds:

```
Page Background:
#F7F8FA

Card:
#FFFFFF

Border:
#E5E7EB

Text Primary:
#111827

Text Secondary:
#6B7280
```

---

Prohibited:

* Pure black text
* Excessive colors
* Colorful buttons
* High-saturation backgrounds

---

# 4. Typography

## Font

Preferred:

```
Inter
SF Pro Display
PingFang SC
Microsoft YaHei
```

Rules:

Headings:

```
font-size: 20-24px
font-weight: 600
```

Body:

```
font-size: 14px
font-weight: 400
```

Auxiliary text:

```
12px
color: #6B7280
```

---

# 5. Spacing System

Use 4px base unit:

```
4
8
12
16
24
32
48
64
```

Prohibited:

Random:

```
13px
17px
29px
37px
```

---

# 6. Border & Shadow

## Radius

Unified:

```
Small:
8px

Medium:
12px

Large:
16px
```

---

## Shadow

Use:

```
soft shadow
```

Example:

```
0 10px 30px rgba(0,0,0,0.08)
```

Prohibited:

```
strong shadow
heavy border
3D effect
```

---

# 7. Dialog / Modal Design

All dialogs must:

## Size

```
width:
520px - 640px

max-height:
80vh
```

---

## Structure

Fixed:

```
Dialog

├── Header
│
│   Avatar
│   Title
│   Description
│
├── Content
│
│   Section
│
│   Section
│
└── Footer
    Cancel
    Confirm
```

---

## Header

Must include:

```
Icon / Avatar

Title

Description
```

Example:

```
👤

Edit User

Update user information and permissions
```

---

# 8. Form Design

## Layout

Prohibited:

```
label | input
```

Recommended:

```
Label

Input
```

---

## Input

Unified:

```
height:
40-44px

radius:
8px

border:
1px solid #E5E7EB
```

---

## Focus

Must:

```
brand color outline
```

---

## Element Plus Form Alignment (Implementation Rules)

> Rules below target Element Plus forms with default layout (`label-position="right"`, label left of input). Mandatory when customizing EP component height or using multi-column layouts, otherwise "visually undetectable but actually misaligned" alignment bugs occur.

### Rule 1 — Label & Input Must Be Vertically Centered

Phenomenon: After customizing input height (`--input-height` etc.), EP default label `line-height` (32px) no longer matches new input height, label and input vertically misaligned.

Must:

```
.el-form-item {
  display: flex;
  align-items: center;
}
```

Prohibited: Using label `padding-top`/`padding-bottom` to "fake" alignment — input height changes again misalign.

### Rule 2 — Same-Row Multi-Column Must Be Symmetrical

Phenomenon: Same row (`.form-row`) with two side-by-side `form-item`, one column overall few px higher/lower than other (measured ~8px).

Must: Vertical spacing only on **row container**, row items `margin-bottom: 0`:

```
.form-row {
  display: flex;
  align-items: center;
  gap: <spacing>;
  margin-bottom: <spacing>;
}
.form-row:last-child {
  margin-bottom: 0;
}
.user-form .form-row .el-form-item {
  margin-bottom: 0;
}
```

Prohibited: Using `.el-form-item:last-child { margin-bottom: 0 }` on **row items** — only clears last column's margin, causing left/right column margin asymmetry in same row.

Reason: Flex row cross-axis height stretched by one item's margin; even if two columns same height, their border-box vertical centers misalign.

---

# 9. Button Design

## Primary

For:

* Save
* Submit
* Create

Style:

```
background:
primary color

height:
40px

radius:
8px
```

---

## Secondary

For:

* Cancel
* Back

Style:

```
background:
transparent

border:
none or subtle
```

---

Prohibited:

```
Oversized buttons

Gradient buttons

3D buttons
```

---

# 10. User Management Page Example

## User Dialog

Structure:

```
Edit User

--------------------------------

Avatar


Basic Information

Username
Email
Display Name


--------------------------------

Access Control

Role
Department
Status


--------------------------------

Security

Password
MFA


--------------------------------

Cancel        Save User
```

---

# 11. Component Rules

When generating components:

Must:

```
Component Driven
Reusable
Typed
Maintainable
```

Example:

Don't:

```
UserEditModal.jsx
1000 lines
```

Recommend:

```
components/

UserDialog

├── UserHeader

├── UserBasicForm

├── UserPermissionForm

├── UserSecurityForm

└── DialogFooter
```

---

# 12. Animation

Allowed:

```
200-300ms
ease-out
```

For:

* Dialog open
* Hover
* Focus

Prohibited:

* Large movements
* Bounce animations
* Flashy effects

---

# 13. Table Design

Backend systems heavily use Tables.

Requirements:

## Header

```
small
gray
```

---

## Row

Height:

```
48-56px
```

---

## Actions

Don't:

```
Edit Delete View
```

Three buttons horizontally.

Recommend:

```
⋮
```

Dropdown menu.

---

# 14. Empty State

Prohibited:

```
No data
```

Shown alone.

Must:

```
Icon

Title

Description

Action Button
```

Example:

```
        📂

No Users Yet

Create your first user

[ Create User ]
```

---

# 15. AI Coding Rules

Before generating code:

Must output first:

```
1. UI Structure
2. Component Tree
3. Design Decisions
```

Then code.

---

When generating code:

Must check:

```
□ Linear style compliance
□ Sufficient whitespace
□ Avoids default admin look
□ Componentized
□ Dark Mode support
□ Responsive
□ Maintainable
□ label↔input vertical center Δ≤3px (Playwright quantified)
□ Same-row multi-column vertical center Δ≤3px (Playwright quantified)
```

---

# 16. Forbidden Patterns

AI must not generate:

## ❌ Old Admin Style

```
Gray big boxes

Dense fields

Heavy borders

Blue buttons everywhere
```

---

## ❌ Bootstrap Look

Prohibited:

```
btn-primary
card-header
table-bordered
```

Default visuals.

---

## ❌ Dashboard Decoration

Prohibited:

```
Gradient backgrounds

Huge numbers

Meaningless icons

Colorful cards
```

---

# 17. Final AI Instruction

Every UI generation:

```
You are a senior product designer and frontend architect.

Follow this UI Design System strictly.

Do not generate a generic admin dashboard.

Prioritize:
- clarity
- hierarchy
- elegance
- usability
- maintainability

The final UI should look like a modern SaaS product,
not a traditional enterprise management system.
```