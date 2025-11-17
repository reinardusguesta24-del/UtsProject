 A[Primary DB: master_db :5432] -->|1. Transaksi (INSERT/UPDATE)| B(WAL Log);
    B -->|2. WAL Stream| C(Jaringan);
    C -->|3. Streaming Connection (primary_conninfo)| D[Replica DB: slave_db :5433];
    D --> E(WAL Apply/Replay);
    E --> F[Replica Data: Sinkron dengan Primary];

    style A fill:#ffdddd,stroke:#cc0000,stroke-width:2px
    style D fill:#ddffdd,stroke:#00cc00,stroke-width:2px
    style B fill:#ffffaa
