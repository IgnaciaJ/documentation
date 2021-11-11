# Documentación API

---

Index:

- [Auth](#auth)
  - [POST api/auth/signup/](#post-authsign-up)
  - [POST api/auth/signin/](#post-authsign-in)

- [User](#user)
  - [GET api/users/](#get-users)
  - [PATCH api/users/{user_id}](#patch-user)
  - [GET api/users/{user_id}/](#get-profile)
  - [DELETE api/users/{user_id}/](#delete-user)
  - [GET api/users/{user_id}/mybookings/](#get-bookings)

- [Housing](#housing)
  - [GET api/housings/](#get-housings)
  - [GET api/users/{user_id}/housings/](#get-userhousings)
  - [PATCH api/users/{user_id}/housings/{housing_id}/](#patch-housing)
  - [GET api/users/{user_id}/housings/{housing_id}/](#get-housingshow)
  - [POST  api/users/{user_id}/housings/new/](#post-housingsnew)
  - [DELETE api/users/{user_id}/housings/{housings_id}](#delete-housing)

- [Booking](#booking)
  - [POST api/housings/{housing_id}/bookings/new/](#post-newbooking)
  - [GET api/housings/{housing_id}/bookings/](#get-bookings)
  - [GET api/housings/{housing_id}/bookings/{booking_id}/](#get-booking)
  - [PATCH api/housings/{housing_id}/bookings/{booking_id}/edit](#patch-bookingdetails)
  - [PATCH api/housings/{housing_id}/bookings/{booking_id}/state](#patch-bookingstate)
  - [DELETE api/housings/{housing_id}/bookings/{booking_id}/](#delete-booking)


- [panorama](#panorama)
  - [GET api/panoramas/](#get-panoramas)
  - [POST  api/panoramas/new/](#post-panoramasnew)
  - [PATCH api/panoramas/{panorama_id}/](#patch-panorama)
  - [GET api/panorama/{panorama_id}/](#get-panoramashow)
  - [DELETE api/panorama/{panorama_id}/](#delete-panorama)

- [ReviewsForPanorama](#ReviewPanorama)
  - [GET api/panoramas/{panoramas_id}/reviews](#get-reviewspanoramas)
  - [POST  api/panoramas/{panoramas_id}/reviews/new/](#post-reviewspanoramasnew)
  - [PATCH api/panoramas/{panoramas_id}/reviews/{ReviewsForPanorama_id}](#patch-reviewpanorama)
  - [DELETE api/panorama/{panorama_id}/reviews/{ReviewsForPanorama_id}/](#delete-reviewpanorama)

- [ReviewsForHousing](#ReviewHousing)
  - [GET api/housings/{housings_id}/reviews](#get-reviewshousing)
  - [POST  api/housings/{housings_id}/reviews/new/](#post-reviewshousingsnew)
  - [PATCH api/housings/{housing_id}/reviews/{ReviewsForHousing_id}](#patch-reviewhousing)
  - [DELETE api/housing/{housing_id}/reviews/{ReviewsForHousing_id}/](#delete-reviewhousing)

---

## Auth

- *TODOS* los request de tipo POST/PATCH/DELETE requieren de tokens, al no tener, tirará un error de status 401.  

### [POST api/auth/signup/](#documentación-api)

- Crea un usuario, recibe `nickname`, `mail`, `password`, `name` e `type`, donde `type` es un string que toma los valores `user` cuando es un usuario común y `administrator` en caso contrario. Retorna un mensaje con los datos del usuario fue registrado en caso de que todo esté bien. y entrega un status 200. 

- INPUT:

```
    {
        "mail": "usuario@uc.cl",
        "password": "usuariopassword",
        "nickname": "username",
        "name": "minombre",
        "age" : 21,
        "gender": "female",
        "type": "user"
    }
```

- OUTPUT:

```
{
    "data": {
        "type": "users",
        "id": "1",
        "attributes": {
            "nickname": "username",
            "name": "minombre",
            "mail": "usuario@uc.cl",
            "gender": "female"
        }
    }
}
```

Posibles casos
- En caso que se cree un usuario con un mail que no cumpla con el formato de email, postman entregará lo siguiente:
```
{
    "Email format": "You have entered an invalid email address!"
}
```
- En caso de que el mail ya se encuentre utilizado:
```
{
    "Duplicate email": "Failed! mail is already in use!"
}
```
lo mismo ocurre con el nickname.  

En cuanto a la edad, se entregan dos tipos de verificaciones.  
-  Si es un número negativo:
```
{
    "Invalid Number": "Must be >0!"
}
```
-  Si es menor de edad:
```
{
    "Age Limit": "You are under-age!"
}
```
### [POST api/auth/signin/](#documentación-api)

- Recibe `mail`  y contraseña y retorna el `id`, `nickname`, `name`, `mail`, `age` y `accessToken`.

- INPUT:

```
{
    "username": "usuario@uc.cl",
    "password": "usuariopassword"
}
```

- OUTPUT:

```
 {
    "id": 1,
    "name": "minombre",
    "nickname": "username",
    "mail": "usuario@uc.cl",
    "age" : 21,
    "access_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOjEsImlhdCI6MTYzNjQ2NDAzNiwiZXhwIjoxNjM2NDY3NjM2fQ.-c8Sn8OI4hxxJ5KKB7-suJELBgdMOZyk9gdDIaqsf7U",
    "token_type": "Bearer"
}
```
En caso de ingresar mal los datos, puede arrojar las siguientes opciones  
- Sí el usuario existe, pero la contraseña no coincide, deberías recibir
```Invalid password```  
- Si no existe un usuario con ese mail deberías recibir
```No user found with email usuario@email.com```  

## Users

### [GET api/users/](#documentación-api)
- Retorna todos los usuarios creados hasta el momento.

- OUTPUT:

```
{
    "users": [
        {
            "id": 1,
            "name": "minombre",
            "nickname": "username",
            "password": "$2b$10$O86FUu4RFfU/PNS6CVcdzus6sqcgEkNUtzIZWV.Q3WcQMD1ieavk.",
            "age": 21,
            "gender": "female",
            "mail": "usuario@uc.cl",
            "type": "user",
            "createdAt": "2021-11-11T21:01:12.522Z",
            "updatedAt": "2021-11-11T21:01:12.522Z",
            "housings": [],
            "panoramas": []
        }
    ]
}

```
### [GET api/users/{user_id}](#documentación-api)
- Retorna al usuario con id = {user_id}.

- OUTPUT:

```
{
    "user": {
        "id": 1,
        "name": "mecambiodenombre",
        "nickname": "username",
        "age": 21,
        "gender": "female",
        "mail": "usuario@uc.cl",
        "type": "user",
        "createdAt": "2021-11-11T21:01:12.522Z",
        "updatedAt": "2021-11-11T21:03:59.165Z",
        "housings": [],
        "panoramas": []
    }
}

```
### [PATCH api/users/{user_id}/](#documentación-api)
- Para estas rutas y las que vienen es importante saber que existen tres tipos de respuesta. Por un lado si, quieres editar tu perfil o eres administrador, y el cuerpo de tu solicitud es correcta, se actualizará el usuario en cuestión.

- INPUT:

```
{
{
        "mail": "usuario@uc.cl",
        "password": "usuariopassword",
        "nickname": "username",
        "name": "mecambiodenombre",
        "age" : 21,
        "gender": "female",
        "type": "user"
    }
}
```

- OUTPUT:

```
{
    "message": "User was edit successfully!"
}
```
- Por otro lado si buscas editar, un usuario que no eres tu, te saldrá un error del tipo 403- Forbbiden, mientras que si tienes un error en el cuerpo de la solicitud te saldran errores de este tipo:  
- INPUT
```
 {
        "mail": "usuariouc.cl",
        "password": "usuariopassword",
        "nickname": "username",
        "name": "mecambiodenombre",
        "age" : 12,
        "gender": "female",
        "type": "user"
    }
``` 
- OUTPUT
```
{
    "Age limit": "You are under-age!",
    "Email format": "You have entered an invalid email address!"
}
```
### [GET api/users/{user_id}/bookings/](#documentación-api)
- Retorna las bookings que ha realizado el usuario de id: `user_id`.

- OUTPUT:

```
{
    "bookings": [
        {
            "id": 1,
            "name": "El leo",
            "arrivalDate": "2022-10-10T00:00:00.000Z",
            "departureDate": "2022-11-11T00:00:00.000Z",
            "state": "pending",
            "housingId": 2,
            "guests": 10,
            "userId": 1,
            "createdAt": "2021-11-11T21:50:21.023Z",
            "updatedAt": "2021-11-11T21:50:21.023Z",
            "housing": {
                "id": 2,
                "name": "Cabañas del Leo",
                "contact": "9 87508666",
                "availability": true,
                "ranking": 4,
                "price": 45000,
                "capacity": 6,
                "direction": "Leo 45",
                "city": "cachagua",
                "photo": "https://www.maitencillo-chile.com/images/leo/cabanas-leo-02.jpg",
                "party": true,
                "userId": 1,
                "createdAt": "2021-11-11T21:16:48.376Z",
                "updatedAt": "2021-11-11T21:44:38.169Z"
            }
        },
        {
            "id": 2,
            "name": "El leo",
            "arrivalDate": "2022-10-10T00:00:00.000Z",
            "departureDate": "2022-11-11T00:00:00.000Z",
            "state": "pending",
            "housingId": 2,
            "guests": 10,
            "userId": 1,
            "createdAt": "2021-11-11T21:52:40.120Z",
            "updatedAt": "2021-11-11T21:52:40.120Z",
            "housing": {
                "id": 2,
                "name": "Cabañas del Leo",
                "contact": "9 87508666",
                "availability": true,
                "ranking": 4,
                "price": 45000,
                "capacity": 6,
                "direction": "Leo 45",
                "city": "cachagua",
                "photo": "https://www.maitencillo-chile.com/images/leo/cabanas-leo-02.jpg",
                "party": true,
                "userId": 1,
                "createdAt": "2021-11-11T21:16:48.376Z",
                "updatedAt": "2021-11-11T21:44:38.169Z"
            }
        }
    ]
}

```

### [DELETE api/users/{user_id}/](#documentación-api)
- Elimina al usuario de id: `user_id` solo si tiene los permisos necesarios.

- OUTPUT:

```
    {
       "message": "User has been destroy!",
    },
```
---
## Housings

### [GET api/housings/](#documentación-api)
- Retorna todos los housings creados hasta el momento.

- OUTPUT:

```
{
    "housings": [
        {
            "id": 2,
            "name": "Cabaña2",
            "contact": "9 87508666",
            "availability": true,
            "ranking": 4,
            "price": 452000,
            "capacity": 26,
            "direction": "Leo 45",
            "city": "Maitencillo",
            "photo": "https://www.maitencillo-chile.com/images/leo/cabanas-leo-02.jpg",
            "party": true,
            "userId": 1,
            "createdAt": "2021-11-11T21:16:48.376Z",
            "updatedAt": "2021-11-11T21:16:48.376Z",
            "user": {
                "id": 1,
                "name": "mecambiodenombre",
                "nickname": "username",
                "password": "$2b$10$O86FUu4RFfU/PNS6CVcdzus6sqcgEkNUtzIZWV.Q3WcQMD1ieavk.",
                "age": 21,
                "gender": "female",
                "mail": "usuario@uc.cl",
                "type": "user",
                "createdAt": "2021-11-11T21:01:12.522Z",
                "updatedAt": "2021-11-11T21:03:59.165Z"
            },
            "ReviewsForHousings": []
        },
        {
            "id": 1,
            "name": "Cabaña",
            "contact": "9 87508666",
            "availability": true,
            "ranking": 4,
            "price": 45000,
            "capacity": 6,
            "direction": "Leo 45",
            "city": "Maitencillo",
            "photo": "https://www.maitencillo-chile.com/images/leo/cabanas-leo-02.jpg",
            "party": true,
            "userId": 1,
            "createdAt": "2021-11-11T21:15:55.635Z",
            "updatedAt": "2021-11-11T21:15:55.635Z",
            "user": {
                "id": 1,
                "name": "mecambiodenombre",
                "nickname": "username",
                "password": "$2b$10$O86FUu4RFfU/PNS6CVcdzus6sqcgEkNUtzIZWV.Q3WcQMD1ieavk.",
                "age": 21,
                "gender": "female",
                "mail": "usuario@uc.cl",
                "type": "user",
                "createdAt": "2021-11-11T21:01:12.522Z",
                "updatedAt": "2021-11-11T21:03:59.165Z"
            },
            "ReviewsForHousings": []
        }
    ]
}
```

### [POST api/users/{user_id}/housings/new/](#documentación-api)

- Recibe `name`, `contact`, `availability`, `price`, `ranking`, `capacity`, `party`, `direction`, `city` y  `photo` y se crea la housing. Existen casos de validaciones donde necesitas cumplir cierto tipo de restricciones.

- INPUT:

```
 {
      "name": "Cabaña",
      "contact": "9 87508666",
      "availability": true,
      "ranking": 4,
      "price": 45000,
      "capacity": 6,
      "party": true,
      "direction": "Leo 45",
      "city": "Maitencillo",
      "photo": "https://www.maitencillo-chile.com/images/leo/cabanas-leo-02.jpg"
 }
```

- OUTPUT:

```
{
    "message": "housing was create successfully!"
}
```
- Un ejemplo de lo que nos entregan estas validaciones puede ser lo siguiente:  
- INPUT:

```
 {
      "name": "Cabaña2",
      "contact": 87508666,
      "availability": true,
      "ranking": 10,
      "price": 452000,
      "party": true,
      "direction": "Leo 45",
      "city": "Maitencillo",
      "photo": "https://www.maitencillo-chile.com/images/leo/cabanas-leo-02.jpg"
 }
```

- OUTPUT:

```
{
    "Invalid Ranking": "Must be a number between 0 and 5!",
    "capacity": "capacity cannot be null",
    "contact": "contact must have type string (received number)"
}
```
### [GET api/users/{user_id}/housings/{housing_id}](#documentación-api)

- Retorna el show de housing

- OUTPUT:

```
{
    "id": 2,
    "name": "Cabaña2",
    "contact": "9 87508666",
    "availability": true,
    "ranking": 4,
    "price": 452000,
    "capacity": 26,
    "direction": "Leo 45",
    "city": "Maitencillo",
    "photo": "https://www.maitencillo-chile.com/images/leo/cabanas-leo-02.jpg",
    "party": true,
    "userId": 1,
    "createdAt": "2021-11-11T21:16:48.376Z",
    "updatedAt": "2021-11-11T21:16:48.376Z",
    "ReviewsForHousings": []
}
```
### [DELETE api/users/{user_id}/housings/{housing_id}](#documentación-api)
- Elimina al housing de id: `housing_id` solo si tiene los permisos necesarios.

- OUTPUT:

```
    {
        "Housing has been destroy!"
    },
```
### [PATCH api/users/{user_id}/housings/{housing_id}/](#documentación-api)

- Actualiza los datos de la housing

- INPUT:

```
 {
      "name": "Cabaña 1",
      "contact": "9 87508666",
      "availability": true,
      "ranking": 5,
      "price": 45000,
      "capacity": 6,
      "party": true,
      "direction": "Leo 45",
      "city": "Maitencillo",
      "photo": "https://www.maitencillo-chile.com/images/leo/cabanas-leo-02.jpg"
 }
```

- OUTPUT:

```
{
    "message": "Housing was edit successfully!"
}
```
Además pasa por los mismos filtros de validación que el método POST

---
## Bookings

### [GET api/housings/{housing_id}/bookings/new](#documentación-api)

- Retorna todas las ordenes pendientes.
- INPUT:
```
{
    "name": "El leo",
    "arrivalDate": "2022-10-10",
    "departureDate": "2022-11-11",
    "housingId": 2,
    "guests": 10
 }
 ```
- OUTPUT:

```
{
    "message": "booking was edit successfully!"
} 
```
- En el caso de el usuario cree un booking en una fecha pasada  ocurrirá lo siguiente
- INPUT
```
{
    "name": "El leo",
    "arrivalDate": "2020-10-10",
    "departureDate": "2022-11-11",
    "housingId": 2,
    "guests": 10
 }
```
- OUTPUT
```
{
    "Invalid arrivalDate": "You cannot book a date in the past!"
}
```
### [GET api/housings/{housing_id}/bookings/](#documentación-api)
- Retorna todas las reservas realizadas a un los housings hasta el momento.

- OUTPUT:

```
{
    "housings": [
        {
            "id": 2,
            "name": "Cabaña2",
            "contact": "9 87508666",
            "availability": true,
            "ranking": 4,
            "price": 452000,
            "capacity": 26,
            "direction": "Leo 45",
            "city": "Maitencillo",
            "photo": "https://www.maitencillo-chile.com/images/leo/cabanas-leo-02.jpg",
            "party": true,
            "userId": 1,
            "createdAt": "2021-11-11T21:16:48.376Z",
            "updatedAt": "2021-11-11T21:16:48.376Z",
            "user": {
                "id": 1,
                "name": "mecambiodenombre",
                "nickname": "username",
                "password": "$2b$10$O86FUu4RFfU/PNS6CVcdzus6sqcgEkNUtzIZWV.Q3WcQMD1ieavk.",
                "age": 21,
                "gender": "female",
                "mail": "usuario@uc.cl",
                "type": "user",
                "createdAt": "2021-11-11T21:01:12.522Z",
                "updatedAt": "2021-11-11T21:03:59.165Z"
            },
            "ReviewsForHousings": []
        },
        {
            "id": 1,
            "name": "Cabaña",
            "contact": "9 87508666",
            "availability": true,
            "ranking": 4,
            "price": 45000,
            "capacity": 6,
            "direction": "Leo 45",
            "city": "Maitencillo",
            "photo": "https://www.maitencillo-chile.com/images/leo/cabanas-leo-02.jpg",
            "party": true,
            "userId": 1,
            "createdAt": "2021-11-11T21:15:55.635Z",
            "updatedAt": "2021-11-11T21:15:55.635Z",
            "user": {
                "id": 1,
                "name": "mecambiodenombre",
                "nickname": "username",
                "password": "$2b$10$O86FUu4RFfU/PNS6CVcdzus6sqcgEkNUtzIZWV.Q3WcQMD1ieavk.",
                "age": 21,
                "gender": "female",
                "mail": "usuario@uc.cl",
                "type": "user",
                "createdAt": "2021-11-11T21:01:12.522Z",
                "updatedAt": "2021-11-11T21:03:59.165Z"
            },
            "ReviewsForHousings": []
        }
    ]
}
```
### [GET api/bookings/{booking_id}](#documentación-api)

- Retorna el show de booking

- OUTPUT:

```
{
    "id": 2,
    "name": "El leo",
    "arrivalDate": "2022-10-10T00:00:00.000Z",
    "departureDate": "2022-11-11T00:00:00.000Z",
    "state": "pending",
    "housingId": 2,
    "guests": 10,
    "userId": 1,
    "createdAt": "2021-11-11T21:52:40.120Z",
    "updatedAt": "2021-11-11T21:52:40.120Z",
    "housing": {
        "id": 2,
        "name": "Cabañas del Leo",
        "contact": "9 87508666",
        "availability": true,
        "ranking": 4,
        "price": 45000,
        "capacity": 6,
        "direction": "Leo 45",
        "city": "cachagua",
        "photo": "https://www.maitencillo-chile.com/images/leo/cabanas-leo-02.jpg",
        "party": true,
        "userId": 1,
        "createdAt": "2021-11-11T21:16:48.376Z",
        "updatedAt": "2021-11-11T21:44:38.169Z"
    }
}
```
### [PATCH api/bookings/{booking_id}/status/](#documentación-api)

- Actualiza los datos de la housing

- INPUT:

```
 {     
    {
        "id": 5,
        "name": "El leo",
        "arrivalDate": "2022-10-10T00:00:00.000Z",
        "departureDate": "2022-11-11T00:00:00.000Z",
        "state": "taken2",
        "housingId": 2,
        "guests": 10,
        "userId": 1,
        "createdAt": "2021-11-11T03:28:41.356Z",
        "updatedAt": "2021-11-11T03:28:41.356Z",
    }
 }
```

- OUTPUT:

```
{
    "message": "booking was edit successfully!"
}
```

### [PATCH api/bookings/{booking_id}/edit/](#documentación-api)

- Actualiza los datos de la housing

- INPUT:

```
 {     
    {
        "id": 5,
        "name": "El leo",
        "arrivalDate": "2022-10-10T00:00:00.000Z",
        "departureDate": "2022-11-11T00:00:00.000Z",
        "state": "taken",
        "housingId": 2,
        "guests": 5,
        "userId": 1,
        "createdAt": "2021-11-11T03:28:41.356Z",
        "updatedAt": "2021-11-11T03:28:41.356Z",
    }
 }
```

- OUTPUT:

```
{
    "message": "booking was edit successfully!"
}
```

### [DELETE api/bookings/{booking_id}](#documentación-api)
- Elimina al booking de id: `booking_id` solo si tiene los permisos necesarios.

- OUTPUT:

```
    {
        "booking has been destroy!"
    },
```

---

## Panoramas


### [GET api/panoramas/](#documentación-api)
- Retorna todos los panoramas creados hasta el momento.

- OUTPUT:

```
[
    {
        "id": 1,
        "name": "playa abanico",
        "description": "muy entretenido, relajado",
        "address": "maitencillo 9",
        "city": "maitencillo",
        "userId": 1,
        "createdAt": "2021-11-11T22:20:16.998Z",
        "updatedAt": "2021-11-11T22:20:16.998Z",
        "user": {
            "id": 1,
            "name": "mecambiodenombre",
            "nickname": "username",
            "password": "$2b$10$O86FUu4RFfU/PNS6CVcdzus6sqcgEkNUtzIZWV.Q3WcQMD1ieavk.",
            "age": 19,
            "gender": "female",
            "mail": "usuario@uc.cl",
            "type": "user",
            "createdAt": "2021-11-11T21:01:12.522Z",
            "updatedAt": "2021-11-11T21:42:56.354Z"
        },
        "ReviewsForPanoramas": []
    }
]
```

### [POST api/panoramas/new/](#documentación-api)

- Recibe `description`, `name`, `address`, `city` y se crea el panorama. Existen casos de validaciones donde necesitas cumplir cierto tipo de restricciones.

- INPUT:

```
{
  "description": "muy entretenido, relajado",
  "name": "playa abanico",
  "address": "maitencillo 9",
  "city": "maitencillo"
}
```

- OUTPUT:

```
{
    "message": "panorama was create successfully!"
}
```
- Un ejemplo de lo que nos entregan estas validaciones puede ser lo siguiente:  
- INPUT:

```
{
  "description": "muy entretenido, relajado",
  "name": "playa abanico",
  "city": "maitencillo"
}
```

- OUTPUT:

```
{
    "address": "address cannot be null"
}
```
### [GET api/panoramas/{panorama_id}](#documentación-api)

- Retorna el show de panorama

- OUTPUT:

```
{
    "id": 1,
    "name": "playa abanico",
    "description": "muy entretenido, relajado",
    "address": "maitencillo 9",
    "city": "maitencillo",
    "userId": 1,
    "createdAt": "2021-11-11T22:20:16.998Z",
    "updatedAt": "2021-11-11T22:20:16.998Z",
    "ReviewsForPanoramas": []
}
```
### [DELETE api/panoramas/{panorama_id}](#documentación-api)
- Elimina al panorama de id: `panorama_id` solo si tiene los permisos necesarios.

- OUTPUT:

```
    {
        "Panorama has been destroy!"
    },
```
### [PATCH api/panoramas/{panorama_id}/](#documentación-api)

- Actualiza los datos de la panorama

- INPUT:

```
 {
  "description": "muy entretenido, relajado",
  "name": "playa abanico",
  "address": "santiago 21",
  "city": "maitencillo"
}
```

- OUTPUT:

```
{
    "message": "panorama was edit successfully!"
}
```
Además pasa por los mismos filtros de validación que el método POST

---

## ReviewForPanoramas

### [GET api/panoramas/{panorama_id}/reviews/](#documentación-api)
- Retorna todos los panoramas creados hasta el momento.

- OUTPUT:

```
[
    {
        "id": 1,
        "body": "gracias por la recomendacion!",
        "ranking": 4,
        "userId": 1,
        "panoramaId": 1,
        "createdAt": "2021-11-11T22:30:12.880Z",
        "updatedAt": "2021-11-11T22:30:12.880Z",
        "user": {
            "id": 1,
            "name": "mecambiodenombre",
            "nickname": "username",
            "password": "$2b$10$O86FUu4RFfU/PNS6CVcdzus6sqcgEkNUtzIZWV.Q3WcQMD1ieavk.",
            "age": 19,
            "gender": "female",
            "mail": "usuario@uc.cl",
            "type": "user",
            "createdAt": "2021-11-11T21:01:12.522Z",
            "updatedAt": "2021-11-11T21:42:56.354Z"
        }
    },
    {
        "id": 2,
        "body": "gracias por la recomendacion! x2 me encantó",
        "ranking": 5,
        "userId": 1,
        "panoramaId": 1,
        "createdAt": "2021-11-11T22:31:24.280Z",
        "updatedAt": "2021-11-11T22:31:24.280Z",
        "user": {
            "id": 1,
            "name": "mecambiodenombre",
            "nickname": "username",
            "password": "$2b$10$O86FUu4RFfU/PNS6CVcdzus6sqcgEkNUtzIZWV.Q3WcQMD1ieavk.",
            "age": 19,
            "gender": "female",
            "mail": "usuario@uc.cl",
            "type": "user",
            "createdAt": "2021-11-11T21:01:12.522Z",
            "updatedAt": "2021-11-11T21:42:56.354Z"
        }
    }
]
```

### [POST api/panoramas/{panorama_id}/reviews/new/](#documentación-api)

- Crea una nueva review para un panorama, esta tiene una calificación y un comentario

- INPUT:

```
{
    "body" : "gracias por la recomendacion!",
    "ranking" : 4
}
```

- OUTPUT:

```
{
    "message": "Review was create successfully!"
}
```

### [DELETE api/panoramas/{panorama_id}/reviews/{reviewForPanorama_id}](#documentación-api)
- Elimina a la review de panorama de id: `reviewForPanorama_id` solo si tiene los permisos necesarios.

- OUTPUT:

```
    {
        "review has been destroy!"
    },
```
### [PATCH api/panoramas/{panorama_id}/reviews/{reviewForPanorama_id}/](#documentación-api)

- Actualiza los datos del panorama 

- INPUT:

```
{
    "body" : "me gustaría cambiar la calificación",
    "ranking" : 2
}
```

- OUTPUT:

```
{
    "message": "Review was edit successfully!"
}
```
Además pasa por los mismos filtros de validación que el método POST

---

## ReviewForHousings

### [GET api/housings/{housing_id}/reviews/](#documentación-api)
- Retorna todos los housings creados hasta el momento.

- OUTPUT:

```
[
    {
        "id": 1,
        "body": "gracias por la recomendacion!",
        "ranking": 4,
        "userId": 1,
        "panoramaId": 1,
        "createdAt": "2021-11-11T22:30:12.880Z",
        "updatedAt": "2021-11-11T22:30:12.880Z",
        "user": {
            "id": 1,
            "name": "mecambiodenombre",
            "nickname": "username",
            "password": "$2b$10$O86FUu4RFfU/PNS6CVcdzus6sqcgEkNUtzIZWV.Q3WcQMD1ieavk.",
            "age": 19,
            "gender": "female",
            "mail": "usuario@uc.cl",
            "type": "user",
            "createdAt": "2021-11-11T21:01:12.522Z",
            "updatedAt": "2021-11-11T21:42:56.354Z"
        }
    },
    {
        "id": 2,
        "body": "gracias por la recomendacion! x2 me encantó",
        "ranking": 5,
        "userId": 1,
        "panoramaId": 1,
        "createdAt": "2021-11-11T22:31:24.280Z",
        "updatedAt": "2021-11-11T22:31:24.280Z",
        "user": {
            "id": 1,
            "name": "mecambiodenombre",
            "nickname": "username",
            "password": "$2b$10$O86FUu4RFfU/PNS6CVcdzus6sqcgEkNUtzIZWV.Q3WcQMD1ieavk.",
            "age": 19,
            "gender": "female",
            "mail": "usuario@uc.cl",
            "type": "user",
            "createdAt": "2021-11-11T21:01:12.522Z",
            "updatedAt": "2021-11-11T21:42:56.354Z"
        }
    }
]
```

### [POST api/houssings/{housing_id}/reviews/new/](#documentación-api)

- Crea una nueva review para un housing, esta tiene una calificación y un comentario

- INPUT:

```
{
    "body" : "gracias por la recomendacion!",
    "ranking" : 4
}
```

- OUTPUT:

```
{
    "message": "Review was create successfully!"
}
```

### [DELETE api/panoramas/{panorama_id}/reviews/{reviewForPanorama_id}](#documentación-api)
- Elimina a la review de panorama de id: `reviewForHousing_id` solo si tiene los permisos necesarios.

- OUTPUT:

```
    {
        "review has been destroy!"
    },
```
### [PATCH api/housings/{housing_id}/reviews/{reviewForHousing_id}/](#documentación-api)

- Actualiza los datos del housing 

- INPUT:

```
{
    "body" : "me gustaría cambiar la calificación",
    "ranking" : 2
}
```

- OUTPUT:

```
{
    "message": "Review was edit successfully!"
}
```
Además pasa por los mismos filtros de validación que el método POST

---

#### [Back to top](#documentación-api)