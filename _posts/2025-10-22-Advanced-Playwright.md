---
layout: post
title: "Notas de estudio curso Advanced Playwright"
date: 2025-10-22
excerpt: "Advanced Playwright - TAU"
project: true
tags: [testing, Playwright, automation]
comments: true
---

- [Recursos:](#recursos)
- [Capítulos:](#capítulos)
  - [Optimización de la autenticación](#optimización-de-la-autenticación)
  - [Page Objects dinámicos y fixtures](#page-objects-dinámicos-y-fixtures)
  - [Interacción con APIs](#interacción-con-apis)
  - [Gestión de datos](#gestión-de-datos)
  - [Regresión visual con Applitools](#regresión-visual-con-applitools)
  - [Integración continua y observabilidad](#integración-continua-y-observabilidad)

[Advanced Playwright de Test Automation University](https://testautomationu.applitools.com/playwright-advanced/) es un curso avanzado sobre el framework Playwright para pruebas automatizadas de aplicaciones web modernas, realizado por [Renata Andrade](https://www.linkedin.com/in/raptatinha/).

## Recursos: 
- [Playwright web oficial](https://playwright.dev/)
- [Github repo del curso](https://github.com/raptatinha/tau-advanced-playwright)
- [Demoqa para practicar](https://demoqa.com/login) 
- [Intro playwright](https://testautomationu.applitools.com/playwright-intro/)
- Se recomienda crear un usuario propio en Demo QA para evitar problemas de sesión.
- El curso está dirigido a quienes ya conocen los [fundamentos de Playwright](https://testautomationu.applitools.com/playwright-intro/) y quieren profundizar en su uso profesional y eficiente.

## Capítulos:

### Optimización de la autenticación 
  > Técnicas para almacenar y reutilizar el estado de inicio de sesión y acelerar las pruebas.

  ¿Cómo optimizar el proceso de autenticación en pruebas automatizadas usando Playwright?

  Existen dos enfoques principales:
  1. El flujo típico de autenticación (método tradicional)
  2. La optimización mediante Project Dependencies (método recomendado)

  Veamos cada uno en detalle:
  
  - **Flujo típico de autenticación:**
  
  Un flujo de inicio de sesión clásico en una prueba, con un paso beforeEach, el cual se ejecuta antes de cada prueba.
  
  ```typescript
  test.beforeEach(async ({ page }) => {
    await page.goto(pages.loginPage);
    loginPage = new LoginPage(page);
  });
  ```
  Luego, 3 típicas pruebas para un inicio de sesión clásico, login exitoso, y dos fallidos por datos invalidos en el nombre de usuario y el password.

  ```typescript
  test.describe('Book Store - Login', () => {
    test(`successfull login`, async () => {
      await loginPage.doLogin(userName, password);
      await loginPage.checkLoggedIn();
    });

    test(`failing login - invalid username`, async () => {
      const invalidUsername = userData.invalidUsername;
      await loginPage.doLogin(invalidUsername, password);
      await loginPage.checkInvalidCredentials();
    });

    test(`failing login - invalid password`, async () => {
      const invalidPassword = userData.invalidPassword;
      await loginPage.doLogin(userName, invalidPassword);
      await loginPage.checkInvalidCredentials();
    });
  });
  ```
  El flujo clásico requiere repetir estos pasos de autenticación para cada test, lo que los hace lentos.

  - **Optimización almacenando el estado con Project Dependencies:** 
  
  Playwright ofrece una solución elegante para optimizar la autenticación mediante Project Dependencies y almacenamiento del estado. Esta técnica permite:

  1. **Configuración del proyecto**: Primero defino un proyecto setup para la autenticación en el archivo `playwright.config.ts`, pensando a futuro, defino un match para incluir posibles setups. Y en mi proyecto ci, agrego como dependencia el proyecto creado, esa es la definición de Project Dependencies y tambien agrego una referencia a un estado almacenado (storageState), ese estado lo escribirá el setup:

  ```typescript
  // playwright.config.ts
  export default defineConfig({
    projects: [
      {
        name: 'setup',
        testMatch: /.*\.setup\.ts/,
      },
      {
      name: 'ci',
      use: { 
         ...devices['Desktop Chrome'] ,
        baseURL: baseEnvUrl.ci.home,
        storageState: '../playwright-report/.auth/user.json', 
      },
      dependencies: ['setup'],
    },
    ],
  });
  ```
  2. **Setup auth**: Creamos un archivo que maneje la autenticación una única vez, este será mi setup:

  ```typescript
  // auth.setup.ts
  import * as path from 'path';
  import { test as setup, expect } from '@playwright/test';
  import { LoginPage } from '../pages/LoginPage';

  let loginPage: LoginPage;
  const authFile = path.join(__dirname, '../playwright-report/.auth/user.json');

  setup('authenticate', async ({ page }, testInfo) => {
    loginPage = await new LoginPage(page).goto();
    const chatHomePage = await loginPage.login('user_basic', 'password123');
    const userName = await chatHomePage.userProfile.getUserName();
    const userEmail = await chatHomePage.userProfile.getUserEmail();

    expect(userName).toContain('user_basic');
    expect(userEmail).toContain('user_basic@localhost');

    await page.context().storageState({ path: authFile });
  });
  ```
  Este setup son las instrucciones para hacer login en el sitio, en mi caso, usé mi page ya definida con POM, al final, almaceno el estado en la misma ruta que definí como storageState en mi proyecto de ci.

  3. **Reutilización del estado**: En los tests, podemos usar el estado guardado:

  ```typescript
  // test.spec.ts
  test.use({ storageState: './auth.json' });

  test('acceder a área protegida', async ({ page }) => {
    await page.goto('https://demoqa.com/profile');
    // El test ya está autenticado
  });
  ```
  Por lo que no debo realizar login en todos mis tests, ya que el setup se ejcutó antes y el estado de autenticación fue almacenado.

  **Beneficios clave:**
  - Reduce significativamente el tiempo de ejecución de las pruebas
  - Minimiza el desgaste de credenciales
  - Evita problemas de rate limiting en el login
  - Hace los tests más confiables y mantenibles

  **Consideraciones importantes:**
  - Debe eliminar el estado almacenado cuando caduque
  - El estado de autenticación se mantiene entre tests
  - Si no necesita conservar el estado entre ejecuciones de prueba, escriba el estado del navegador en testProject.outputDir, que se limpia automáticamente antes de cada ejecución de prueba.
  - Es recomendable ejecutar los tests en serie si comparten estado
  - Hay que renovar el estado almacenado si expira la sesión

  Esta optimización es especialmente útil en suites de pruebas grandes donde el proceso de login repetitivo puede consumir mucho tiempo.

### Page Objects dinámicos y fixtures
  > Creación flexible de objetos de página y uso de fixtures para agilizar flujos en los tests.

  El Capítulo se centra en mejorar la reutilización y escalabilidad de pruebas automatizadas mediante dos conceptos clave: **Dynamic Page Objects (POM dinámicos)** y **Fixtures**

  - **1. Dynamic Page Objects**
  La idea de los Page Objects dinámicos es crear y reutilizar instancias de páginas sin repetición de código. En lugar de crear un objeto de página distinto dentro de cada test, se centraliza la lógica usando un método común dentro de un archivo de hooks (por ejemplo, hooks.beforeEach).

  Este método recibe parámetros como:

  - page (el driver de Playwright), PageObjectParam (el tipo de página: LoginPage, BookPage, ProfilePage), targetPage (una cadena para identificar la URL), y extra params (opcionales).

  Con esto, el hook puede construir automáticamente la URL (gracias al método buildUrl) y devolver el Page Object correcto, reduciendo líneas de código y haciendo las pruebas más limpias y rápidas.

  Beneficio principal: una sola línea reemplaza la creación y navegación manual a la página, acelerando el flujo de desarrollo.

  - **2. El método buildUrl**
  El método buildUrl genera rutas dinámicas dependiendo del nombre de la página y sus parámetros. Por ejemplo:

  ```typescript
  const url = buildUrl('books', { id: 123 }) // → '/books?id=123'
  ```
  Esto permite un acceso dinámico y flexible a las distintas secciones del sitio sin necesidad de hardcodear rutas.

  - **3. Introducción a Fixtures**
  Una fixture se puede entender como una evolución del hook. Mientras los hooks se usan para preparar el entorno técnico, las fixtures agrupan pruebas por su propósito funcional.

  Una fixture define:

  - qué objetos de página estarán disponibles,
  - qué acciones se ejecutan antes y después del test,
  - y cómo compartir el contexto entre distintas pruebas.

  El flujo típico es:
  - El test detecta una fixture y la invoca automáticamente.
  - La fixture crea el Page Object usando los hooks.
  - Llama al método use() para entregar el control al test.
  - Tras finalizar el test, vuelve a ejecutar las acciones de limpieza post-prueba.

  Esto permite implementar distintos escenarios (como limpieza de datos, login con diferentes usuarios, etc.) sin reescribir el flujo base.

### Interacción con APIs
  > El capítulo 3 explica cómo integrar la interacción con APIs directamente en tus tests, utilizando Playwright para automatizar y validar distintos flujos de tu aplicación de forma rápida y mantenible.

  **1. APIRequestContext**
  Playwright abstrae las llamadas HTTP para poder hacer requests sin necesidad de usar la interfaz gráfica del navegador. Así se puede crear conexiones autenticadas en el hook beforeAll:

  ```typescript
  import { request } from '@playwright/test';

  let apiContext;

  beforeAll(async ({ playwright }) => {
    apiContext = await playwright.request.newContext({
      baseURL: 'https://demoqa.com',
      extraHTTPHeaders: {
        Authorization: `Basic ${Buffer.from(`usuario:contraseña`).toString('base64')}`,
        Accept: 'application/json'
      }
    });
  });
  ```
  Este contexto permite hacer peticiones GET, POST, DELETE, etc., directamente en las pruebas.

  **2. Estructura estándar de una petición API**
  Cada petición sigue el mismo patrón reutilizable:

  ```typescript
  const response = await apiContext[method](requestUrl, requestOptions);
  ```

  Componentes clave:
  method: get, post, put, delete
  requestUrl: dirección construida dinámicamente
  requestOptions: cuerpo de la petición, parámetros, headers

  Ejemplo:
  ```typescript
  await apiContext.post('/BookStore/v1/Books', {
    data: { userId, collectionOfIsbns: [{ isbn }] }
  });
  ```

  **3. Funciones y utilidades reutilizables**
  El curso recomienda separar la lógica en archivos de utilidades:

  apiEndpoints.ts → mapea endpoints.
  apiMethods.ts → mapea métodos HTTP.
  apiUrlBuilder.ts → construye URLs y query params.
  apiRequestUtils.ts → ejecuta la petición y gestiona el manejo de errores.

  Esto permite que todo el manejo de llamadas API sea centralizado y fácil de replicar.

  **4. Combinar API y pruebas UI**
  La gran ventaja de usar APIs es preparar datos o limpiar el estado antes de las pruebas de interfaz:

  ```typescript
  async function cleanBooks(userId: string, page) {
    await deleteBookAPIRequest.deleteAllBooksByUser(apiContext, userId);
  }

  test('Añadir libro nuevo', async ({ page, bookPage }) => {
    await cleanBooks(userId, page);
    await bookPage.goto(userData.books.new);
  });
  ```

  Así se garantiza condiciones iniciales limpias y repetibles.

  **5. Manejo centralizado de errores**
  La utilidad executeRequest envuelve la ejecución para capturar y controlar errores uniformemente:

  ```typescript
  try {
    const response = await apiContext[method](requestUrl, requestOptions);
    if (!response.ok()) {
      throw new Error(`Error ${response.status()}: ${await response.text()}`);
    }
  } catch (error) {
    console.error('Fallo la petición:', error);
  }
  ```
  Esto ayuda a diagnosticar fallos de autenticación o integridad de datos de manera ágil.

### Gestión de datos
  > El Capítulo 4 enseña cómo administrar correctamente los datos en pruebas automatizadas, asegurando entornos reproducibles y seguros. Se centra en dónde y cómo almacenar los datos de prueba y qué buenas prácticas seguir para evitar fugas de información sensible.

  **1. Variables de entorno (.env)**
  Los archivos .env se usan para almacenar credenciales o configuraciones sensibles, como URLs, usuarios o claves API:

  ```bash
  USERNAME=myUser
  PASSWORD=myPassword
  BASE_URL=https://demoqa.com
  ```

  En el código, se acceden con:

  ```typescript
  console.log(process.env.BASE_URL);
  ```
  **Importante**: estos archivos no deben subirse al repositorio (usa .gitignore). En su lugar, puedes incluir un .env.example para referencia.

  **2. Archivos JSON**
  Los archivos .json son útiles para mantener datos de prueba estructurados. Ejemplo:

  ```json
  {
    "user": {
      "name": "jorge",
      "role": "admin",
      "email": "jorge@example.com"
    }
  }
  ```
  Pueden ser importarlos fácilmente en los test:

  ```typescript
  import testData from '../data/testData.json';
  console.log(testData.user.name);
  ```

  Esto permite mantener los datos separados de la lógica y simplificar su edición y mantenimiento.

  **3. Datos gestionados vía API**
  Manipular datos mediante API calls es más recomendable que hacerlo directamente en la base de datos, porque la API incluye reglas de negocio y validaciones que garantizan la consistencia del sistema:

  ```typescript
  await apiContext.post('/BookStore/v1/Books', {
    data: { userId, collectionOfIsbns: [{ isbn }] }
  });
  ```

  Esto asegura que los datos creados en pruebas sigan las mismas reglas del producto real.

  **4. Mocking y datos falsos**
  Si no hay datos reales disponibles o el entorno de prueba no puede conectarse, se pueden mockear las respuestas API para simular escenarios:

  ```typescript
  await page.route('**/api/books', route => {
    route.fulfill({
      status: 200,
      body: JSON.stringify({ books: [{ title: 'Libro simulado', author: 'Playwright Bot' }] })
    });
  });
  ```

  El mocking permite probar la interfaz incluso cuando el backend no está listo.

  **5. Datos desde CSV**
  Playwright permite parametrizar tests a partir de archivos CSV:

  ```typescript
  import { test } from '@playwright/test';

  const { readFileSync } = require('fs');
  const data = readFileSync('users.csv', 'utf8').split('\n').map(line => line.split(','));

  data.forEach(([username, password]) => {
    test(`Login test - ${username}`, async ({ page }) => {
      await page.goto('/login');
      await page.fill('#user', username);
      await page.fill('#password', password);
      await page.click('#submit');
    });
  });
  ```

  Esto facilita ejecutar el mismo test con diferentes conjuntos de datos.

  **Ejemplos de aplicación práctica**
  - Configuración segura: Define claves y URLs en un .env (nunca en el código).
  - Manejo centralizado de datos: Crear un archivo JSON con todos los perfiles de prueba.
  - Validación de API antes del test UI: Usar la API para crear o limpiar datos.
  - Uso de mocks: Simular respuestas del servidor si el backend está en mantenimiento.
  - Parámetros desde CSV: Ejecutar pruebas masivas con diferentes usuarios.

  Con estas prácticas, las pruebas serán más seguras, modulares y confiables.

### Regresión visual con Applitools
  > El Capítulo 5 enseña cómo incorporar la regresión visual automatizada en las pruebas utilizando Applitools. Este enfoque permite detectar cambios inesperados en la interfaz de usuario de forma inteligente, más allá de simples comparaciones de imágenes.

  **1. ¿Qué es la regresión visual?**
  Una regresión visual ocurre cuando un cambio de código afecta el diseño o apariencia de la UI sin intención. Las pruebas visuales ayudan a detectarlas al comparar capturas de pantalla actuales con una línea base (baseline).

  [Applitools](https://applitools.com/), con su tecnología de IA, detecta sólo las diferencias visuales significativas, evitando falsos positivos provocados por cambios menores o dinámicos.

  **2. Integración básica de Applitools y Playwright**
  Para habilitar la integración, se debe instalar el SDK de Applitools:

  ```bash
  npm install @applitools/eyes-playwright --save-dev
  ```

  Y configurar la API Key en un archivo .env:

  ```bash
  APPLITOOLS_API_KEY=tu_clave_api
  ```

  **3. Flujo principal de un test visual**
  
  ```typescript
  import { test } from '@playwright/test';
  import { Eyes, VisualGridRunner, Target, Configuration } from '@applitools/eyes-playwright';

  test('Prueba de control visual', async ({ page }) => {
    const runner = new VisualGridRunner({ testConcurrency: 1 });
    const eyes = new Eyes(runner);

    await eyes.open(page, 'Demo App', 'Visual Test', { width: 1024, height: 768 });
    await page.goto('https://demoqa.com/books');

    await eyes.check('Página de libros', Target.window().fully());
    await eyes.close();
  });
  ```

  Explicación del flujo:
  - eyes.open → abre la sesión visual.
  - eyes.check → toma y compara la captura visual.
  - eyes.close → cierra y envía los resultados al dashboard.

  **4. Configuraciones y modos de comparación**
  Applitools ofrece flexibilidad en cómo se comparan las imágenes:
  
  - Target.window().fully() → captura toda la ventana visible.
  - Target.window().fully().layout() → ignora cambios de contenido (solo verifica estructura del layout).
  - Target.region(locator) → verifica una región específica de la página.
  - También se pueden ignorar áreas dinámicas, como banners o anuncios, para evitar fallos falsos:

  ```typescript
  Target.window().fully().ignoreRegions('#popup');
  ```

  **5. Resultados de comparación**
  Una vez completada la ejecución, Applitools mostrará los resultados en su panel web, donde se puede:

  - Revisar diferencias visuales entre el baseline y la nueva UI.
  - Aprobar o rechazar cambios visuales.
  - Administrar equipos, builds, y versiones de línea base.

  **Ejemplos de aplicación práctica**
  - Validar apariencia tras un despliegue: ejecutar pruebas visuales después de actualizaciones de CSS o frameworks.
  - Verificar consistencia entre navegadores: usar el VisualGrid para correr pruebas visuales en navegadores y resoluciones diferentes simultáneamente.
  - Monitorear cambios inesperados: programar comparaciones nocturnas para detectar regresiones introducidas por nuevas dependencias o cambios de diseño.
  
### Integración continua y observabilidad
  > En el Capítulo 6 enseña las prácticas y configuraciones necesarias para ejecutar pruebas automatizadas dentro de un entorno de integración continua (CI), además de cómo mejorar la observabilidad de los resultados mediante reportes y notificaciones automáticas.

  **1. Pipelines con GitHub Actions**
  El chapter muestra cómo crear y configurar un archivo YAML dentro de la carpeta .github/workflows para ejecutar los tests automáticamente con cada push o pull request.

  Ejemplo básico del archivo playwright.yml:

  ```yml
  name: Playwright Tests

  on:
    push:
      branches: [ main ]
    pull_request:

  jobs:
    test:
      runs-on: ubuntu-latest

      steps:
        - uses: actions/checkout@v3
        - uses: actions/setup-node@v3
          with:
            node-version: 20
        - run: npm ci
        - run: npx playwright install
        - run: npm run test-ui-c
        - run: npx playwright show-report
  ```

  Esto configura un flujo que instala las dependencias, ejecuta los tests, y genera un reporte visual.

  **2. Reportes y observabilidad**
  Los reportes son fundamentales para interpretar rápidamente el estado de los tests. En lugar de revisar logs extensos, los reportes HTML generados por Playwright o publicados como artifacts en el pipeline proporcionan una vista estructurada de resultados, errores y capturas de pantalla.

  Ejemplo:

  ```bash
  npx playwright show-report
  ```

  El capítulo resalta que agregar reportes al pipeline agiliza la investigación de errores y ayuda a mantener la calidad de manera continua.

  **3. Integración con herramientas de comunicación (Slack)**
  Conectar el pipeline a un canal de comunicación (como Slack o Teams) permite recibir feedback inmediato tras cada ejecución.

  Ejemplo de buenas prácticas en la notificación:
  - Incluir autor del commit y su foto.
  - Agregar links al commit, reporte HTML y logs del build.
  - Incorporar emojis o etiquetas de estado (✅ o ❌) para identificar rápidamente el resultado.
  - Esto mejora la transparencia y promueve la colaboración entre QA, desarrollo y DevOps.

  **4. Paralelización y Sharding**
  Para proyectos grandes, ejecutar tests en paralelo ahorra tiempo. El parámetro shardTotal determina cuántas divisiones de tests se ejecutan simultáneamente.

  Ejemplo de ejecución con sharding:

  ```bash
  npx playwright test --shard=1/4
  ```

  Esto ejecuta el primer cuarto de la suite en un job. Ajustar shardTotal requiere balancear la infraestructura y cantidad de tests.

  La clave es experimentar hasta encontrar la configuración que reduzca tiempos sin saturar recursos.

  Ejemplos de aplicación práctica
  - Pipeline con múltiples jobs: separar tests de UI, API y regresión visual en diferentes workflows para acelerar feedback.
  - Reportes versionados: guarda el reporte HTML como artifact o publícalo con GitHub Pages.
  - Notificaciones automáticas: configura un webhook de Slack que notifique sólo si la build falla.
  - Paralelización inteligente: define shards según tamaño de test suite o tipo de navegador.

  Al adoptar esta cultura de observabilidad continua, se reducen tiempos de detección de errores y fortalecen el trabajo colaborativo dentro del equipo.
