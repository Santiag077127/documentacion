# Estructura inicial del repositorio

## Organización de carpetas

```
new-project-db/
│
│
├── 01_ddl/
│   ├── changelog.yaml
│   ├── 00_extensions/
│   │   └── changelog.yaml
│   ├── 01_schemas/
│   │   └── changelog.yaml
│   ├── 02_types/
│   │   └── changelog.yaml
│   ├── 03_tables/
│   │   └── changelog.yaml
│   ├── 04_views/
│   │   └── changelog.yaml
│   ├── 05_materialized_views/
│   │   └── changelog.yaml
│   ├── 06_functions/
│   │   └── changelog.yaml
│   ├── 07_procedures/
│   │   └── changelog.yaml
│   ├── 08_triggers/
│   │   └── changelog.yaml
│   └── 09_indexes/
│       └── changelog.yaml
│
├── 02_dml/
│   ├── changelog.yaml
│   ├── 00_inserts/
│   │   └── changelog.yaml
│   ├── 01_updates/
│   │   └── changelog.yaml
│   ├── 02_deletes/
│   │   └── changelog.yaml
│   ├── 03_upserts/
│   │   └── changelog.yaml
│   └── 04_patches/
│       └── changelog.yaml
│
├── 03_dcl/
│   ├── changelog.yaml
│   ├── 00_roles/
│   │   └── changelog.yaml
│   ├── 01_grants/
│   │   └── changelog.yaml
│   └── 02_policies/
│       └── changelog.yaml
│
├── 04_tcl/
│   ├── changelog.yaml
│   ├── 00_transaction_blocks/
│   │   └── changelog.yaml
│   ├── 01_manual_recoveries/
│   │   └── changelog.yaml
│   └── 02_release_tags/
│
│── 05_rollbacks/
│    ├── 01_ddl/
│    │   ├── 00_extensions/
│    │   ├── 01_schemas/
│    │   ├── 02_types/
│    │   ├── 03_tables/
│    │   ├── 04_views/
│    │   ├── 05_materialized_views/
│    │   ├── 06_functions/
│    │   ├── 07_procedures/
│    │   ├── 08_triggers/
│    │   └── 09_indexes/
│    ├── 02_dml/
│    │   ├── 00_inserts/
│    │   ├── 01_updates/
│    │   ├── 02_deletes/
│    │   ├── 03_upserts/
│    │   └── 04_patches/
│    ├── 03_dcl/
│    │   ├── 00_roles/
│    │   ├── 01_grants/
│    │   └── 02_policies/
│    └── 04_tcl/
│        ├── 00_transaction_blocks/
│        └── 01_manual_recoveries/
├── docker/
│   └── liquibase/
│       └── Dockerfile
│
├── docs/
│   └── sql-layer-architecture.md
│
├── scripts/
│   ├── rollback-by-id.ps1
│   ├── reapply-after-rollback.ps1
│   ├── 01_rollback_by_id.ps1
│   ├── 02_rollback_by_id_preview.ps1
│   ├── 03_rollback_by_id_cascade.ps1
│   ├── 04_reapply_next.ps1
│   └── 05_reapply_all.ps1
```

## Archivos en la raíz del proyecto

```
new-project-db/
├── changelog-master.yaml
├── docker-compose.yml
├── .env.example
├── .gitignore
├── .dockerignore
└── liquibase.properties.example
```

## Descripción de capas

| Carpeta | Capa | Contenido |
|---------|------|-----------|
| `01_ddl/` | DDL — Data Definition Language | Extensiones, schemas, tablas, vistas, vistas materializadas, funciones, procedimientos, triggers e índices |
| `02_dml/` | DML — Data Manipulation Language | Inserts, updates, deletes, upserts y patches de datos |
| `03_dcl/` | DCL — Data Control Language | Roles, grants y policies de seguridad |
| `04_tcl/` | TCL — Transaction Control Language | Bloques de transacción, recuperaciones manuales y release tags |
| `05_rollbacks/` | Rollbacks | Espejo de las capas 01–04 con los scripts `.rollback.sql` de cada changeset |

## Convención de archivos SQL

```
NNN_descripcion-corta.sql
NNN_descripcion-corta.rollback.sql   ← en 05_rollbacks/
```

Ejemplo:
```
01_ddl/03_tables/001_create_users_table.sql
05_rollbacks/01_ddl/03_tables/001_create_users_table.rollback.sql
```

## Convención de changesets

```yaml
- changeSet:
    id: 001-create-users-table
    author: tu-usuario
    labels: "ddl,tables"
    comment: "Crea la tabla de usuarios"
    changes:
      - sqlFile:
          path: 001_create_users_table.sql
          relativeToChangelogFile: true
    rollback:
      - sqlFile:
          path: ../../05_rollbacks/01_ddl/03_tables/001_create_users_table.rollback.sql
          relativeToChangelogFile: true
```