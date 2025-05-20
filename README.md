# Hyperf Skeleton Project

This is a skeleton application using the [Hyperf framework](https://hyperf.wiki/), designed as a starting point for developers looking to build high-performance PHP applications with a microservices architecture. Hyperf leverages PHP coroutines for efficient, scalable web applications and APIs.

## Table of Contents
- [Introduction](#introduction)
- [Requirements](#requirements)
- [Installation](#installation)
- [Project Structure](#project-structure)
- [Configuration](#configuration)
- [Running the Application](#running-the-application)
- [Available Components](#available-components)
- [Testing](#testing)
- [Contributing](#contributing)
- [License](#license)
- [Community](#community)

## Introduction
This skeleton application provides a pre-configured Hyperf project to help developers quickly get started with building modern PHP applications. It includes essential components and configurations for database access, message queues, RPC services, and more, making it ideal for microservices and high-performance APIs.

## Requirements
Hyperf requires specific system environments and PHP extensions. It runs natively on Linux or Mac but can also be used on Windows via Docker.

### System Requirements
- **Operating System**: Linux, Mac, or Windows with Docker
- **PHP**: >= 8.1
- **Network Engines** (one of the following):
  - Swoole PHP extension >= 5.0 (set `swoole.use_shortname = Off` in `php.ini`)
  - Swow PHP extension >= 1.3
- **Required PHP Extensions**:
  - JSON
  - Pcntl
  - OpenSSL (for HTTPS)
  - PDO (for MySQL client)
  - Redis (for Redis client)
  - Protobuf (for gRPC server or client)
- **Optional Dependencies** (based on selected components):
  - MySQL (for `hyperf/database`)
  - Redis (for `hyperf/redis` and `hyperf/async-queue`)
  - RabbitMQ (for `hyperf/amqp`)
  - Elasticsearch (for `hyperf/elasticsearch`)

For Docker-based environments, use the official [hyperf/hyperf](https://hub.docker.com/r/hyperf/hyperf) image or refer to the [hyperf/hyperf-docker](https://github.com/hyperf/hyperf-docker) repository for Dockerfile examples.

## Installation
Follow these steps to set up the project:

1. **Create the Project**:
   Use Composer to create a new Hyperf project:
   ```bash
   composer create-project hyperf/hyperf-skeleton hyperf-skeleton
   ```
   For Docker-based environments, use:
   ```bash
   docker run --rm -it -v $(pwd):/app composer create-project --ignore-platform-reqs hyperf/hyperf-skeleton hyperf-skeleton
   ```

2. **Navigate to the Project Directory**:
   ```bash
   cd hyperf-skeleton
   ```

3. **Configuration Setup**:
   During installation, the following components were selected:
   - **Timezone**: Default PHP timezone
   - **Database**: MySQL client (`hyperf/database`, `hyperf/db-connection`)
   - **Redis**: Enabled (`hyperf/redis`)
   - **RPC Protocol**: JSON RPC (`hyperf/json-rpc`, `hyperf/rpc`, `hyperf/rpc-client`, `hyperf/rpc-server`)
   - **Config Center**: None
   - **Constants**: Enabled (`hyperf/constants`)
   - **Async Queue**: Enabled (`hyperf/async-queue`)
   - **AMQP**: Enabled (`hyperf/amqp`)
   - **Model Cache**: Enabled (`hyperf/model-cache`)
   - **Elasticsearch**: Enabled (`hyperf/elasticsearch`)
   - **Tracer**: Enabled (`hyperf/tracer` for Zipkin compatibility)

   A `.env` file is created from `.env.example`. Update it with your environment-specific settings (e.g., database, Redis, AMQP credentials).

4. **Install Dependencies**:
   Dependencies are installed during project creation. To update or add dependencies, run:
   ```bash
   composer install
   ```

## Project Structure
The project follows the standard Hyperf directory structure:

```
.
├── app
│   ├── Constants
│   │   └── ErrorCode.php             # Error code definitions
│   ├── Controller
│   │   ├── AbstractController.php    # Base controller
│   │   └── IndexController.php       # Sample controller
│   ├── Exception
│   │   ├── BusinessException.php     # Custom business exception
│   │   └── Handler
│   │       └── AppExceptionHandler.php # Exception handler
│   ├── Listener
│   │   ├── DbQueryExecutedListener.php # Database query listener
│   │   ├── QueueHandleListener.php    # Async queue listener
│   │   └── ResumeExitCoordinatorListener.php # Coroutine coordinator
│   ├── Model
│   │   └── Model.php                 # Base model with caching
│   └── Process
│       └── AsyncQueueConsumer.php    # Async queue consumer process
├── bin
│   └── hyperf.php                    # Entry point for Hyperf CLI
├── config
│   ├── autoload
│   │   ├── amqp.php                 # AMQP configuration
│   │   ├── async_queue.php          # Async queue configuration
│   │   ├── databases.php            # Database configuration
│   │   ├── redis.php                # Redis configuration
│   │   ├── services.php             # RPC services configuration
│   │   ├── opentracing.php          # Tracer configuration
│   │   └── ...                      # Other auto-loaded configs
│   ├── config.php                   # Main configuration
│   ├── container.php                # DI container configuration
│   └── routes.php                   # Route definitions
├── docker-compose.yml               # Docker Compose configuration
├── Dockerfile                       # Docker configuration
├── runtime
│   ├── container                    # Runtime cache and proxies
│   └── hyperf.pid                   # Process ID file
├── test
│   ├── Cases
│   │   └── ExampleTest.php          # Sample test case
│   ├── HttpTestCase.php             # HTTP test case
│   └── bootstrap.php                # Test bootstrap
├── composer.json                    # Composer dependencies
├── .env                             # Environment variables
├── .env.example                     # Environment variable template
├── .gitignore                       # Git ignore file
├── LICENSE                          # License file
├── phpstan.neon.dist                # PHPStan configuration
├── phpunit.xml.dist                 # PHPUnit configuration
└── README.md                        # This file
```

### Hints
- Rename `hyperf-skeleton` in `composer.json` and `docker-compose.yml` to your project name for consistency.
- Check `config/routes.php` and `app/Controller/IndexController.php` for an example HTTP entrypoint.

## Configuration
- **Environment Variables**: Configure your `.env` file for services like database, Redis, and AMQP. Example:
  ```env
  DB_HOST=127.0.0.1
  DB_PORT=3306
  DB_DATABASE=hyperf
  DB_USERNAME=root
  DB_PASSWORD=secret
  REDIS_HOST=127.0.0.1
  REDIS_PORT=6379
  ```
- **Config Files**: Modify files in `config/autoload/` for component-specific settings (e.g., `databases.php`, `redis.php`).

## Running the Application
1. **Using Docker Compose**:
   Start the application with Docker:
   ```bash
   docker-compose up -d
   ```
   The application will be available at `http://localhost:9501`.

2. **Using Hyperf CLI**:
   Start the Hyperf server directly:
   ```bash
   php bin/hyperf.php start
   ```

3. **Verify Running Containers**:
   Check Docker container status:
   ```bash
   docker-compose ps
   ```

## Available Components
This project includes the following Hyperf components:
- **Database**: MySQL support with model caching (`hyperf/database`, `hyperf/model-cache`)
- **Redis**: Redis client for caching and async queues (`hyperf/redis`, `hyperf/async-queue`)
- **JSON RPC**: For building RPC-based microservices (`hyperf/json-rpc`, `hyperf/rpc-*`)
- **AMQP**: RabbitMQ integration for message queues (`hyperf/amqp`)
- **Elasticsearch**: For search and analytics (`hyperf/elasticsearch`)
- **Tracer**: OpenTracing support with Zipkin compatibility (`hyperf/tracer`)
- **Constants**: Centralized error code management (`hyperf/constants`)

Refer to the [Hyperf Documentation](https://hyperf.wiki/) for detailed usage.

## Testing
Run tests using PHPUnit:
```bash
vendor/bin/phpunit
```

Tests are located in the `test/` directory, with a sample test case in `test/Cases/ExampleTest.php`. Use `phpstan.neon.dist` for static analysis with PHPStan.

## Contributing
Contributions are welcome! Follow these steps:
1. Fork the repository: [Giatechindo/hyperf-skeleton](https://github.com/Giatechindo/hyperf-skeleton)
2. Create a new branch (`git checkout -b feature/your-feature`)
3. Commit your changes (`git commit -m 'Add your feature'`)
4. Push to the branch (`git push origin feature/your-feature`)
5. Create a Pull Request

## License
This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Community
- Follow [Giatechindo](https://github.com/Giatechindo) for project updates and contributions.
- Follow [ugunNet21](https://github.com/ugunNet21) for additional community contributions.
- Join the [Hyperf Community](https://github.com/hyperf/hyperf) for support and discussions.