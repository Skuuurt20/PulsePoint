# PulsePoint

PulsePoint is a role-based healthcare appointment management web application. Patients book clinic appointments online, secretaries review and manage the schedule (including walk-ins), and doctors see their confirmed appointments and mark them as completed.

Built with **ASP.NET Web Forms (.NET Framework 4.7.2)**, **C#**, and **Microsoft SQL Server**.

---

## Features

### Patients
- Register and log in with an email and password
- Book an appointment from a calendar with available 15-minute time slots (1:00 PM to 4:45 PM)
- Choose a medical reason: Consultation, Clearance, Follow Up, or Check Up
- See each calendar day's booking load (for example `5/14`); fully booked days are disabled
- Appointments must be booked at least one day in advance
- Only one active (Pending or Confirmed) appointment at a time
- View appointment history and status, and cancel pending appointments
- Dashboard showing the current active appointment

### Secretaries
- Dashboard with counts of Pending, Confirmed, and Completed appointments
- Confirm or cancel pending appointments
- Cancel confirmed appointments and search confirmed appointments by patient name
- Book walk-in appointments on behalf of a patient (an existing patient is found, or a new patient record is created)
- Patient records: search, edit, and delete (deletion is blocked if the patient has appointments)
- Monthly reports: total, completed, and cancelled appointments, with key insights (busiest month, month with the most cancellations, overall completion rate)

### Doctors
- Dashboard showing today's appointment count and upcoming confirmed appointments
- Appointment view with filters: Today (Confirmed), Today (Completed), Tomorrow (Confirmed)
- Mark appointments as completed
- Search appointments by date and patient name

### Public pages
Landing/onboarding page, About Us, About Our Doctors, Contact Us, Privacy Policy, and Terms of Service.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Front end | ASP.NET Web Forms (`.aspx`), HTML/CSS, server controls (GridView, Calendar, DropDownList) |
| Back end | C#, .NET Framework 4.7.2 |
| Database | Microsoft SQL Server / SQL Server Express (ADO.NET with `System.Data.SqlClient`, parameterized queries) |
| IDE | Visual Studio (solution: `Pulsepoint.sln`) |

---

## Project Structure

```
PulsePoint/
├── PulsePointDB.sql                   # Database schema + sample data
├── .github/workflows/                 # GitHub Pages workflows (static content only)
└── Final_PulsePoint/
    └── PulsePoint/                    # Main application
        ├── Pulsepoint.sln             # Visual Studio solution
        ├── Web.config                 # Connection string and framework settings
        ├── DBHelper.cs                # Reads the connection string
        ├── Onboarding.aspx            # Landing page
        ├── LoginRegister.aspx         # Login / registration
        ├── Patient_Dashboard.aspx     # Patient pages
        ├── Patient_Appointment.aspx
        ├── Doctor_Dashboard.aspx      # Doctor pages
        ├── Doctor_Appointment.aspx
        ├── Secretary.aspx             # Secretary pages
        ├── ManageAppointments.aspx
        ├── Reports.aspx
        └── PulsePoint/                # Earlier copy of the project (see note below)
```

> The nested `Final_PulsePoint/PulsePoint/PulsePoint/` folder is an earlier version of the project (with pages such as `BookAppointment` and `GenerateReports`). The top-level `Pulsepoint.sln` is the current application.

---

## Database

`PulsePointDB.sql` creates the `PulsePointDB` database with four tables:

| Table | Purpose |
|---|---|
| `Patients` | Patient accounts (name, contact number, email, password) |
| `Doctors` | Doctor accounts (name, specialty, email, phone, password) |
| `Secretaries` | Secretary accounts (name, email, contact number, password) |
| `Appointments` | Appointment date/time, patient, doctor, medical reason, status (`Pending`, `Confirmed`, `Completed`, `Cancelled`), queue number |

The script also inserts sample records: two patients, one doctor, one secretary, and two appointments.

---

## Getting Started

### Prerequisites
- Windows with **Visual Studio 2019 or 2022** and the **ASP.NET and web development** workload
- **.NET Framework 4.7.2** targeting pack
- **SQL Server** or **SQL Server Express**, plus SQL Server Management Studio (SSMS) or another way to run a `.sql` script

### Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/Skuuurt20/PulsePoint.git
   ```

2. **Create the database.** Open `PulsePointDB.sql` in SSMS and run it.

3. **Update the connection string.** In `Final_PulsePoint/PulsePoint/Web.config`, change the `Data Source` to your own SQL Server instance:
   ```xml
   <add name="PulsePointConnectionString"
        connectionString="Data Source=YOUR_SERVER\SQLEXPRESS;Initial Catalog=PulsePointDB;Integrated Security=True;"
        providerName="System.Data.SqlClient" />
   ```
   The committed value points to the author's machine and will not work on yours.

4. **Open and run.** Open `Final_PulsePoint/PulsePoint/Pulsepoint.sln` in Visual Studio, restore NuGet packages if prompted, and press **F5** (IIS Express). `Onboarding.aspx` is the landing page.

### Sample logins

The seeded accounts use the email and password values in `PulsePointDB.sql`:

| Role | Email |
|---|---|
| Patient | `johndoe@example.com`, `janesmith@example.com` |
| Doctor | `fabio.posas@example.com` |
| Secretary | `alice@example.com` |

Choose the matching role in the login dropdown. New patients can register from the same page; doctor and secretary accounts are created directly in the database.

---

## Known Limitations

This is a student project and not production-ready. Things to be aware of:

- **Passwords are stored and compared as plain text.** They should be hashed (for example with PBKDF2 or bcrypt) before any real use.
- **Bookings are assigned to a fixed doctor** (`DoctorId = 1`); patients cannot choose a doctor yet.
- **`QueueNumber` is stored but not yet used** to run a live queue.
- Booking hours and the 14-appointments-per-day limit are hard-coded in `Patient_Appointment.aspx.cs`.
- The `.github/workflows` files deploy the repository as static content to GitHub Pages, which cannot run an ASP.NET application. The app must be run locally or hosted on a Windows/IIS (or Azure App Service) environment.
- Build output (`bin/`) and Visual Studio files (`.vs/`) are committed in places and could be removed with a `.gitignore`.

---

## Author

**Kurt Gabriel C. Pernia**
B.S. in Computer Science (Data Science Specialization), Mapúa Malayan Colleges Laguna
[GitHub](https://github.com/Skuuurt20) · [LinkedIn](https://linkedin.com/in/kurt-pernia)
