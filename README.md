# .NET Core Project with ElasticSearch, Kibana & Serilog Integration

## Features
- Structured logging with Serilog
- Centralized log storage in Elasticsearch
- Visualization of logs using Kibana

## Prerequisites
Ensure you have the following installed before proceeding:
- [.NET Core SDK](https://dotnet.microsoft.com/)
- [Docker](https://www.docker.com/)
- [Docker Compose](https://docs.docker.com/compose/)
- ELK Stack (can be run via Docker)

## Setup Instructions

### 1. Clone the Repository
```sh
git clone https://github.com/duypham-11/ELK-Serilog.git
```

### 2. Configure Logging in .NET Core
Modify `appsettings.json` to configure Serilog for logging to Elasticsearch:
```json
"Serilog": {
  "MinimumLevel": {
    "Default": "Information",
    "Override": {
      "Microsoft": "Information",
      "System": "Warning"
    }
  }
},
"ElasticConfiguration": {
  "Uri": "http://localhost:9200"
}
```

### 3. Run ELK Stack Using Docker
Create a `docker-compose.yml` file for Elasticsearch, Logstash, and Kibana:
```yaml
version: '3.1'

services:
  elasticsearch:
    container_name: els
    image: docker.elastic.co/elasticsearch/elasticsearch:8.7.1
    ports:
      - 9200:9200
    volumes:
      - elasticsearch-data:/usr/share/elasticsearch/data
    environment:
      - xpack.security.enabled=false
      - discovery.type=single-node
    networks:
      - elastic

  kibana:
    container_name: kibana
    image: docker.elastic.co/kibana/kibana:8.7.1
    ports:
      - 5601:5601
    depends_on:
      - elasticsearch
    environment:
      - ELASTICSEARCH_URL=http://localhost:9200
    networks:
      - elastic

networks:
  elastic:
    driver: bridge

volumes:
  elasticsearch-data:
```

### 4. Start ELK Stack
```sh
docker-compose up -d
```

### 5. Run the .NET Core Application
```sh
dotnet run
```

## Viewing Logs in Kibana
1. Open Kibana in your browser: `http://localhost:5601`
2. Open ElasticSearch in your browser: `http://localhost:9200`
3. Go to **Discover** and configure an index pattern for `logstash-*`
4. Explore logs using Kibana dashboards

## Troubleshooting
- Ensure Docker containers are running using `docker ps`
- Check logs using `docker logs elasticsearch`
- Verify .NET Core logs using `dotnet run --verbosity detailed`

## Contributing
Feel free to fork this project, create issues, and submit pull requests to improve the functionality.


