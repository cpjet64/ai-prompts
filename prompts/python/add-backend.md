---
description: Coding Standards & Rules for Python Backend Frameworks 2025 
globs: **/*.py 
alwaysApply: false
---

# Python Backend Development (2025)

## General Best Practices (All Frameworks)

- **Project Structure & Modularity**: Organize code by component (routing, business logic, data models). Avoid monolithic files – for example, separate routes/controllers, models, and utility modules. Follow framework conventions for project layout (Django apps, Flask blueprints, FastAPI routers) to make the project intuitive to navigate.

- **Asynchronous Safety**: Where using an async-capable framework, prefer async functions and libraries to prevent blocking the event loop. For instance, use asynchronous database clients or HTTP libraries in FastAPI/Starlette endpoints. Never perform long blocking operations in request handlers; delegate those to background tasks or worker threads/processes (Celery, RQ) if needed.
 
- **Configuration & Secrets**: Keep settings (database URLs, API keys, etc.) out of code. Use config files or environment variables and the framework's configuration system to manage these. For example, use Django's settings module with environment variables, or Flask's app.config.from_envvar. This ensures sensitive data isn't hard-coded and can vary per environment.

- **Input Validation & Security**: Leverage framework features to validate and sanitize inputs. All frameworks (Django Forms/serializers, Pydantic models in FastAPI, etc.) offer ways to ensure data is well-formed. Use them rather than writing custom validation for common cases. Always encode or escape output in templates to prevent XSS, and use built-in protections (Django's CSRF middleware, Flask-WTF for forms with CSRF token, etc.).

- **Logging & Error Handling**: Implement structured logging of requests and errors. Use the framework's logging configuration to include important context (timestamp, request ID, user) in logs. Handle exceptions gracefully: return appropriate HTTP error codes or JSON error responses rather than crashing. For example, use Django's error views or FastAPI's exception handlers to catch and format errors. Avoid overly-generic exception catching that could hide issues.

- **Testing & Maintainability**: Write unit and integration tests for critical pieces (views, model methods, API endpoints). Use each framework's testing utilities (Django's TestCase, Flask's test client, FastAPI's TestClient) to simulate requests and ensure correct behavior. Keep functions small and single-purpose to facilitate testing and comprehension.

## Django (5.x)

- **Latest Django Features**: Use Django's newest capabilities (as of 5.x) for better performance and clarity. For example, take advantage of asynchronous views and ORM support introduced in Django 4+ when dealing with IO-bound tasks. If an endpoint is heavy on DB or external calls, consider making it an `async def` view and running under an ASGI server (Daphne/Uvicorn). Ensure any ORM usage in async views uses Django's async ORM features or runs in the thread pool (Django will do this for you if using the ORM in async context).

- **Project Structure**: Stick to the Django app structure. Separate your project into logical apps, and keep each app focused. Use Django's default project layout (with manage.py, a project settings package, and multiple reusable apps). Don't put all code in views.py – use models, forms/serializers, and utility modules to organize logic. Keep urls.py clean by including URLs from each app (using `include(...)`).

- **Configuration**: Utilize settings modules for different environments (development, production, etc.). For instance, maintain a settings.py base and override specifics in settings_dev.py, settings_prod.py, etc., or use environment-specific patterns. Use Django's DEBUG flag properly – never leave DEBUG=True in production. Configure allowed hosts, security middleware (XSS, Content Security Policy via django-csp, etc.) and logging as recommended by Django's deployment checklist.

- **Database Migrations & Models**: Always create migrations for model changes and review them into version control. Follow Django's best practices for models: use Django's ORM methods instead of raw SQL when possible (to benefit from protection against SQL injection and easy migrations). For complex queries, use the QuerySet API (annotate, aggregate) or database functions provided by Django. If you need raw SQL for performance, keep it parameterized and within Django's connection.cursor() or RawSQL utilities.

- **Templates & Static Files**: When rendering HTML, use Django's template engine safely – it autoescapes by default, so prefer it over building HTML manually. Store templates in the proper templates/ directories of apps or the project, and static assets in static/ directories, using Django's static file finders. For front-end integration (if using React/Vue separately, or PyScript), consider Django's built-in tags (like `{% static %}`) to reference assets or JSON script data to pass server-side data safely to client scripts.

- **APIs on Django**: For building APIs, prefer using Django REST Framework or Django Ninja (see below) rather than hand-rolling JSON responses, to leverage robust parsing/validation and auth. If the project is lightweight, Django's built-in JsonResponse is fine for simple cases, but ensure to set safe=False when returning lists and use Django's JSON encoder for date/time if needed.

## Flask (2.x)

- **Application Factory & Blueprints**: Use the application factory pattern to create your Flask app (especially for larger projects) instead of a single module global app. This means having a function `create_app(config_name)` that configures and returns the app. Register Blueprints for different parts of the application (e.g., auth, blog, api) to modularize routes and avoid one huge app.py. This structure improves maintainability and testing (you can create an app instance for tests with different configs).

- **Configuration**: Manage configuration via a config object or file. Use classes or dictionaries for config, and load them with app.config.from_object or from_envvar. Keep secrets and environment-specific settings (DB URI, secret keys) in environment variables and load them, rather than hardcoding in code. Leverage Flask's built-in config system to handle different environments (DevelopmentConfig, ProductionConfig classes, etc.), and never expose secret keys or credentials in your repository.

- **Request Handling**: Use Flask's request lifecycle properly. Access request data via flask.request (for JSON, use request.get_json()) and never trust client input—validate it. Prefer Flask's url_for() to build URLs for redirects or API locations rather than string concatenation, to avoid errors and make refactoring routes easier. When rendering templates, pass only necessary data to avoid exposing server internals.

- **Extensions & Security**: Use Flask extensions to handle common tasks: e.g., Flask-Login for authentication management, Flask-Migrate for database migrations (with SQLAlchemy), Flask-WTF for form handling with CSRF protection, etc. These provide battle-tested implementations. Always enable CSRF protection for forms (Flask-WTF auto handles this). If building an API, consider an extension like Flask-RESTX or Flask API, or even using FastAPI for new projects requiring many API features.

- **Async in Flask**: Flask 2.x has limited support for async routes (you can define async def routes, but a production WSGI server won't truly execute them concurrently without an ASGI server). If you need to handle high-concurrency or long-lived streaming connections, Flask might not be ideal. For simple async needs, you can use an ASGI server (Hypercorn, Gunicorn with uvicorn workers) to run Flask and get some benefit, but for substantial async workloads consider using an async framework. Keep this in mind and don't misuse async in Flask – if used, ensure all called code is also non-blocking.

- **Testing**: Utilize Flask's test client to simulate requests to your routes. Structure your app to allow injecting test configuration (such as an in-memory database) and avoid global state that would carry over between tests. This ensures your Flask app is easier to test and follows the 12-factor app principles.

## FastAPI (>=0.95+ / 2025)

- **Structure with Routers**: Split your FastAPI application using APIRouter for different functional areas (for example, users_router, items_router). Mount these on the main app. This keeps route declarations organized and allows mounting sub-applications or prefixing routes easily. Also factor out business logic from route functions – keep endpoint functions thin (parsing input and calling service functions), which makes them easier to test.

- **Pydantic Models & Validation**: Use Pydantic models (updated for Pydantic v2 in 2025) for request bodies, response models, and settings. Define a Settings Pydantic model to load environment variables (possibly via Pydantic's BaseSettings) for config. This ensures type-checked configuration and eliminates many parsing errors. Use response_model in your path operations to enforce and document the schema of responses. This way, FastAPI will automatically filter out any extraneous fields and ensure the response conforms to the model.

- **Dependency Injection**: Leverage FastAPI's Depends for shared logic like database sessions, authentication, or common parameters. For example, use Depends(get_db) to inject a database session into endpoints instead of creating one manually in each route. This promotes reusability and centralizes setup/teardown (like closing DB connections). Keep your dependency functions fast – heavy computations should be avoided in dependency resolution to keep overall request latency low.

- **Asynchronous Performance**: Write path functions as async (`async def`) whenever they perform IO or use awaitable calls (database, HTTP requests, etc.), and use await inside them for those operations. Ensure to call asynchronous database libraries (e.g., with async SQLAlchemy or Motor for Mongo) so you fully benefit from non-blocking IO. If you call any standard blocking code (like a pandas operation or a blocking HTTP call), be aware it will block the event loop; offload such work to thread pools or background tasks if it's CPU-heavy or long-running.

- **Middleware & Security**: Use FastAPI/Starlette middleware for cross-cutting concerns (authentication, CORS, rate limiting). For example, include the CORSMiddleware to handle cross-origin requests if your frontend is separate. If certain endpoints need protection, use OAuth2PasswordBearer or other dependencies for auth and ensure to check credentials in a dependency or use a library like fastapi-users. Add rate limiting or request size limiting via middleware or dependencies to prevent abuse. Also take advantage of FastAPI's automated docs – document your endpoints with summaries, descriptions, and include example responses where helpful to make the OpenAPI spec useful for consumers.

- **Docs and Validation**: In FastAPI, the automatically generated docs (Swagger UI) are a strength – keep them accurate. Use tags to group endpoints and dependencies to inject standardized error handling. Validate data thoroughly with Pydantic – e.g., use regex validators for fields like email, custom root validators for inter-field conditions, etc. Also handle errors with HTTPException or custom exception handlers so that the client receives meaningful error responses (and the docs can reflect possible error status codes).

## Starlette (0.2x)

- **Lightweight Design**: Starlette is a lightweight ASGI toolkit. Use it for smaller services or as a foundation. Structure code by separating ASGI app initialization, routes, and background tasks. Because Starlette doesn't impose a specific project layout, establish one (for example, a routes.py or package, a middlewares.py, etc., to organize code logically).

- **Routing & Endpoints**: Use Starlette's Route and Mount to define endpoints. You can organize routes in lists and then apply them to the app via `app = Starlette(routes=[...])`. For larger projects, consider breaking out routes by concern and mounting sub-apps or using Starlette's Router include patterns. Keep endpoint callables small and delegate complex logic to helper functions or services, similar to other frameworks.

- **Middleware & Event Handlers**: Take advantage of Starlette's middleware for adding functionality like CORS, authentication, or sessions. Because Starlette doesn't come with batteries included, you might need to bring your own libraries (for auth, etc.), but use Starlette's middleware interface to integrate them. Also implement startup and shutdown events (with `@app.on_event("startup")`) for initializing resources (like DB connections, thread pools) and closing them on shutdown. This ensures your app handles resource lifecycles cleanly.

- **Async Usage**: Starlette is fully async, so write your request handlers as async functions. If you perform background work, use asyncio.create_task or Starlette's BackgroundTask for one-off tasks triggered by a request (like sending an email after responding). Ensure any database or network operations use async libraries or are offloaded, similarly to FastAPI guidelines, to keep the server responsive under load.

- **Error Handling**: Use Starlette's exception handling to manage errors globally. You can add exception handlers on the app (e.g., for HTTPException or generic Exception) to return JSON errors or custom HTML error pages as needed. This avoids duplicating try/except in every route. Also utilize Starlette's built-in features like requests and websockets handling – for websockets, ensure to handle accept, receive, and send properly within an async loop, and close connections gracefully on app shutdown.

## Django Ninja (1.x)

- **Integration with Django**: Django Ninja is an API framework on top of Django. Keep using Django's fundamental patterns (models, forms, admin) for core logic, and use Ninja for the API layer. Organize your Ninja API in its own module or app (e.g., an api app) separate from your traditional Django views, to distinguish API routes from web routes.

- **Type Hints & Pydantic**: Leverage Django Ninja's support for Python type hints and Pydantic models. Define schema models using Pydantic for your request and response bodies; Ninja (with Pydantic v2 in version 1.x) will handle validation and documentation. This provides a FastAPI-like developer experience within Django. Ensure your Pydantic models are kept in sync with Django models or serializers for consistency. Use Schema subclasses or django-ninja provided utilities to create schemas that can .from_orm() your Django models when needed (Ninja supports from_orm for converting ORM objects to schema).

- **Views and Routing**: Use the @api.get, @api.post, etc., decorators provided by Ninja to define endpoints, and group them logically in API router instances. Just like Django's URL patterns or FastAPI routers, Django Ninja allows grouping endpoints by tags or by router instances. Use this to separate concerns (e.g., a set of user-related API endpoints in one router). Mount the Ninja API under a specific URL prefix (e.g., /api/) in your Django urls.py so it doesn't conflict with other routes.

- **Async Support**: Django Ninja 1.x supports async view functions fully. If your Django project is running with ASGI (Django 4+ with an ASGI server), you can define async API functions (and even depend on async Django ORM calls if using Django's async capabilities). Use this for IO-bound API calls (like calling external services or handling high-concurrency requests). If the Django ORM or other parts are still sync-only for your use case, be mindful that calling them in an async view will run in a thread pool (which is fine, but avoid long CPU-bound work).

- **Auth & Security**: Reuse Django's authentication and permission system in Ninja views. Ninja provides easy ways to hook Django auth (e.g., AuthBearer or using Django's User in dependencies) – use these rather than writing custom auth for the API. Also enable CSRF protection for browser-facing endpoints; Ninja can integrate with Django's CSRF middleware when using cookie-based auth. If building an open API for third-party, consider using token or OAuth2 authentication and ensure to disable Django's CSRF for those endpoints if tokens are used. Document your API with Ninja's auto-generated OpenAPI docs, and use response models and status codes in decorators to keep the docs accurate.

- **Performance**: Ninja is quite fast; however, keep using Django best practices to maintain performance. Use select_related/prefetch_related in Django ORM to avoid N+1 queries when your API returns related data. Paginate list endpoints rather than returning huge querysets in one go, possibly using Ninja's pagination classes or manual slicing. Since Ninja is tightly integrated with Django, continue to leverage Django's caching (per-view or low-level caching) for expensive API calls when appropriate.