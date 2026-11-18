# Flujos del curso de Apache NiFi

Dos formas de usar estos flujos. La versión de referencia es **Apache NiFi 2.12.0**. Los ejemplos usan pedidos sintéticos y no necesitan servicios externos.

## Descargar e importar un JSON

| Sesión 03 | Archivo | Para qué sirve |
|---|---|---|
| Demo D01 | [Descargar S03_D01.json](https://raw.githubusercontent.com/andgosds/nifi-curso-flows/main/descargas/S03/S03_D01.json) | Seguir cambios de atributos y contenido y demostrar replay. |
| Inicio de E01 | [Descargar S03_E01_inicio.json](https://raw.githubusercontent.com/andgosds/nifi-curso-flows/main/descargas/S03/S03_E01_inicio.json) | Investigar una transformación incorrecta y contrastar una entrega nueva con replay. |

Guarda el JSON como archivo. En el canvas, arrastra **Process Group**, pulsa **Browse**, selecciona el JSON, asigna un nombre y pulsa **Add**. Los datos de entrada sintéticos están incluidos en el generador del flujo. Antes de ejecutar, ajusta `Directory` de **Entregar pedido** a una carpeta de pruebas del equipo que ejecuta NiFi. Conserva `${uuid}` al final. Usa **Run Once** y deja los componentes `NO ARRANCAR` deshabilitados.

## Importar o versionar desde el canvas

En **Controller Settings → Registry Clients**, añade un cliente de tipo **GitHubFlowRegistryClient** con estos valores:

| Propiedad | Valor |
|---|---|
| GitHub API URL | `https://api.github.com/` |
| Repository Owner | `andgosds` |
| Repository Name | `nifi-curso-flows` |
| Default Branch | `main` |
| Repository Path | `registry` |
| Parameter Context Values | `Remove` |
| Authentication Type | `None` para consultar o importar desde este repositorio público |

Al crear un **Process Group**, usa **Import from Registry**, elige el cliente, la rama `main` y el bucket `S03`. Los flujos aparecen como `S03_D01` y `S03_E01_inicio`. Si tu instalación no incluye el cliente, no permite configurarlo o GitHub limita las consultas anónimas, usa las descargas de arriba.

Para publicar cambios desde NiFi necesitas autorización de escritura. Configura en la propiedad sensible del cliente un token de alcance fino, limitado a este repositorio, con **Contents: Read and write**, o utiliza una GitHub App autorizada. En el menú del grupo hijo, usa **Version → Start version control** para un flujo nuevo o **Version → Commit local changes** para un grupo ya versionado. Nunca compartas un token con el alumnado ni lo guardes en Git.

`registry/` contiene el formato gestionado por NiFi. `descargas/` contiene JSON preparados para importar por archivo. Un commit desde el canvas **no actualiza automáticamente** `descargas/`: al preparar una publicación nueva, exporta la definición aprobada, revisa sus valores y actualiza la descarga con el mismo código. Este repositorio público excluye soluciones, credenciales y datos reales.
