# 🧪 OrangeHRM Quality Engineer Automation Assessment
### End-to-End Employee Lifecycle Management Test Automation Framework

![Java](https://img.shields.io/badge/Java-17%2B-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![Selenium](https://img.shields.io/badge/Selenium_WebDriver-4.20-43B02A?style=for-the-badge&logo=selenium&logoColor=white)
![TestNG](https://img.shields.io/badge/TestNG-7.10-FF7F00?style=for-the-badge&logo=testng&logoColor=white)
![REST Assured](https://img.shields.io/badge/REST_Assured-5.4-2D8CFF?style=for-the-badge&logo=postman&logoColor=white)
![ExtentReports](https://img.shields.io/badge/ExtentReports-5.1-green?style=for-the-badge)

---

## 📋 Table of Contents
1. [Project Overview](#-project-overview)
2. [End-to-End Test Scenario](#-end-to-end-test-scenario)
3. [Technology Stack & Dependencies](#-technology-stack--dependencies)
4. [Framework Architecture & Folder Structure](#-framework-architecture--folder-structure)
5. [Key Design Patterns & Best Practices](#-key-design-patterns--best-practices)
6. [Prerequisites](#-prerequisites)
7. [Setup & Installation](#-setup--installation)
8. [Executing the Tests](#-executing-the-tests)
9. [Reporting & Video Recording](#-reporting--video-recording)
10. [Submission & Git Instructions](#-submission--git-instructions)

---

## 🎯 Project Overview
This repository contains an enterprise-grade automated testing solution for the **OrangeHRM Open Source** platform (`https://opensource-demo.orangehrmlive.com/`), implementing a comprehensive **Employee Lifecycle Management** workflow.

The framework is constructed using **Java**, **Selenium WebDriver 4**, **TestNG**, **REST Assured**, and **ExtentReports 5**, demonstrating:
- Strict **Page Object Model (POM)** separation of concerns.
- **Data-Driven Testing** utilizing external JSON input.
- File upload automation for employee avatar pictures.
- Dynamic Vue.js / oxd custom dropdown manipulation and loader spinner synchronization.
- **Hybrid UI and API validation** with data consistency cross-checks.
- Automated **video recording** of test execution and interactive **HTML reporting** with embedded failure screenshots.

---

## 🧩 End-to-End Test Scenario
The automated test suite executes the complete 6-stage lifecycle:

| Stage | Action | Description & Validations |
|---|---|---|
| **1. Login** | Authentication | Login with valid credentials (`Admin` / `admin123`) and assert dashboard visibility. |
| **2. Add Employee** | Data-Driven Creation | Navigate to PIM module, inject details from `employee.json`, upload profile picture, and verify record creation. |
| **3. Edit Information** | Search & Update | Search by generated Employee ID, update Job Title and Employment Status in Job tab, and assert UI updates. |
| **4. API Validation** | UI-API Cross-Check | Query REST API (via REST Assured), assert HTTP 200/201 response, and cross-check Name/Job against UI values. |
| **5. Delete Employee** | Cleanup & Verification | Delete employee from PIM table UI, confirm dialog, verify "No Records Found" in UI, and assert deletion via API (HTTP 204). |
| **6. Logout** | Session Invalidation | Logout, verify redirect to login page, and verify direct access to protected routes is blocked. |

---

## ⚙ Technology Stack & Dependencies

| Tool / Dependency | Version | Purpose |
|---|---|---|
| **Java Development Kit (JDK)** | 17+ | Core Programming Language |
| **Selenium WebDriver** | 4.20.0 | Browser automation & W3C interaction |
| **WebDriverManager** | 5.8.0 | Automated browser driver binary management |
| **TestNG** | 7.10.0 | Test runner, assertions, and test lifecycle orchestration |
| **REST Assured** | 5.4.0 | API execution, payload verification, and UI-API cross-check |
| **ExtentReports** | 5.1.1 | Modern HTML execution reports with step-by-step logs |
| **Jackson Databind** | 2.17.0 | JSON data deserialization for data-driven testing |
| **Monte Screen Recorder** | 0.7.7.0 | Automatic test run video recording (.avi) |
| **Apache Commons IO** | 2.16.1 | File handling and screenshot persistence |

---

## 📂 Framework Architecture & Folder Structure

```
INTERVIEW/
├── pom.xml                                           # Maven dependencies & build plugins
├── testng.xml                                        # TestNG test suite configuration
├── README.md                                         # Project documentation & execution guide
├── src/
│   ├── main/
│   │   ├── java/com/orangehrm/
│   │   │   ├── base/
│   │   │   │   ├── DriverFactory.java                # Thread-safe WebDriver initialization
│   │   │   │   └── BasePage.java                     # Reusable explicit waits & Vue/oxd helpers
│   │   │   ├── pages/
│   │   │   │   ├── LoginPage.java                    # Login page actions & locators
│   │   │   │   ├── DashboardPage.java                # Dashboard navigation & logout
│   │   │   │   ├── PIMPage.java                      # Employee search, table, & delete modal
│   │   │   │   ├── AddEmployeePage.java              # Employee creation & file upload
│   │   │   │   └── EmployeeDetailsPage.java          # Job details and personal details edit
│   │   │   ├── api/
│   │   │   │   └── EmployeeApiClient.java            # REST Assured API client & cross-validation
│   │   │   ├── utils/
│   │   │   │   ├── ConfigReader.java                 # Configuration properties reader
│   │   │   │   ├── JsonDataReader.java               # Jackson JSON test data parser
│   │   │   │   ├── ScreenshotUtil.java               # Failure and step screenshot capture
│   │   │   │   ├── VideoRecorderUtil.java            # Monte Media screen recorder
│   │   │   │   └── ImageGeneratorUtil.java           # Generates test avatar if not present
│   │   │   └── listeners/
│   │   │       ├── ExtentManager.java                # ExtentReports 5 spark configuration
│   │   │       └── TestListener.java                 # TestNG listener for logging & reporting
│   │   └── resources/
│   │       ├── config.properties                     # Global configurations (URLs, timeouts)
│   │       └── testdata/
│   │           ├── employee.json                     # Data-driven employee input payload
│   │           └── avatar.png                        # Profile avatar for upload
│   └── test/
│       └── java/com/orangehrm/tests/
│           ├── BaseTest.java                         # Test setup/teardown & video hooks
│           └── EmployeeLifecycleTest.java            # Master E2E 6-step lifecycle test
└── test-output/
    ├── ExtentReport.html                             # Generated HTML Test Execution Report
    ├── screenshots/                                  # Captured failure/milestone screenshots
    └── test-recordings/                              # Captured execution video files
```

---

## 💡 Key Design Patterns & Best Practices

1. **Page Object Model (POM)**:
   - Each web page has a dedicated class encapsulating its locators and user interactions.
   - Tests remain clean, readable, and decoupled from HTML/DOM selectors.

2. **ThreadSafe Driver Architecture**:
   - `DriverFactory` utilizes `ThreadLocal<WebDriver>` to support parallel test execution without browser collision.

3. **Robust Handling of Dynamic Vue.js Elements**:
   - Explicit waits handle `.oxd-loading-spinner` transitions to prevent race conditions.
   - Custom method `selectCustomDropdown` accurately clicks and selects options from non-standard `oxd-select-wrapper` custom dropdowns.
   - Clears text inputs using keyboard chords (`Ctrl+A`, `Backspace`) to ensure Vue two-way data bindings (`v-model`) react properly.

4. **Hybrid UI + API Testing**:
   - Selenium browser cookies are synchronized into REST Assured for authenticated API calls.
   - End-to-end data consistency is validated between UI rendering and API payload responses.

---

## 💻 Prerequisites

Ensure the following are installed on your machine:
- **Java JDK 17** or higher (`java -version`)
- **Apache Maven 3.8+** (`mvn -version`)
- **Google Chrome** (or Firefox / Edge)

---

## 🚀 Setup & Installation

1. **Clone the repository**:
   ```bash
   git clone <YOUR_REPO_URL>
   cd INTERVIEW
   ```

2. **Install project dependencies**:
   ```bash
   mvn clean install -DskipTests
   ```

---

## 🏃 Executing the Tests

### Option 1: Run via Maven CLI (Default Chrome)
```bash
mvn clean test
```

### Option 2: Run with Headless Mode
```bash
mvn clean test -Dheadless=true
```

### Option 3: Run on Specific Browser (Firefox / Edge / Chrome)
```bash
mvn clean test -Dbrowser=firefox
mvn clean test -Dbrowser=edge
```

### Option 4: Run via TestNG XML directly
Right-click `testng.xml` in your IDE (IntelliJ IDEA / Eclipse) and select **Run 'testng.xml'**.

---

## 📊 Reporting & Video Recording

### 1. Interactive ExtentReports HTML Report
Upon test execution, open the generated HTML report:
```
test-output/ExtentReport.html
```
- Includes detailed step-by-step logs, execution times, status badges, environment info, and embedded Base64 screenshots on failure.

### 2. Test Execution Video Recording
Screen recordings of the execution are automatically generated under:
```
test-recordings/testEmployeeLifecycleManagement_<timestamp>.avi
```
*(You can play the video using VLC Media Player or Windows Media Player).*

---

## 📦 Submission & Git Instructions

To upload your solution to GitHub and submit:

```bash
# 1. Initialize git repository (if not already done)
git init

# 2. Add all files
git add .

# 3. Commit changes
git commit -m "feat: complete OrangeHRM employee lifecycle E2E automation framework"

# 4. Set main branch and remote URL
git branch -M main
git remote add origin https://github.com/<YOUR_GITHUB_USERNAME>/orangehrm-automation-assessment.git

# 5. Push to GitHub
git push -u origin main
```

### 📅 Deliverable & Recipient
Share the repository link (or invite as collaborator) to:
- **`shweta.george@reflectionsinfos.com`**
