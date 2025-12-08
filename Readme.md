* Apache Kafka Deployment using Ansible (Multi-Node) *

This repository contains an Ansible automation setup for installing and configuring an Apache Kafka multi-node cluster.
It includes reusable roles, templates, and a single playbook.yml to orchestrate the deployment.

📂 Repository Structure
.
├── inventory.ini
├── playbook.yml
├── README.md
└── roles
    ├── common
    │   └── main.yml
    └── kafka
        ├── handlers
        │   └── main.yaml
        ├── tasks
        │   └── main.yml
        └── templates
            ├── kafka.service.j2
            └── server.properties.j2

* roles/common/main.yml

Common setup tasks such as installing required packages, creating system users, or preparing directories.

* roles/kafka/tasks/main.yml

Main logic to:

Install Kafka

Configure Kafka directories

Deploy server.properties

Configure systemd service

* roles/kafka/templates/*

kafka.service.j2 — systemd service template

server.properties.j2 — Kafka broker configuration

* roles/kafka/handlers/main.yaml

Service restart handlers executed when configuration changes.

* Inventory (inventory.ini)

Example:

[kafka]
192.168.10.11 broker_id=1
192.168.10.12 broker_id=2
192.168.10.13 broker_id=3

[all:vars]
kafka_version=3.7.0
kafka_install_dir=/opt/kafka
kafka_data_dir=/var/lib/kafka


You may add variables such as broker_id and hostname per server.

* How to Run
1. Check connectivity
ansible -i inventory.ini kafka -m ping

2. Run the Kafka deployment playbook
ansible-playbook -i inventory.ini playbook.yml


The playbook automatically:

✔ Installs Kafka
✔ Generates server.properties for each broker
✔ Configures & enables Kafka systemd service
✔ Starts the Kafka node

* Service Management

Start Kafka:

systemctl start kafka


Check logs:

journalctl -u kafka -f

* Verification

Create a topic:

kafka-topics.sh --create \
  --topic demo \
  --bootstrap-server <broker>:9092 \
  --replication-factor 3 \
  --partitions 3


Producer:

kafka-console-producer.sh --bootstrap-server <broker>:9092 --topic demo


Consumer:

kafka-console-consumer.sh --bootstrap-server <broker>:9092 --topic demo --from-beginning

* Systemd Service Template

Your Kafka systemd service template (kafka.service.j2) includes:

ExecStart to run Kafka server

Logs written to /opt/kafka/logs

Uses KAFKA_HEAP_OPTS from environment

* Contributing

Pull requests and improvements are welcome.