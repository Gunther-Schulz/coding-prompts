# Project Architecture

**Version:** 1.0
**Last Updated:** YYYY-MM-DD

## 1. Introduction

This document describes the high-level architecture, key technical patterns, directory structure, and major components of the `beat-the-books` project. It complements `PROJECT_STANDARDS.md`, which covers general coding principles and style.

## 2. High-Level Architecture

The project follows a layered architecture pattern, separating concerns into distinct layers:

*   **Domain Layer:** Contains the core business logic, entities (models), and interfaces defining repository contracts. It has no dependencies on other layers.
*   **Application Layer:** Orchestrates use cases, coordinating domain objects and repository interfaces to perform tasks. Contains application services and DTOs (Data Transfer Objects) if needed.
*   **Infrastructure Layer:** Implements interfaces defined in the domain or application layers. Handles external concerns like database interactions, API clients, caching, and configuration loading.
*   **Presentation Layer (CLI):** Provides the user interface (Command Line Interface in this case). Interacts with the Application Layer.

```mermaid
graph TD
    CLI --> AppLayer[Application Layer];
    AppLayer --> DomainLayer[Domain Layer];
    AppLayer --> DomainInterfaces[Domain Interfaces];
    InfraLayer[Infrastructure Layer] -- Implements --> DomainInterfaces;
    InfraLayer --> External[External Systems (DB, APIs)];

    subgraph Presentation
        CLI
    end

    subgraph Application
        AppLayer
    end

    subgraph Domain
        DomainLayer
        DomainInterfaces
    end

    subgraph Infrastructure
        InfraLayer
    end
```

## 3. Key Design Patterns

*   **Dependency Injection (DI):**
    *   Used extensively to decouple components and manage dependencies.
    *   We use the `dependency-injector` library.
    *   Containers are defined in `src/beat_the_books/containers.py` (or similar module).
    *   Configuration typically involves wiring interfaces (e.g., `IGameRepository`) to their concrete implementations (e.g., `SqlAlchemyGameRepository`).
    *   Components (services, repositories) declare their dependencies in `__init__` and are injected by the container.
*   **Repository Pattern:**
    *   Abstracts data persistence logic.
    *   Repository interfaces are defined in the Domain Layer (e.g., `src/beat_the_books/domain/interfaces/repositories.py`).
    *   Concrete implementations reside in the Infrastructure Layer (e.g., `src/beat_the_books/storage/implementations/sqlalchemy/repositories.py`).
    *   Application services interact only with the repository interfaces.
*   **Unit of Work (UoW) Pattern:**
    *   Manages atomic operations involving multiple repository actions (typically related to database transactions).
    *   The interface `IUnitOfWork` might be defined in the Application or Domain layer.
    *   The concrete implementation (e.g., `SqlAlchemyUnitOfWork`) resides in the Infrastructure Layer, managing the lifecycle of a SQLAlchemy session and transaction.
*   **Configuration Management:**
    *   Uses a combination of YAML files (e.g., `config/config.yaml`) and environment variables.
    *   Pydantic models are used to parse and validate configuration structure (e.g., in `src/beat_the_books/config/models.py`).
    *   Configuration is loaded and made available via the DI container.

## 4. Directory Structure (`src/beat_the_books/`)

```
src/beat_the_books/
├── __init__.py
├── analyser/             # Analysis logic, strategies
├── application/          # Application services, interfaces (e.g., UoW)
│   └── interfaces/
├── cli/                  # Command Line Interface (Typer commands)
│   └── commands/
├── config/               # Configuration loading, models, logging setup
├── containers.py         # Dependency Injection container definitions
├── core/                 # Core utilities, shared components
├── discovery/            # Service/component discovery mechanisms
├── domain/               # Core domain logic
│   ├── __init__.py
│   ├── interfaces/       # Domain interfaces (e.g., Repositories)
│   └── models/           # Domain entities (Pydantic models)
├── providers/            # Data provider implementations (API clients)
│   ├── __init__.py
│   ├── clients/          # Low-level API client wrappers
│   └── implementations/  # Concrete provider logic adapting clients
├── storage/              # Data persistence implementations
│   ├── __init__.py
│   └── implementations/  # Concrete repository/UoW implementations
│       └── sqlalchemy/
└── utils/                # General utility functions
```

*   **Separation:** Structure reflects the layered architecture.
*   **Modularity:** Components are organized into logical packages.
*   **Testing:** The `tests/` directory mirrors this structure.

## 5. Key Libraries & Frameworks

*   **CLI:** `Typer` (built on `Click`) - For building the command-line interface.
*   **DI Container:** `dependency-injector` - For managing dependency injection.
*   **Data Validation/Models:** `Pydantic` - Used for domain models, configuration models, API data validation.
*   **ORM:** `SQLAlchemy` (Core and ORM) - For database interactions.
*   **Async HTTP Client:** `aiohttp` - Preferred for making asynchronous API calls within providers.
*   **Logging:** `Loguru` - For application logging.
*   **Testing:** `pytest`, `pytest-asyncio` - For writing and running tests.

## 6. Component Interaction Examples

### Example: Fetching Games via CLI

1.  **CLI Command (`cli/commands/games.py`):** User runs `beat-the-books games list --provider pinnacle`.
2.  **Typer:** Invokes the corresponding command function.
3.  **DI Container:** The command function gets the `GameService` injected.
    ```python
    # cli/commands/games.py (Simplified)
    import typer
    from dependency_injector.wiring import inject, Provide
    from beat_the_books.containers import Container
    from beat_the_books.application.services import GameService

    app = typer.Typer()

    @app.command()
    @inject
    def list_games(
        provider_name: str,
        game_service: GameService = Provide[Container.game_service]
    ):
        games = game_service.get_games_from_provider(provider_name)
        # ... display games ...
    ```
4.  **GameService (`application/services.py`):**
    *   Uses the injected `ProviderFactory` (or similar mechanism) to get the correct `IGameProvider` implementation based on `provider_name`.
    *   Calls `provider.fetch_upcoming_games()`.
    *   Might interact with `IGameRepository` via `IUnitOfWork` to store/update fetched games.
    ```python
    # application/services.py (Simplified)
    class GameService:
        def __init__(self, provider_factory: IProviderFactory, uow: IUnitOfWork):
            self._provider_factory = provider_factory
            self._uow = uow

        def get_games_from_provider(self, provider_name: str) -> list[Game]:
            provider = self._provider_factory.get_provider(provider_name)
            external_games = provider.fetch_upcoming_games() # Returns provider-specific data
            # ... mapping/processing logic ...
            domain_games = map_to_domain_models(external_games)
            with self._uow:
                for game in domain_games:
                    self._uow.games.add_or_update(game)
                self._uow.commit()
            return domain_games
    ```
5.  **Provider Implementation (`providers/implementations/pinnacle/provider.py`):**
    *   Uses its injected `PinnacleApiClient`.
    *   Calls the client to fetch data from the external API.
    *   Parses the response.
6.  **UnitOfWork (`storage/implementations/sqlalchemy/uow.py`):**
    *   Manages the SQLAlchemy session and transaction.
    *   Provides access to specific repository implementations (e.g., `uow.games` which is an instance of `SqlAlchemyGameRepository`).

## 7. Error Handling Strategy

*   **Provider Errors:** API clients or provider implementations should catch low-level HTTP/network errors and raise specific domain-level exceptions (e.g., `ProviderApiError`, `ProviderRateLimitError`) defined in `providers/exceptions.py`.
*   **Application Errors:** Services catch provider exceptions or validation errors and may log them or wrap them in application-specific exceptions if needed for the presentation layer.
*   **Validation Errors:** Pydantic validation errors are typically handled at the boundaries (API parsing, config loading, CLI input) and result in specific `ValidationError` exceptions or user-friendly error messages.
*   **Database Errors:** Repository implementations catch `SQLAlchemy` exceptions and may raise specific `RepositoryError` exceptions.

## 8. Future Considerations

*   [Placeholder for planned architectural changes, e.g., adding caching, message queues, etc.]
