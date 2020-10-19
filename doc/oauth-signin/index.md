# LabLab OAuth SignIn

[Inicio](../../README.md) | LabLab OAuth SignIn

---

Bienvenido a **LabLab OAuth SignIn**.

Con esta integración podrá ingresar a LabLab a través de su plataforma utilizando OAuth.

## Antes de comenzar

- Deberá contar con una cuenta de cliente en LabLab.
- Deberá contar con un `client_id` y un `client_secret` proveídos por LabLab.
- Deberá indicarnos los `ips` desde donde recibiremos las solicitudes de integración.
- Utilice el HTTPS HOST `qa.lablab.cl` en fase de `pre-integración` para realizar pruebas.
- Cuando las pruebas hallan sido exitosas y le notifiquemos la integración apunte al HTTPS HOST `www.lablab.cl` para producción.

## 1. Obtener un Token de Acceso

El primer paso para acceder a LabLab es solicitar un Token de Acceso o `Access Token`.

Para hacer esto, realice la siguiente solicitud HTTPS POST con un encabezado `Content-Type` con `x-www-form-urlencoded` a:

    POST
    https://www.lablab.cl/oauth/access-token

### Parámetros

| Parametro | Descripción                                                                        | Requerido
| ---       | ---                                                                                | ---
| client_id | Identificador único de cliente provisto por LabLab.                                | Si
| scope     | Ámbito para acceder a los servicios de LabLab. Ámbitos disponibles: `outplacement` | Si
| data      | Token JWT utilizando el `client_secret` para encriptar la información compartida.  | Si

### Ejemplo de solicitud

    POST /oauth/access-token HTTP/1.1
    Host: www.lablab.cl
    Content-Type: application/x-www-form-urlencoded

    client_id=465bdb4e51987df7d015cc&scope=outplacement&data=4e51987df7d015cc465bdb...

Una solicitud de token de acceso exitosa devuelve un objeto JSON que contiene los siguientes campos:

- `access_token`: el token de acceso para la aplicación. Este valor debe mantenerse seguro.
- `expires_in`: el número de segundos que quedan hasta que expire el token. Actualmente, todos los tokens de acceso se emiten con una vida útil de 5 minutos.

### Ejemplo de respuesta de Éxito

    {
        "access_token": "0c3d46946bbf99c8213...",
        "expires_in": 300
    }

### Ejemplo de respuesta de Error

    {
        "code": "101",
        "description": "Acceso no autorizado"
    }

## 2. Ingreso con un Token de Acceso

Una vez que haya recibido un token de acceso, puede ingresar a la plataforma.

Para hacer esto, realice la siguiente solicitud HTTPS POST utilizando el token en un encabezado `Authorization` con `Bearer 0c3d46946bbf99c8213...` a:

    POST
    https://www.lablab.cl/oauth/signin

Una solicitud de ingreso exitosa devuelve un objeto JSON que contiene los siguientes campos:

- `code`: un código interno de respuesta.
- `message`: un mensaje de respuesta.
- `redirect_url`: una url de redirección para concluir el ingreso.

### Ejemplo de solicitud

    POST /oauth/signin HTTP/1.1
    Host: www.lablab.cl
    Authorization: Bearer 0c3d46946bbf99c8213...

### Ejemplo de respuesta de Éxito

    {
        "code": "N101",
        "description": "Accso autorizado. Redirección a Login",
        "redirect_url": "https://www.lablab.cl/outplacement",
    }

### Ejemplo de respuesta de Error

    {
        "code": "E101",
        "description": "Acceso no autorizado"
        "message": "Su programa ha finalizado"
    }

## Mensajes de Error

Los mensajes de error tendrán la siguiente estructura. Preste atención a las propiedades que puedan ser utilizada de manera pública.

### Propiedades

| Propiedad   | Descripción                | Público | Requerido
| ---         | ---                        | ---     | ---
| code        | Código de error            | NO      | SI
| description | Descripción de error       | NO      | SI
| message     | Mensaje para usuario final | SI      | NO

### Códigos de error de cliente

Estos van acompañados por códigos de estado HTTP de la gama de los 400 ~ 499, Siendo 400, 403, 404 los más utilizados.

| Código | Descripción
| ---    | ---
| E101   | Dato client_id no válido.
| E102   | Dato scope no válido.
| E103   | Dato data no válido
| E105   | El Access Token es requerido.
| E106   | El Access Token no es válido.
| E107   | El Access Token payload no válido.
| E109   | El Access Token no válido.
| E110   | El Access Token no fue validado.
| E111   | El Usuario no está habilitado para ingresar a la plataforma
| E112   | El Access Token no está debidamente firmado.
| E113   | El Access Token ya expiró.
| E114   | Dato user_id no válido.
| E115   | No quedan licencias disponibles.

### Códigos de error de servidor

Estos van acompañados por códigos de estado HTTP de la gama de los 500 ~ 599, Siendo 500 el más utilizado.

| Código | Descripción
| ---    | ---
| C100   | Error al obtener datos de cliente.
| C104   | Error al registrar el Access Token
| C108   | Error al obtener datos de Access Token
| C116   | Error al registrar la Licencia.

## Mensajes de Éxito

Los mensajes de error tendrán la siguiente estructura. Preste atención a las propiedades que puedan ser utilizada de manera pública.

Estos van acompañados por códigos de estado HTTP 200.

### Propiedades

| Propiedad    | Descripción                 | Público | Requerido
| ---          | ---                         | ---     | ---
| code         | Código de notificación      | NO      | SI
| description  | Descripción de notificación | NO      | SI
| message      | Mensaje para usuario final  | SI      | NO
| redirect_url | URL de para redirección     | SI      | SI

### Códigos de notificación

| Código | Descripción
| ---    | ---
| N100   | Validación correcta. Redirección a Login
| N101   | Se requiere registro para continuar. Redirección a Registro
