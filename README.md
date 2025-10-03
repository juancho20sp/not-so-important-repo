# 📝 Enunciado del examen

Vas a construir una **arquitectura de tres capas**:

1. **Frontend (FE) (se recomienda trabajar todo sobre index.html para evitar problemas con las URLs en el FE)**  
2. **Backend (BE)**  
3. **API externa (pokeapi)**  

⚠️ Importante:  
Solo el **backend** puede comunicarse con la API externa. El frontend no tiene permiso para hacerlo directamente.  

# Bosquejo de la arquitectura
<img width="1011" height="303" alt="image" src="https://github.com/user-attachments/assets/18450165-2000-4eb3-ab87-ff9cec1e30c7" />

---

## 🔑 Autenticación

1. Implementa un **endpoint de login** en el backend.  
   - Ruta:  

     ```
     POST /api/v1/auth
     ```  

   - Debe recibir en el **body** un objeto con esta forma:  

     ```json
     {
       "email": "email",
       "password": "password"
     }
     ```  

   - Las credenciales válidas son:  

     ```
     email: admin@admin.com
     password: admin
     ```

2. Respuestas esperadas:  
   - ✅ Autenticación exitosa:  

     - Código: `200`  
     - Body:  

       ```json
       {
         "token": "token"
       }
       ```  
     - El token debe tener una validez mínima de **1 hora**.  

   - ❌ Credenciales inválidas:  

     - Código: `400`  
     - Body:  

       ```json
       {
         "error": "invalid credentials"
       }
       ```  

3. Desde el **frontend** no es necesario un formulario.  
   - Puedes resolverlo con un **botón “login”** que, al hacer clic, envíe automáticamente las credenciales al endpoint. Por favor, almacene y acceda a estas credenciales mediante variables de entorno.  
   - Si la autenticación es exitosa, el token recibido debe guardarse en **localStorage** para usarse más adelante al momento de comunicarse con el BE, el nombre de la llave en el localStorage debe ser:  

     ```
     sessionToken
     ```

---

## 🚫 Control de acceso

- Si el **frontend** intenta hacer una petición sin incluir el token, el backend debe responder:  

  - Código: `403`  
  - Body:  

    ```json
    {
      "error": "User not authenticated"
    }
    ```  

- Cuando el **frontend** sí tenga un token válido, debe enviarlo como **header de autorización** como en el siguiente ejemplo:
```javascript
fetch('/api/protected-route', {
  method: 'GET', // Replace with your actual verb
  headers: {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer YOUR_JWT_TOKEN_HERE' // Replace with your actual JWT
  }
})
```

## 🐱‍👤 Endpoint protegido: `/api/v1/pokemonDetails`

1. Este endpoint solo debe ser accesible si hay un **token JWT válido**.  
2. Recibe un **body** con la siguiente estructura:  

 ```json
 {
   "pokemonName": "XXXX"
 }
```

## El backend debe conectarse a pokeapi para buscar el Pokémon
- Si el nombre es válido y existe:
   - Código: `200`
   - Respuesta con únicamente la siguiente información:
 ```json
{
  "name": "pikachu",
  "species": "pikachu",
  "weight": "20",
  "img_url": "www.img.com"
}
```
- Esta información debe presentarse al usuario a través de la aplicación web de manera visual.

- Si el Pokémon no existe:
   - Código: `400`
   - Respuesta con la misma estructura, pero con todos los campos vacíos:
  ```json
  {
  "name": "",
  "species": "",
  "weight": "",
  "img_url": ""
  }
  ```
  - Se le debe presentar al usuario un mensaje que diga: `Ups! Pokémon no encontrado`


# 📌 Reglas del examen

- El examen inicia a las 7:00am del 03 de octubre de 2025 y termina a las 9:30am del 03 de octubre de 2025.
- El examen debe realizarse en **parejas**. Si se entrega en pareja, la calificación será la misma para ambos estudiantes.  
- El **Backend (BE)** y el **Frontend (FE)** deben estar **desplegados y funcionando públicamente**. No se aceptarán entregas privadas.  
- Toda entrega debe estar **marcada correctamente**:  
  - Agregar los **nombres de los estudiantes** al inicio del archivo `README`.  
  - Incluir en el `README` la **URL del proyecto desplegado y funcional**.  
  - Si la entrega no cumple con esto, **no será calificada**.  
- Si el proyecto no está desplegado, **no será calificado**.  
- Si el frontend se comunica directamente con **pokeAPI**, la entrega será **descalificada** (no se califica).  
- Penalizaciones:  
  - Subir la carpeta `node_modules` al repositorio → **-1.0** unidad en la nota final.
  - Si la entrega es extemporánea, es decir, después de las **9:30am del 03 de octubre de 2025** -> **-1.0** unidades en la nota final.

# 📊 Rúbrica de evaluación

- **Despliegue**: 1.0  
- **Seguridad de los endpoints** (uso correcto de tokens JWT): 2.0  
- **Buenas prácticas de desarrollo** (variables de entorno, `.gitignore`, `express.Router`, middlewares): 0.5  
- **Funcionalidad**: 1.5  






