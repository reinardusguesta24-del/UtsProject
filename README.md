version: '3.8'
services:
  primary:
    build: ./primary
    container_name: pg_primary
    environment:
      POSTGRES_PASSWORD: password
      POSTGRES_USER: postgres
    ports:
      - "5433:5432"
    volumes:
      - primary_data:/var/lib/postgresql/data

  replica:
    build: ./replica
    container_name: pg_replica
    depends_on:
      - primary
    environment:
      POSTGRES_PASSWORD: password
      POSTGRES_USER: postgres
    ports:
      - "5434:5432"
    volumes:
      - replica_data:/var/lib/postgresql/data

volumes:
  primary_data:
  replica_data:
