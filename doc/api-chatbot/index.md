# LabLab Api Chatbot

[Inicio](../../README.md) | LabLab Api Chatbot

---

Bienvenido a **LabLab Api Chatbot**.

Con esta integración podrá obtener datos desde LabLab para utilizarlas en el ChatBot.

## Antes de comenzar

- Deberá contar con un `client_id`.
- Deberá indicarnos los `ips` desde donde recibiremos las solicitudes de integración.
- Utilice el HTTPS HOST `qa.lablab.cl` en fase de `pre-integración` para realizar pruebas.
- Cuando las pruebas hallan sido exitosas y le notifiquemos la integración apunte al HTTPS HOST `www.lablab.cl` para producción.

## Recursos

- Usuarios
- Medias

## Usuarios

Este recurso le permite obtener el programa al que está inscripto el usuario.

### Petición

    GET
    /api/chatbot/users/{hash}

### Respuesta de Exito

    {
        "success": true,
        "data": {
            "id": "ecb7937db58ec9dea0c47db88463d85e81143032",
            "full_name": "Miguel Jordán",
            "phone": "+569 4229 4079",
            "email": "ingenierojordan@gmail.com",
            "program": "ejecutivo"
        }
    }

### Respuesta de Error

    {
        "success": false,
        "message": "Not Found."
    }

## Medias

Este recurso le permite obtener una url de la sección de un curso para ser visualizada.

### Petición

    GET
    /api/chatbot/medias/{slug}

### Respuesta de Exito

    {
        "success": true,
        "data": {
            "url": "https://lablab.test/outplacement/capacitacion/curso/4/detalles?autoplay=6"
        }
    }

### Respuesta de Error

    {
        "success": false,
        "message": "Not Found."
    }