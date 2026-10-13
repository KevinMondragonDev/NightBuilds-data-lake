# 🏞️ Reto 1 — Data lake en AWS local

**Repositorio:** `NightBuilds-data-lake` · **Nivel:** intermedio · **Entorno:** Floci (S3, Glue, Athena) · **Método y evaluación:** [`README.md`](docs/METODO.md)

## El caso

Una empresa de movilidad guarda años de viajes en archivos sueltos. Tres equipos distintos piden cosas distintas a los mismos datos: operaciones quiere la demanda por hora, finanzas el ingreso diario y ciencia de datos el detalle limpio. Nadie quiere copiar los datos tres veces ni esperar a que un servidor termine. Les construyes un data lake.

## Lo que debe existir al terminar

- **Tres capas en S3** (cruda, limpia y de negocio), en Parquet y particionadas con criterio.
- **Un catálogo** (Glue) que describa cada tabla, para que cualquiera la consulte con SQL (Athena) sin saber dónde están los archivos.
- **Un proceso repetible** que lleve un día de datos de la capa cruda a la de negocio y que se pueda relanzar.
- **Cifras de control** que demuestren que no se perdió nada entre capas.
- **Una API y una visualización en D3.js** que respondan a la pregunta de uno de los tres equipos.

## Datos

Viajes de taxi de Nueva York (NYC TLC), al menos 3 meses. Es mucho volumen a propósito: tiene que doler si particionas mal.

- Página oficial con todos los archivos y el diccionario de datos: <https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page>
- Un mes de ejemplo (enero de 2025, unos 59 MB en Parquet): <https://d37ci6vzurychx.cloudfront.net/trip-data/yellow_tripdata_2025-01.parquet>

No subas estos archivos a tu repositorio: descárgalos con un script.

## Restricciones

- Todo corre con `docker compose up` y un comando. Nada de pasos manuales en una consola.
- Ninguna consulta de la capa de negocio debe leer todos los archivos: demuéstralo.
- En Floci, Athena **no** escribe tablas nuevas: las capas las escribe tu código (pandas, Polars o Spark) y después las registras en el catálogo.
- `grep -r localhost` en tu código no devuelve nada: el destino sale de la configuración.

## Preguntas que tu diseño debe responder

- ¿Por qué columna particionas cada capa y cuántos archivos genera? ¿Qué pasa con una columna de un millón de valores?
- ¿Qué va en cada capa y quién puede leerla?
- Llega un archivo con una columna nueva. ¿Qué se rompe?
- ¿Cómo reprocesas un mes sin tocar los demás?

## Defensa: lo que te van a preguntar

- Enséñame una consulta que lea solo una partición y otra que las lea todas. ¿Cómo lo sabes?
- ¿Qué diferencia hay entre este data lake y una carpeta con archivos?
- ¿Qué cambia para desplegarlo en AWS de verdad y cuánto costaría al mes?

## Súbelo de nivel

- Convierte la capa limpia a **Apache Iceberg** y demuestra una ventaja concreta (viaje en el tiempo o evolución del esquema).
- Haz las transformaciones con **PySpark** y compara con tu primera versión.
- Añade una capa de permisos: quién puede ver qué columnas.
