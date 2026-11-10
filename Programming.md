# Programming Style Guide

**Project:** Car Rental Platform  
**Technology:** PHP and Laravel 13  
**Nature:** Mandatory technical standard

> This document exclusively governs code style, standardization, and internal documentation. Every provision is mandatory.

---

## 1. Laravel Pint and Base Standard

### 1.1 What Is Laravel Pint?

**Laravel Pint** is the official code style fixer included in Laravel projects. It automatically analyzes and formats PHP code according to an established coding standard. In this project, Pint enforces the **Laravel preset**, which is based on **PSR-12** and Laravel's official formatting conventions.

Pint corrects formatting issues such as indentation, spacing, line breaks, brace placement, import organization, and other style inconsistencies. Pint does not replace testing, validation, code review, or architectural review; it only standardizes code format.

### 1.2 Where Must Laravel Pint Be Used?

- Pint must be executed from the **root directory of the Laravel project**, where the `artisan`, `composer.json`, and `vendor` directory are located.
- Pint must be used on all PHP code covered by the project, including controllers, models, Form Requests, migrations, factories, seeders, middleware, services, helpers, traits, routes, tests, and configuration files.
- Pint must be executed locally by every developer before every commit.
- The command must not be executed from inside `app`, `routes`, `resources`, or any other subdirectory.

Example of the correct location:

```text
safety-rental/
├── app/
├── routes/
├── vendor/
├── artisan
└── composer.json
```

```bash
cd safety-rental
./vendor/bin/pint
```

### 1.3 How Must Laravel Pint Be Used?

- The base standard is **PSR-12**, using **4 spaces** for indentation. Tabs must not be used for indentation.
- **Laravel Pint** must use its default `laravel` preset. No different preset may be adopted without the coordinator's authorization.
- Before making **any commit**, every developer must run Pint from the project root:

```bash
./vendor/bin/pint
```

- The command automatically analyzes and fixes the applicable PHP files.
- After Pint finishes, the developer must review the files changed by Pint and verify the project again before creating the commit.
- To check formatting without modifying files, the following command may be used:

```bash
./vendor/bin/pint --test
```

- The `--test` command is only a verification option. It does not replace the mandatory execution of `./vendor/bin/pint` before a commit when formatting corrections are required.
- If the `vendor` directory does not exist, project dependencies must first be installed from the project root:

```bash
composer install
```

- A commit must not be created while Pint reports or leaves formatting issues unresolved.

### 1.4 Base Language Rule

- All code, comments, and technical documentation must be written in **English**. This includes PHPDoc, class names, variables, properties, methods, parameters, and internal keys.
- **Project code must never be written in Spanish**.

---

## 2. Naming Conventions

| Element | Convention | Rule | Valid examples |
|---|---|---|---|
| Classes and models | `PascalCase` | Singular | `Car`, `Reserve`, `Location` |
| Methods | `camelCase` | Descriptive English name | `calculateRentalCost()` |
| Database tables | `snake_case` | Plural | `cars`, `reserves`, `rental_locations` |
| Columns | `snake_case` | Descriptive English name | `license_plate`, `start_date` |
| URI routes | `kebab-case` | Lowercase segments | `/admin/vehicle-catalog` |
| Branches | `type/task-name` | Lowercase type and task; kebab-case task | `feature/admin-panel` |

### Additional Rules

Names must clearly communicate their purpose. Ambiguous abbreviations, context-free generic names, and mixed Spanish-English identifiers are prohibited.

---

## 3. Services, REST APIs, and Containerization Architecture

### 3.1 Laravel API Controllers: Structure and Naming
- All API-oriented controllers must reside strictly in the `app/Http/Controllers/Api/*` directory.
- The `Api` suffix must be explicitly added to every API controller class name (e.g., `ProductApiController`, `CarApiController`) to clearly separate them from conventional web controllers.

### 3.2 Routing Design and Scope
- All REST API routes and service endpoints must be registered exclusively in `routes/api.php` and adhere strictly to stateless communication.
- **Strict Prohibition:** Business logic, query handling, or data transformations must never be placed inside route files. All routing files must merely delegate requests to their corresponding controllers.

### 3.3 Data Transformation and Formatting (API Resources)
- Raw Eloquent models must never be returned directly in API responses.
- Developers must use **API Resources** (`JsonResource`) as dedicated transformers to define the exact JSON payload, filter out sensitive or internal model attributes, and format calculated attributes.
- Use **Resource Collections** (`ResourceCollection`) whenever serving lists or paginated datasets requiring extra metadata alongside the collection.

### 3.4 Decoupled Frontend/Backend Architecture (SPA)
- In decoupled single-page application architectures (e.g., Vue.js), endpoint consumption must be centralized and encapsulated within dedicated service classes (`Service`).
- Do not implement traditional OOP getters and setters inside the frontend client layer.

### 3.5 System Architecture and Evolution
- Design the application architecture to start as a well-structured monolith that can smoothly evolve into independent services or microservices (and conversely revert to a monolith) without needing a full system rewrite.
- Every microservice must remain focused on a single business responsibility and guarantee autonomous deployment (avoiding release-coupling or cross-deployment dependencies with other services).

### 3.6 Containerization and Data Persistence (Docker)
- Design and manage all containerized environments under the principle of treating infrastructure as disposable and easily replaceable ("cattle, not pets").
- Use lightweight, minimal base images (such as `alpine` or `slim` variants) to optimize image size, build times, and runtime performance.
- **Data Persistence Rule:** Volatile or application-critical state (databases, user-uploaded files, cache directories) must never be stored inside the container's writable layer. Always bind this data to external Docker volumes or dedicated managed cloud storage services.

---


## Compliance

No personal preference, delivery urgency, or difference among team members authorizes an exception to this guide.
