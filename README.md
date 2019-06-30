# UniEngine

[![Build Status](https://travis-ci.org/mdziekon/UniEngine.svg?branch=master)](https://travis-ci.org/mdziekon/UniEngine)
[![Release: Stable (latest)](https://img.shields.io/github/release/mdziekon/UniEngine.svg?label=release%3Astable&logo=github&logoColor=FFFFFF)](https://github.com/mdziekon/UniEngine/releases)

OGame-clone browser based game engine.

---

- [Requirements](#requirements)
- [Installation](#installation)
- [Updating from older versions](#updating-from-older-versions)
- [Development guides](#development-guides)
- [Documentation](#documentation)
- [Languages](#languages)
- [Credits](#credits)
- [License](#license)

## Requirements

- PHP
    - ``>= 7.0 && < 7.3``
    - ``>= 5.4`` (deprecated)
- Composer
    - ``>= 1.6``
- MySQL
    - ``>= 5``
- A webserver (eg. nginx)

### (Additional) Development requirements

- Node.js
    - ``>= 11``

## Installation

1. Setup a webserver capable of running PHP scripts.
    - ``php.ini`` file should have ``E_NOTICE`` reporting disabled, eg.:
        - ``error_reporting = E_ALL & ~E_NOTICE & ~E_STRICT``
    - PHP needs to have write permissions to these files / directories:
        - ``config.php``
            - (one-off, installation purposes)
        - ``includes/constants.php``
            - (one-off, installation purposes)
        - ``tmp/``
            - (permanent, eg. for Smarty cache)
1. Setup a MySQL server.
    - Disable ``STRICT_TRANS_TABLES`` mode.
1. Create a DB user and DB database for your game server.
1. Move source files of the project to your webserver's directory.
1. Install PHP dependencies.
    - ``composer install --no-dev``
1. Run installation wizard: http://your_server_address:port/install
1. Remove ``install/`` directory

## Updating from older versions

1. Check [__Releases__ section](https://github.com/mdziekon/UniEngine/releases) to see if migration scripts have been provided between your current version and the latest version you want to upgrade to.
    - In case of missing migration scripts, **do not try to use the auto-migrate functionality!** It may completely break your game server.
    - In case of major breaking changes (which for some reason were not possible to auto-migrate), there should be a release note explaining why and what manual actions have to be performed to proceed with migration.
1. Close your game server (prevent players from accessing the game).
1. Perform a full backup of your game server's state (database, configuration, files, etc...).
1. Make sure that your PHP server has write access to game server's files (in case if one of the migration scripts might need this).
1. Update files to the desired version.
1. Update PHP dependencies.
    - ``composer install --no-dev``
1. Run migration script from project's root directory:
    - ``composer run-script migrate:run``
    - For more details go to [Available scripts](#available-scripts) section.
1. Restart PHP server.

## Development guides

### Preparations

1. Install PHP dependencies (normal & dev):
    - ``composer install --dev``
1. Install Node.js dependencies:
    - ``npm ci``

### Available scripts

- Run PHP code linting (powered by PHP Code Sniffer)
    - ``composer run-script ci-php-phpcs``
- Run all not-yet-applied migrations
    - ``composer run-script migrate:run [-- --confirm-manual-action]``
    - Script will automatically apply all outstanding migrations.
    - A manual interaction might be required. If that's the case, follow the instructions printed to the command line and then run the script again with ``--confirm-manual-action`` flag.
    - After successful migration, a marker file will be created (``config/latest-applied-migration``) for future migrations.
- Generate new migration file using a template
    - ``composer run-script migrate:make -- <MIGRATION_FILE_NAME>``
    - New migration file with autogenerated ID (date) will be created in ``utils/migrator/migrations/`` directory.
- Run JavaScript code linting (powered by ESLint):
    - ``npm run ci-js-eslint``
- Run CSS code linting (powered by stylelint):
    - ``npm run ci-css-stylelint``
- Rebuild (minification + cachebusting) JS & CSS files:
    - ``npm run build-minify``
    - All files from ``js/`` and ``css/`` directories will be re-minified (only when actually changed) and saved in their respective ``dist/`` directories.
    - _Note:_ when a file has no changes, this script **won't** remove the old minified & cachebusted file from ``dist/``. File replacement happens only if a source file has changes, or there is no result file yet.
    - _Note:_ this script does **not** automatically replace filepaths in templates. For now, this has to be done manually by a developer.
    - _Note:_ due to legacy reasons, all files in ``dist/`` are stored in the repo.

---

## Documentation

Visit [docs/index.md](./docs/index.md) to see project's documentation.

## Languages

- English 🇬🇧
- Polish / Polski 🇵🇱

## Credits

### Authors

- Michał Dziekoński (https://github.com/mdziekon)

### Contributors

- Alessio <nicoales@live.it> (https://github.com/XxidroxX)

## License

GPL-2.0

See ``LICENSE`` file for this project's license details.

See ``OTHERLICENSES`` for the licenses of included external resources.
