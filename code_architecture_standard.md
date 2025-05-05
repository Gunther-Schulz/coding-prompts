# Code Architecture Standards & Examples

**Version: 1.0**

**ATTENTION AI ASSISTANT: This document provides specific technical guidelines, patterns, and concrete examples that complement the core principles outlined in `STANDARDS.md`. You MUST adhere to these guidelines when implementing solutions.**

---

## 1. Dependency Injection (DI)

*   **Framework:** Use `dependency-injector`.
*   **Configuration:** Centralize DI container definition and configuration in `src/beat_the_books/containers.py`.
*   **Injection Points:** Prefer constructor injection for mandatory dependencies. Use property/method injection for optional dependencies where appropriate. Resolve top-level components (like services or command handlers) from the container.
*   **Example (`src/beat_the_books/containers.py` - illustrative):**
    ```python
    # Example using 'dependency-injector'
    from dependency_injector import containers, providers

    # Import necessary components (assuming these exist)
    from beat_the_books.config.loader import load_app_config
    from beat_the_books.logging_config import setup_logging, get_logger
    from beat_the_books.storage.implementations.sqlalchemy import SQLAlchemyRepository
    from beat_the_books.providers.clients import HttpClient
    from beat_the_books.providers.implementations.pinnacle import PinnacleProvider, PinnacleClient
    from beat_the_books.resolution import ResolutionService
    # ... other imports

    class Container(containers.DeclarativeContainer):
        # Configuration Loading (typically done once)
        config = providers.Singleton(load_app_config)

        # Logging Setup (depends on config)
        logger = providers.Singleton(
            lambda cfg: setup_logging(cfg.logging) or get_logger(),
            cfg=config
        )

        # Core Services/Clients (example)
        repository = providers.Singleton(
            SQLAlchemyRepository,
            db_url=config.provided.storage.database_url,
            logger=logger,
        )
        http_client = providers.Singleton(
            HttpClient,
            retry_config=config.provided.http_client.retry_policy,
            logger=logger,
            repository=repository, # Example: passing repo for debug saving
        )

        # Provider Example (Pinnacle)
        pinnacle_client = providers.Factory(
            PinnacleClient,
            config=config.provided.providers.pinnacle, # Pass specific provider config
            http_client=http_client,
            logger=logger,
        )
        pinnacle_provider = providers.Factory(
            PinnacleProvider,
            config=config.provided.providers.pinnacle,
            client=pinnacle_client,
            # ... other mapper/converter dependencies ...
            logger=logger,
            provider_name=providers.Object("pinnacle"), # Example provider name
            repository=repository,
            # ... discovery_service ...
        )

        # Resolution Service Example
        resolution_service = providers.Factory(
            ResolutionService,
            repository=repository,
            # ... discovery_service ...
            logger=logger,
            config=config.provided.resolution,
            reference_provider=config.provided.pipeline.reference_provider,
        )

        # Other providers, services, coordinators...

    # Main application setup (e.g., in cli.py or main.py)
    # container = Container()
    # container.wire(modules=[ ... modules needing injection ... ])

    # Usage example (after wiring or direct resolution)
    # resolved_provider = container.pinnacle_provider()
    ```

## 2. Error Handling

*   **Hierarchy:** Define a clear custom exception hierarchy inheriting from a base application exception (`AppBaseError`) located in `src/beat_the_books/errors.py`.
*   **Logging:** Use `loguru` (configured via `src/beat_the_books/logging_config.py`). Log exceptions with stack traces using `logger.exception()`. Log expected errors (e.g., validation errors) at `INFO` or `WARNING` level with relevant context, but without stack trace unless debugging.
*   **API Responses:** N/A (Project is primarily CLI/Pipeline). For CLI errors, use appropriate exit codes and print informative messages to stderr via the logger. Unhandled exceptions should result in a non-zero exit code, logged with full details.
*   **Example (`src/beat_the_books/errors.py` - illustrative, assuming `AppBaseError` exists):**
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

*   **Location:** Define SQLAlchemy ORM models in `src/beat_the_books/storage/implementations/sqlalchemy/models.py`.
*   **Base Model:** Define a declarative base (`Base = declarative_base()`) for all models within that module.
*   **Relationships:** Clearly define relationships (`relationship()`) with appropriate `back_populates` or `backref` settings. Specify lazy loading strategies explicitly (`lazy='selectin'`, `lazy='joined'`) where needed to avoid N+1 query problems.
*   **Typing:** Use type hints (`Mapped`, `mapped_column`) for all model attributes.
*   **Example (`src/beat_the_books/storage/implementations/sqlalchemy/models.py` - illustrative snippet):**
    ```python
    from sqlalchemy.orm import declarative_base, relationship, Mapped, mapped_column
    from sqlalchemy import String, Integer, ForeignKey, DateTime, Float, Enum as SQLEnum
    from typing import List, Optional
    import enum # Assuming python enum is used for domain enums

    # Assuming Domain Enums are defined elsewhere, e.g., beat_the_books.domain.types
    from beat_the_books.domain.types import GameStatus, SportType, PeriodType

    Base = declarative_base()

    class OrmGame(Base):
        __tablename__ = 'games'

        game_id: Mapped[int] = mapped_column(primary_key=True)
        # Canonical entity foreign keys
        home_team_id: Mapped[Optional[int]] = mapped_column(ForeignKey('canonical_teams.id'))
        away_team_id: Mapped[Optional[int]] = mapped_column(ForeignKey('canonical_teams.id'))
        league_id: Mapped[Optional[int]] = mapped_column(ForeignKey('canonical_leagues.id'))

        sport: Mapped[SportType] = mapped_column(SQLEnum(SportType))
        start_time: Mapped[Optional[datetime]] = mapped_column(DateTime(timezone=True))
        status: Mapped[Optional[GameStatus]] = mapped_column(SQLEnum(GameStatus))
        # Provider specific details might be stored separately or in related tables

        # Example Relationships (adjust as per actual schema)
        home_team: Mapped[Optional["OrmCanonicalTeam"]] = relationship(foreign_keys=[home_team_id])
        away_team: Mapped[Optional["OrmCanonicalTeam"]] = relationship(foreign_keys=[away_team_id])
        league: Mapped[Optional["OrmCanonicalLeague"]] = relationship()
        markets: Mapped[List["OrmMarket"]] = relationship(back_populates="game", cascade="all, delete-orphan")

    class OrmMarket(Base):
        __tablename__ = 'markets'
        # ... columns for market_id, game_id (FK to OrmGame), market_type, period, etc. ...
        market_id: Mapped[int] = mapped_column(primary_key=True)
        game_id: Mapped[int] = mapped_column(ForeignKey('games.game_id'))
        period: Mapped[PeriodType] = mapped_column(SQLEnum(PeriodType))
        # ... other fields ...

        game: Mapped["OrmGame"] = relationship(back_populates="markets")
        outcomes: Mapped[List["OrmOutcome"]] = relationship(back_populates="market", cascade="all, delete-orphan")

    # ... other models like OrmOutcome, OrmCanonicalTeam, OrmCanonicalLeague, OrmProviderXMap etc.
    ```

## 5. CLI Design

*   **Framework:** Use `Typer` for defining CLI commands and arguments.
*   **Structure:** Organize commands logically, potentially using subcommands for different functional areas (e.g., `fetch`, `analyze`, `resolve`). Define entry points in `src/beat_the_books/cli/main.py` or similar.
*   **Validation:** Use Pydantic models for complex command parameters requiring validation, passed via `typer.Argument` or `typer.Option` with appropriate type hints.
*   **Dependency Injection:** Resolve necessary services (e.g., `Pipeline`, `Coordinator`) from the main `dependency-injector` container at the beginning of command functions. Avoid using `typer.Depends` unless specifically justified for very simple, reusable dependencies.
*   **Output:** Use `rich` for formatted console output (tables, progress bars, colored text) where appropriate, leveraging `Typer`'s integration or direct `rich` usage via the logger or console object.
*   **Example (`src/beat_the_books/cli/commands/analyze.py` - illustrative):**
    ```python
    import typer
    from typing_extensions import Annotated
    from typing import List, Optional

    # Assume container is initialized and wired elsewhere (e.g., cli/main.py)
    from beat_the_books.containers import Container
    from beat_the_books.analyser.coordinator import Coordinator # For type hint

    # Create a Typer app for analysis commands
    app = typer.Typer(help="Run analysis strategies on market data.")

    @app.command("run")
    def run_analysis(
        ctx: typer.Context,
        strategies: Annotated[Optional[List[str]], typer.Option(help="Analysis strategies to run (e.g., comparison arbitrage). Runs defaults if None.")] = None,
        output_format: Annotated[str, typer.Option(help="Output format (e.g., console, json, excel).")] = "console",
        output_file: Annotated[Optional[str], typer.Option(help="File path to save the report.")] = None,
        # ... other options like providers, runtime overrides ...
    ):
        \"\"\"
        Runs specified analysis strategies on available matched game data.
        \"\"\"
        container: Container = ctx.obj["container"] # Get container from context
        coordinator: Coordinator = container.analysis_coordinator() # Resolve coordinator
        logger = container.logger() # Resolve logger

        logger.info(f"Running analysis with strategies: {strategies or 'defaults'}...")

        try:
            # Use coordinator to run analysis
            analysis_results = coordinator.run_analysis(
                strategies=strategies,
                # runtime_overrides=...
            )

            # Format and output/save results
            formatted_output = coordinator.format_result(
                analysis_results, format_name=output_format
            )

            if output_file:
                coordinator.save_result(
                    analysis_results, filename=output_file, format_name=output_format
                )
                logger.info(f"Results saved to {output_file}")
            else:
                # Print to console (assuming format_result returns printable object/string)
                # Might use rich directly here or have coordinator handle it
                print(formatted_output)

        except Exception as e:
            logger.exception("Analysis failed!")
            raise typer.Exit(code=1)

        logger.info("Analysis complete.")

    # Add other analysis subcommands if needed
    ```

## 6. Code Style & Formatting

*   **Tooling:** Use `Ruff` for linting, formatting, and import sorting.
*   **Configuration:** Configure `Ruff` via the `[tool.ruff]` section in `pyproject.toml`. Ensure configuration aligns with common standards (e.g., using Black compatibility for formatting if desired).
*   **Imports:** Group imports as standardized by `Ruff`'s import sorter (e.g., standard library, third-party, first-party). Use absolute imports where possible.

---
*This document provides project-specific standards. Refer to `STANDARDS.md` for general principles.*
