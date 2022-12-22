This is currently a work in progress, and I'd appreciate any help anyone is willing to give on this. Obviously, change all the occurences of `.domain.tld`, and anything that is redacted. Adjust anything that suits your needs. As it is currently set up, salt-rim will not connect to bar-assistant. but bar assistant connects to meilisearch, redis works fine and connects to everything. They all show in traefik dash. Something isn't jiving with traefik or something. I'm not sure. 

## Original compose
From [Bar Assistant](https://github.com/bar-assistant/docker/blob/master/docker-compose.yml) & [Salt Rim](https://github.com/karlomikus/vue-salt-rim)
```yaml
version: "3"

services:
  # Setup Meilisearch instance
  meilisearch:
    image: getmeili/meilisearch:v0.29
    environment:
      - MEILI_MASTER_KEY=$MEILI_MASTER_KEY
      - MEILI_ENV=production
    restart: unless-stopped
    ports:
      - 7700:7700
    volumes:
      - ba-meilidata:/meili_data

  # Redis for session and file cache
  redis:
    image: redis
    environment:
      - ALLOW_EMPTY_PASSWORD=yes
    restart: unless-stopped

  # Bar Assistant API
  bar-assistant:
    image: kmikus12/bar-assistant-server
    depends_on:
      - meilisearch
      - redis
    environment:
      - APP_URL=$API_URL
      - MEILISEARCH_KEY=$MEILI_MASTER_KEY
      - MEILISEARCH_HOST=$MEILI_HOST
      - REDIS_HOST=redis
    restart: unless-stopped
    volumes:
      - ba-storage:/var/www/cocktails/storage
    ports:
      - 8000:80

  # Bar Assistant Client
  salt-rim:
    image: kmikus12/salt-rim
    depends_on:
      - meilisearch
      - bar-assistant
    environment:
      - API_URL=$API_URL
    restart: unless-stopped
    ports:
      - 8080:8080

volumes:
  ba-meilidata:
  ba-storage:
```
