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

| Parametro | Descripción | Requerido
| --- | --- | --- |
| client_id | Identificador único de cliente provisto por LabLab. | Si
| scope | Ámbito para acceder a los servicios de LabLab. Ámbitos disponibles: `outplacement` | Si
| data | Token JWT utilizando el `client_secret` para encriptar la información compartida. | Si

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
        "message": "Acceso no autorizado"
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

    POST /oauth/authorization HTTP/1.1
    Host: www.lablab.cl
    Authorization: Bearer 0c3d46946bbf99c8213...

### Ejemplo de respuesta de Éxito

    {
        "code": "101",
        "message": "Acceso no autorizado",
        "redirect_url": "https://www.lablab.cl/outplacement",
    }

### Ejemplo de respuesta de Error

    {
        "code": "101",
        "message": "Acceso no autorizado"
    }