# Proyecto CuentaTDD

Este es un proyecto de ejemplo utilizando Test Driven Development (TDD) para manejar una cuenta bancaria con las funcionalidades de creación, ingreso, retiro y transferencia de fondos.

## Instalación

1. Clonar el repositorio:
   ```bash
   git clone https://github.com/tu-usuario/CuentaTDD.git
   ```

2. Navegar al directorio del proyecto:
   ```bash
   cd CuentaTDD
   ```

3. Instalar las dependencias de Composer:
   ```bash
   composer install
   ```

4. Copiar el archivo `.env.example` a `.env` y configurar la base de datos:
   ```bash
   cp .env.example .env
   ```

5. Generar la clave de la aplicación:
   ```bash
   php artisan key:generate
   ```

6. Ejecutar las migraciones:
   ```bash
   php artisan migrate
   ```

## Ejecución de pruebas

Para ejecutar las pruebas, usa el siguiente comando:
```bash
php artisan test
```

## GitHub Actions

Este proyecto utiliza GitHub Actions para automatizar la ejecución de pruebas. Cada vez que se realiza un push, las pruebas se ejecutarán automáticamente.

### Configuración de GitHub Actions

El archivo de configuración para GitHub Actions se encuentra en `.github/workflows/laravel.yml` y tiene el siguiente contenido:

```yaml
name: Run Laravel Tests

on: [push, pull_request]

jobs:
  test:

    runs-on: ubuntu-latest

    services:
      mysql:
        image: mysql:5.7
        env:
          MYSQL_ROOT_PASSWORD: password
          MYSQL_DATABASE: test_db
        options: >-
          --health-cmd="mysqladmin ping --silent"
          --health-interval=10s
          --health-timeout=5s
          --health-retries=3
        ports:
          - 3306:3306
        volumes:
          - /var/lib/mysql

    steps:
    - uses: actions/checkout@v2

    - name: Setup PHP
      uses: shivammathur/setup-php@v2
      with:
        php-version: '7.4'
        extensions: mbstring, pdo, pdo_mysql, xml
        ini-values: post_max_size=256M, upload_max_filesize=256M
        coverage: none

    - name: Install Composer dependencies
      run: composer install

    - name: Copy .env.example to .env
      run: cp .env.example .env

    - name: Generate application key
      run: php artisan key:generate

    - name: Run migrations
      run: php artisan migrate

    - name: Run Tests
      run: php artisan test
```

## Enlace al repositorio de GitHub

[Enlace al repositorio de GitHub](https://github.com/Diegocentric/CuentaTDD)
