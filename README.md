# cypress-docker-novnc
Run Cypress inside a Docker container with noVNC to access Cypress Test Runner in the web browser. This lets you develop tests with others through (e.g.) [Visual Studio Live Share](https://visualstudio.microsoft.com/services/live-share/) allowing participants to share the Cypress Test Runner screen.

## docker-compose.yml

```yml
version: '3.2'
services:
  cypress:
    image: cypress/included:6.3.0
    environment:
      - DISPLAY=novnc:0.0
    depends_on:
      - novnc
    entrypoint: cypress open --project /e2e
    working_dir: /e2e
    volumes:
      - ./:/e2e
  novnc:
    image: theasp/novnc:latest
    environment:
      - DISPLAY_WIDTH=1280
      - DISPLAY_HEIGHT=720
      - RUN_XTERM=no
    ports:
      - "8080:8080"
```
