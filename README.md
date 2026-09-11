# 🧪 OrangeHRM Quality Engineer Automation Assessment
### UI, API & Performance Test Automation Framework

![Java](https://img.shields.io/badge/Java-17%2B-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![Selenium](https://img.shields.io/badge/Selenium_WebDriver-4.20-43B02A?style=for-the-badge&logo=selenium&logoColor=white)
![TestNG](https://img.shields.io/badge/TestNG-7.10-FF7F00?style=for-the-badge&logo=testng&logoColor=white)
![REST Assured](https://img.shields.io/badge/REST_Assured-5.4-2D8CFF?style=for-the-badge&logo=postman&logoColor=white)
![JMeter](https://img.shields.io/badge/Apache_JMeter-5.6-D22128?style=for-the-badge&logo=apachejmeter&logoColor=white)
![k6](https://img.shields.io/badge/Grafana_k6-Performance-7D64FF?style=for-the-badge&logo=k6&logoColor=white)
![ExtentReports](https://img.shields.io/badge/ExtentReports-5.1-green?style=for-the-badge)

---

## 📋 Table of Contents
1. [Project Overview](#-project-overview)
2. [End-to-End Test Scenario](#-end-to-end-test-scenario)
3. [Performance Testing (JMeter & k6)](#-performance-testing-jmeter--k6)
4. [Technology Stack & Dependencies](#-technology-stack--dependencies)
5. [Framework Architecture & Folder Structure](#-framework-architecture--folder-structure)
6. [Key Design Patterns & Best Practices](#-key-design-patterns--best-practices)
7. [Prerequisites](#-prerequisites)
8. [Setup & Installation](#-setup--installation)
9. [Executing the Tests](#-executing-the-tests)
10. [Reporting & Video Recording](#-reporting--video-recording)
11. [Submission & Git Instructions](#-submission--git-instructions)

---

## 🎯 Project Overview
This repository contains a comprehensive QA automation framework for the **OrangeHRM Open Source** platform (`https://opensource-demo.orangehrmlive.com/`), covering both **Functional UI/API Lifecycle Automation** and **Performance & Load Testing**.

The framework combines:
- **Java 17 + Selenium WebDriver 4 + TestNG**: Strict Page Object Model (POM), data-driven inputs, custom Vue.js dropdown handling, loader synchronization, and session management.
- **REST Assured**: API authentication via synchronized WebDriver session cookies, data consistency cross-checks, and lifecycle verification.
- **Apache JMeter & Grafana k6**: Multi-user load testing simulating concurrent employee lifecycle flows, measuring throughput, response times, and error rates against SLAs.
- **Reporting & Video**: ExtentReports 5 Spark HTML reports, automated screen recordings, and JMeter HTML performance dashboards.

---

## 🧩 End-to-End Test Scenario

| Stage | Action | Validations |
|---|---|---|
| **1. Login** | Authentication | Valid credentials (`Admin` / `admin123`) & verify Dashboard header visibility. |
| **2. Add Employee** | Data-Driven Creation | Reads from `employee.json`, creates employee with custom/generated ID, uploads avatar photo, and verifies record creation. |
| **3. Edit Information** | Search & Update | Searches by Employee ID, updates Job Title & Employment Status in Job tab, and verifies changes. |
| **4. API Validation** | UI-API Cross-Check | Synchronizes browser cookies into REST Assured, performs API operations, and cross-checks UI data vs API data. |
| **5. Delete Employee** | Cleanup & Verification | Deletes employee from UI, confirms modal, verifies "No Records Found" in UI, and asserts HTTP 204 via API. |
| **6. Logout** | Session Invalidation | Logs out, verifies redirect to login page, and ensures protected dashboard routes redirect back to login. |

---

## ⚡ Performance Testing (JMeter & k6)

Located in [`performance-tests/`](file:///C:/Users/HP/Desktop/Shankar-%20coding%20test/performance-tests):

### 1. Apache JMeter Test Plan (`orangehrm_performance_test.jmx`)
A complete, parameterized JMeter test plan measuring:
- `01_Navigate_To_Login_Page` (GET login HTML, extract CSRF token)
- `02_Submit_Login_Credentials` (POST credentials, assert 200/302)
- `03_View_Dashboard` (GET dashboard HTML)
- `04_Search_PIM_Employee_API` (GET `/web/index.php/api/v2/pim/employees`, assert JSON `$.data`)
- `05_User_Logout` (GET logout, assert session termination)

#### Run JMeter Non-GUI with HTML Dashboard:
```bash
jmeter -n -t performance-tests/orangehrm_performance_test.jmx \
       -l performance-tests/results.jtl \
       -e -o performance-tests/html-report/
```

### 2. Grafana k6 Performance Script (`k6_orangehrm_test.js`)
Modern JavaScript load test script with automated ramp-up stages and SLA thresholds (`p(95) < 3000ms`, `error rate < 5%`):

```bash
k6 run performance-tests/k6_orangehrm_test.js
```

---

## ⚙ Technology Stack & Dependencies

| Tool / Dependency | Version | Purpose |
|---|---|---|
| **Java Development Kit (JDK)** | 17+ | Core Language |
| **Selenium WebDriver** | 4.20.0 | Browser automation |
| **WebDriverManager** | 5.8.0 | Driver binary management |
| **TestNG** | 7.10.0 | Test runner & assertions |
| **REST Assured** | 5.4.0 | API validation & cross-checks |
| **Apache JMeter** | 5.6.3 | Enterprise load & performance testing |
| **Grafana k6** | Modern | Cloud-native performance scripting |
| **ExtentReports** | 5.1.1 | Interactive HTML test reports |
| **Jackson Databind** | 2.17.0 | JSON data-driven testing |
| **Monte Screen Recorder** | 0.7.7.0 | Native execution video recording |

---

## 📂 Framework Architecture & Folder Structure

```
Shankar- coding test/
├── pom.xml                                           # Maven dependencies & build configuration
├── testng.xml                                        # TestNG test runner suite XML
├── README.md                                         # Main documentation
├── performance-tests/                                # Performance testing suite
│   ├── orangehrm_performance_test.jmx                # Apache JMeter test plan
│   ├── k6_orangehrm_test.js                          # Grafana k6 performance script
│   └── README.md                                     # Dedicated performance test guide
├── src/
│   ├── main/
│   │   ├── java/com/orangehrm/
│   │   │   ├── base/
│   │   │   │   ├── DriverFactory.java                # ThreadSafe WebDriver initialization
│   │   │   │   └── BasePage.java                     # Explicit waits & Vue/oxd helpers
│   │   │   ├── pages/
│   │   │   │   ├── LoginPage.java                    # Login page locators & actions
│   │   │   │   ├── DashboardPage.java                # Dashboard navigation & logout
│   │   │   │   ├── PIMPage.java                      # Employee search, table, & delete
│   │   │   │   ├── AddEmployeePage.java              # Employee creation & file upload
│   │   │   │   └── EmployeeDetailsPage.java          # Job and Personal details edit
│   │   │   ├── api/
│   │   │   │   └── EmployeeApiClient.java            # REST Assured client & cross-validation
│   │   │   ├── utils/
│   │   │   │   ├── ConfigReader.java                 # Configuration reader
│   │   │   │   ├── JsonDataReader.java               # Jackson JSON test data reader
│   │   │   │   ├── ScreenshotUtil.java               # Screenshot capture (Base64/File)
│   │   │   │   ├── VideoRecorderUtil.java            # Monte Media video recorder
│   │   │   │   └── ImageGeneratorUtil.java           # Auto-generates test avatar
│   │   │   └── listeners/
│   │   │       ├── ExtentManager.java                # ExtentReports 5 configuration
│   │   │       └── TestListener.java                 # TestNG lifecycle & report hooks
│   │   └── resources/
│   │       ├── config.properties                     # Environment configurations
│   │       └── testdata/
│   │           ├── employee.json                     # Data-driven employee input
│   │           └── avatar.png                        # Avatar image asset
│   └── test/
│       └── java/com/orangehrm/tests/
│           ├── BaseTest.java                         # Driver setup, teardown & video hooks
│           └── EmployeeLifecycleTest.java            # Complete 6-step E2E lifecycle test
└── test-output/
    ├── ExtentReport.html                             # Interactive HTML execution report
    ├── screenshots/                                  # Captured failure screenshots
    └── test-recordings/                              # Captured video recordings (.avi)
```

---

## 🏃 Executing the Tests

### Functional UI & API Automation:
```bash
# Run full suite via Maven
mvn clean test

# Run in headless mode
mvn clean test -Dheadless=true

# Cross-browser execution
mvn clean test -Dbrowser=firefox
mvn clean test -Dbrowser=edge
```

### Performance Load Testing:
```bash
# JMeter (Non-GUI with HTML dashboard)
jmeter -n -t performance-tests/orangehrm_performance_test.jmx -l performance-tests/results.jtl -e -o performance-tests/html-report/

# k6
k6 run performance-tests/k6_orangehrm_test.js
```

---

## 📊 Reports & Artifacts

- **Functional HTML Report**: `test-output/ExtentReport.html`
- **Execution Video**: `test-recordings/testEmployeeLifecycleManagement_<timestamp>.avi`
- **JMeter HTML Dashboard**: `performance-tests/html-report/index.html`

---

## 📦 Submission & Git Instructions

```bash
git init
git add .
git commit -m "feat: complete OrangeHRM UI, API, and Performance testing framework"
git branch -M main
git remote add origin https://github.com/<YOUR_GITHUB_USERNAME>/orangehrm-automation-assessment.git
git push -u origin main
```
*Share repository access with: **`shweta.george@reflectionsinfos.com`**.*
