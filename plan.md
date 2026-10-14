 # Flask Application Plan

This plan builds a small Flask application from an empty project into a tested, secure, deployable application. Complete each phase in order and keep the application runnable after every phase.

## 0. Define the application

- [X] Choose the first useful version of the application: a todo app, blog, notes app, or small API.
- [x] Write down the main users, the data they create, and the actions they need to perform.
- [X] List the first version's routes, for example:
	- `GET /` - home page
	- `GET /items` - list records
	- `GET /items/new` - show a creation form
	- `POST /items` - create a record
	- `GET /items/<id>` - show one record
	- `POST /items/<id>/delete` - delete a record
- [X] Decide which features are required for version one and which can wait.

## 1. Prepare Python and the project

- [X] Confirm Python is installed:

	```bash
	python3 --version
	```

- [X] Create and activate a virtual environment:

	```bash
	python3 -m venv .venv
	source .venv/bin/activate
	```

- [X] Install the initial dependencies:

	```bash
	python -m pip install Flask python-dotenv pytest
	```

- [X] Record dependencies:

	```bash
	python -m pip freeze > requirements.txt
	```

- [ ] Create a `.gitignore` containing `.venv/`, `__pycache__/`, `.env`, and test/cache files.
- [ ] Keep secrets in `.env`; never commit passwords, tokens, or production secret keys.

## 2. Create a clean Flask structure

Use this structure once the application grows beyond a single file:

```text
flask_application/
├── app/
│   ├── __init__.py       # application factory
│   ├── routes.py         # web routes
│   ├── models.py         # database models
│   ├── templates/        # Jinja HTML templates
│   └── static/           # CSS, JavaScript, and images
├── tests/
│   ├── conftest.py
│   └── test_routes.py
├── .env.example
├── .gitignore
├── requirements.txt
└── run.py
```

- [ ] Start with `main.py` if that is easier for learning.
- [ ] Move to an application factory (`create_app`) before adding database and authentication features.
- [ ] Keep configuration, routes, database logic, templates, and tests in separate modules.

## 3. Build and verify the minimal app

- [ ] Create a Flask application instance.
- [ ] Add a health-check route such as `GET /health` that returns a simple response or JSON.
- [ ] Run the development server:

	```bash
	flask --app main run --debug
	```

- [ ] Open the local URL in a browser.
- [ ] Verify that the health-check route returns HTTP 200.
- [ ] Turn off debug mode before any production deployment.

## 4. Learn routing and HTTP

- [ ] Add routes with `@app.route`.
- [ ] Use route parameters such as `/items/<int:item_id>`.
- [ ] Understand `GET`, `POST`, `PUT`, `PATCH`, and `DELETE`.
- [ ] Read query parameters with `request.args`.
- [ ] Read form data with `request.form`.
- [ ] Read JSON request bodies with `request.get_json()`.
- [ ] Return the correct response type: HTML, JSON, redirect, and status code.
- [ ] Handle missing resources with `404` responses.
- [ ] Use `url_for()` instead of hard-coding links between routes.

## 5. Add templates and static files

- [ ] Create a base template with shared HTML structure.
- [ ] Add page templates using Jinja inheritance.
- [ ] Pass data from route functions to templates.
- [ ] Use loops and conditional blocks in Jinja.
- [ ] Escape user-provided content by default.
- [ ] Add CSS under `static/` and reference it with `url_for('static', filename=...)`.
- [ ] Keep presentation logic in templates and application logic in Python.
- [ ] Check pages at both desktop and mobile widths.

## 6. Handle forms and validation

- [ ] Add HTML forms with the correct method and action.
- [ ] Validate required fields, lengths, formats, and numeric ranges on the server.
- [ ] Redisplay the form with clear errors when validation fails.
- [ ] Normalize input where appropriate, such as trimming surrounding whitespace.
- [ ] Never trust browser-side validation alone.
- [ ] Use Flask-WTF when the application needs reusable forms and CSRF protection.
- [ ] Redirect after a successful state-changing request to avoid duplicate submissions.
- [ ] Show one-time feedback with Flask's `flash()` messages.

## 7. Add persistent data

- [ ] Learn basic SQL: tables, rows, primary keys, foreign keys, indexes, and joins.
- [ ] Use SQLite for local development and a production database appropriate to the host.
- [ ] Add Flask-SQLAlchemy or SQLAlchemy for database access.
- [ ] Define models with explicit fields, constraints, and relationships.
- [ ] Create, read, update, and delete records through the database layer.
- [ ] Use parameterized queries or the ORM; never build SQL by concatenating user input.
- [ ] Add Flask-Migrate/Alembic for schema migrations.
- [ ] Decide how empty, missing, duplicate, and invalid records should behave.
- [ ] Add indexes for fields used frequently in lookups.
- [ ] Test database behavior with a separate test database.

## 8. Add authentication and authorization

- [ ] Store passwords only as secure hashes using a password-hashing library.
- [ ] Implement registration, login, logout, and session handling if users are required.
- [ ] Use a strong, environment-provided `SECRET_KEY`.
- [ ] Require authentication for private routes.
- [ ] Check ownership or permissions for every record-changing operation.
- [ ] Do not rely on hidden form fields or URL IDs to enforce authorization.
- [ ] Add protections for session expiration, password reset, and login errors as the application requires.

## 9. Secure the application

- [ ] Disable debug mode in production.
- [ ] Keep secrets and configuration outside source control.
- [ ] Validate and limit uploaded files, if uploads are supported.
- [ ] Add CSRF protection to browser forms.
- [ ] Use safe output escaping and review any intentionally marked-safe HTML.
- [ ] Set secure cookie options in production: `Secure`, `HttpOnly`, and an appropriate `SameSite` policy.
- [ ] Return generic authentication errors so the application does not reveal whether an account exists.
- [ ] Add rate limiting for login and other abuse-prone endpoints when needed.
- [ ] Keep Flask and dependencies updated.
- [ ] Review logs to ensure passwords, tokens, and personal data are never logged.

## 10. Add error handling and observability

- [ ] Add friendly handlers for common errors such as `404`, `400`, `403`, and `500`.
- [ ] Log useful diagnostic information without logging secrets.
- [ ] Give each request a trace or correlation ID if the application is distributed.
- [ ] Add `/health` or `/ready` checks for deployment platforms.
- [ ] Decide what users should see when the database or another service is unavailable.

## 11. Test the application

- [ ] Configure `pytest` and a Flask test client.
- [ ] Test that public routes return the expected status codes.
- [ ] Test form success, invalid input, missing input, and duplicate submission behavior.
- [ ] Test database create, read, update, and delete operations.
- [ ] Test authentication and authorization boundaries.
- [ ] Test that unauthenticated users cannot access private routes.
- [ ] Test `404`, `403`, and validation error responses.
- [ ] Run tests before every meaningful change:

	```bash
	python -m pytest
	```

- [ ] Add a small regression test whenever a bug is fixed.

## 12. Improve code quality

- [ ] Format code consistently.
- [ ] Add type hints to public functions where they improve clarity.
- [ ] Use clear names and small functions.
- [ ] Keep route functions thin; put business logic in services or helpers when it becomes non-trivial.
- [ ] Avoid global mutable state.
- [ ] Run a linter and type checker if the project needs them.
- [ ] Review dependency versions and remove unused packages.

## 13. Prepare configuration and deployment

- [ ] Create separate development, testing, and production configuration.
- [ ] Set `SECRET_KEY`, database URL, allowed hosts, and other settings through environment variables.
- [ ] Add `.env.example` with placeholder values only.
- [ ] Run database migrations during deployment.
- [ ] Use a production WSGI server instead of Flask's development server.
- [ ] Configure HTTPS, domain settings, static files, and secure cookies.
- [ ] Add backups and a recovery procedure for production data.
- [ ] Configure application logs and error monitoring.
- [ ] Document how to install, run, test, migrate, and deploy the application.
- [ ] Verify the deployed health-check route and the main user workflow.

## 14. Suggested implementation milestones

### Milestone 1: Hello Flask

- [ ] One app file
- [ ] One route
- [ ] Local development server
- [ ] A passing health-check test

### Milestone 2: A usable page

- [ ] Base template
- [ ] Static CSS
- [ ] List and detail routes
- [ ] Friendly `404` page

### Milestone 3: User input

- [ ] Create form
- [ ] Server-side validation
- [ ] Success redirect and flash message
- [ ] Tests for valid and invalid submissions

### Milestone 4: Persistence

- [ ] Database model
- [ ] CRUD operations
- [ ] Migration workflow
- [ ] Database tests

### Milestone 5: Production readiness

- [ ] Authentication and authorization, if needed
- [ ] CSRF and security configuration
- [ ] Error logging
- [ ] Production WSGI server
- [ ] Deployment documentation

## Definition of done

- [ ] A new developer can install and run the project from the README.
- [ ] The main user workflow works locally and in the deployed environment.
- [ ] Automated tests cover the important success and failure paths.
- [ ] User input is validated and database queries are safe.
- [ ] Authentication and authorization protect private data.
- [ ] Secrets are not committed to source control.
- [ ] Debug mode is disabled in production.
- [ ] Errors are logged without exposing sensitive information.
- [ ] Database changes are managed through migrations.
- [ ] The application has a documented backup and recovery approach.
