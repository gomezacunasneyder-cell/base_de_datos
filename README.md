# Sistema de Ventas de Repuestos y Accesorios — Modelo E-R

Proyecto académico de diseño de base de datos: del caos inicial (requerimientos en texto) hasta el modelo Entidad-Relación y su traducción a tablas SQL, siguiendo la metodología de 3 pasos ("Del Concepto a la Arquitectura Final") y la notación vista en clase.

## Contenido del proyecto

| Archivo | Descripción |
|---|---|
| `Caso_Estudio_Sistema_Ventas_Repuestos.docx` | Documento completo con los 3 pasos: requerimientos, modelo conceptual (entidades/atributos/cardinalidades) y estructura física (SQL, datos de prueba, consultas) |
| `ER_Completo_Sistema_Ventas_Repuestos.drawio` | Diagrama E-R importable en [app.diagrams.net](https://app.diagrams.net/), con las 6 entidades y sus atributos |
| `ER_Sistema_Ventas_Repuestos.drawio` | Versión inicial del diagrama (solo las 4 entidades base: Cliente, Venta, Producto, Detalle_Venta) |

## Notación utilizada

Según el material de clase ("Componentes Esenciales del Modelo E-R"):

- **Rectángulo simple** → entidad fuerte (tiene clave primaria propia)
- **Rectángulo doble** → entidad débil (depende de otra entidad, no tiene clave propia)
- **Óvalo** → atributo (el atributo que es clave primaria va subrayado)
- **Rombo `<nombre_relacion>`** → relación entre dos entidades
- **Números junto a la línea** (`1`, `N`) → cardinalidad de la relación

## Resumen del modelo

**Entidades:**
- `CATEGORIA` (fuerte) — id_categoria, nombre_categoria
- `CLIENTE` (fuerte) — id_cliente, documento, nombre, correo, telefono
- `EMPLEADO` (fuerte) — id_empleado, documento, nombre, cargo, fecha_contratacion
- `PRODUCTO` (fuerte) — id_producto, codigo, nombre_producto, precio_unitario, stock
- `VENTA` (fuerte) — id_venta, num_factura, fecha, total
- `DETALLE_VENTA` (**débil**) — cantidad, precio_unitario_venta, subtotal (clave compuesta: id_venta + id_producto)

**Relaciones:**
| Relación | Entre | Cardinalidad |
|---|---|---|
| `<clasifica>` | Categoria → Producto | 1:N |
| `<realiza>` | Cliente → Venta | 1:N |
| `<procesa>` | Empleado → Venta | 1:N |
| `<incluye>` | Producto → Detalle_Venta | 1:N |
| `<contiene>` | Venta → Detalle_Venta | 1:N |

La relación conceptual original Venta–Producto es N:M; se resuelve con la entidad débil `Detalle_Venta`, que la descompone en dos relaciones 1:N.
🎯 Pistas para la Modelación (Entidad-Relación) — Sistema de Ventas de Repuestos y Accesorios

Para facilitarte la estructuración del diagrama, aquí tienes una guía rápida de las relaciones que deberías mapear:

Entidades a Relacionar	Tipo de Relación	Notas/Consideraciones
Categoría ↔ Producto	1:N	Una categoría (frenos, motor, suspensión, eléctrico) agrupa muchos productos, pero cada producto pertenece a una sola categoría.
Cliente ↔ Venta	1:N	Un cliente realiza muchas ventas (facturas), pero cada venta pertenece a un solo cliente.
Empleado ↔ Venta	1:N	Un empleado (vendedor) procesa muchas ventas, pero cada venta la procesa un solo empleado.
Venta ↔ Producto	M:N	Requerirá una tabla intermedia (Detalle_Venta), guardando la cantidad vendida y el precio unitario histórico en el momento de la transacción.
Producto ↔ Proveedor (opcional, si se extiende el modelo)	M:N	Requerirá tabla de detalle (ej. Producto_Proveedor), guardando el precio de compra y tiempos de entrega — útil si quieres modelar de dónde se abastece cada repuesto.
Cliente ↔ Producto (Reseña) (opcional, si se extiende el modelo)	M:N	O relación ternaria según tu enfoque, guardando calificación y comentario — simula reseñas de calidad de un repuesto.

## Historial de prompts enviados durante el desarrollo

Registro de las solicitudes que diste para construir este proyecto, en orden:

1. > soy estudiante y estoy aprediendo base de datos dame un caso de estudio de ventas de cualquier modelo dame la entidad relacion

2. > debo hacer una base de datos en base al modelo de ventas que me des y lo debo plasmar [Diagrama sin título - draw.io](https://app.diagrams.net/) con estas reglas
   *(adjuntando la imagen de "Componentes Esenciales del Modelo E-R")*

3. > Caso de Estudio: Sistema de Ventas de Repuestos y Accesorios
   > **Paso 1: Caos y Necesidad (Requerimientos del Negocio)** — Clientes, Productos, Ventas (Facturas) y Detalle de Venta, con sus atributos.
   > **Paso 2: Plano Conceptual** — Entidades Cliente, Producto y Venta; relaciones `realiza` (1:N) y `contiene` (N:M); nota sobre la entidad intermedia Detalle_Venta.
   > **Paso 3: Estructura Física** — inicio de la traducción a tablas relacionales (`[ TABLA: Clientes ]`)

4. > ok ahora debo plasmar este proyecto en la pagina que te dije draw.io siguiendo estos casos

5. > si
   *(pidiendo ver cómo se ve la notación de rombo con línea doble para la relación identificadora)*

6. > siguie estas inidicaciones
   *(adjuntando 15 capturas del material del curso: ejemplos de cardinalidad 1:1, 1:N, N:M, componentes del modelo E-R, llaves primarias/foráneas, SQL vs NoSQL, etc., para ajustar la notación exacta usada en clase)*

7. > generame mas cosas una estructura completa y detallada

8. > como se veria
    *(pidiendo ver el resultado visual de los rombos con los nombres de relación ya escritos)*

9. > creame un readme sobre eso y el promt que yo te mande
    *(este documento)*
