# AI Workshop - OpenEdge ABL Project

This repository contains an OpenEdge ABL sample project derived from the ProjectGengar Step-16 branch.

It demonstrates:
- ABL Business Entity architecture for data access
- Separation of UI (AppBuilder .w windows) and business logic
- Example business rules and refactoring patterns

Project root files:
- `openedge-project.json` – OpenEdge project descriptor
- `build.xml` – Ant build script

See `doc/business-entity-pattern.md` for architectural documentation.

---

## Prerequisites

- OpenEdge 12.8 installed (`DLC` environment variable set)
- Java + Ant available to build the sports2000 database
- Access to a Windows GUI session (for running `.w` windows)

---

## 1. Get the code

```bash
git clone https://github.com/ppyck/aiworkshop.git
cd aiworkshop
```

---

## 2. Build and start the sports2000 database

The project includes a schema reference for the standard sports2000 DB.

1. **Build the database files** using Ant (from the project root):

   ```bash
   ant db
   ```

   This will create a `db/sports2000` database using the `dump/sports2000.df` schema.

2. **Start the database** (from project root), for example using the provided script:

   ```bash
   db/startDb.bat
   ```

3. Confirm that the DB is running and accessible as `sports`/`sp2k` according to `openedge-project.json`.

---

## 3. PROPATH and project layout

Typical PROPATH for running the samples:

- `src` (contains `CustomerWin.w` and business classes)
- `${DLC}` and standard OpenEdge directories

The `openedge-project.json` already describes a suitable build/PROPATH configuration.

---

## 4. Running the Customer window

From a `proenv` (OpenEdge) command prompt with `DLC` set and the DB running:

1. Change to the project root (`aiworkshop`).
2. Start the GUI client and run the window:

   ```bash
   prowin.exe -db db/sports2000 -p src/CustomerWin.w -basekey ini
   ```

   or with aliases as defined in `openedge-project.json`:

   ```bash
   prowin.exe -db db/sports2000 -ld sp2k -p src/CustomerWin.w -basekey ini
   ```

3. Use the UI:

- Enter a customer number.
- Click **Get Customer** to load data via `CustomerEntity`.
- Edit the name and click **Save** to validate and persist changes via the Business Entity.

---

## 5. Extending the project

- Add new Business Entities under `src/business/` following the pattern in `CustomerEntity.cls`.
- Register them in `EntityFactory.cls`.
- Create new AppBuilder windows in `src/` that:
  - Include the appropriate dataset `.i` file.
  - Use `EntityFactory` to obtain entities.
  - Avoid direct database access from the UI.

For detailed architectural guidance, refer to:

- `doc/business-entity-pattern.md`
- `src/business/CustomerEntity.cls`
- `src/CustomerWin.w`
