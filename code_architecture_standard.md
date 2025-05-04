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

## 3. Data Modeling (ORM Example - SQLAlchemy)

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

## 4. API Design (RESTful Example - FastAPI)

*   **Structure:** Use FastAPI routers (`APIRouter`) to organize endpoints by resource/feature.
*   **Input/Output Models:** Use Pydantic models for request bodies, query parameters, and response models to ensure validation and clear contracts.
*   **Status Codes:** Use appropriate HTTP status codes (200, 201, 204, 400, 401, 403, 404, 500).
*   **Dependency Injection:** Use FastAPI's dependency injection (`Depends`) to inject services/repositories into route functions.
*   **Example (`src/api/v1/users.py`):**
    ```python
    from fastapi import APIRouter, Depends, HTTPException, status
    from pydantic import BaseModel, EmailStr
    from typing import List

    from src.services import UserService
    # Assume get_user_service is a dependency provider function using DI framework
    from src.core.dependencies import get_user_service
    from src.core.exceptions import NotFoundError, ValidationError as AppValidationError

    router = APIRouter(prefix="/users", tags=["Users"])

    # --- Pydantic Models ---
    class UserCreate(BaseModel):
        username: str
        email: EmailStr
        password: str # Example only - handle password securely

    class UserResponse(BaseModel):
        id: int
        username: str
        email: EmailStr

        class Config:
            orm_mode = True # Or from_attributes = True for Pydantic v2

    # --- Routes ---
    @router.post("/", response_model=UserResponse, status_code=status.HTTP_201_CREATED)
    async def create_user(
        user_in: UserCreate,
        user_service: UserService = Depends(get_user_service)
    ):
        try:
            # Assume create_user method handles hashing password etc.
            created_user = await user_service.create_user(user_in.dict())
            return UserResponse.from_orm(created_user)
        except AppValidationError as e:
            raise HTTPException(status_code=status.HTTP_400_BAD_REQUEST, detail=str(e))
        except Exception as e: # Catch broader exceptions for logging/generic error
             # logger.exception("Failed to create user") # Add logging
             raise HTTPException(status_code=status.HTTP_500_INTERNAL_SERVER_ERROR, detail="Internal server error")

    @router.get("/{user_id}", response_model=UserResponse)
    async def get_user(
        user_id: int,
        user_service: UserService = Depends(get_user_service)
    ):
        try:
            user = await user_service.get_user_by_id(user_id)
            return UserResponse.from_orm(user)
        except NotFoundError:
            raise HTTPException(status_code=status.HTTP_404_NOT_FOUND, detail="User not found")
        except Exception as e:
             # logger.exception(f"Failed to get user {user_id}") # Add logging
             raise HTTPException(status_code=status.HTTP_500_INTERNAL_SERVER_ERROR, detail="Internal server error")

    # Add other endpoints (list, update, delete) following similar patterns
    ```

## 5. Code Style & Formatting

*   **Formatter:** Use `[Specify Formatter, e.g., Black]`.
*   **Linter:** Use `[Specify Linter, e.g., Ruff, Flake8 with plugins]`.
*   **Configuration:** Store configuration in `pyproject.toml`.
*   **Imports:** Use `[Specify Import Sorter, e.g., isort, Ruff's import sorter]`. Group imports (standard library, third-party, first-party).

---
*This document is illustrative. Real projects should tailor these sections with specific choices, configurations, and more detailed examples relevant to their stack and requirements.*
