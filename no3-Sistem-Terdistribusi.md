1. Konsep Singkat Streaming ReplicationStreaming Replication adalah mekanisme di PostgreSQL di mana:
Primary mengirim WAL (Write-Ahead Log) ke
Replica standby secara real-time
Replica akan menyalin dan menerapkan WAL sehingga datanya mengikuti Primary
Ada dua mode sifatnya:
Mode	Penjelasan
Asynchronous	Replica menerima WAL tetapi Primary tidak menunggu. Cepat tetapi berpotensi kehilangan data jika Primary mati.
Synchronous	Primary menunggu Replica menulis WAL terlebih dahulu. Aman dari kehilangan data, tetapi sedikit lebih lambat.
2. Struktur Direktori
 Buat folder proyek:
pg-replication/
 ├── docker-compose.yml
 ├── primary/
 │    └── postgres.conf
 ├── replica/
      └── postgres.conf
File postgres.conf untuk Primary
wal_level = replica
max_wal_senders = 3
wal_keep_size = 64
archive_mode = off
Buat docker-compose.yml
version: '3.8'

services:
  primary:
    image: postgres:15
    container_name: pg_primary
    restart: always
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: 123
      POSTGRES_DB: mydb
    volumes:
      - ./primary/postgres.conf:/etc/postgresql/postgresql.conf
      - primary_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"
    command:
      - "postgres"
      - "-c"
      - "config_file=/etc/postgresql/postgresql.conf"

  replica:
    image: postgres:15
    container_name: pg_replica
    restart: always
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: 123
    volumes:
      - ./replica/postgres.conf:/etc/postgresql/postgresql.conf
      - replica_data:/var/lib/postgresql/data
    ports:
      - "5433:5432"
    depends_on:
      - primary
    command: |
      bash -c "
      until pg_basebackup -h primary -D /var/lib/postgresql/data \
        -U postgres -Fp -Xs -P -R; do
        echo 'Waiting for primary...'
        sleep 3
      done
      postgres -c config_file=/etc/postgresql/postgresql.conf
      "

volumes:
  primary_data:
  replica_data:
