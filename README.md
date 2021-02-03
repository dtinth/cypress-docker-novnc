# cypress-docker-novnc
Run Cypress inside a Docker container with noVNC to access Cypress Test Runner in the web browser.

## Use cases
- Develop tests in a cloud-based development platform such as [GitHub Codespaces](https://github.com/codespaces) without having to install Cypress locally.
- Develop tests [collaboratively in real-time](https://dt.in.th/synchronous-remote-collaboration.html) with others through (e.g.) [Visual Studio Live Share](https://visualstudio.microsoft.com/services/live-share/) allowing participants to share the Cypress Test Runner screen.

## Usage

Copy and paste this into `docker-compose.yml`:

```yml
version: '3.2'
services:
  cypress:
    image: cypress/included:6.3.0
    environment:
      - DISPLAY=novnc:0.0
    depends_on:
      - novnc
    entrypoint: []
    command: bash -c 'npx wait-on http://novnc:8080 && cypress open --project /e2e'
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

Then run `docker-compose up` (append `-d` to go to background). Once the container starts up, go to <http://localhost:8080/vnc.html?autoconnect=true>. You should now see this:

![screenshot](https://user-images.githubusercontent.com/193136/105757006-fae92200-5f7f-11eb-8e4b-eed64ad9d305.png)
