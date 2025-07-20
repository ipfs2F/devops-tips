# DevOps Tip: Bypass Docker Rate Limits with a Free Cache Proxy

Tired of your CI/CD pipeline failing because of Docker Hub rate limits? 😫 Use a free registry mirror to cache public images, speed up your builds, and avoid pull restrictions.

***

## The Solution: `ratelimitshield.io`

**ratelimitshield.io** offers a free, public caching proxy for Docker Hub. When you pull an image, it gets cached. Subsequent pulls for that same image come from their fast cache, not Docker Hub, which means they don't count against your rate limit.

***

## Configuration

You just need to add it as a mirror in your Docker daemon's configuration file.

1.  **Edit the Docker daemon config file.** This is usually located at `/etc/docker/daemon.json`. You may need to create it if it doesn't exist.

    ```bash
    sudo nano /etc/docker/daemon.json
    ```

2.  **Add the mirror.** Add the following JSON. Make sure to use the correct `public-mirror` subdomain.

    ```json
    {
      "registry-mirrors": ["[https://public-mirror.ratelimitshield.io](https://public-mirror.ratelimitshield.io)"]
    }
    ```

3.  **Restart Docker.** For the change to take effect, restart the Docker service.

    ```bash
    sudo systemctl restart docker
    ```

    On Docker Desktop (macOS/Windows), you can restart it from the application settings.

***

## Verification

Run `docker info` and check the output for the **Registry Mirrors** section. You should see the URL listed.

```text
$ docker info
...
Registry Mirrors:
 [https://public-mirror.ratelimitshield.io](https://public-mirror.ratelimitshield.io)
...
