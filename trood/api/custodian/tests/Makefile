CURRENT_DIR = $(shell pwd)

default: test-all

test-unit:
	@echo running unit tests
	@pytest -v -m "not integration"

test-integration:
	@echo running integration tests
	@docker-compose up -d
	@make drop-db
	@make migrate
	@pytest -v -m integration

test-all:
	@docker-compose up -d
	@make drop-db
	@make migrate
	@pytest

drop-db:
	@echo Drop migrations ...
	@docker exec custodian sh -c 'rm -rf /home/custodian/*'
	@docker exec postgres.custodian psql -U custodian -d custodian -c "drop schema public cascade; create schema public;"

migrate:
	@curl -X POST -H "Content-Type: application/json" -d @person_test_migration.json http://localhost:8000/custodian/migrations

cleanup:
	@docker-compose down