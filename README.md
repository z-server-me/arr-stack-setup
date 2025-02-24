# Docker Media Services Setup

## Overview
This setup includes multiple media management services running in Docker containers using images from LinuxServer.io. The stack includes:

- **Lidarr** (Music Management)
- **Radarr** (Movie Management)
- **Sonarr** (TV Show Management)

## Services Configuration

### Lidarr (ldr)
- **Container Name**: `ldr`
- **Image**: `lscr.io/linuxserver/lidarr:${TAG_NIG}`
- **Ports**: `8686:8686`
- **Volumes**:
  - Configuration: `${VOLUME_VAR_LIB}/lidarr/config:/config:rw`
  - Music Library: `${VOLUME_MUSICS}`
  - Downloads: `${VOLUME_DOWNLOAD}`
- **Healthcheck**: Monitored via API with interval `${INTERVAL}`
- **Additional Features**:
  - FLAC to MP3 conversion enabled via `DOCKER_MODS=linuxserver/mods:lidarr-flac2mp3`

### Radarr (rdr)
- **Container Name**: `rdr`
- **Image**: `lscr.io/linuxserver/radarr:${TAG_DEV}`
- **Ports**: `7878:7878`
- **Volumes**:
  - Configuration: `${VOLUME_VAR_LIB}/radarr/config:/config:rw`
  - Movies Library: `${VOLUME_MOVIE}`
  - Downloads: `${VOLUME_DOWNLOAD}`
- **Healthcheck**: API-based monitoring with interval `${INTERVAL}`

### Sonarr (snr)
- **Container Name**: `snr`
- **Image**: `lscr.io/linuxserver/sonarr:${TAG_LAT}`
- **Ports**: `8989:8989`
- **Volumes**:
  - Configuration: `${VOLUME_VAR_LIB}/sonarr/config:/config:rw`
  - TV Shows Library: `${VOLUME_TVSHOW}`
  - Anime Library: `${VOLUME_TVANIM}`
  - Downloads: `${VOLUME_DOWNLOAD}`
- **Healthcheck**: API-based monitoring with interval `${INTERVAL}`

## Environment Variables
- `RESTART=always` - Restart policy for containers
- `UID=1031`, `GID=100` - User and group permissions
- `UMASK=002` - File permission mask
- `TZ=Europe/Paris` - Timezone setting
- `INTERVAL=2m`, `RETRIES=5`, `TIMEOUT=300s` - Healthcheck parameters
- `MEM_G=2g` - Memory limits per container
- `VOLUME_VAR_LIB=/var/lib/docker/volumes`
- `VOLUME_DOWNLOAD=/downloads/incoming:/downloads:rw`
- `VOLUME_MOVIE=/movies:/movies:rw`
- `VOLUME_TVSHOW=/serie:/serie:rw`
- `VOLUME_TVANIM=/anime:/anime:rw`
- `VOLUME_MUSICS=/musics:/music:rw`

## Usage
To start the services, run:
```sh
docker-compose up -d
```

To stop the services:
```sh
docker-compose down
```

## Notes
- Ensure the `.env` file is properly configured with necessary variables.
- Volumes should be correctly mapped to avoid data loss.
- Use `docker logs <container_name>` to troubleshoot issues.

## Disclaimer
Use this setup at your own risk. Ensure compliance with legal regulations for media management in your jurisdiction.

