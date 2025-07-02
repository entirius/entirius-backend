# Entirius Backend

Backend API service for Entirius - an open-source e-commerce AI platform built with Django and Django Rest Framework.

**Repository:** https://github.com/entirius/entirius-backend

## Overview

This service provides the core backend functionality for the Entirius e-commerce platform, implementing a modular monolith architecture with comprehensive API endpoints for product management, order processing, user management, and AI-powered features.

## Technology Stack

- **Python 3.11+**
- **Django 5.0+** - Backend API framework
- **Django Rest Framework** - API development
- **PostgreSQL** - Primary database
- **OpenAPI 3.0** - API documentation and schema validation
- **Celery** - Asynchronous task processing (planned)
- **Redis** - Caching and session storage (planned)

## Project Structure

```
entirius-backend/
├── entirius/                 # Main Django project
│   ├── settings/            # Environment-specific settings
│   ├── urls.py             # URL routing
│   └── wsgi.py             # WSGI configuration
├── requirements/            # Dependencies
│   ├── base.txt           # Common dependencies
│   ├── development.txt    # Development dependencies
│   └── production.txt     # Production dependencies
├── docker/                 # Docker configuration
├── scripts/                # Utility scripts
└── tests/                  # Test suite
```

## Quick Start

### Prerequisites

- Python 3.11 or higher
- PostgreSQL 13+
- Git

### Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/entirius/entirius-backend.git
   cd entirius-backend
   ```

2. **Create virtual environment**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements/development.txt
   ```

4. **Environment configuration**
   ```bash
   cp .env.example .env
   # Edit .env with your configuration
   ```

5. **Database setup**
   ```bash
   # Create PostgreSQL database
   createdb entirius_dev
   
   # Run migrations
   python manage.py migrate
   
   # Create superuser
   python manage.py createsuperuser
   ```

6. **Run development server**
   ```bash
   python manage.py runserver
   ```

The API will be available at `http://localhost:8000/`

## Configuration

### Environment Variables

Create a `.env` file in the project root:

```env
# Database
DATABASE_URL=postgresql://user:password@localhost:5432/entirius_dev

# Django
SECRET_KEY=your-secret-key-here
DEBUG=True
ALLOWED_HOSTS=localhost,127.0.0.1

# API Documentation
SWAGGER_SETTINGS_ENABLED=True

# External Services (optional)
REDIS_URL=redis://localhost:6379/0
CELERY_BROKER_URL=redis://localhost:6379/0
```

### Settings Structure

The project uses environment-based settings:

- `settings/base.py` - Common settings
- `settings/development.py` - Development environment
- `settings/production.py` - Production environment
- `settings/testing.py` - Test environment

## API Documentation

The API is documented using OpenAPI 3.0 specification and is available at:

- **Development**: `http://localhost:8000/api/docs/`
- **API Schema**: `http://localhost:8000/api/schema/`

### Key API Endpoints

```
/api/v1/
├── accounts/          # User management
├── products/          # Product catalog
├── orders/            # Order processing
├── config/            # Configuration
└── ai/               # AI features
```

## Development

### Running Tests

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
# Linting
flake8 .
black --check .

# Format code
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

## Docker Support

### Development with Docker

```bash
# Build and run
docker-compose -f docker-compose.dev.yml up --build

# Run commands in container
docker-compose exec backend python manage.py migrate
docker-compose exec backend python manage.py createsuperuser
```

### Production Deployment

```bash
# Build production image
docker build -f docker/Dockerfile.prod -t entirius-backend:latest .

# Run with docker-compose
docker-compose -f docker-compose.prod.yml up -d
```

## Architecture

### Modular Monolith

The backend follows a modular monolith architecture ([ADR-001](../../../docs-entirius/development/architecture/adr/adr-001-modular-monolith-architecture)) with:

- **Clear module boundaries** - Each Django app represents a business domain
- **Shared database** - Single PostgreSQL instance with proper schema design
- **Internal APIs** - Well-defined interfaces between modules
- **Independent deployment** - Single deployable unit

### Key Modules

1. **Accounts** - User management, authentication, authorization
2. **Products** - Product Information Management (PIM)
3. **Orders** - Order processing, inventory management
4. **Config** - Dynamic configuration management
5. **AI** - Machine learning features, recommendations

## Contributing

### Development Workflow

1. Create feature branch from `main`
2. Make changes following coding standards
3. Write/update tests
4. Run quality checks
5. Submit pull request

### Coding Standards

- Follow PEP 8 style guide
- Use type hints where appropriate
- Write comprehensive docstrings
- Maintain test coverage above 80%
- Use meaningful commit messages

### Commit Message Format

```
type(scope): description

[optional body]

[optional footer]
```

Types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`

## Deployment

### Environment Setup

1. **Staging**: Mirrors production configuration
2. **Production**: Optimized for performance and security

### Key Considerations

- Use environment variables for all configuration
- Enable proper logging and monitoring
- Configure database connection pooling
- Set up proper backup strategies
- Use HTTPS in production

## Monitoring & Logging

### Health Checks

- **Health endpoint**: `/health/`
- **Database connectivity**: Included in health checks
- **External service status**: Redis, external APIs

### Logging

- Structured logging with JSON format
- Log levels: DEBUG, INFO, WARNING, ERROR, CRITICAL
- Request/response logging for API calls
- Performance metrics logging

## Security

### Authentication & Authorization

- JWT-based authentication
- Role-based access control (RBAC)
- API rate limiting
- CORS configuration

### Security Headers

- CSRF protection enabled
- Secure cookie settings
- Content Security Policy
- Security middleware configured

## Support

### Documentation

- [Main Documentation](../../../docs-entirius/)
- [API Reference](../../../docs-entirius/api-reference/)
- [Architecture Decisions](../../../docs-entirius/development/architecture/adr/)

### Getting Help

- Create issue in GitHub repository
- Check existing documentation
- Review architectural decision records

## License

This project is licensed under the Mozilla Public License 2.0 - see the [LICENSE](../../LICENSE) file for details.
