default:
  @just --list

composer *args:
  just _exec php composer {{ args }}

phpunit *args:
  just _run --rm php phpunit {{ args }}

alias test := phpunit

drush *args:
  just _exec php drush {{ args }}

_exec +args:
  docker compose exec {{ args }}

_run +args:
  docker compose run {{ args }}
