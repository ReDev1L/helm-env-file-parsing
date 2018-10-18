# Helm env file parsing function
Helm helper function to parse .env file and output in yaml format (useful for kubernetes secrets generation)

## Usage:
```
 {{ tuple . "configs/backend/php-fpm/.env" | include "env.parseFile" | indent 2}}
```
