# Docker deployment

## Run

```bash
docker run -d --name subconverter --restart=always \
  -p 25500:25500 \
  ghcr.io/hooleeas/subconverter-new:latest
```

Verify the container:

```bash
curl http://localhost:25500/version
```

The expected response is:

```text
subconverter-new v0.9.10 backend
```

## Docker Compose

```yaml
services:
  subconverter:
    image: ghcr.io/hooleeas/subconverter-new:latest
    container_name: subconverter
    ports:
      - "25500:25500"
    restart: always
```

## Updating preferences

Upload a preference file with the configured API token:

```bash
curl -F "data=@newpref.ini" \
  "http://localhost:25500/updateconf?type=form&token=password"
```

## Custom image

Copy replacement preferences, rules, snippets, or profiles into `/base/`:

```dockerfile
FROM ghcr.io/hooleeas/subconverter-new:latest
COPY replacements/ /base/
EXPOSE 25500
```

Build and run it:

```bash
docker build -t subconverter-new-custom:latest .
docker run -d --name subconverter --restart=always \
  -p 25500:25500 subconverter-new-custom:latest
```
