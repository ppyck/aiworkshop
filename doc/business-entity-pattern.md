# ABL Business Entity Architecture Pattern (Condensed)

This document summarizes the Business Entity–based data access architecture used in this project and how it differs from traditional direct database access.

## Core Concepts

- **Business Entity (BE)**
  - Encapsulates data access and business rules for a domain (e.g. `CustomerEntity`).
  - Inherits from `OpenEdge.BusinessLogic.BusinessEntity`.
  - Works with datasets/temp-tables instead of direct UI widgets or DB buffers.

- **Datasets / Temp-Tables**
  - Define the contract between UI and BE.
  - UI reads/writes only temp-tables; **no direct DB access in UI**.

- **EntityFactory**
  - Singleton/service locator for BE instances.
  - E.g. `EntityFactory:GetInstance():GetCustomerEntity()`.

## Modern Flow (Example: Customer)

- UI calls BE methods (e.g. `GetCustomerByNumber`) with criteria.
- BE builds filter (e.g. `WHERE Customer.CustNum = ?`) and calls `ReadData`.
- Dataset is populated; UI reads from `ttCustomer` and updates widgets.
- For updates:
  - UI enables tracking changes on temp-table.
  - UI modifies temp-table fields.
  - UI calls BE validation (e.g. `ValidateCustomer`).
  - If valid, UI calls BE update (e.g. `UpdateCustomer`) which calls `UpdateData`.

## Legacy vs. BE Architecture

**Legacy (to refactor):**
- UI does `FIND`/`FOR EACH` directly on DB tables.
- UI implements validation and business rules.
- UI performs locking (`NO-LOCK`, `EXCLUSIVE-LOCK`).
- Business rules are scattered and hard to reuse.

**Business Entity (recommended):**
- UI never touches DB tables or locks.
- All DB access goes through BE methods and datasets.
- Validation and rules live in the BE.
- Factory mediates access to BE instances.

## Refactoring Guidelines

1. **Create a Business Entity** for each domain (Customer, Item, etc.).
2. **Define dataset/temp-table include** (e.g. `CustomerDataset.i`).
3. **Configure data-source(s)** in the BE and pass dataset handle to `SUPER()`.
4. **Move reads** from UI `FIND`/`FOR EACH` into BE methods using `ReadData`.
5. **Move validation** rules into BE methods (e.g. `ValidateCustomer`).
6. **Move updates** into BE methods that wrap `UpdateData`.
7. **Make UI call BE methods only**, passing datasets by reference.

## Checklist for Refactored Code

- UI has **no direct DB access** (`FIND`, `FOR EACH`, `BUFFER`).
- UI interacts only with datasets and BE methods.
- Validation and business rules are centralized in BE.
- Reads use `ReadData`; updates use `UpdateData` via BE methods.
- Entities are obtained via `EntityFactory`, not `NEW` scattered in UI.
