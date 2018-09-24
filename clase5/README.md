# Clase 3

### Firebase

![FirebaseLogo](../assets/firebase.png)

- [Features](https://firebase.google.com/products/)
- [Casos de uso](https://firebase.google.com/use-cases/)
- [Precio](https://firebase.google.com/pricing/)
- [Clientes](https://firebase.google.com/customers/)
- [Documentación](https://firebase.google.com/docs/)
  - [Primeros Pasos](https://firebase.google.com/docs/web/setup)
  - [Referencia de la API](https://firebase.google.com/docs/reference/js/)
  - [Ejemplos](https://firebase.google.com/docs/samples/#web)
  - [Codelabs](https://codelabs.developers.google.com/codelabs/firebase-web/#0)
- [Librerías y ayudas](https://firebase.google.com/docs/libraries/)

### Aprendiendo con Firebase 5.x

![firebase](../assets/firebase-features.png)

- [Firebase Docs](https://firebase.google.com/docs/web/setup)
- [Firebase - Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/js/client)
- [Firebase - Authentication](https://firebase.google.com/docs/auth/web/firebaseui)
- [Firebase - Real Time DB](https://firebase.google.com/docs/database/web/start)
- [Firebase - Storage](https://firebase.google.com/docs/storage/web/start)
- [Firebase - Hosting](https://firebase.google.com/docs/hosting/)
- [Firebase - Cloud Functions](https://firebase.google.com/docs/functions/)
- [Muchos más](https://firebase.google.com/docs/)

### Creación de una cuenta

1. Darse de alta en la [we de Firebase](https://www.firebase.com/signup/) mediante la cuenta de Google.
2. Crear un proyecto de Firebase en la [consola de administración](https://console.firebase.google.com).
3. Crear o asociar un proyecto de Google.
4. Crear un proyecto de Firebase.

### Configuración cliente

```html
<script src="https://www.gstatic.com/firebasejs/5.4.1/firebase.js"></script>
<script>
  // Initialize Firebase
  // TODO: Replace with your project's customized code snippet
  var config = {
    apiKey: "<API_KEY>",
    authDomain: "<PROJECT_ID>.firebaseapp.com",
    databaseURL: "https://<DATABASE_NAME>.firebaseio.com",
    storageBucket: "<BUCKET>.appspot.com",
    messagingSenderId: "<SENDER_ID>",
  };

  firebase.initializeApp(config);
</script>
```

En caso de no necesitar todas las funcionalidades podemos importarlas por separado:

```html
<!-- Firebase App is always required and must be first -->
<script src="https://www.gstatic.com/firebasejs/5.4.1/firebase-app.js"></script>

<!-- Add additional services that you want to use -->
<script src="https://www.gstatic.com/firebasejs/5.4.1/firebase-auth.js"></script>
<script src="https://www.gstatic.com/firebasejs/5.4.1/firebase-database.js"></script>
<script src="https://www.gstatic.com/firebasejs/5.4.1/firebase-firestore.js"></script>
<script src="https://www.gstatic.com/firebasejs/5.4.1/firebase-messaging.js"></script>
<script src="https://www.gstatic.com/firebasejs/5.4.1/firebase-functions.js"></script>
<script src="https://www.gstatic.com/firebasejs/5.4.1/firebase-storage.js"></script>
```

### Configuración Nodejs

1. Instalar la dependencia

  ```bash
  $ npm install firebase --save
  ```

2. Importar el paquete
  
  ```javascript
  var firebase = require("firebase");
  ```

3. Inicializar el SDK

  ```javascript
  var config = {
    apiKey: "<API_KEY>",
    authDomain: "<PROJECT_ID>.firebaseapp.com",
    databaseURL: "https://<DATABASE_NAME>.firebaseio.com",
    storageBucket: "<BUCKET>.appspot.com",
  };
  firebase.initializeApp(config);
  ```

### Servicios disponibles en el SDK

- `firebase.auth()`: Authentication
- `firebase.storage()`: Cloud Storage
- `firebase.database()`: Realtime Database
- `firebase.firestore()`: Cloud Firestore

### Authentication

![auth](../assets/firebase-auth.png)

[Todas las funcionalidades](https://firebase.google.com/docs/auth/web/start)

**FirebaseUI**

FirebaseUI es una capa que recubre el módulo de autenticación de Firebase que provee de flujos visuales de autenticación con las plataformas más utilizadas de la web.

> Añade fácilmente autenticación visual mediante FirebaseUI

- **Multiple Providers**: Email, teléfono, Google, Facebook, Twitter and GitHub sign-in.
- **Account Linking**: Flujos definidos para almacenar la información del usuario.
- **Customization**: Permite sobreescribir los estilos CSS por defecto.
- **One-tap**: Un único click para autenticarse
- **Localized UI**: Internacionalización para 40 idiomas.

**Login con Google** (sólo cliente)

```javascript
const provider = new firebase.auth.GoogleAuthProvider();

// Open login pop-up
firebase.auth().signInWithPopup(provider).then((result) => {
  // This gives you a Google Access Token. You can use it to access the Google API.
  var token = result.credential.accessToken;
  // The signed-in user info.
  var user = result.user;
  // ...
}).catch((error) => {
  // Handle Errors here.
  var errorCode = error.code;
  var errorMessage = error.message;
  // The email of the user's account used.
  var email = error.email;
  // The firebase.auth.AuthCredential type that was used.
  var credential = error.credential;
  // ...
});
```

**Login con Google** (Nodejs)

1. Recupera el `idToken` de la respuesta de la autenticación y envíalo al back-end:

  ```javascript
  const provider = new firebase.auth.GoogleAuthProvider();

  // Open login pop-up
  firebase.auth().signInWithPopup(provider).then((result) => {
    const token = result.client.idToken;
    // Send the token to the server
  });
  ```

2. Utiliza el token para autenticarte en el servidor

  ```javascript
  // Build Firebase credential with the Google ID token.
  const credential = firebase.auth.GoogleAuthProvider.credential(id_token);

  // Sign in with credential from the Google user.
  firebase.auth().signInWithCredential(credential).then((response) =>{
    // ...
  });
  ```

**Detectar cambios de usuario**

```javascript
firebase.auth().onAuthStateChanged((user) => {
  if (!user) {
    // Do something...
  }
});
```

### Cloud Storage

![storage](../assets/firebase-cloud-storage.jpeg)

[Todas las funcionalidades](https://firebase.google.com/docs/storage/web/start)

**Subir ficheros**

```javascript
// Create a root reference
const storageRef = firebase.storage().ref();

// use the Blob or File API
const file = ...

// Upload the file
ref.put(file).then((snapshot) => {
  console.log('Uploaded a blob or file!');
});
```

**Descargar ficheros**

```javascript
// Create a root reference
const storageRef = firebase.storage().ref();

storageRef.child('images/stars.jpg').getDownloadURL().then((url) => {
  // `url` is the download URL for 'images/stars.jpg'

  // This can be downloaded directly:
  fetch(url)
      .then((response) => response.blob())
      .then((file) => {
          console.log('Here is the file!', file);
      });

  // Or inserted into an <img> element:
  var img = document.getElementById('myimg');
  img.src = url;
}).catch(function(error) {
  // Handle any errors
});
```

### Realtime Database

![realtime](../assets/firebase-realtime.gif)

[Todas las funcionalidades](https://firebase.google.com/docs/database/web/start)

**[Configurando las reglas de lectura/escritura](https://firebase.google.com/docs/database/security/quickstart#sample-rules)**

**¿Cómo se estructuran los datos?**

La base de datos es un simple JSON

```json
{
  "users": {
    "alovelace": {
      "name": "Ada Lovelace",
      "contacts": { "ghopper": true },
    },
    "ghopper": { ... },
    "eclarke": { ... }
  }
}
```

**Lectura/escritura de datos**

```javascript
var userRef = firebase.database().ref(`users/${userId}`);

// LEscuchar cambios en `users/${userId}`
userRef.on('value', (snapshot) => {
  console.log(snapshot.val());
});

// Escuchar una única vez cambios en `users/${userId}`
userRef.once('value', (snapshot) => {
  console.log(snapshot.val());
});

// Almacenar datos en `users/${userId}`
userRef.set({
  username: name,
  email: email,
  profile_picture : imageUrl
});
```

**Actualizando varios datos simultáneamente**

```javascript
var updates = {
    'timestamp': new Date().getTime()
};

updates[`users/${userId}`] = {
  username: name,
  email: email,
  profile_picture : imageUrl
};

firebase.database().ref().update(updates);
```

**¿Cómo borrar datos?**

Las referencias tienen una función `remove()`.

**¿Cómo saber si se ha actualizado un dato?**

Las funciones `set` y `update` devuelven promesas.

**¿Cómo quitar un listener o una referencia?**

Las referencias tienen una función `off(listener)`.
Si no se le pasa ningún parámetro elimina todos los listeners.

**"Arrays" -> Listas**

```javascript
var userRef = firebase.database().ref(`users`);

// Almacenar datos en `users` (genera una clave aleatoria)
userRef.push({
  username: name,
  email: email,
  profile_picture : imageUrl
});
```

**Escuchar cambios en las listas**

```javascript
var userRef = firebase.database().ref(`users`);

// Cuando se añade un elemento
userRef.on('child_added', (snapshot) => { /* ... */ });

// Cuando cambia un elemento
userRef.on('child_changed', (snapshot) => { /* ... */ });

// Cuando se elimina un elemento
userRef.on('child_removed', (snapshot) => { /* ... */ });
```

**Recuperar todos los elementos de una lista**

```javascript
var userRef = firebase.database().ref(`users`);

// Cuando se añade un elemento
userRef.on('value', (snapshot) => {
    snapshot.forEach((childSnapshot) => {
        const element = childSnapshot.val();
    });
});
```

**Ordenar una lista**

```javascript
const ref = firebase.database().ref(`users`);

// Ordena por el valor de una clave
ref.orderByChild('name');
ref.orderByChild('location/latitude');

// Ordena por el nombre de la clave
ref.orderByKey();

// Ordena por el valor de cada elemento
ref.orderByValue();
```

**Filtrar una lista**

```javascript
const ref = firebase.database().ref(`users`);

// Limita el número de elementos de la lista desde el inicio hasta la cantidad de elementos indicada
ref.limitToFirst(10);

// Limita el número de elementos de la lista desde el final hasta la cantidad de elementos indicada
ref.limitToLast(5);

// Elementos mayores o iguales al valor indicado
ref.startAt(70);

// Elementos menores o iguales al valor indicado
ref.endAt();

// Elementos iguales al valor indicado
ref.equalTo();
```

**Trabajar Offline**

- [Documentación](https://firebase.google.com/docs/database/web/offline-capabilities)

```javascript
var connectedRef = firebase.database().ref('.info/connected');

connectedRef.on('value', (snap) => {
  if (snap.val() === true) {
    alert('connected');
  } else {
    alert('not connected');
  }
});
```

### Hosting

1. Instalar cli

  ```bash
  $ npm install -g firebase-tools
  ```

2. Inicializar aplicación:

  ```bash
  $ firebase init
  ```

3. Desplegar

  ```bash
  $ firebase deploy
  ```

### Ejemplos

- [Social Login](http://jsfiddle.net/firebase/a221m6pb/embedded/result,js,css/)
- [Código Fuente demo de Velocidad](https://github.com/UlisesGascon/raspberrypi-system-info-data-to-firebase)
- [Curratelo](https://github.com/UlisesGascon/curratelo)
- [Raspberrypi system info data to Firebase](https://github.com/UlisesGascon/raspberrypi-system-info-data-to-firebase)
- [Calidad del Aire con Firebase](https://github.com/UlisesGascon/Calidad-del-Aire-con-Firebase)
- [Agroduino](https://github.com/UlisesGascon/Agroduino)
- [Aire Madrid](https://github.com/UlisesGascon/Aire-Madrid)
- [EMT Bus Live](https://github.com/Borjagodoy/emtBusLive)
- [Bitcoin price ticker with Arduino](https://github.com/UlisesGascon/Bitcoin-price-ticker-with-Arduino)
- [MovieFire con Nodejs](https://github.com/UlisesGascon/Simple-API-REST-with-Firebase-and-IMBD)

### Ejercicios

**1 -** Partiendo del repositorio [twitter-firebase-stream#template](https://github.com/josex2r/twitter-firebase-stream/tree/template) completar los siguientes ficheros añadiendo la lógica necesaria de firebase:

- **[Solución](https://github.com/josex2r/twitter-firebase-stream)**
