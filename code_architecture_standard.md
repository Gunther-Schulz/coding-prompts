# Code Architecture Standards & Examples

**Version: 1.0**

**ATTENTION AI ASSISTANT: This document provides specific technical guidelines, patterns, and concrete examples that complement the core principles outlined in `STANDARDS.md`. You MUST adhere to these guidelines when implementing solutions.**

---

## 1. Dependency Injection (DI)

*   **Framework:** Use `[Specify DI Framework, e.g., `injector`, `python-dependency-injector`]`.
*   **Configuration:** Centralize DI configuration in `[Specify Module, e.g., `src.core.di`]`.
*   **Injection Points:** Prefer constructor injection for mandatory dependencies. Use property/method injection for optional dependencies where appropriate. Avoid direct service locator patterns within application logic.
*   **Example (`src/core/di.py`):**
    ```python
    # Example using 'injector'
    import injector
    from src.services import UserService, AuthService
    from src.repositories import UserRepository, AuthRepository

    class AppModule(injector.Module):
        @injector.singleton
        @injector.provider
        def provide_user_repository(self) -> UserRepository:
            # Configuration for repository (e.g., DB connection) would go here
            return UserRepository(...)

        @injector.singleton
        @injector.provider
        def provide_auth_repository(self) -> AuthRepository:
            return AuthRepository(...)

        @injector.provider
        def provide_user_service(self, repo: UserRepository) -> UserService:
            return UserService(user_repository=repo)

        @injector.provider
        def provide_auth_service(self, user_repo: UserRepository, auth_repo: AuthRepository) -> AuthService:
            return AuthService(user_repository=user_repo, auth_repository=auth_repo)

    # Main application setup
    injector_instance = injector.Injector([AppModule()])

    # Usage example in another module
    # from src.core.di import injector_instance
    # user_service = injector_instance.get(UserService)
    ```

## 2. Error Handling

*   **Hierarchy:** Define a clear custom exception hierarchy inheriting from a base application exception (e.g., `AppBaseError`).
*   **Logging:** Use standard logging (`logging` module). Log exceptions with stack traces using `logger.exception()`. Log expected errors (e.g., validation errors) at `INFO` or `WARNING` level with relevant context, but without stack trace unless debugging.
*   **API Responses:** Map specific custom exceptions to standardized API error responses (e.g., `ValidationError` -> 400 Bad Request, `NotFoundError` -> 404 Not Found, `UnauthorizedError` -> 401/403). Unhandled exceptions should result in a generic 500 Internal Server Error response, logged with full details.
*   **Example (`src/core/exceptions.py`):**
    ```python
    class AppBaseError(Exception):
        """Base exception for the application."""
        pass

    class ValidationError(AppBaseError):
        """Data validation failed."""
        def __init__(self, field: str, message: str):
            self.field = field
            self.message = message
            super().__init__(f"Validation failed for '{field}': {message}")

    class NotFoundError(AppBaseError):
        """Resource not found."""
        pass

    class DatabaseError(AppBaseError):
        """Database interaction failed."""
        pass
    ```

## 3. Standard Component Structure

*   **Purpose:** To ensure consistent handling of configuration and logging across different application components (e.g., Providers, Analysers, Pipeline).
*   **Base Class:** Use `src.beat_the_books.core.base.ConfigurableComponent[ConfigT]`. This base class provides standard `self.config` and `self.logger` attributes.
*   **Configuration (`config/`)**:
    *   Configuration structures are defined using Pydantic models in `src.beat_the_books.config.types.py`.
    *   Configuration is loaded and validated from external sources (e.g., YAML files) using logic in `src.beat_the_books.config.loader.py`.
*   **Logging (`logging_config.py`)**:
    *   The `src.beat_the_books.logging_config.setup_logging` function configures the global `loguru` logger based on the `LoggingConfig` section of the loaded application configuration.
    *   The configured logger instance is retrieved using `src.beat_the_books.logging_config.get_logger`.
*   **Initialization Pattern:** Components inheriting from `ConfigurableComponent` should receive their specific configuration slice (a Pydantic model instance, e.g., `ProviderConfig`) and the pre-configured logger instance via dependency injection (typically constructor injection).
*   **Example (`src/beat_the_books/providers/some_provider.py` - illustrative):**
    ```python
    import loguru
    from beat_the_books.core.base import ConfigurableComponent
    from beat_the_books.config.types import ProviderConfig # Specific config slice
    from beat_the_books.domain.models import ProviderGame # Example domain model

    class SomeProvider(ConfigurableComponent[ProviderConfig]):
        """Example provider adhering to the standard structure."""

        def __init__(
            self,
            config: ProviderConfig, # Specific config injected
            logger: 'loguru.Logger', # Configured logger injected
            # ... other dependencies like http_client, repository injected ...
        ):
            # Call super or directly assign - ensure self.config and self.logger are set
            # super().__init__(config=config, logger=logger) # Option 1
            self.config = config # Option 2 (Direct assignment)
            self.logger = logger # Option 2 (Direct assignment)
            # self.http_client = http_client # Store other dependencies

            self.logger.info(f"Initialized SomeProvider with base URL: {self.config.base_url}")

        async def fetch_data(self) -> list[ProviderGame]:
            """Fetches data using config and logs messages."""
            self.logger.debug(f"Fetching data from {self.config.base_url}...")
            # ... implementation using self.config, self.logger, other dependencies ...
            return []

    # Instantiation (typically handled by DI container based on config):
    # from beat_the_books.config.loader import load_app_config
    # from beat_the_books.logging_config import setup_logging, get_logger

    # app_config = load_app_config()
    # setup_logging(app_config.logging)
    # configured_logger = get_logger()

    # # DI container would do this automatically based on AppModule configuration:
    # provider_instance = SomeProvider(
    #     config=app_config.providers['some_provider'], # Pass the specific provider's config
    #     logger=configured_logger,
    #     # http_client=injector_instance.get(HttpClient), # etc.
    # )
    ```

## 4. Data Modeling (ORM Example - SQLAlchemy)

*   **Base Model:** Define a declarative base (`Base = declarative_base()`) for all models.
*   **Relationships:** Clearly define relationships (`relationship()`) with appropriate `back_populates` or `backref` settings. Specify lazy loading strategies explicitly (`lazy='selectin'`, `lazy='joined'`) where needed to avoid N+1 query problems.
*   **Typing:** Use type hints (`Mapped`, `mapped_column`) for all model attributes.
*   **Example (`src/models/user.py`):**
    ```python
    from sqlalchemy.orm import declarative_base, relationship, Mapped, mapped_column
    from sqlalchemy import String, Integer, ForeignKey
    from typing import List

    Base = declarative_base()

    class User(Base):
        __tablename__ = 'users'

        id: Mapped[int] = mapped_column(primary_key=True)
        username: Mapped[str] = mapped_column(String(50), unique=True, index=True)
        email: Mapped[str] = mapped_column(String(100), unique=True)

        # Example relationship
        posts: Mapped[List["Post"]] = relationship(back_populates="author", lazy="selectin")

    class Post(Base):
         __tablename__ = 'posts'

         id: Mapped[int] = mapped_column(primary_key=True)
         title: Mapped[str] = mapped_column(String(100))
         user_id: Mapped[int] = mapped_column(ForeignKey('users.id'))

         author: Mapped["User"] = relationship(back_populates="posts")
    ```
