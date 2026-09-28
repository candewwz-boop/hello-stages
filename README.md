Hello stages

Es una app sencilla en Node que tienen 3 rutas y corre en tres entornos distintos usando Docker Compose (Desarrollo, prueba y produccion).

¿Que hace?

- / Devuelve un Hello World con info del proyecto
- /saludo?nombre=Ana Devuelve un saludo con el nombre que le pases
- /api/info muestra en que stage esta corriendo (devlopment,test o production) y la version de Node

Para correrlo local: Necesitas Node 24 y Git instalados
-  npm run dev
  Con esto levantas en http://localhost:3000 y se reinicia solo si cambias aldo en src/server.js

Para correr los test:
-npm test
Son 3 pruebas que verifican las tres rutas de arriba

Con Docker: Hay 3 perfiles configurados en compose.yaml. Hace falta tener Docker Dektop abierto
Desarrollo:
- docker compose --profile dev up --build
Se ve en http://localhost:3000 y /api/info tiene que decir development

Modo de prueba: corre los test dentro del contenedor
- docker compose --profile test up --build --abort-on-container-exit --exit-code-from app-test

Produccion: esta es la que se usa el puerto 8080 en vez de 3000
- docker compose --profile prod up --build -d
/api/info en http://localhost:8080 tiene que decir production

Para bajar cualquiera de los 3 es: docker compose --profile <nombre> down

Cada perfil usa su propio archivo de variables dentro de una carpeta env/ (dev.env, test.env, prod.env), con el stage y la version correspondiente.
No tiene ningun dato sensible, son solo esas dos variables mas el puerto.

Sobre el repositorio: Trabajamos cada funcionalidad en una rama distinta (feature/saludo y feature/info) y despues las integramos a main. Al hacer la 2da rama se genero un conflicto real porque ambas tocaban la misma linea (PROJECT_NAME), que resolvimos manteniendo el proposito de las dos features. Se puede ver todo el historial con:git log --oneline --graph --decorate --all

Integrantes:
-Sanchez Candela
-Acoria Lionel
