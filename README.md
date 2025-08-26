# WordPress Docker Development Environment

A complete Docker-based WordPress development stack with WordPress, MySQL, phpMyAdmin, and WP-CLI.

## 📋 Overview

This repository provides a Docker Compose setup for running a local WordPress development environment. It includes:

- **WordPress** - Latest version with custom PHP configuration
- **MySQL** - Database with utf8mb4 support
- **phpMyAdmin** - Database management tool
- **WP-CLI** - Command-line interface for WordPress management

## 🚀 Quick Start

### Prerequisites

- Docker and Docker Compose installed on your system
- Git for cloning the repository

### Installation

1. Clone this repository:
   ```bash
   git clone https://github.com/mohammadBagherMehrabi/DockPress.git
   cd DockPress
   ```

2. Copy the environment example file:
   ```bash
   cp env.example .env
   ```

3. Edit the `.env` file with your preferred settings:
   ```bash
   nano .env  # or use your favorite text editor
   ```

   **Important**: Change the default passwords:
    - `DB_PASSWORD=your_secure_db_password_here`
    - `DB_ROOT_PASSWORD=your_secure_root_password_here`

4. Start the containers:
   ```bash
   docker-compose up -d
   ```

5. Access your WordPress site at: http://127.0.0.1:8080 (or your configured IP:PORT)

### Additional Services

- **phpMyAdmin**: http://127.0.0.1:8081 (or your configured IP:PMA_PORT)
- **MySQL Direct**: 127.0.0.1:3306 (or your configured IP:DB_PORT)

## ⚙️ Configuration

### Environment Variables

Edit the `.env` file to customize your setup:

| Variable | Description | Default |
|----------|-------------|---------|
| `IP` | IP address to bind services | 127.0.0.1 |
| `PORT` | WordPress port | 8080 |
| `PMA_PORT` | phpMyAdmin port | 8081 |
| `DB_NAME` | Database name | wordpress |
| `DB_USER` | Database user | wordpress_user |
| `DB_PASSWORD` | Database password | *change this* |
| `DB_ROOT_PASSWORD` | Root database password | *change this* |
| `*_CONTAINER_NAME` | Custom container names | project-specific |

### PHP Configuration

Custom PHP settings are configured in:
- `wordpress-files/wordpress-config/wp_php.ini` - For WordPress
- `wordpress-files/wordpress-config/pma_php.ini` - For phpMyAdmin

Default settings include:
- Increased memory limit (500M)
- File uploads enabled (30M limit)
- Extended execution time (600 seconds)

### phpMyAdmin Configuration

Additional phpMyAdmin settings are in:
- `wordpress-files/wordpress-config/pma_config.php`

## 🛠️ Usage

### WordPress CLI

Use WP-CLI through the dedicated container:
```bash
docker-compose exec wpcli wp <command>
```

Examples:
```bash
# List installed plugins
docker-compose exec wpcli wp plugin list

# Install a new plugin
docker-compose exec wpcli wp plugin install akismet --activate

# Update WordPress core
docker-compose exec wpcli wp core update
```

### Database Management

1. **Using phpMyAdmin**: Access through the web interface
2. **Using command line**:
   ```bash
   docker-compose exec db mysql -u root -p
   ```

### Database Export

Use the provided script to export your database:
```bash
chmod +x export.sh
./export.sh
```

This will create a backup in `wp-data/data_MM_DD_YYYY.sql`

### Plugin and Theme Development

The configuration includes volume mounts for easy development:

- Plugins: `wordpress-files/plugin-name/trunk/` → `/var/www/html/wp-content/plugins/plugin-name`
- Themes: `wordpress-files/theme-name/trunk/` → `/var/www/html/wp-content/themes/theme-name`

Create these directories to start developing your own plugins and themes.

## 🔧 Maintenance

### Stopping the Environment
```bash
docker-compose down
```

### Restarting the Environment
```bash
docker-compose restart
```

### Viewing Logs
```bash
# WordPress logs
docker-compose logs wp

# Database logs
docker-compose logs db

# All logs
docker-compose logs
```

### Updating Images
```bash
docker-compose pull
docker-compose up -d
```

## 🗂️ Project Structure

```
├── docker-compose.yml    # Main Docker configuration
├── .env                  # Environment variables (create from env.example)
├── export.sh             # Database export script
├── wordpress-files/
│   ├── wp-app/           # WordPress installation
│   ├── wp-data/          # Database exports and initializations
│   └── wordpress-config/
│       ├── pma_config.php    # phpMyAdmin configuration
│       ├── pma_php.ini       # phpMyAdmin PHP settings
│       └── wp_php.ini        # WordPress PHP settings
└── README.md             # This file
```

## 🤝 Contributing

Feel free to submit issues and enhancement requests!

## 📝 License

This project is open source and available under the [MIT License](LICENSE).

## ⚠️ Troubleshooting

### Common Issues

1. **Port conflicts**: Change ports in `.env` if default ports are already in use
2. **Permission errors**: Ensure Docker has proper permissions to mount volumes
3. **Container conflicts**: Stop any other local web servers or MySQL instances

### Getting Help

If you encounter issues:
1. Check that all containers are running: `docker-compose ps`
2. Examine logs for errors: `docker-compose logs`
3. Ensure your `.env` file is properly configured

---

**Happy WordPress developing!** 🚀