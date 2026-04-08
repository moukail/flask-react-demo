Vitejs
======
```bash
podman run -it --rm -w /var/www/app -v "$(pwd):/var/www/app:z" docker.io/library/node:25.8-alpine3.23 \
 npm create vite@latest frontend -- --template react-ts

podman run -it --rm -w /var/www/app -v "$(pwd):/var/www/app:z" docker.io/library/node:25.8-alpine3.23 \
 npm install @mui/material @mui/icons-material @emotion/react @emotion/styled axios react-router-dom @tanstack/react-query react-hook-form
```

React
=====
```bash
podman run -it --rm -w /var/www/app -v "$(pwd)/frontend:/var/www/app:z" docker.io/library/node:25.8-alpine3.23 \
 npm install -D vitest @vitest/browser-webdriverio @testing-library/react @testing-library/user-event @testing-library/jest-dom jsdom msw

#podman run -it --rm -w /var/www/app -v "$(pwd):/var/www/app:z" docker.io/library/node:25.8-alpine3.#23 \
# npm install -D vitest @vitest/browser-playwright vitest-browser-react @testing-library/react @testing-library/user-event @testing-library/jest-dom jsdom msw

podman run -it --rm -w /var/www/app -v "$(pwd):/var/www/app:z" docker.io/library/node:25.8-alpine3.23 npx vitest run

```