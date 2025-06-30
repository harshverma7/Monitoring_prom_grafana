# Express Monitoring with Prometheus & Grafana

A complete monitoring solution for Node.js Express applications using Prometheus for metrics collection and Grafana for visualization.

## 🚀 Features

- **Express.js API** with sample endpoints
- **Prometheus metrics** collection
- **Docker containerization** for easy deployment
- **Grafana dashboards** for visualization
- **Custom metrics** including:
  - HTTP request counter with labels (method, route, status)
  - Active requests gauge
  - Request duration histogram

## 📋 Prerequisites

- [Docker](https://docs.docker.com/get-docker/) installed
- [Docker Compose](https://docs.docker.com/compose/install/) installed
- [Git](https://git-scm.com/downloads) installed

## 🛠️ Quick Start

1. **Clone the repository**

   ```bash
   git clone https://github.com/harshverma7/Monitoring_prom_grafana.git
   cd Monitoring_prom_grafana
   ```

2. **Start the monitoring stack**

   ```bash
   docker compose up --build -d
   ```

3. **Access the services**
   - **Express API**: http://localhost:3000
   - **Prometheus**: http://localhost:9090
   - **Grafana**: http://localhost:3002 (admin/admin)

## 📊 Available Endpoints

### Express API

- `GET /user` - Returns user data (with 1-second delay)
- `POST /user` - Creates a user
- `GET /metrics` - Prometheus metrics endpoint

### Sample API Usage

```bash
# Test the user endpoint
curl http://localhost:3000/user

# Create a user
curl -X POST http://localhost:3000/user \
  -H "Content-Type: application/json" \
  -d '{"name":"John","age":30}'

# View raw metrics
curl http://localhost:3000/metrics
```

## 📈 Monitoring Setup

### Prometheus Queries

Access Prometheus at http://localhost:9090 and try these queries:

```promql
# Total HTTP requests
http_requests_total

# Request rate (per second)
rate(http_requests_total[5m])

# Active requests
active_requests

# 95th percentile response time
histogram_quantile(0.95, rate(http_request_duration_ms_bucket[5m]))

# Requests by status code
sum by (status_code) (http_requests_total)
```

### Grafana Dashboards

1. Open http://localhost:3002
2. Login with `admin/admin`
3. Add Prometheus data source: `http://prometheus:9090`
4. Create dashboards using the metrics above

## 🗂️ Project Structure

```
├── src/
│   ├── index.ts              # Main Express application
│   └── metrics/
│       ├── index.ts          # Metrics middleware
│       ├── requestCount.ts   # Counter & histogram metrics
│       └── activeRequests.ts # Gauge metrics
├── Dockerfile                # Express app container
├── docker-compose.yml        # Multi-service orchestration
├── prometheus.yml            # Prometheus configuration
├── package.json              # Node.js dependencies
└── tsconfig.json            # TypeScript configuration
```

## 🔧 Available Metrics

| Metric Name                | Type      | Description                      | Labels                     |
| -------------------------- | --------- | -------------------------------- | -------------------------- |
| `http_requests_total`      | Counter   | Total HTTP requests              | method, route, status_code |
| `active_requests`          | Gauge     | Current active requests          | -                          |
| `http_request_duration_ms` | Histogram | Request duration in milliseconds | method, route, code        |

## 🛠️ Development

### Local Development (without Docker)

```bash
# Install dependencies
npm install

# Build TypeScript
npm run build

# Start the application
npm start
```

### Stop Services

```bash
docker compose down
```

### View Logs

```bash
# All services
docker compose logs

# Specific service
docker compose logs node-app
docker compose logs prometheus
docker compose logs grafana
```

## 🔍 Troubleshooting

### Port Conflicts

If you encounter port conflicts:

```bash
# Check what's using the ports
lsof -i :3000
lsof -i :9090
lsof -i :3002

# Kill conflicting processes
kill <PID>
```

### Container Issues

```bash
# Rebuild containers
docker compose up --build --force-recreate

# Remove all containers and volumes
docker compose down -v
```

## 📚 Learn More

- [Prometheus Documentation](https://prometheus.io/docs/)
- [Grafana Documentation](https://grafana.com/docs/)
- [Express.js Documentation](https://expressjs.com/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

**Happy Monitoring!** 📊🚀
