version: "3.7"

services:
  litecoincash:
    image: litecoincash/litecoincash:latest
    restart: unless-stopped
    volumes:
      - lcc-data:/home/lcc/.litecoincash
    ports:
      - "62458:62458"

  ckpool:
    image: ghcr.io/ckolivas/ckpool:latest
    restart: unless-stopped
    depends_on:
      - litecoincash
    ports:
      - "3333:3333"
    volumes:
      - ./ckpool.conf:/ckpool.conf
    command: ["-c", "/ckpool.conf"]

volumes:
  lcc-data:
