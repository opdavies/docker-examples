# docker-example-drupal-localgov

## Installation

```bash
# With just
just drush site:install -y --db-url=mysql://app:app@database/app localgov

# Without just
docker compose exec php drush site:install -y --db-url=mysql://app:app@database/app localgov
```
