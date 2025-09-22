# Elastic Stack (ELK) Docker Compose Setup

Complete Elastic Stack setup with Docker Compose including Elasticsearch, Kibana, Logstash, Filebeat, and Metricbeat with SSL/TLS security.

## Quick Start

1. **Clone and setup**
```bash
git clone https://github.com/mhdhaikalll/elastic-dashboard-docker.git
cd elastic-dashboard-docker
```

2. **Configure environment**
```bash
cp .env.example .env
# Edit .env file with your passwords
```

3. **Start the stack**
```bash
docker-compose up -d
```

4. **Access services**
- Kibana: http://localhost:5601 (elastic / your-password)
- Elasticsearch: https://localhost:9200

## Prerequisites

- Docker & Docker Compose
- 4GB+ RAM available
- Generate secure passwords in `.env` file

## Environment Variables

Copy `.env.example` to `.env` and update:

- `ELASTIC_PASSWORD` - Main admin password
- `KIBANA_PASSWORD` - Kibana system password  
- `ENCRYPTION_KEY` - Random 32+ character string
- Other settings can use defaults

## Components

### Elasticsearch
- Single-node cluster with SSL/TLS
- Data stored in `esdata01` volume
- Available on port 9200

### Kibana  
- Web dashboard on port 5601
- Connects to Elasticsearch with authentication

### Logstash
- Processes CSV files from `logstash_ingest_data/`
- Creates daily indices: `logstash-YYYY.MM.dd`
- Includes geolocation processing

### Filebeat
- Monitors Docker container logs
- Processes files from `filebeat_ingest_data/`
- Ships logs to Elasticsearch

### Metricbeat
- Collects system and Docker metrics
- Monitors Elasticsearch and Logstash
- 10-second collection interval

## Data Ingestion

### Adding Data to Logstash
```bash
# Copy CSV files to logstash directory
cp your-data.csv ./logstash_ingest_data/
docker-compose restart logstash01
```

### Adding Logs to Filebeat
```bash
# Copy log files to filebeat directory  
cp application.log ./filebeat_ingest_data/
```

## Common Commands

```bash
# View all services status
docker-compose ps

# View logs
docker-compose logs -f [service-name]

# Stop all services
docker-compose down

# Remove all data (reset)
docker-compose down -v
```

## Troubleshooting

### Services won't start
```bash
# Check Docker resources
docker system df

# Verify environment file
cat .env

# Check specific service logs
docker-compose logs setup
docker-compose logs es01
```

### Memory issues (Linux/macOS)
```bash
sudo sysctl -w vm.max_map_count=262144
```

### Reset certificates
```bash
docker-compose down
docker volume rm elastic-dashboard-docker_certs
docker-compose up -d
```

- All inter-service communication uses SSL/TLS
- Certificates auto-generated on first run
- Authentication required for all access
- Passwords configurable via environment variables


## Disclaimer

This is part of ITT450: Introduction to Cybersecurity projects