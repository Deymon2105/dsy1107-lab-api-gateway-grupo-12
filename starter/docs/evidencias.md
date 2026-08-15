# Evidencias · Laboratorio API Gateway

## Integrantes
- Nombre:
- Nombre:
- Nombre:

## 1. Backend directo

Antes de utilizar el gateway, registrar las pruebas directas contra JSONPlaceholder.

| Método | URL | Status | Observación |
|---|---|---:|---|
| GET | `https://jsonplaceholder.typicode.com/posts` | | |
| GET | `https://jsonplaceholder.typicode.com/posts/1` | | |

**¿Qué información del backend conoce el cliente en este escenario?**

Respuesta:

---

## 2. Arquitectura final

```mermaid
flowchart LR
    WEB[Cliente web :5500]
    P[Postman]
    G[Spring Cloud Gateway :8080]
    B[JSONPlaceholder]

    WEB --> G
    P --> G
    G --> B
    B --> G
    G --> WEB
    G --> P
```

Explicar brevemente qué responsabilidad cumple cada componente.

---

## 3. Pruebas HTTP mediante gateway

| Método | URL | Status | Headers relevantes | Interpretación |
|---|---|---:|---|---|
| GET | `/api/v1/posts` | | | colección |
| GET | `/api/v1/posts/1` | | | recurso individual |
| POST | `/api/v1/posts` | | | creación simulada |
| PUT | `/api/v1/posts/1` | | | actualización simulada |
| DELETE | `/api/v1/posts/1` | | | eliminación simulada |

Para POST y PUT incluir también el body enviado.

---

## 4. Routing

- URL solicitada por el cliente:
- `id` de la route:
- predicate que hizo match:
- URI/integration configurada:
- path recibido finalmente por el backend:
- función de `RewritePath`:

### Recorrido de una petición

Explicar con sus palabras:

```text
cliente → gateway → backend → gateway → cliente
```

---

## 5. Versionado

- Evidencia `/api/v1`:
- Header `X-API-Version` observado:
- Evidencia `/api/v2`:
- Header `X-API-Version` observado:

Responder:

1. ¿Por qué mantener v1 y v2 simultáneamente?
2. ¿Qué consumidores podrían seguir usando v1?
3. ¿Cuándo retirarían una versión?
4. ¿Versionar el contrato público es lo mismo que versionar el servidor desplegado?

---

## 6. Header transversal

- Header esperado: `X-Gateway-Lab: DSY1107`
- Evidencia observada: presente en todas las respuestas del gateway. En `GET /api/v1/posts/1` y `GET /api/v2/posts/1` se obtuvo `HTTP/1.1 200 OK` con `X-Gateway-Lab: DSY1107` y el `X-API-Version` correspondiente (v1 y v2).
- ¿Por qué este comportamiento puede considerarse transversal?: porque se define una sola vez en `spring.cloud.gateway.server.webflux.default-filters` y se aplica a **todas** las rutas sin repetir la configuración en cada una. Es comportamiento de infraestructura (identificación del gateway), no lógica de negocio del backend: cualquier ruta nueva hereda el header automáticamente.

---

## 7. CORS

### Antes de configurar CORS

- URL del cliente web: `http://localhost:5500`
- Endpoint consultado:
- Resultado visible:
- Mensaje relevante en Console/Network:

### Después de configurar CORS

- Resultado visible:
- `Access-Control-Allow-Origin`:
- `Access-Control-Allow-Methods`:

### Preflight OPTIONS

- Request utilizado:
- Status:
- Headers relevantes:

Responder:

1. ¿Por qué Postman puede funcionar cuando el navegador falla?
2. ¿Qué es un preflight?
3. ¿CORS autentica o autoriza usuarios?
4. ¿Qué riesgo tendría permitir cualquier origen sin analizar el contexto?

---

## 8. Richardson Maturity Model nivel 2

Explicar qué elementos observados en el laboratorio permiten afirmar que la API utiliza recursos, métodos HTTP y status codes con semántica HTTP.

---

## 9. Responsabilidades

| Responsabilidad | Cliente | Gateway | Backend | Justificación |
|---|:---:|:---:|:---:|---|
| routing | | X | | El gateway decide a qué backend va cada petición según el predicate `Path`. |
| lógica de negocio | | | X | El dominio y su cálculo viven en el backend (JSONPlaceholder). |
| autenticación/autorización | | X | X | El gateway puede validar el token en el punto de entrada; el backend también debe validar para no confiar solo en el gateway. |
| transformación de rutas | | X | | `RewritePath`/`StripPrefix` ocurren en el gateway antes de enrutar. |
| persistencia | | | X | El almacenamiento de datos pertenece al backend. |
| rate limiting | | X | X | Suele implementarse en el gateway (límite global de entrada); el backend puede reforzarlo según su capacidad. |
| reglas de negocio | | | X | Validaciones propias del dominio, no de infraestructura. |
| observabilidad | | X | X | El gateway registra el tráfico de entrada; el backend reporta métricas internas. |

Autenticación, rate limiting y observabilidad son **capacidades transversales**: aunque el gateway las centralice, el backend debe mantener sus propios controles de seguridad y monitoreo.

---

## 10. Problemas encontrados

1. Problema:
   - causa:
   - solución:

---

## 11. Colaboración GitHub

| Integrante | Rama | Pull Request | Aporte principal |
|---|---|---|---|
| | | | |

Agregar enlaces a los Pull Requests.

---

## 12. Conclusiones

- ¿Qué problema resolvió el gateway?
- ¿Qué concepto del laboratorio sería equivalente al trabajar posteriormente con Amazon API Gateway?
- ¿Qué aprendió el grupo que no depende específicamente de Spring Cloud Gateway?
