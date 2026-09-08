# runt-simit-colombia

Guía de referencia — abierta y sin código — sobre **qué datos vehiculares publica el Estado
colombiano, dónde, y qué devuelve realmente cada fuente**.

Nace de una frustración concreta: casi toda la información que circula sobre "consultar el RUNT
por placa" es imprecisa o directamente falsa. Este repositorio documenta lo verificado contra
las fuentes oficiales.

> Mantenido por el equipo de [PlacApi](https://placapi.com). Aquí no hay código del servicio:
> es documentación de las fuentes públicas.

## Las fuentes

| Fuente | Qué publica | Qué pide | Portal |
|---|---|---|---|
| **RUNT** | Ficha del vehículo, SOAT, tecnomecánica, gravámenes, licencias de conducción | Placa **+ documento del propietario** | [runt.gov.co](https://www.runt.gov.co) |
| **SIMIT** | Comparendos, acuerdos de pago, resoluciones, paz y salvo | Placa o documento | [fcm.org.co/simit](https://www.fcm.org.co/simit) |
| **FASECOLDA** | Guía de valores: avalúo comercial por referencia | Placa o código FASECOLDA | [fasecolda.com](https://www.fasecolda.com) |
| **RGM** | Garantías mobiliarias sobre el vehículo | Placa | [garantiasmobiliarias.com.co](https://www.garantiasmobiliarias.com.co) |
| **Secretarías de movilidad** | Pico y placa por ciudad | — | cada alcaldía |

## Diez cosas que casi todo el mundo publica mal

**1. El RUNT no entrega la ficha solo con la placa.**
Exige placa **y** documento del propietario. No es un obstáculo técnico: es control de
privacidad. Cualquier servicio que prometa "placa → nombre y cédula del dueño" está saltándose
ese control, y eso es zona gris de la Ley 1581 de 2012 (Habeas Data).

**2. Pero el SOAT suelto sí se consulta solo con placa.**
El portal público del RUNT tiene una consulta ciudadana específica de SOAT que no pide el
documento. Son dos consultas distintas del mismo portal.

**3. Un vehículo nuevo sin tecnomecánica no está "vencido": está exento.**
La exención es de **2 años para particulares** y **1 año para servicio público**, contados desde
la matrícula. Mostrar "vencida" en un carro de 6 meses es un error de lectura, no de la fuente.

**4. "No registra antecedentes" es un dato, no una consulta vacía.**
Es exactamente el dato que se fue a buscar. Cualquier servicio serio lo cobra igual.

**5. Los antecedentes judiciales de la Policía no son antecedentes penales.**
Por la Sentencia SU-458 de 2012 de la Corte Constitucional, la consulta que hace un tercero
**no revela condenas ya cumplidas o prescritas**. Un "no tiene asuntos pendientes" no significa
que la persona nunca haya sido condenada.

**6. La Contraloría responde "sin antecedentes" a documentos que no existen.**
El certificado de responsabilidad fiscal no valida que la cédula exista. Un documento inventado
sale limpio. Si necesitas saber si la persona existe, eso lo decide otra fuente.

**7. El SISBEN se comporta igual.**
`ConsultarGrupoSisben` asigna un grupo a documentos inexistentes. La existencia la decide la
consulta al RUI, no la del grupo.

**8. El pico y placa no es nacional ni uniforme.**
Cada municipio lo fija por acto administrativo propio, y las reglas cambian cada pocos meses.
Hay tres formas de rotación conviviendo: tabla fija por día de la semana, paridad del día
calendario (Bogotá) y ciclos que corren. Y en la mayoría de ciudades **la moto se rige por el
último dígito, no por el primero** — confundirlo da el día equivocado a cerca del 80% de las
placas de moto.

**9. Barranquilla y Neiva no tienen pico y placa general para particulares.**
Aparecen en muchas listas por inercia. Al cierre de esta revisión no había medida vigente.

**10. La guía de valores de FASECOLDA no cubre los modelos más nuevos.**
La consulta por VIN no resuelve vehículos de 2025 en adelante. Para esos hay que ir por
marca → línea → versión.

## Códigos de respuesta que vas a encontrar

Válido para casi cualquier integración contra estas fuentes, no solo la nuestra:

- **404** — la fuente no tiene el registro. Reintentar da lo mismo.
- **502 / timeout** — la fuente oficial falló. Reintentar **sí** funciona: son portales con
  disponibilidad irregular, no APIs con SLA.
- **Captcha** — todas las fuentes ciudadanas lo tienen, en formatos distintos (imagen,
  reCAPTCHA, preguntas de texto). No hay API pública oficial para desarrolladores en ninguna.

## Marco legal

- **Ley 769 de 2002** — Código Nacional de Tránsito
- **Ley 1581 de 2012** — protección de datos personales (Habeas Data)
- **Sentencia SU-458 de 2012** — límites a la publicidad de antecedentes penales
- **Ley 1005 de 2006** — creación del RUNT

## Contribuir

¿Encontraste un dato desactualizado o un municipio que cambió su medida? Abre un issue con el
enlace al acto administrativo o al portal oficial. Se acepta cualquier corrección con fuente
verificable.

## Si necesitas esto en JSON

[PlacApi](https://placapi.com) resuelve estas consultas por API REST, sin captchas.
Especificación: [placapi-openapi](https://github.com/pipe0919/placapi-openapi).

## Licencia

[CC BY 4.0](LICENSE) — usa el contenido citando la fuente.
