# Ejercicio 03 - Profundizando en Workflows y Eventos

Ahora para esta sección del curso he preparado otro proyecto de ejemplo, que en realidad está basado en el mismo proyecto de ejemplo anterior, pero que ya viene con un workflow, un archivo **eventos.yml** situado en la carpeta **.github>workflows** donde tenemos definido un workflow de demostración muy simple. Sin embargo, falta el evento porque eso es exactamente lo que revisaremos y en lo que nos sumergiremos en este módulo del taller.

<pre>
name: Ejercicio con Eventos
<span style="color:DarkRed;">on: ...</span>
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Mostrar datos del evento
        run: echo "${{ toJSON(github.event) }}"
      - name: Obtener código
        uses: actions/checkout@v4
      - name: Instalar dependencias
        run: npm ci
      - name: Ejecutar pruebas
        run: npm run test
      - name: Compilar código
        run: npm run build
      - name: Desplegar el proyecto
        run: echo "Desplegando..."
</pre>

A continuación tenemos 2 secciones claramente diferenciadas. La primera detallará los pasos que debemos seguir para preparar nuestro entorno para el desarrollo del ejercicio. Y la segunda parte detalla el desarrollo completo de los ejercicios, paso a paso.

## Preparación del ejercicio

1. Para empezar a trabajar en este ejercicio, debemos crear nuestra propia copia de este repositorio. Para esto, debemos crear un **fork** del repositorio haciendo click en el desplegable al lado de la opción de **Fork** y seleccionando la opción *Create a new fork*.
   
   	![Crear fork](img/fork-repo.png)

2. Luego, en los detalles del fork, seleccionamos nuestra cuenta personal en el desplegable de **Owner** y nos aseguramos que el nombre del repositorio está disponible y con el mensaje en verde, y hacemos clic en el botón verde **Create fork**.
   
   	![Detalles del fork](img/fork-details.png)

    En caso el nombre del repositorio no esté disponible, simplemente cambiamos el nombre por uno que sí lo esté. Esto no cambiará en nada el resultado o la lógica de nuestro desarrollo del ejercicio.

3.  Una vez cargue nuestro nuevo repositorio, hacemos clic en el botón verde **Code**, cambiamos a la pestaña Codespaces en el panel que se muestra y hacemos clic en el botón verde **Create codespace on main**.
   
   	![Crear codespace](img/create-codespace.png)

    En caso el bloqueador de pop-ups del browser interrumpa el lanzamiento del Codespace, otorgar los permisos necesarios y repetir el proceso.

4.  Se abrirá una nueva pestaña en nuestro browser con un entorno de desarrollo basado en Visual Studio Code, hospedado en la Nube de GitHub y con nuestro proyecto cargado y listo para trabajar.
   
   	![Codespace creado](img/codespace-created.png)

    Este entorno de desarrollo elimina nuestra necesidad de instalar localmente las herramientas y librerías necesarias para el desarrollo de nuestros ejercicios.

5. Este paso y el siguiente son meramente opcionales. En caso la presentación del entorno de desarrollo en su configuración por defecto no sea muy cómoda para ti, siempre puedes cambiar el tema de color haciendo clic en el botón de **Manage** (el engranaje en la esquina inferior izquierda), y seleccionar las opciones **Themes > Color Theme**.
   
   	![Cambiar tema de color](img/change-theme.png)

6. Se desplegarán las opciones de tema de color disponibles y, para elegir aquella con la que estemos más cómodos, simplemente debemos navegar las opciones con las flechas arriba y abajo de nuestro teclado y dar **Enter**.
   
   	![Seleccionar tema de color](img/select-theme.png)

    El tema mostrado en esta imagen es sólo referencial.

7. Una vez cargada nuestra copia del repositorio del ejercicio en nuestro entorno de desarrollo, vemos que también se muestra el terminal listo para empezar a ejecutar los comandos que sean necesarios. En este caso, lo primero que vamos a hacer es probar lo que hace nuestra aplicación de ejemplo. Para esto, ingresamos el siguiente comando en nuestro terminal:
   
   ```
   npm install
   ```

    Esto nos debería devolver una salida similar a la siguiente:
   
   ![Salida de compilar nuestra aplicación](img/npm-build-output.png)

8. Luego, para lanzar nuestra aplicación en modo desarrollo, ingresamos este comando:

   ```
   npm run dev
   ```

   Esto nos debería devolver una salida similar a la siguiente, así como una notificación de que podemos acceder a nuestra aplicación haciendo clic en el botón **Open in Browser**, que es justo lo que vamos a hacer:
   
   ![Salida de ejecutar nuestra aplicación](img/npm-run-dev-output.png)

9. Esto lanza nuestra aplicación en una nueva pestaña de nuestro browser y podemos probar su simple funcionalidad.
   
   ![Nuestra aplicación de ejemplo](img/our-sample-app.png)
   
   Una vez probada nuestra aplicación, para detenerla, simplemente volvemos a nuestro entorno de desarrollo, hacemos clic en el terminal para asegurarnos que el foco se encuentra allí y digitamos `Ctrl+C`. Una vez realizado esto, el terminal volverá a estar listo para ingresar nuevos comandos.

10. FinalmenPor último, como todo el propósito de nuestro ejercicio consiste en practicar con las funcionalidades de GitHub Actions, es un buen momento para instalar la extensión de GitHub Actions. Para esto, seleccionamos la opción **Extensions** (aquella que parece una pieza de tetris) en la barra lateral izquierda, en el buscador de extensiones ingresamos el texto `GitHub Actions`, y le damos al botón **Install** a la derecha de la primera extensión mostrada en el listado (aquella que tiene a GitHub como autor verificado).
   
    ![Nuestra aplicación de ejemplo](img/install-github-actions-extension.png)

11. Una vez instalada la extensión de GitHub Actions, seleccionar su ícono en la barra lateral, y hacer clic en el botón **Sign in to GitHub**.
    
    ![Inicio de sesión en GitHub](img/github-signin.png)
   
    Se mostrará un aviso y seleccionamos **Allow**.

    Si el browser nos pide las credenciales de GitHub, las ingresamos y se mostrará el mensaje **You can now close the window.**.

12. Luego de confirmar las credenciales, el panel de GitHub actions nos debe mostrar sus 3 secciones: Current Branch, Workflows y Settings.

    ![Extensión de GitHub Actions](img/github-actions-extension.png)

13. Abrimos nuestro archivo de definición de workflow **eventos.yml**, y agregamos los eventos `push` y `workflow_dispatch`, y guardarmos nuestro archivo.
    
    <pre>
    name: Ejercicio con Eventos
    <span style="color:Navy;"><b>on: [push, workflow_dispatch]</b></span>
    jobs:
      deploy:
        runs-on: ubuntu-latest
        steps:
          - name: Mostrar datos del evento
            run: echo "${{ toJSON(github.event) }}"
          - name: Obtener código
            uses: actions/checkout@v4
          - name: Instalar dependencias
            run: npm ci
          - name: Ejecutar pruebas
            run: npm run test
          - name: Compilar código
            run: npm run build
          - name: Desplegar el proyecto
            run: echo "Desplegando..."
    </pre>

14. En el terminal de nuestro entorno de desarrollo sincronizamos nuestros cambios ingresando los siguientes comandos:

    <pre>
    git add .
    git commit -m "Adición de eventos básicos"
    git push
    </pre>

15. Volvemos a la página de nuestro proyecto e ingresamos a la pestaña de **Actions**, donde se nos mostrará una advertencia indicando que los workflows están desactivados porque venían ya en el repo desde el que se hizo el fork.

    ![Lanzamos nuestro workflow](img/workflows-disabled.png)

    Para activar nuestro workflow, simplemente le damos clic al botón verde **I understand my workflows, go ahead and enable them**.

16. Una vez activados los workflows nuevamente, seleccionamos en el panel de la izquierda nuestro workflow **Ejercicio con Eventos** y en el panel derecho le damos clic a **Run workflow**. Luego de unos segundos, obtendremos el resultado de dicha ejecución:

    ![Lanzamos nuestro workflow](img/run-workflow.png)

17. Luego, podremos ver nuestro workflow en el segundo panel de la extensión de GitHub Actions en nuestro entorno de desarrollo (probablemente sea necesario darle clic al botón **Refresh**).

    ![Lanzamos nuestro workflow](img/workflows-updated.png)

18. (Opcional) Si intentamos lanzar nuestro workflow con el botón **Trigger workflow**, la paleta de comandos nos solicitará confirmar la referencia para la ejecución de nuestro workflow. Aquí podemos confirmar el valor por defecto dándole simplemente a ***Enter***.

    ![Lanzamos nuestro workflow](img/enter-ref.png)

    Sin embargo, esta es una funcionalidad que por el momento todavía no está soportada en GitHub CodeSpaces, por lo que se nos mostrará el siguiente mensaje de error:

    ![Lanzamos nuestro workflow](img/trigger-workflow-error.png)

    A pesar de esto, la extensión no tiene problemas en mostrarnos el estado de los workflows gatillados de cualquier otra forma, tal como en el caso de nuestra primera ejecución manual. 

Por fin, con este paso tenemos nuestro entorno listo para proceder con el desarrollo del ejercicio a continuación.

## Desarrollo del ejercicio

1. Para iniciar el desarrollo del ejercicio, debemos crear nuestro archivo de workflow en la estructura de carpetas correspondiente (.gitflow>worflows>test.yml) e ingresamos los detalles iniciales del workflow:
   
   	<pre>
    name: Mi Demo con React
    on: push
    jobs:
      test:
        runs-on: ubuntu-latest
        steps:
         - name: Obtener código</pre>
    
   A partir de este punto, en vez de digitar el código vamos a continuar agregando código con los gists compartidos en clase. Y luego de cada cambio, vamos a verificar el estado de nuestros workflows tanto en el portal como en la extensión habilitada en nuestro entorno de desarrollo.
   
2. Completamos ahora el código de nuestro workflow con las líneas resaltadas en el siguiente bloque.
   
    <pre>
    name: Mi Demo con React
    on: push
    jobs:
      test:
        runs-on: ubuntu-latest
        steps:
         - name: Obtener código
           <b>uses: actions/checkout@v4
         - name: Instalar Node.JS
           uses: actions/setup-node@v3
           with:
             node-version: 18
         - name: Instalar Dependencias
           run: npm ci
         - name: Ejecutar pruebas
           run: npm test</b></pre>

3. Renombramos nuestro workflow como **deploy.yml** y agregamos el job correspondiente.

    <pre>
    name: Mi Demo con React
    on: push
    jobs:
      test:
        runs-on: ubuntu-latest
        steps:
         - name: Obtener código
           uses: actions/checkout@v4
         - name: Instalar Node.JS
           uses: actions/setup-node@v3
           with:
             node-version: 18
         - name: Instalar Dependencias
           run: npm ci
         - name: Ejecutar pruebas
           run: npm test
     <b>deploy:
       runs-on: ubuntu-latest
       steps:
         - name: Obtener código
           uses: actions/checkout@v4
         - name: Instalar Node.JS
           uses: actions/setup-node@v3
           with:
             node-version: 18
         - name: Instalar Dependencias
           run: npm install
         - name: Compilar el proyecto
           run: npm run build
         - name: Desplogar el artefacto
           run: echo "Desplegando..."</b></pre>

4. Agregamos una expresión condicional para asegurar la secuencialidad de los jobs:

    <pre>
    name: Mi Demo con React
    on: push
    jobs:
      test:
        runs-on: ubuntu-latest
        steps:
         - name: Obtener código
           uses: actions/checkout@v4
         - name: Instalar Node.JS
           uses: actions/setup-node@v3
           with:
             node-version: 18
         - name: Instalar Dependencias
           run: npm ci
         - name: Ejecutar pruebas
           run: npm test
     deploy:
       <b>needs: test</b>
       runs-on: ubuntu-latest
       steps:
         - name: Obtener código
           uses: actions/checkout@v4
         - name: Instalar Node.JS
           uses: actions/setup-node@v3
           with:
             node-version: 18
         - name: Instalar Dependencias
           run: npm install
         - name: Compilar el proyecto
           run: npm run build
         - name: Desplogar el artefacto
           run: echo "Desplegando..."</pre>

5. Agregamos el soporte de múltiples triggers a nuestro workflow.

    <pre>
    name: Mi Demo con React
    on: <b>[push, workflow_dispatch]</b>
    jobs:
      test:
        runs-on: ubuntu-latest
        steps:
         - name: Obtener código
           uses: actions/checkout@v4
         - name: Instalar Node.JS
           uses: actions/setup-node@v3
           with:
             node-version: 18
         - name: Instalar Dependencias
           run: npm ci
         - name: Ejecutar pruebas
           run: npm test
     deploy:
       needs: test
       runs-on: ubuntu-latest
       steps:
         - name: Obtener código
           uses: actions/checkout@v4
         - name: Instalar Node.JS
           uses: actions/setup-node@v3
           with:
             node-version: 18
         - name: Instalar Dependencias
           run: npm install
         - name: Compilar el proyecto
           run: npm run build
         - name: Desplogar el artefacto
           run: echo "Desplegando..."</pre>

6. Finalmente, vamos a crear otro workflow (**output.yml**), asociado a un trigger de eventos en el módulo de issues de nuestro repositorio, y que muestre información de contexto de GitHub y hace uso de expresiones para formatear esa información de forma adecuada. 
   
   <pre>
   name: Información de Salida
   on: issues
     jobs:
       info:
         runs-on: ubuntu-latest
         steps:
           - name: Mostrar contexto de GitHub
             run: echo "${{ toJSON(github) }}"
           - name: Mostrar detalles del Job
             run: echo "${{ toJSON(job) }}"
   </pre> 
