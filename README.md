# Clank Net, build your net of clankers: A Local AI Docker Network

A comprehensive Docker Compose setup for running a complete local AI ecosystem with Ollama, Open WebUI, SearXNG search engine and n8n for automation.

## 🚚 Containers

- **Ollama**: Local LLM inference server
- **Open WebUI**: Modern chat interface for interacting with AI models
- **SearXNG**: Privacy-respecting metasearch engine for web searches
- **n8n**: Automation framework

All services are interconnected on a private Docker network for seamless communication.

## 📋 Prerequisites

- Docker Engine 20.10 or higher
- Docker Compose V2
- At least 8GB RAM (16GB recommended)
- 20GB free disk space for models and data

## 🛠️ Installation

1. Clone the repository:
```bash
git clone https://github.com/colbdavis/local-ai-docker-net.git
cd local-ai-docker-net
```

2. Create required directories:
```bash
mkdir -p ~/docker/{ollama,openwebui,searxng,n8n}
```

3. Copy the SearXNG configuration:
```bash
cp searxng/settings.yml ~/docker/searxng/settings.yml
```


4. Start the services:
```bash
docker-compose up -d
```

## 🌐 Access Points

| Service | URL | Description |
|---------|-----|-------------|
| Open WebUI | http://localhost:8080 | Main chat interface |
| SearXNG | http://localhost:8081 | Search engine |
| Ollama API | http://localhost:11434 | LLM API endpoint |
| n8n | http://localhost:5678 | n8n web UI |


## 🔧 Configuration

### Resource Limits

Default resource allocations:
- **Ollama**: 2GB RAM, 4 CPUs
- **Open WebUI**: 1GB RAM, 2 CPUs
- **SearXNG**: 500MB RAM, 1 CPU
- **n8n**: 1GB RAM, 2 CPUs


Adjust these in `docker-compose.yml` based on your system resources.

### SearXNG Configuration

The SearXNG configuration is stored in `searxng/settings.yml`. Key settings:

- **Port**: 8081
- **Safe Search**: Disabled (level 0)
- **API**: Enabled for Open WebUI integration
- **Allowed Hosts**: Configured for container network access

To modify search engines or settings, edit `~/docker/searxng/settings.yml` and restart the container.

### Ollama Models

Download models after starting Ollama:

```bash
# Pull a model
docker exec -it ollama ollama pull llama2

# List installed models
docker exec -it ollama ollama list

# Popular models
docker exec -it ollama ollama pull mistral
docker exec -it ollama ollama pull codellama
docker exec -it ollama ollama pull llava
```

## 🔗 Service Integration

### Open WebUI ↔ Ollama
Open WebUI automatically connects to Ollama via `http://ollama:11434`

### Open WebUI ↔ SearXNG
Web search functionality is enabled through `http://searxng:8081`

### And everything you want...

## 🔒 Security Notes

- All services run on localhost by default
- SearXNG is configured for local use only
- No external API keys required
- Data is stored locally in `~/docker/`

**For production use:**
- Configure proper authentication
- Set up HTTPS with reverse proxy
- Restrict network access
- Enable API authentication

## 🐛 Troubleshooting

### Ollama not responding
```bash
docker logs ollama
docker restart ollama
```

### SearXNG returns no results
Check the allowed hosts configuration in `settings.yml`:
```yaml
allowed_hosts: ["openwebui", "172.18.0.6"]
```

### Port conflicts
If ports are already in use, modify the port mappings in `docker-compose.yml`:
```yaml
ports:
  - "NEW_PORT:8080"  # Change NEW_PORT to available port
```

### Out of memory errors
Reduce resource limits or increase Docker's memory allocation:
```bash
# Check current usage
docker stats

# Adjust limits in docker-compose.yml
mem_limit: 1g  # Reduce as needed
```
### n8n restarting for priviledge conflict
Chown the n8n directory to the container
```bash
sudo chown -R 1000:1000 ${HOME}/docker/n8n
```
## 📁 Data Persistence

All data is persisted in `~/docker/`:
- `~/docker/ollama`: Model files and cache
- `~/docker/openwebui`: Chat history and settings
- `~/docker/searxng`: Search engine configuration

## 🔄 Updates

Update all services:
```bash
docker-compose pull
docker-compose up -d
```

Update specific service:
```bash
docker-compose pull ollama
docker-compose up -d ollama
```

## 🛑 Stopping Services

Stop all services:
```bash
docker-compose down
```

Stop and remove volumes (⚠️ deletes all data):
```bash
docker-compose down -v
```

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🙏 Acknowledgments

- [Ollama](https://ollama.ai/) - Local LLM runtime
- [Open WebUI](https://github.com/open-webui/open-webui) - Chat interface
- [SearXNG](https://github.com/searxng/searxng) - Privacy-respecting search
- [n8n](https://github.com/n8n-io/n8n) - Automation platform
- [Ai Hub Searxng Docker](https://github.com/spacecodee/ai-hub-searxng-docker) - One of the most similar projects, which I take inspiration from

## 📧 Support

For issues and questions:
- Open an issue on GitHub
- Check existing issues for solutions
- Review the logs: `docker-compose logs [service-name]`
