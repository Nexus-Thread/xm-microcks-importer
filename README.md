# xm-microcks-importer

A slim Python 3.11 Docker image with predefined dependencies, designed to run custom scripts at runtime.

## 🐳 Docker Image

The Docker image is automatically built and published to **Docker Hub** via GitHub Actions.

### Pull the Image

```bash
docker pull owinter92/xm-microcks-importer:latest
```

### Available Tags

- `latest` - Latest build from the main branch
- `v*.*.*` - Semantic version tags (e.g., `v1.0.0`)
- `main-<sha>` - Specific commit builds
- Multi-platform support: `linux/amd64` and `linux/arm64`

## 📦 Usage

### Run a Python Script

Mount your script directory and execute your Python script:

```bash
docker run --rm -v $(pwd)/scripts:/scripts \
  owinter92/xm-microcks-importer:latest \
  python /scripts/my_script.py
```

### Interactive Python Shell

```bash
docker run --rm -it owinter92/xm-microcks-importer:latest python
```

### Run with Environment Variables

```bash
docker run --rm \
  -e MY_VAR=value \
  -v $(pwd)/scripts:/scripts \
  owinter92/xm-microcks-importer:latest \
  python /scripts/my_script.py
```

### Run with Volume Mounts for Input/Output

```bash
docker run --rm \
  -v $(pwd)/input:/input:ro \
  -v $(pwd)/output:/output \
  owinter92/xm-microcks-importer:latest \
  python /scripts/process_data.py
```

## 🔧 Local Development

### Prerequisites

- Docker (with BuildKit support)
- Python dependencies listed in `requirements.txt`

### Build the Image Locally

```bash
docker build -t xm-microcks-importer:local .
```

### Test the Image

```bash
# Check Python version
docker run --rm xm-microcks-importer:local

# List installed packages
docker run --rm xm-microcks-importer:local pip list
```

## 📝 Customizing Dependencies

1. Edit the `requirements.txt` file and add your Python dependencies:

```txt
requests==2.31.0
pyyaml==6.0.1
click==8.1.7
```

2. Rebuild the image:

```bash
docker build -t xm-microcks-importer:local .
```

3. Commit and push your changes to trigger the GitHub Actions workflow.

## 🚀 CI/CD

The project uses GitHub Actions to automatically build and publish Docker images.

### Workflow Triggers

- **Push to `main`**: Builds and publishes with `latest` tag
- **Version tags** (`v*.*.*`): Builds and publishes with semantic version tags
- **Pull requests**: Builds but does not publish (for testing)
- **Manual trigger**: Via workflow_dispatch

### Publishing Process

1. Make your changes to `requirements.txt` or `Dockerfile`
2. Commit and push to the `main` branch
3. GitHub Actions will automatically:
   - Build the multi-platform image (linux/amd64, linux/arm64)
   - Push to Docker Hub
   - Tag appropriately based on the trigger

### Creating a Release

To publish a versioned release:

```bash
git tag v1.0.0
git push origin v1.0.0
```

This will build and publish the image with tags: `v1.0.0`, `v1.0`, `v1`, and `latest`.

## 🔒 Security Features

- **Multi-stage build**: Minimal attack surface
- **Slim base image**: `python:3.11-slim` reduces vulnerabilities
- **Non-root user**: Runs as `appuser` (UID 1000)

## 📄 License

See [LICENSE](LICENSE) file for details.

## 🤝 Contributing

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add some amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request
