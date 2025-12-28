# Laravel Dockerized Project

A fully dockerized Laravel 12 application with PHP 8.2, Nginx, MySQL 8.0, and Node.js, providing a complete development environment with all necessary services containerized.

## 🚀 Features

- **Laravel 12** - Latest Laravel framework
- **PHP 8.2** with FPM and essential extensions (PDO, MySQL)
- **Nginx** - High-performance web server
- **MySQL 8.0** - Relational database management system
- **Docker Compose** - Multi-container orchestration
- **Node.js 14** - For frontend asset management
- **Composer** - PHP dependency management

## 📋 Prerequisites

Before you begin, ensure you have the following installed:

- [Docker](https://www.docker.com/get-started) (version 20.10 or higher)
- [Docker Compose](https://docs.docker.com/compose/install/) (version 2.0 or higher)

## 🛠️ Installation

1. **Clone the repository** (if applicable) or navigate to the project directory:
   ```bash
   cd laravel&dockerized-project-test
   ```

2. **Start the Docker containers**:
   ```bash
   docker-compose up -d
   ```

3. **Install PHP dependencies**:
   ```bash
   docker-compose run --rm composer install
   ```

4. **Install Node.js dependencies**:
   ```bash
   docker-compose run --rm npm install
   ```

5. **Set up environment file** (if not already present):
   ```bash
   cp src/.env.example src/.env
   ```

6. **Generate application key**:
   ```bash
   docker-compose run --rm artisan key:generate
   ```

7. **Run database migrations**:
   ```bash
   docker-compose run --rm artisan migrate
   ```

## 🎯 Usage

### Starting Services

Start all services in detached mode:
```bash
docker-compose up -d
```

View logs:
```bash
docker-compose logs -f
```

### Stopping Services

Stop all services:
```bash
docker-compose down
```

Stop and remove volumes (⚠️ This will delete your database data):
```bash
docker-compose down -v
```

### Accessing the Application

- **Web Application**: http://localhost:8000
- **PHP-FPM**: Available on port 3000 (internal use)

### Running Artisan Commands

Execute Laravel artisan commands:
```bash
docker-compose run --rm artisan [command]
```

Examples:
```bash
# Run migrations
docker-compose run --rm artisan migrate

# Clear cache
docker-compose run --rm artisan cache:clear

# Run tests
docker-compose run --rm artisan test
```

### Running Composer Commands

Execute Composer commands:
```bash
docker-compose run --rm composer [command]
```

Examples:
```bash
# Install dependencies
docker-compose run --rm composer install

# Update dependencies
docker-compose run --rm composer update

# Require a package
docker-compose run --rm composer require package/name
```

### Running NPM Commands

Execute NPM commands:
```bash
docker-compose run --rm npm [command]
```

Examples:
```bash
# Install dependencies
docker-compose run --rm npm install

# Build assets
docker-compose run --rm npm run build

# Watch for changes
docker-compose run --rm npm run dev
```

## 🏗️ Project Structure

```
.
├── docker-compose.yaml      # Docker Compose configuration
├── dockerfiles/             # Custom Dockerfiles
│   ├── php.dockerfile       # PHP 8.2 FPM with extensions
│   └── composer.dockerfile  # Composer service
├── env/                     # Environment configuration files
│   └── mysql.env           # MySQL environment variables
├── nginx/                   # Nginx configuration
│   └── nginx.conf          # Nginx server configuration
└── src/                     # Laravel application source code
    ├── app/                 # Application logic
    ├── config/              # Configuration files
    ├── database/            # Migrations, seeders, factories
    ├── public/              # Public assets (web root)
    ├── resources/           # Views, assets, language files
    ├── routes/              # Route definitions
    └── tests/               # Test files
```

## 🐳 Docker Services

The project includes the following Docker services:

### `server` (Nginx)
- **Image**: `nginx:stable-alpine`
- **Port**: `8000:80`
- **Purpose**: Web server serving the Laravel application
- **Configuration**: `nginx/nginx.conf`

### `php` (PHP-FPM)
- **Image**: Custom build from `php.dockerfile`
- **Base**: `php:8.2-fpm-alpine`
- **Port**: `3000:9000`
- **Extensions**: PDO, PDO_MySQL
- **Purpose**: PHP processing for Laravel application

### `mysql` (Database)
- **Image**: `mysql:8.0`
- **Environment**: Configured via `env/mysql.env`
- **Purpose**: Database server for the application
- **Default Database**: `homestead`
- **Default User**: `homestead`
- **Default Password**: `secret`

### `composer`
- **Image**: Custom build from `composer.dockerfile`
- **Base**: `composer:latest`
- **Purpose**: PHP dependency management

### `artisan`
- **Image**: Same as PHP service
- **Purpose**: Laravel CLI command execution
- **Entrypoint**: `php /var/www/html/artisan`

### `npm`
- **Image**: `node:14`
- **Purpose**: Node.js package management and asset compilation

## 🗄️ Database Configuration

The MySQL service is configured with the following default credentials (defined in `env/mysql.env`):

- **Database**: `homestead`
- **User**: `homestead`
- **Password**: `secret`
- **Root Password**: `secret`

To change these values, edit the `env/mysql.env` file and restart the MySQL service:

```bash
docker-compose restart mysql
```

Update your Laravel `.env` file in the `src/` directory to match:

```env
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=homestead
DB_USERNAME=homestead
DB_PASSWORD=secret
```

## 🔧 Development

### Running Tests

```bash
docker-compose run --rm artisan test
```

### Code Formatting

Laravel Pint is available for code formatting:
```bash
docker-compose run --rm artisan pint
```

### Accessing the Database

You can connect to the MySQL database using any MySQL client with:
- **Host**: `localhost` (or `127.0.0.1`)
- **Port**: `3306` (if exposed, check docker-compose.yaml)
- **Database**: `homestead`
- **Username**: `homestead`
- **Password**: `secret`

Or use the MySQL command line:
```bash
docker-compose exec mysql mysql -uhomestead -psecret homestead
```

### Viewing Logs

View application logs:
```bash
docker-compose logs -f php
docker-compose logs -f server
docker-compose logs -f mysql
```

View Laravel logs:
```bash
tail -f src/storage/logs/laravel.log
```

## 📝 Notes

- All Laravel application code is located in the `src/` directory
- The `src/` directory is mounted as a volume, so changes are reflected immediately
- Database data persists in Docker volumes unless explicitly removed with `docker-compose down -v`
- For production deployment, ensure to:
  - Change default database credentials
  - Set proper environment variables
  - Configure proper security settings
  - Use production-optimized Docker images

## 🤝 Contributing

1. Make your changes in the `src/` directory
2. Ensure all tests pass: `docker-compose run --rm artisan test`
3. Format your code: `docker-compose run --rm artisan pint`

## 📄 License

This project is open-sourced software licensed under the [MIT license](https://opensource.org/licenses/MIT).

