init:
	@cp .env.default .env
	@echo "Created -  .env file with default values, please configure it as needed"
	@docker network create traefik
	@echo "Created - docker network"

up:
	@docker compose up -d

down:
	@docker compose down


