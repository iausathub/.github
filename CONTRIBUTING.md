# Developer Guidelines

These guidelines aim to establish consistency and clarity across all SatHub projects while allowing developers the flexibility to work effectively. 
They focus on maintaining code quality, ensuring reliable functionality, and facilitating collaboration.

---

## Core Python Guidelines

### Code Style & Standards
- **Follow PEP 8 Style Guide**: Code should adhere to [PEP 8](https://peps.python.org/pep-0008/), Python's official style guide. This ensures a consistent look and feel across all Python code.
- **Use Type Hints**: All function parameters and return values must include type hints. This improves readability and helps tools like linters and IDEs catch potential issues.
  - Example:
    ```python
    def add(a: int, b: int) -> int:
        return a + b
    ```
- **Use `ruff` for Linting**: Use `ruff` as the standard linter to enforce coding standards. Ensure no linter errors remain before committing code. If specific rules need exceptions, they must be documented.
- **Use `ruff` or `black` for Formatting**: Both tools work similarly to enforce formatting standards: ruff is preferred over black just to simplify things, but either is good.

### General Standards
- **Consistent Naming Conventions**:
  - Use `snake_case` for variables and functions.
  - Use `PascalCase` for class names.
  - Prefix private methods with an underscore (e.g., `_helper_method`).
- **Descriptive Variable Names**: It should be reasonably obvious between a variable or function name and context to determine it's purpose; avoid abbreviations, acronyms, or single character variable names unless there is common/established meaning.
- **Avoid Code Duplication**: Refactor repeated code into reusable functions or modules.
- **Write Readable Code**: Prioritize clear, concise, and descriptive code over clever but opaque solutions.

---

## Django-Specific Guidelines

### Model Best Practices
- **Use Model Managers**: Use model managers for complex queries instead of repeating query logic.
- **Define `__str__` Methods**: Implement `__str__` methods for all models to improve readability in logs and the admin interface.
- **Use `related_name`**: Add `related_name` to foreign keys for clearer reverse relationships.
- **Keep Models Thin**: Move business logic to services or utility modules to maintain clear separation of concerns.

### View Organization
- **Focus on Request/Response**: Keep views simple and move business logic to services or utilities.
- **Use Django Forms**: Leverage Django forms for validation instead of manual validation.
- **Use `get_object_or_404`**: Replace manual `try/except` blocks for object lookups with `get_object_or_404`.

### URL Patterns
- **Use Named URL Patterns**: Use consistent naming for URL patterns.
- **Include `app_name`**: Define `app_name` in `urls.py` for proper namespacing.
- **Prefer `path()`**: Use `path()` instead of `url()`.
- **Keep URLs RESTful**: Follow RESTful conventions where applicable.

### Template Structure
- **Use Template Inheritance**: Build templates with `base.html` as a foundation.
- **Keep Template Logic Minimal**: Use custom template tags or filters for complex operations.
- **Use `{% static %}`**: Serve static files using the `{% static %}` template tag.
- **Use `{% url %}`**: Dynamically generate URLs with the `{% url %}` tag instead of hardcoding.

### Forms
- **Use `ModelForm`**: When working with models, prefer `ModelForm` for simplicity.
- **Define `clean_` Methods**: Add `clean_` methods for field-specific validation.
- **Leverage Form Mixins**: Use mixins for shared form functionality.

### Testing
- **Use `pytest-django`**: Adopt `pytest-django` for testing.
- **Leverage Factories**: Use libraries like `factory_boy` for creating test data.
- **Test Views**: Use Django’s test client to test views.
- **Limit Fixtures**: Use factories over fixtures to maintain test clarity and flexibility.

### Settings
- **Environment Variables**: Store sensitive data in environment variables.
- **Split Settings**: Organize settings into `base.py`, `development.py`, and `production.py`.
- **Use `django-environ`**: Manage environment variables with `django-environ`.

### Security
- **Enable CSRF Protection**: Avoid disabling CSRF protection (`@csrf_exempt`) unless absolutely necessary.
- **Use Built-in Protections**: Leverage Django’s default XSS and SQL injection protections.
- **Disable DEBUG in Production**: Always set `DEBUG=False` in production.
- **Secure Sessions**: Use secure session settings, such as `SESSION_COOKIE_SECURE`.

### Database
- **Use Migrations**: Always include migrations for schema changes.
- **Make Migrations Atomic**: Ensure migrations are atomic to avoid partial updates.
- **Index Fields**: Use `db_index=True` for frequently filtered fields.
- **Optimize Queries**: Use `select_related()` and `prefetch_related()` to reduce N+1 query issues.

### Admin Interface
- **Customize for Usability**: Use `list_display`, `list_filter`, and `search_fields` to improve the admin interface.
- **Register Models**: Ensure models are registered with `admin.site.register()`.

### Celery Tasks
- **Use `shared_task`**: Mark tasks with the `@shared_task` decorator for app-agnostic usage.
- **Keep Tasks Simple**: Focus tasks on a single responsibility.
- **Handle Failures Gracefully**: Include error handling and retries where appropriate.

---

## API-Specific Guidelines

### General Practices
- **Framework Agnosticism**: These guidelines apply regardless of the specific API framework used (e.g., Flask, FastAPI, Django REST Framework).
- **Serialization and Validation**: Always validate input and serialize output for consistency and security.
- **Consistent Naming**: Follow consistent naming conventions for endpoints and resources.
- **RESTful Design**: Adhere to REST principles where applicable (e.g., use proper HTTP methods and status codes).

### Documentation
- **Endpoint Documentation**: Include clear documentation for all API endpoints, describing input parameters, expected outputs, and error conditions.
- **Examples**: Provide sample requests and responses to help developers understand usage.
- **Generated API Docs**: Generate API documentation using tools like Sphinx/ReadTheDocs, OpenAPI, or Swagger.

### Testing
- **Test Endpoints**: Write comprehensive tests for all API endpoints to ensure proper behavior.
- **Mock External Dependencies**: Use mocking to simulate external service behavior during tests.
- **Maintain Coverage**: Ensure test coverage exceeds 80%, focusing on edge cases and error handling.

### Performance
- **Optimize Query Usage**: Minimize database queries for endpoints, especially in high-traffic APIs.
- **Optimize Database/Table Design and Queries**: If data set size (and specific performance benchmarks) are a concern, such as with SatChecker, consider:
  -  Testing new queries separately from the related feature
  -  Creating/maintaining relevant database indexes
  -  Optimizing individual queries - using SQLAlchemy vs. SQL can occasionally differ in how the query is created and whether it is actually the most optimal version
  -  Minimize unnecessary joins and ensure proper join conditions for optimal query performance.
- **Caching**: Implement caching for expensive operations or frequently accessed endpoints.
- **Profiling**: Use tools to profile API performance and resolve bottlenecks.

---

