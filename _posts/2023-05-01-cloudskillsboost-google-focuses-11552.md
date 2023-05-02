---
layout: post
title: Deploy and Manage Cloud Environments with Google Cloud - Challenge Lab
date: 2023-05-01 06:14:27 +0700
categories: [cloud]
tags: [lab, gcp]
---

Deploy and Manage Cloud Environments with Google Cloud: Challenge Lab - 11552

## Task 1: Migrate a stand-alone PostgreSQL database to a Cloud SQL for PostgreSQL instance

- Practicing lab: https://partner.cloudskillsboost.google/focuses/23649?parent=catalog

- In the Google Cloud Console, on the Navigation menu (Navigation menu icon), click Compute Engine > VM instances.

- In the entry for `postgresql-vm`, under Connect click SSH. If prompted, click Connect. In the terminal in the new browser window, install the `pglogical` database extension:

```
sudo apt install postgresql-13-pglogical
```

- Download and apply some additions to the PostgreSQL configuration files (to enable `pglogical` extension) and restart the `postgresql` service:

```
sudo su - postgres -c "gsutil cp gs://cloud-training/gsp918/pg_hba_append.conf ."
sudo su - postgres -c "gsutil cp gs://cloud-training/gsp918/postgresql_append.conf ."
sudo su - postgres -c "cat pg_hba_append.conf >> /etc/postgresql/13/main/pg_hba.conf"
sudo su - postgres -c "cat postgresql_append.conf >> /etc/postgresql/13/main/postgresql.conf"
sudo systemctl restart postgresql@13-main
```

- Launch the `psql` tool:

```
sudo su - postgres
psql
```

- Add the `pglogical` database extension to the `postgres`, `orders` databases.

```
\c postgres;
CREATE EXTENSION pglogical;
\c orders;
CREATE EXTENSION pglogical;
```

- Create the database migration user, you will create a dedicated user for managing database migration.

- In `psql`, enter the commands below to create a new user with the replication role:

```
\c postgres;
CREATE USER import_admin PASSWORD 'DMS_1s_cool!';
ALTER DATABASE orders OWNER TO import_admin;
ALTER ROLE import_admin WITH REPLICATION;
```

### Assign permissions to the migration user

- In this step you will assign the necessary permissions to the `import_admin` user to enable **Database Migration Service** to migrate your database.

- In `psql`, grant permissions to the `pglogical` schema and tables for the `postgres` database.

```
\c postgres;
GRANT USAGE ON SCHEMA pglogical TO import_admin;
GRANT ALL ON SCHEMA pglogical TO import_admin;
GRANT SELECT ON pglogical.tables TO import_admin;
GRANT SELECT ON pglogical.depend TO import_admin;
GRANT SELECT ON pglogical.local_node TO import_admin;
GRANT SELECT ON pglogical.local_sync_status TO import_admin;
GRANT SELECT ON pglogical.node TO import_admin;
GRANT SELECT ON pglogical.node_interface TO import_admin;
GRANT SELECT ON pglogical.queue TO import_admin;
GRANT SELECT ON pglogical.replication_set TO import_admin;
GRANT SELECT ON pglogical.replication_set_seq TO import_admin;
GRANT SELECT ON pglogical.replication_set_table TO import_admin;
GRANT SELECT ON pglogical.sequence_state TO import_admin;
GRANT SELECT ON pglogical.subscription TO import_admin;
```

- In `psql`, grant permissions to the `pglogical` schema and tables for the `orders` database.

```
\c orders;
GRANT USAGE ON SCHEMA pglogical TO import_admin;
GRANT ALL ON SCHEMA pglogical TO import_admin;
GRANT SELECT ON pglogical.tables TO import_admin;
GRANT SELECT ON pglogical.depend TO import_admin;
GRANT SELECT ON pglogical.local_node TO import_admin;
GRANT SELECT ON pglogical.local_sync_status TO import_admin;
GRANT SELECT ON pglogical.node TO import_admin;
GRANT SELECT ON pglogical.node_interface TO import_admin;
GRANT SELECT ON pglogical.queue TO import_admin;
GRANT SELECT ON pglogical.replication_set TO import_admin;
GRANT SELECT ON pglogical.replication_set_seq TO import_admin;
GRANT SELECT ON pglogical.replication_set_table TO import_admin;
GRANT SELECT ON pglogical.sequence_state TO import_admin;
GRANT SELECT ON pglogical.subscription TO import_admin;
```

- In `psql`, grant permissions to the public schema and tables for the `orders` database.

```
\c postgres;
GRANT USAGE ON SCHEMA public TO import_admin;
GRANT ALL ON SCHEMA public TO import_admin;
GRANT SELECT ON public.distribution_centers TO import_admin;
GRANT SELECT ON public.inventory_items TO import_admin;
GRANT SELECT ON public.order_items TO import_admin;
GRANT SELECT ON public.products TO import_admin;
GRANT SELECT ON public.users TO import_admin;
```

- Make the `import_admin` user the owner of the tables in the `orders` database, so that you can edit the source data later, when you test the migration.

- In psql, run the following commands:

```
\c orders;
\dt
ALTER TABLE public.distribution_centers OWNER TO import_admin;
ALTER TABLE public.inventory_items OWNER TO import_admin;
ALTER TABLE public.order_items OWNER TO import_admin;
ALTER TABLE public.products OWNER TO import_admin;
ALTER TABLE public.users OWNER TO import_admin;
\dt
```

- The **Database Migration Service** requires all tables to be migrated to have a primary key. Once you have granted the user the required privileges, run the following to add a primary key to the `inventory_items` table and exit `psql`.

```
\c orders;
ALTER TABLE public.inventory_items ADD PRIMARY KEY(id);
\c postgres;
ALTER TABLE public.inventory_items ADD PRIMARY KEY(id);
```

## Task 2: Update permissions and add IAM roles to users

- Practicing lab: https://partner.cloudskillsboost.google/focuses/42345?parent=catalog

- To examine a list of roles:

```
gcloud iam roles list | grep "name:"
```

- Assigning Role to a User or a Service Account:

```
gcloud projects add-iam-policy-binding PROJECT_ID_REPLACE_ME --member user:USER_ID_REPLACE_ME --role=roles/viewer
```

`USER_ID` is in email format

Reference: https://cloud.google.com/sdk/gcloud/reference/projects/add-iam-policy-binding

## Task 3: Create networks and firewalls

- Practicing lab: https://partner.cloudskillsboost.google/focuses/42462?parent=catalog

- Create a new VPC:

```
gcloud compute networks create taw-custom-network --subnet-mode custom
```

- Create a new subnet:

```
gcloud compute networks subnets create subnet-us-central \
   --network taw-custom-network \
   --region us-central1 \
   --range 10.10.10.0/24
```

- Create new firewall rules:
  e.g:

```
gcloud compute firewall-rules create nw101-allow-http-rule \
--allow tcp:22-65535 --network taw-custom-vpc-network --source-ranges 0.0.0.0/0 \
--target-tags http
```

```
gcloud compute firewall-rules create nw101-allow-http-rule \
--allow tcp:3389-65535 --network taw-custom-vpc-network --source-ranges 0.0.0.0/0 \
--target-tags http
```

```
gcloud compute firewall-rules create nw101-allow-http-rule \
--allow icmp-65535 --network taw-custom-vpc-network --source-ranges 0.0.0.0/0 \
--target-tags http
```

with customize allow: `tcp:0-65535,udp:0-65535,icmp`

Reference: https://cloud.google.com/sdk/gcloud/reference/compute/firewall-rules/create

## Task 4: Troubleshoot and fix a broken GKE cluster

- Practicing lab: https://partner.cloudskillsboost.google/focuses/42343?parent=catalog

- To examine a list of roles:

```
gcloud iam roles list | grep "name:"
```

- Assigning Role to a **User** or a **Service Account**:

```
gcloud projects add-iam-policy-binding PROJECT_ID_REPLACE_ME --member user:USER_ID_REPLACE_ME --role=roles/viewer
```

`USER_ID` is in email format
