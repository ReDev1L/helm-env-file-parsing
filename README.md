# Helm env file parsing function
Helm helper function to parse .env file and output in yaml format (useful for kubernetes secrets generation)
```
KEY_ENV1=VAL_ENV1           KEY_ENV1: "VAL_ENV1"
KEY_ENV2=VAL_ENV2       =>  KEY_ENV2: "VAL_ENV2"
KEY_ENV3="${VAL_ENV3}"      KEY_ENV3: "\"${VAL_ENV3}\""
```

## Usage:
```
 {{ tuple . "configs/backend/php-fpm/.env" | include "env.parseFile" | indent 2}}
```
## Example file:
```
KEY_ENV1=VAL_ENV1
KEY_ENV2=VAL_ENV2
KEY_ENV3="${VAL_ENV3}"
```

## Output:
```
  KEY_ENV1: "VAL_ENV1"
  KEY_ENV2: "VAL_ENV2"
  KEY_ENV3: "\"${VAL_ENV3}\""
```
