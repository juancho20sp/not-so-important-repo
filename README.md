# 📝 Enunciado del examen

Vas a construir una **arquitectura de tres capas**:

1. **Frontend (FE)**  
2. **Backend (BE)**  
3. **API externa (pokeapi)**  

⚠️ Importante:  
Solo el **backend** puede comunicarse con la API externa. El frontend no tiene permiso para hacerlo directamente.  

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
   - Puedes resolverlo con un **botón “login”** que, al hacer clic, envíe automáticamente las credenciales al endpoint.  
   - Si la autenticación es exitosa, el token recibido debe guardarse en **localStorage** con la llave:  

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

## 🐱‍👤 Endpoint protegido: `/api/pokemonDetails`

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

