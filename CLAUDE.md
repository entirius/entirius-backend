# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with the Entirius Backend codebase.

## Project Overview

This is the Entirius Backend service - a Django-based API server for the Entirius open-source e-commerce AI platform. The project follows a modular monolith architecture with Django Rest Framework for API development.

**Repository:** https://github.com/entirius/entirius-backend

## Technology Stack

- **Python 3.11+** - Primary programming language
- **Django 5.0+** - Backend framework
- **Django Rest Framework** - API development
- **PostgreSQL** - Primary database
- **OpenAPI 3.0** - API documentation and schema validation
- **Celery** - Asynchronous task processing (planned)
- **Redis** - Caching and session storage (planned)

## Development Commands

### Environment Setup
```bash
# Create and activate virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements/development.txt

# Database setup
createdb entirius_dev
python manage.py migrate
python manage.py createsuperuser
```

### Development Server
```bash
# Run development server
python manage.py runserver

# API will be available at http://localhost:8000/
# API documentation at http://localhost:8000/api/docs/
```

### Testing
```bash
# Run all tests
python manage.py test

# Run specific app tests
python manage.py test apps.products

# Run with coverage
coverage run --source='.' manage.py test
coverage report
```

### Code Quality
```bash
# Linting and formatting
flake8 .
black --check .
black .
isort .

# Type checking
mypy .
```

### Database Operations
```bash
# Create migrations
python manage.py makemigrations

# Apply migrations
python manage.py migrate

# Reset database (development only)
python manage.py flush
```

## Project Structure

- `entirius/` - Main Django project with settings and configuration
- `apps/` - Django applications organized by business domain
- `requirements/` - Environment-specific dependencies
- `docker/` - Docker configuration files
- `scripts/` - Utility scripts
- `tests/` - Test suite

## Key Modules

1. **Accounts** - User management, authentication, authorization
2. **Products** - Product Information Management (PIM)
3. **Orders** - Order processing, inventory management
4. **Config** - Dynamic configuration management
5. **AI** - Machine learning features, recommendations

## Configuration

The project uses environment-based settings:
- `settings/base.py` - Common settings
- `settings/development.py` - Development environment
- `settings/production.py` - Production environment
- `settings/testing.py` - Test environment

Environment variables are configured in `.env` file (copy from `.env.example`).

## API Structure

```
/api/v1/
├── accounts/          # User management
├── products/          # Product catalog
├── orders/            # Order processing
├── config/            # Configuration
└── ai/               # AI features
```

## Development Guidelines

### Code Standards
- Follow PEP 8 style guide
- Use type hints where appropriate
- Write comprehensive docstrings
- Maintain test coverage above 80%
- Use meaningful commit messages

### Architectural Principles
- Modular monolith architecture (ADR-001)
- Clear module boundaries with Django apps
- Well-defined interfaces between modules
- Single database with proper schema design

### Security Considerations
- JWT-based authentication
- Role-based access control (RBAC)
- API rate limiting
- CSRF protection enabled
- Secure cookie settings

## Docker Support

### Development
```bash
# Build and run with Docker Compose
docker-compose -f docker-compose.dev.yml up --build

# Run commands in container
docker-compose exec backend python manage.py migrate
docker-compose exec backend python manage.py createsuperuser
```

### Production
```bash
# Build production image
docker build -f docker/Dockerfile.prod -t entirius-backend:latest .

# Run with production compose
docker-compose -f docker-compose.prod.yml up -d
```

## Related Documentation

- Main project documentation: `/home/zolv/desktop/entirius/docs-entirius/`
- Architecture Decision Records: `/home/zolv/desktop/entirius/docs-entirius/development/architecture/adr/`
- API documentation available at runtime: `http://localhost:8000/api/docs/`

## Important Notes for Claude Code

- Always run tests after making code changes: `python manage.py test`
- Use the proper linting commands: `flake8 .` and `black --check .`
- Check type hints with: `mypy .`
- Follow Django best practices and existing code patterns
- Respect the modular monolith architecture boundaries
- When creating new endpoints, ensure proper OpenAPI documentation
- Always validate database changes with migrations: `python manage.py makemigrations`