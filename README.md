#Bot de Discord

<p>
**Comandos que utilice:**
</p>

- npm init
- npm install discord.js
- npm install axios
- npm install dotenv
- node index.js



###Construyendo tu primera aplicación Discord
Las aplicaciones Discord le permiten personalizar y extender sus servidores utilizando una colección de API y funciones interactivas. Esta guía lo guiará a través de la construcción de su primera aplicación Discord usando JavaScript, y al final tendrá una aplicación que usa comandos de barra diagonal, envía mensajes y responde a las interacciones de los componentes.

Construiremos una aplicación Discord que permita a los miembros del servidor reproducir tijeras de papel de roca ( con 7 opciones en lugar de las habituales 3 ). Esta guía está centrada en el principiante, pero supone una comprensión básica de JavaScript.

Lo que estaremos construyendo
Recursos utilizados en esta guía
Descripción general de las herramientas y tecnologías que usaremos

- **Paso 1: crear una aplicación**

Primero, deberá crear una aplicación en el portal del desarrollador si aún no tiene una:

Ingrese un nombre para su aplicación, luego presione Crear.

Después de crear su aplicación, aterrizará en el Resumen general página de la configuración de la aplicación donde puede actualizar información básica sobre su aplicación, como su descripción e icono. También verás un ID de aplicación y Interacciones Endpoint URL, que usaremos un poco más adelante en la guía.

 **Configurando tu bot**

A continuación configuraremos el usuario de bot para su aplicación, que le permite aparecer y comportarse de manera similar a otros miembros del servidor.

En la barra lateral izquierda, haga clic en Bot. En esta página, puede configurar configuraciones como esta intenciones privilegiadas o si puede ser instalado por otros usuarios.

**¿Qué son las intenciones?**

Introducción a intenciones estándar y privilegiadas
Pestaña Bot en la configuración de la aplicación

También hay un Token sección sobre el Bot página, que le permite copiar y restablecer el token de su bot.

Los tokens de bot se utilizan para autorizar solicitudes de API y llevar los permisos de su usuario de bot, lo que los convierte altamente sensible. Asegúrate de nunca comparta su token o verifíquelo en cualquier tipo de control de versión.

Siga adelante y copie el token y almacene el token en algún lugar seguro ( como en un administrador de contraseñas ).

No podrá volver a ver su token a menos que lo regenere, así que asegúrese de mantenerlo en un lugar seguro.
Agregar alcances y permisos de bot
Las aplicaciones necesitan la aprobación de la instalación de usuarios para realizar acciones en Discord ( como crear un comando de barra o buscar una lista de miembros del servidor ). Seleccione algunos ámbitos y permisos para solicitar antes de instalar la aplicación.

**¿Qué son los alcances y permisos?**

Introducción a los alcances y permisos de bot
Hacer clic en OAuth2 en la barra lateral izquierda, luego seleccione Generador de URL.

El generador de URL crea un enlace de instalación basado en los ámbitos y permisos que seleccione para su aplicación. Puede usar el enlace para instalar la aplicación en su propio servidor, o compartirlo con otros para que puedan instalarlo.
Por ahora, agregue dos ámbitos:

applications.commands que permite crear su aplicación comandos.
bot agrega su usuario bot. Después de seleccionar bot, también puede seleccionar diferentes permisos para su bot. Por ahora, solo verifique Enviar mensajes.
Ver una lista de todos Alcance OAuth2, o leer más sobre permisos en la documentación.

Instalar tu aplicación
Una vez que agregue los ámbitos, debería ver una URL que pueda copiar para instalar su aplicación.

Captura de pantalla del generador de URL

Al desarrollar aplicaciones, debe compilar y probar en un servidor que no sea utilizado activamente por otros. Si aún no tiene su propio servidor, puede crea uno gratis.
Copie la URL de arriba y péguela en su navegador. Se lo guiará a través del flujo de instalación, donde debe asegurarse de instalar su aplicación en un servidor donde pueda desarrollarla y probarla.

Después de instalar su aplicación, puede dirigirse a su servidor y ver que se ha unido ✨

Con su aplicación configurada e instalada, comencemos a desarrollarla.

- **Paso 2: ejecutar tu aplicación**

Todo el código utilizado en la aplicación de ejemplo se puede encontrar en el repositorio de Github.

Para simplificar un poco el desarrollo, la aplicación usa interacciones de discordia, que proporciona tipos y funciones auxiliares. Si prefiere usar otros idiomas o bibliotecas, consulte el Recursos comunitarios documentación.

**Remixing el proyecto**
Esta guía utiliza Glitch, que le permite clonar y desarrollar dentro de su navegador. Si prefiere desarrollar su aplicación localmente, hay instrucciones sobre el uso de ngrok en la LECTURA.

Si bien Glitch es excelente para el desarrollo y las pruebas, tiene restricciones técnicas por lo tanto, otros proveedores de alojamiento deben considerarse para aplicaciones de producción.
Para empezar, remix ( o clone ) el proyecto Glitch 🎏.

Una vez que remezcle el proyecto, aterrizará en un nuevo proyecto Glitch.

Interfaz de proyecto Glitch
Descripción general de la interfaz del proyecto de Glitch
Estructura del proyecto
Todos los archivos para el proyecto están en el lado izquierdo de su proyecto Glitch. A continuación se muestra una descripción general de las principales carpetas y archivos:
**
├── examples    -> short, feature-specific sample apps
│   ├── app.js  -> finished app.js code
│   ├── button.js
│   ├── command.js
│   ├── modal.js
│   ├── selectMenu.js
├── .env        -> your credentials and IDs
├── app.js      -> main entrypoint for app
├── commands.js -> slash command payloads + helpers
├── game.js     -> logic specific to RPS
├── utils.js    -> utility functions and enums
├── package.json
├── README.md
└── .gitignore
**
Agregar credenciales
Ya hay algún código en tu app.js archivo, pero necesitará el token y la identificación de su aplicación para hacer solicitudes. Todas sus credenciales pueden almacenarse directamente en el .env archivo.

Primero, copie el token de su usuario de bot de antes y péguelo en el DISCORD_TOKEN variable en tu .env archivo.

A continuación, navegue a su aplicación Resumen general página, luego copie el ID de aplicación y Clave pública. Pegue los valores en su .env archivo como APP_ID y PUBLIC_KEY.

Con sus credenciales configuradas, instalemos y manejemos comandos de barra diagonal.

Instalar comandos de barra diagonal
Para instalar comandos de barra diagonal, la aplicación está usando node-fetch. Puede ver la implementación de la instalación en utils.js dentro del DiscordRequest() función.
El proyecto contiene un register script que puede usar para instalar los comandos en ALL_COMMANDS, que se define en la parte inferior de commands.js. Instala los comandos como comandos globales llamando a la API HTTP PUT /applications/<APP_ID>/commands punto final.

Si desea personalizar sus comandos o agregar otros adicionales, puede hacer referencia a la estructura de comandos en el documentación de comandos.

Corre el register guión haciendo clic Terminal en la parte inferior de su proyecto Glitch y pegando el siguiente comando:

npm run register
Presiona enter para ejecutar el comando.

Si navega de regreso a su servidor, debería ver aparecer los comandos de barra diagonal. Pero si intenta ejecutarlos, no pasará nada ya que su aplicación no recibe ni maneja ninguna solicitud de Discord.

¿Cuáles son las API de Discord?
Descripción general de las API HTTP y Gateway de Discord
Paso 3: Manejo de la interactividad
Para permitir que su aplicación reciba solicitudes de comando de barra diagonal ( y otras interacciones ), Discord necesita una URL pública para enviarlas. Esta URL se puede configurar en la configuración de su aplicación como URL de punto final de interacción.

Agregar una URL de punto final de interacción
Los proyectos Glitch tienen una URL pública expuesta de forma predeterminada. Copie la URL de su proyecto haciendo clic en el Compartir abotone en la esquina superior derecha, luego copie el enlace del proyecto "Sitio en vivo" cerca de la parte inferior del modal.

Si está desarrollando localmente, hay instrucciones para solicitudes de túnel a su entorno local en el Github README.
Con el enlace copiado, vaya a la configuración de su aplicación desde el portal del desarrollador.

En tu aplicación Información general página, hay un URL de punto final interactivo opción, donde puede pegar la URL de su aplicación y agregar /interactions a él, que es donde está configurada la aplicación Express para escuchar las solicitudes.

Interacciones de punto final URL en la configuración de la aplicación

Hacer clic en Guardar cambios y asegúrese de que su punto final se verifique correctamente.

La aplicación de muestra maneja la verificación de dos maneras:

Utiliza el PUBLIC_KEY y paquete de interacciones de discordia con una función de envoltura ( importada de utils.js) que lo hace conforme a Express's verify interfaz. Esto se ejecuta en cada solicitud entrante a su aplicación.
Responde a entrante PING solicitudes.
Puede obtener más información sobre cómo preparar su aplicación para recibir interacciones en la documentación de interacciones.

Manejo de solicitudes de comando de barra diagonal
Con el punto final verificado, navegue a su proyecto app.js archivo y encuentre el bloque de código que maneja el /test comando:
```
// "test" command
if (name === 'test') {
    // Send a message into the channel where command was triggered from
    return res.send({
    type: InteractionResponseType.CHANNEL_MESSAGE_WITH_SOURCE,
    data: {
        // Fetches a random emoji to send from a helper function
        content: 'hello world ' + getRandomEmoji(),
    },
    });
}
```
El código anterior responde a la interacción con un mensaje en el canal del que se originó. Puede ver todos los tipos de respuesta disponibles, como responder con un modal, en la documentación.

InteractionResponseType.CHANNEL_MESSAGE_WITH_SOURCE es una constante exportado de discord-interactions
Vaya a su servidor y asegúrese de que su aplicación sea /test el comando de barra funciona. Cuando lo activa, su aplicación debe enviar un mensaje que contenga “ hello world ” seguido de un emoji aleatorio.

En la siguiente sección, agregaremos un comando adicional que usa opciones de comando de barra diagonal, botones y menús seleccionados para construir el juego de tijeras de papel de roca.

Paso 4: Agregar componentes de mensajes
los /challenge El comando será cómo se inicia nuestro juego de estilo rock-paper-scissors. Cuando se activa el comando, la aplicación enviará componentes de mensajes al canal, lo que guiará a los usuarios a completar el juego.

Agregar un comando con opciones
los /challenge comando, llamado CHALLENGE_COMMAND en commands.js, tiene una matriz de options. En nuestra aplicación, las opciones son objetos que representan diferentes cosas que un usuario puede seleccionar mientras juega piedra-papel-tijera, generadas usando las teclas de RPSChoices en game.js.

Puede leer más sobre las opciones de comando y su estructura en la documentación.

Si bien esta guía no tocará mucho en el game.js archivo, no dude en buscar y cambiar comandos o las opciones en los comandos.
Manejo de la interacción del comando.
Código para manejar el comando de desafío y responder con un mensaje que contiene un botón
Para manejar el /challenge comando, agregue el siguiente código después del if name === “test” si bloque:

// "challenge" command
if (name === 'challenge' && id) {
    const userId = req.body.member.user.id;
    // User's object choice
    const objectName = req.body.data.options[0].value;

    // Create active game using message ID as the game ID
    activeGames[id] = {
        id: userId,
        objectName,
    };

    return res.send({
    type: InteractionResponseType.CHANNEL_MESSAGE_WITH_SOURCE,
    data: {
        // Fetches a random emoji to send from a helper function
        content: `Rock papers scissors challenge from <@${userId}>`,
        components: [
        {
            type: MessageComponentTypes.ACTION_ROW,
            components: [
            {
                type: MessageComponentTypes.BUTTON,
                // Append the game ID to use later on
                custom_id: `accept_button_${req.body.id}`,
                label: 'Accept',
                style: ButtonStyleTypes.PRIMARY,
            },
            ],
        },
        ],
    },
    });
}
Si no está seguro de dónde pegar el código, puede ver el código completo en examples/app.js en el proyecto Glitch o la raíz app.js en Github.
El código anterior está haciendo algunas cosas:

Analiza el cuerpo de solicitud para obtener la identificación del usuario que activó el comando de barra diagonal (userId), y la opción ( opción de objeto ) seleccionaron (objectName).
Agrega un nuevo juego al activeGames objeto usando la ID de interacción. El juego activo registra el userId y objectName.
Envía un mensaje al canal con un botón con un custom_id de accept_button_<SOME_ID>.
El código de muestra utiliza un objeto como almacenamiento en memoria, pero para las aplicaciones de producción debe usar una base de datos.
Al enviar un mensaje con componentes del mensaje, las cargas individuales se agregan a un components matriz. Los componentes accionables ( como botones ) deben estar dentro de un fila de acción, que puedes ver en la muestra de código.

Tenga en cuenta lo único custom_id enviado con componentes de mensaje, en este caso accept_button_ con la identificación del juego activo adjunta. A custom_id se puede usar para manejar solicitudes que Discord le envía cuando alguien interactúa con el componente, que verá en un momento.

Ahora cuando ejecutas el /challenge comando y elegir una opción, su aplicación enviará un mensaje con un Aceptar botón. Agreguemos código para manejar el botón.

Manejo de interacciones del botón
Código para manejar clics en el botón y responder con un mensaje efímero
Cuando los usuarios interactúan con un componente de mensaje, Discord enviará una solicitud con un tipo de interacción de 3 ( o el MESSAGE_COMPONENT valor cuando se usa discord-interactions).

Para configurar un controlador para el botón, revisaremos el type de interacción, seguido de coincidencia custom_id.

Pegue el siguiente código debajo del controlador de tipo para APPLICATION_COMMANDs:
```
if (type === InteractionType.MESSAGE_COMPONENT) {
// custom_id set in payload when sending message component
const componentId = data.custom_id;

  if (componentId.startsWith('accept_button_')) {
    // get the associated game ID
    const gameId = componentId.replace('accept_button_', '');
    // Delete message with token in request body
    const endpoint = `webhooks/${process.env.APP_ID}/${req.body.token}/messages/${req.body.message.id}`;
    try {
      await res.send({
        type: InteractionResponseType.CHANNEL_MESSAGE_WITH_SOURCE,
        data: {
          // Fetches a random emoji to send from a helper function
          content: 'What is your object of choice?',
          // Indicates it'll be an ephemeral message
          flags: InteractionResponseFlags.EPHEMERAL,
          components: [
            {
              type: MessageComponentTypes.ACTION_ROW,
              components: [
                {
                  type: MessageComponentTypes.STRING_SELECT,
                  // Append game ID
                  custom_id: `select_choice_${gameId}`,
                  options: getShuffledOptions(),
                },
              ],
            },
          ],
        },
      });
      // Delete previous message
      await DiscordRequest(endpoint, { method: 'DELETE' });
    } catch (err) {
      console.error('Error sending message:', err);
    }
  }
}
```
**El código de arriba:**

Verificaciones para un custom_id que coincide con lo que enviamos originalmente ( en este caso, comienza con accept_button_). La ID personalizada también tiene adjunta la ID de juego activa, por lo que la almacenamos en gameID.
Elimina el mensaje original llamando a un webhook usando node-fetch y pasando la interacción única token en el organismo de solicitud. Esto se hace para limpiar el canal, por lo que otros usuarios no pueden hacer clic en el botón.
Responde a la solicitud enviando un mensaje que contiene un menú de selección con las opciones de objeto para el juego. La carga útil debe ser bastante similar a la anterior, con la excepción de options matriz y flags: 64, que indica que el mensaje es efímero.
los options la matriz se llena usando el getShuffledOptions() método en game.js, que manipula el RPSChoices valores para ajustarse a la forma de opciones de componentes de mensajes.

Manejo de interacciones de menú selectas
Código para responder a interacciones de menú seleccionadas y actualizar el estado del juego
Lo último que debe agregar es código para manejar interacciones de menú seleccionadas y enviar el resultado del juego al canal.

Como los menús seleccionados son solo otro componente de mensaje, el código para manejar sus interacciones será casi idéntico a los botones.

Modifique el código de arriba para manejar el menú de selección:

if (type === InteractionType.MESSAGE_COMPONENT) {
// custom_id set in payload when sending message component
const componentId = data.custom_id;

  if (componentId.startsWith('accept_button_')) {
    // get the associated game ID
    const gameId = componentId.replace('accept_button_', '');
    // Delete message with token in request body
    const endpoint = `webhooks/${process.env.APP_ID}/${req.body.token}/messages/${req.body.message.id}`;
    try {
      await res.send({
        type: InteractionResponseType.CHANNEL_MESSAGE_WITH_SOURCE,
        data: {
          // Fetches a random emoji to send from a helper function
          content: 'What is your object of choice?',
          // Indicates it'll be an ephemeral message
          flags: InteractionResponseFlags.EPHEMERAL,
          components: [
            {
              type: MessageComponentTypes.ACTION_ROW,
              components: [
                {
                  type: MessageComponentTypes.STRING_SELECT,
                  // Append game ID
                  custom_id: `select_choice_${gameId}`,
                  options: getShuffledOptions(),
                },
              ],
            },
          ],
        },
      });
      // Delete previous message
      await DiscordRequest(endpoint, { method: 'DELETE' });
    } catch (err) {
      console.error('Error sending message:', err);
    }
  } else if (componentId.startsWith('select_choice_')) {
    // get the associated game ID
    const gameId = componentId.replace('select_choice_', '');

    if (activeGames[gameId]) {
      // Get user ID and object choice for responding user
      const userId = req.body.member.user.id;
      const objectName = data.values[0];
      // Calculate result from helper function
      const resultStr = getResult(activeGames[gameId], {
        id: userId,
        objectName,
      });

      // Remove game from storage
      delete activeGames[gameId];
      // Update message with token in request body
      const endpoint = `webhooks/${process.env.APP_ID}/${req.body.token}/messages/${req.body.message.id}`;

      try {
        // Send results
        await res.send({
          type: InteractionResponseType.CHANNEL_MESSAGE_WITH_SOURCE,
          data: { content: resultStr },
        });
        // Update ephemeral message
        await DiscordRequest(endpoint, {
          method: 'PATCH',
          body: {
            content: 'Nice choice ' + getRandomEmoji(),
            components: []
          }
        });
      } catch (err) {
        console.error('Error sending message:', err);
      }
    }
  }
}
Similar al código anterior, el código anterior obtiene la ID de usuario y su selección de objetos de la solicitud de interacción.

Esa información, junto con la ID del usuario original y la selección de la activeGames objeto, se pasan al getResult() función. getResult() determina el ganador, luego construye una cadena legible para enviar de vuelta al canal.

También estamos llamando a otro webhook, esta vez a actualizar el mensaje efímero de seguimiento ya que no se puede eliminar.

Finalmente, los resultados se envían en el canal utilizando el CHANNEL_MESSAGE_WITH_SOURCE tipo de respuesta de interacción.

....y eso es todo 🎊 Siga adelante y pruebe su aplicación y asegúrese de que todo funcione.

