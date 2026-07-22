# Sistema de Ventas de Repuestos y Accesorios — Modelo E-R

Diseñar y comprender el Modelo Entidad-Relación (MER) utilizando llaves primarias (PK), llaves foráneas (FK) y definiendo correctamente la cardinalidad entre las entidades, aplicado a un entorno real de gestión de inventario y facturación. Este proyecto académico recorre el proceso completo de diseño de base de datos: parte del caos inicial (requerimientos en texto) hasta llegar al modelo Entidad-Relación y su traducción a tablas SQL, siguiendo la metodología de 3 pasos ("Del Concepto a la Arquitectura Final") y la notación vista en clase.

## Funcionalidades del sistema

El sistema modelado debe soportar las siguientes operaciones sobre el negocio de venta de repuestos y accesorios para motos:

**Gestión de inventario**
- Registrar, consultar, actualizar y eliminar productos (repuestos y accesorios).
- Clasificar cada producto dentro de una categoría (frenos, motor, suspensión, eléctrico, etc.).
- Controlar el stock disponible de cada producto y alertar cuando esté por debajo de un mínimo.
- Actualizar el precio unitario de un producto sin afectar el precio ya facturado en ventas anteriores.

**Gestión de clientes**
- Registrar clientes con su documento, nombre, correo y teléfono.
- Consultar el historial de compras de un cliente específico.

**Gestión de empleados**
- Registrar empleados (vendedores) con su documento, nombre, cargo y fecha de contratación.
- Asociar cada venta al empleado que la atendió, para control de desempeño y comisiones.

**Gestión de ventas y facturación**
- Generar una nueva venta (factura) asociada a un cliente y a un empleado.
- Registrar el detalle de cada venta: qué productos, en qué cantidad y a qué precio se vendieron.
- Calcular automáticamente el subtotal por línea y el total de la factura.
- Conservar el precio histórico de venta de cada producto, aunque el precio en el catálogo cambie después.
- Consultar facturas por número, fecha o cliente.

**Reportes**
- Productos más vendidos por cantidad.
- Total facturado por cliente o por empleado en un periodo.
- Productos con stock bajo, para gestionar reabastecimiento.

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

## Cómo importar el diagrama en draw.io

1. Entra a [app.diagrams.net](https://app.diagrams.net/)
2. Menú **Extras → Importar desde → Dispositivo**
3. Selecciona el archivo `.drawio`
4. El diagrama se carga en el lienzo, editable

---

## Caso de Estudio: Modelo Entidad-Relación para "MotoRepuestos Xtreme"

### Objetivo de la Asignación

Diseñar y comprender el Modelo Entidad-Relación (MER) utilizando llaves primarias (PK) y llaves foráneas (FK), definiendo correctamente la cardinalidad entre las entidades, aplicado a un entorno real de gestión de inventario, clientes, empleados y ventas en una tienda de repuestos y accesorios para motocicletas.

### 1. Prompt Utilizado (One-Shot Prompt)

Para iniciar la conceptualización de este proyecto y estructurar el caso de estudio, se utilizó la siguiente instrucción estructurada dirigida a un modelo de Inteligencia Artificial. Este "One-Shot Prompt" define el rol, el alcance y los requerimientos técnicos en una sola petición:

> "Actuarás como experto en manejo de base de datos. Quiero aprender el modelo entidad-relación, usando primary key (PK) y foreign key (FK), así que he decidido tomar un caso de estudio basado en una tienda de repuestos y accesorios para motos. Crea un plano conceptual, buscando enfatizar ejemplos con la relación de cardinalidad para entender cómo se conectan las tablas de clientes, empleados, categorías, productos (repuestos como pastillas de freno, filtros, amortiguadores, baterías) y las ventas con su respectivo detalle."

### 2. Explicación del Modelo Conceptual (Notación de Chen)

A partir del análisis generado, se diseñó el diagrama conceptual adjunto (referenciado como `ER_Completo_Sistema_Ventas_Repuestos.drawio` en el contexto del taller). El sistema se compone de seis entidades principales y sus respectivas relaciones:

- **CLIENTE**: Almacena la información de contacto de los compradores. Su atributo principal (PK) es `id_cliente`.
- **EMPLEADO**: Representa al vendedor que atiende cada transacción. Su PK es `id_empleado`. Se relaciona con Venta bajo una cardinalidad 1:N (un empleado procesa muchas ventas, pero cada venta la procesa un solo empleado).
- **CATEGORIA**: Clasifica los repuestos por tipo (frenos, motor, suspensión, eléctrico). Su PK es `id_categoria`, y se relaciona con Productos bajo cardinalidad 1:N.
- **PRODUCTO**: Representa el inventario físico de repuestos y accesorios (ej. pastillas de freno, filtros de aceite, amortiguadores). Su PK es `id_producto`. Posee atributos adicionales como `codigo`, `precio_unitario` y `stock` para el control de inventario.
- **VENTA**: Representa la factura generada. Su PK es `id_venta`. Contiene las llaves foráneas `id_cliente` e `id_empleado` para establecer las relaciones de cardinalidad 1:N correspondientes (un cliente realiza muchas ventas, pero cada venta pertenece a un solo cliente).
- **DETALLE_VENTA**: Actúa como la entidad intermedia (entidad débil) que resuelve la cardinalidad de Muchos a Muchos (N:M) entre las ventas y los productos.
  - Posee una cardinalidad 1:N con Venta (una venta contiene muchos detalles).
  - Posee una cardinalidad N:1 con Producto (muchos detalles incluyen un mismo repuesto).
  - Utiliza `id_venta` e `id_producto` como llaves foráneas (que en conjunto forman su clave primaria compuesta) para registrar la cantidad vendida y el precio al que se vendió en esa transacción específica, manteniendo la integridad de la facturación aunque el precio del repuesto cambie después en el catálogo.

### 3. Conclusión del Diseño

El uso de la entidad `Detalle_Venta` garantiza que no exista duplicidad de datos y que el modelo cumpla con las reglas de normalización de bases de datos relacionales, separando correctamente la cabecera de la factura (Venta) de los ítems individuales vendidos (Detalle_Venta). Esto permite llevar un control preciso de qué repuestos componen cada venta, conservar el precio histórico de cada transacción y mantener actualizado el stock disponible de cada producto, sin redundancia de información entre las tablas.

---

## 🎯 Pistas para la Modelación (Entidad-Relación)

Guía rápida de las relaciones mapeadas en el proyecto:

| Entidades a Relacionar | Tipo de Relación | Notas/Consideraciones |
|---|---|---|
| Categoría ↔ Producto | 1:N | Una categoría (frenos, motor, suspensión, eléctrico) agrupa muchos productos, pero cada producto pertenece a una sola categoría. |
| Cliente ↔ Venta | 1:N | Un cliente realiza muchas ventas (facturas), pero cada venta pertenece a un solo cliente. |
| Empleado ↔ Venta | 1:N | Un empleado (vendedor) procesa muchas ventas, pero cada venta la procesa un solo empleado. |
| Venta ↔ Producto | M:N | Requerirá una tabla intermedia (`Detalle_Venta`), guardando la cantidad vendida y el precio unitario histórico en el momento de la transacción. |
| Producto ↔ Proveedor *(opcional, extensión)* | M:N | Requerirá tabla de detalle (ej. `Producto_Proveedor`), guardando el precio de compra y tiempos de entrega — útil si quieres modelar de dónde se abastece cada repuesto. |
| Cliente ↔ Producto (Reseña) *(opcional, extensión)* | M:N | O relación ternaria según tu enfoque, guardando calificación y comentario — simula reseñas de calidad de un repuesto. |

---

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

7. > ok damelo para pasarselo a draw.io

8. > que parte
   *(captura del menú Archivo de draw.io, preguntando dónde importar el archivo)*

9. > generame mas cosas una estructura completa y detallada

10. > como se veria
    *(pidiendo ver el resultado visual de los rombos con los nombres de relación ya escritos)*

11. > creame un readme sobre eso y el promt que yo te mande
