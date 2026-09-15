# Safety Blur

Safety Blur is a license key generation and verification management system built around a Python CLI and a web/API-based licensing workflow.

It appears to support:

- generating unique license keys
- storing license keys in a MariaDB/MySQL database
- tracking products associated with keys
- identifying potentially reused keys
- listing unused keys
- exporting generated licenses to SQL
- managing verification logs

## Features

### Python CLI
The Python app in `python/` provides an interactive terminal interface for:

- generating a single license key
- generating multiple license keys
- exporting license keys to a SQL file
- viewing database information
- adding test products
- scanning for suspicious key usage across domains
- showing unused license keys
- clearing verification logs with confirmation

### Database-backed license storage
License keys are stored in a database table with fields such as:

- `license_key`
- `product`
- `status`
- `created_at`

The app can also create the database and license table if they do not already exist.

### Product management
Products can be loaded from and saved to `python/src/products.json`, and the CLI allows adding new product names interactively.

### License verification workflow
The repository also contains:

- `api/`
- `controller/`
- `routers/`
- `dashboard/`
- `admin/`
- `private/`
- release blueprints

This suggests Safety Blur includes a broader licensing / verification / dashboard workflow beyond the Python CLI.

## Repository Structure

```text
.
├── admin/                  # Admin views
├── api/                    # API endpoints
├── assets/                 # Static assets
├── controller/             # Backend controller logic
├── dashboard/              # Dashboard views
├── private/                # Install/remove scripts and license data
├── python/                 # Python license generator CLI
├── release/                # Release blueprints
├── routers/                # Web routes
└── root.css                # Shared stylesheet
```

## Python CLI

The CLI entry point is:

```bash
python/run.py
```

It loads:

- `python/config.json`
- `python/src/license_generator.py`
- `python/assets/ascii.txt`
- `python/src/products.json`

### Requirements

The Python code uses:

- `mysql-connector-python`
- standard library modules such as `secrets`, `base64`, `json`, and `datetime`

You will also need:

- Python 3.10+
- a MariaDB/MySQL database

## Configuration

The CLI expects a `config.json` file with database and license settings.

A typical configuration should include:

- database connection details
- table name
- license key length
- optional product name

Example structure:

```json
{
  "database": {
    "host": "localhost",
    "user": "root",
    "password": "password",
    "database": "safetyblur"
  },
  "table": {
    "name": "licenses"
  },
  "license": {
    "key_length": 32
  },
  "product": {
    "name": "default-product"
  }
}
```

## Usage

### Run the CLI

```bash
cd python
python run.py
```

### Common CLI actions

From the menu, you can:

1. Generate a single license key
2. Look for warnings about possible reused keys
3. View unused license keys
4. Add a test product
5. Clear the `verification_logs` table
6. View database information
7. Exit

## Database Schema

The Python app creates a table similar to:

```sql
id INT AUTO_INCREMENT PRIMARY KEY
license_key VARCHAR(255) UNIQUE NOT NULL
product VARCHAR(255) NOT NULL
status VARCHAR(50) DEFAULT 'active'
created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
```

## Exporting Licenses

The CLI can export generated licenses to SQL files under:

```text
database/exports/
```

## Notes

- The repository includes multiple release blueprint files under `release/`.
- The Python CLI is interactive and relies on a local config file.
- Some web components in the repo may be part of a broader licensing system not fully documented here.

## License

No license file was detected in the repository.
