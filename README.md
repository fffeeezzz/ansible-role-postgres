Role Name
=========

Роль "ansible-role-postgres" для установки PostgreSQL с мастером и 1 репликой.

Default Role Variables
--------------

```yml
postgres_repo_key: "https://www.postgresql.org/media/keys/ACCC4CF8.asc"
postgres_repo: "deb http://apt.postgresql.org/pub/repos/apt/ {{ ansible_distribution_release }}-pgdg main"
postgres_repo_filename: pgdg
postgres_version: 15
postgres_data_dir: "/var/lib/postgresql/{{ postgres_version }}"
postgres_custom_data_dir: "/tmp/postgresql"
postgres_config_dir: "/etc/postgresql/{{ postgres_version }}/main"
postgres_db_name: lab5-db
postgres_db_port: 5432
postgres_db_role: lab5-role
postgres_db_privs: ALL
postgres_user_name: lab5-user-name
postgres_user_password: lab5-user-password
postgres_hba_method: md5
postgres_replica_hba_method: md5
postgres_master_address: 89.169.132.157
postgres_replica_address: 84.201.135.122
postgres_default_user_password: qwerty
postgres_role: unknown
```

Example Playbook
----------------

Для запуска необходимо передавать переменную `postgres_role`.
Можно указать ее в playbook, как продемонстрированно ниже, либо использоваться group_vars в inventory.

```yml
---
- name: Устанавливаем мастер PostgreSQL
  hosts: master
  become: true
  tasks:
    - name: Подключаем таски PostgreSQL для мастера
      include_role:
        name: postgres
      vars:
        postgres_role: master

- name: Устанавливаем реплику
  hosts: replica
  become: true
  tasks:
    - name: Подключаем таски PostgreSQL для реплики
      include_role:
        name: postgres
      vars:
        postgres_role: replica
```

Ensure replication is working
----------------------

Для проверки рабостопособности репликации можно воспользоваться встроенным механизмом в PostgreSQL.
Можно посмотреть на содержимое таблицы `pg_stat_replication`.
https://stackoverflow.com/questions/43388243/check-postgres-replication-status

License
-------

MIT

