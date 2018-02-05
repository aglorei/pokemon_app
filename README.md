# Pokemon App
This respository couples the Rails server implemented in [Pokemon Battle Rails](https://github.com/aglorei/pokemon_battle_rails/) with a Nginx reverse proxy that implements a [proxy cache](https://www.nginx.com/blog/nginx-caching-guide/) for requests that fetch upstream information from [Pokeapi](https://pokeapi.co/). Since requests from the battle server are passed through the reverse proxy and collected in cache, the strain towards hitting Pokeapi's rate-limiting threshold is eased.

## Quick Start
### Prerequisites
- [Docker](https://docs.docker.com/install)
- [Docker Compose](https://github.com/docker/compose/releases)
### Install
```
git submodule update --init --recursive
docker-compose up
```
### Show Pokemon Passthrough
```
curl localhost:8080/api/v2/pokemon/1/
```
### Show Move Passthrough
```
curl localhost:8080/api/v2/move/1/
```
### Create Battle
```
curl -X POST \
  -H 'Content-type: application/json' \
  -d '{"pokemon1": "pikachu", "pokemon2": "charmander"}' \
  localhost:8080/battle
```
### Tests
```
docker-compose run --rm \
  -e RAILS_ENV=test \
  battle_service \
  bundle exec rake test
```
### Code Analyze ([Rubocop](https://github.com/bbatsov/rubocop))
```
docker-compose run --rm \
  -e RAILS_ENV=development \
  battle_service \
  bundle exec rubocop
```

## Pokemon
### GET /api/v2/pokemon/{id or name}/
This is a passthrough-endpoint. Please reference the [Pokeapi Documentation](https://pokeapi.co/docsv2/) for more details
## Move
### GET /api/v2/move/{id or name}/
This is a passthrough-endpoint. Please reference the [Pokeapi Documentation](https://pokeapi.co/docsv2/) for more details
## Battle
### POST /battle
| Field | Type | Description |
| --- | --- | --- |
| `pokemon1` | {id or name} | ID or name of Pokemon assigned as offensive in the first round |
| `pokemon2` | {id or name} | ID or name of Pokemon assigned as defensive in the first round |
