(Mi proyecto sin zip esta en la rama Master)

# Proyecto2Análisis Proyecto

1. ¿Por qué se utiliza SCOPE_IDENTITY() en el método Crear?
En el método Crear de JugadorService, usamos SCOPE_IDENTITY() para obtener el ID del jugador recién insertado en la base de datos. Esto es útil porque el ID se genera automáticamente en la base de datos, y necesitamos ese valor para asignarlo al objeto Jugador en el código. Esto asegura que el ID sea único y correcto.
---
2. ¿Por qué se verifica la existencia de elementos en el inventario antes de eliminar un jugador?
Esto lo hicimos para evitar inconsistencias en la base de datos. Si un jugador tiene elementos en su inventario y lo eliminamos sin verificar, esos elementos quedarían "huérfanos" (sin un jugador asociado). Esto podría causar errores en otras partes del sistema.
---
3. ¿Qué ventaja ofrece using var connection = _dbManager.GetConnection();?
Usamos esta línea para asegurarnos de que la conexión a la base de datos se cierre automáticamente al terminar de usarla. Esto evita fugas de recursos y asegura que la conexión no quede abierta accidentalmente, lo que podría causar problemas de rendimiento.
---
4. ¿Por qué _connectionString está marcado como readonly?
Marcamos _connectionString como readonly para que su valor no pueda cambiar después de ser inicializado. Esto mejora la seguridad, ya que evita que alguien modifique accidentalmente la cadena de conexión durante la ejecución del programa.
---
5. ¿Cómo agregarías un sistema de logros para los jugadores?
·	Modelo: Crearíamos una nueva clase Logro con propiedades como Id, Nombre, Descripcion y JugadorId.
·	Base de datos: Agregaríamos una tabla Logros con una relación de clave foránea a Jugadores.
·	Servicio: Crearíamos un LogroService para manejar la lógica de negocio de los logros, como agregar, eliminar y listar logros.
---
6. ¿Qué sucede con la conexión cuando ocurre una excepción en un bloque using?
La conexión se cierra automáticamente, incluso si ocurre una excepción. Esto es gracias a que SqlConnection implementa la interfaz IDisposable.
---
7. ¿Qué ocurre si la consulta SQL no devuelve ningún jugador en ObtenerTodos()?
Devuelve una lista vacía. Esto es útil porque evita errores de referencia nula y permite manejar el caso de "sin resultados" de manera uniforme en la interfaz gráfica.
---
8. ¿Cómo registrarías el tiempo jugado por cada jugador?
·	Modelo: Agregaríamos una propiedad TiempoJugado en la clase Jugador.
·	Base de datos: Agregaríamos una columna TiempoJugado en la tabla Jugadores.
·	Servicio: Actualizaríamos el método Actualizar para incluir la lógica de tiempo jugado.
---
9. ¿Por qué es importante devolver un valor booleano en TestConnection()?
Esto permite al sistema saber si la conexión fue exitosa sin lanzar excepciones. Es útil para manejar errores de manera controlada y mostrar mensajes claros al usuario.
---
10. ¿Por qué se separaron las clases en carpetas como Models, Services y Utils?
Esto sigue el principio de separación de responsabilidades:
·	Models: Representan las entidades del sistema.
·	Services: Contienen la lógica de negocio.
·	Utils: Proveen utilidades generales, como la gestión de la base de datos. Esta estructura facilita el mantenimiento y la escalabilidad del proyecto.
---
11. ¿Por qué usar una transacción SQL en AgregarItem?
Para asegurarnos de que todas las operaciones relacionadas con la adición de un ítem se completen correctamente o se deshagan en caso de error. Esto evita inconsistencias en la base de datos.
---
12. ¿Por qué JugadorService recibe un DatabaseManager como parámetro?
Esto sigue el patrón de inyección de dependencias, lo que facilita las pruebas y el mantenimiento del código. Nos permite cambiar la implementación de DatabaseManager sin modificar JugadorService.
---
13. ¿Qué ocurre si se busca un ID que no existe en ObtenerPorId?
Devuelve null. Esto permite manejar el caso de "no encontrado" de manera explícita en el código.
---
14. ¿Cómo implementarías un sistema de "amigos"?
·	Modelo: Crearíamos una tabla Amigos con columnas JugadorId y AmigoId.
·	Servicio: Agregaríamos métodos en JugadorService para agregar y eliminar amigos.
---
15. ¿Cómo se maneja la fecha de creación de un jugador?
La fecha de creación se establece automáticamente en la base de datos mediante una columna con un valor predeterminado de GETDATE(). Esto asegura que la fecha sea precisa y consistente.
---
16. ¿Por qué se crea una nueva instancia de SqlConnection cada vez?
Para garantizar que cada operación tenga su propia conexión, evitando problemas de concurrencia y asegurando que las conexiones se cierren correctamente.
---
17. ¿Qué ocurre si dos usuarios modifican el mismo recurso simultáneamente?
Podría haber conflictos. Esto se puede manejar con bloqueos o control de concurrencia optimista, como verificaciones de versiones.
---
18. ¿Por qué verificar rowsAffected en Actualizar?
Para saber si la operación fue exitosa y notificar al usuario si no se encontró el registro.
---
19. ¿Cómo implementarías un sistema de logging?
Agregaríamos un servicio de logging que registre todas las operaciones en un archivo o base de datos. Este servicio se llamaría desde los métodos de los servicios existentes.
---
20. ¿Cómo agregarías la entidad "Mundo"?
·	Modelo: Crearíamos una tabla Mundos y una tabla intermedia JugadorMundos.
·	Servicio: Agregaríamos métodos en JugadorService para manejar la relación.
---
21. ¿Qué es un SqlConnection y cómo se usa?
Es una clase que representa una conexión a una base de datos SQL Server. Se usa para ejecutar comandos y consultas.
---
22. ¿Para qué sirven los SqlParameter?
Se usan para evitar inyecciones SQL al parametrizar las consultas, asegurando que los valores se manejen de forma segura.
---



proyecto_parcial2

·	DatabaseManager: Gestiona las conexiones a la base de datos.
·	JugadorService, BloqueService, InventarioService: Encapsulan la lógica de negocio para cada entidad.
·	Modelos (Jugador, Bloque, Inventario): Representan las entidades del sistema.
Componentes Clave
1. DatabaseManager
·	Propósito:
·	Centralizar la lógica de conexión a la base de datos.
·	Asegurar que todas las operaciones de base de datos usen la misma configuración.
·	Métodos principales:
·	GetConnection: Devuelve una nueva conexión a la base de datos.
·	TestConnection: Permite probar si la conexión a la base de datos es válida.
·	Ventajas:
·	Simplifica el manejo de conexiones.
·	Evita fugas de recursos al usar using para cerrar conexiones automáticamente.
2. Servicios
Los servicios encapsulan la lógica de negocio y la interacción con la base de datos para cada entidad.
a. JugadorService
·	Propósito:
·	Gestionar las operaciones relacionadas con los jugadores.
·	Métodos principales:
·	Crear: Inserta un nuevo jugador en la base de datos.
·	ObtenerTodos: Recupera todos los jugadores.
·	Actualizar: Modifica un jugador existente.
·	Eliminar: Borra un jugador, verificando primero si tiene ítems en el inventario.
·	Ejemplo: Método Crear
·	Usa SqlCommand para ejecutar una consulta SQL que inserta un nuevo jugador.
·	Usa SCOPE_IDENTITY() para obtener el ID generado automáticamente.
b. BloqueService
·	Propósito:
·	Gestionar las operaciones relacionadas con los bloques.
·	Métodos principales:
·	Crear: Inserta un nuevo bloque en la base de datos.
·	ObtenerTodos: Recupera todos los bloques.
·	BuscarPorTipo: Filtra bloques por tipo.
·	BuscarPorRareza: Filtra bloques por rareza.
c. InventarioService
·	Propósito:
·	Gestionar las operaciones relacionadas con el inventario.
·	Métodos principales:
·	Agregar: Agrega un ítem al inventario, actualizando la cantidad si ya existe.
·	ObtenerTodos: Recupera todos los registros del inventario, incluyendo información del jugador y del bloque.
·	Actualizar: Modifica la cantidad de un ítem en el inventario.
·	Eliminar: Borra un ítem del inventario.
·	Ejemplo: Método ObtenerTodos
·	Usa una consulta SQL con INNER JOIN para combinar datos de las tablas Inventario, Jugadores y Bloques.
·	Devuelve una lista de objetos Inventario con información adicional como NombreJugador y NombreBloque.
3. Modelos
Los modelos representan las entidades del sistema y se utilizan para transferir datos entre la base de datos y la aplicación.
a. Jugador
·	Propiedades:
·	Id: Identificador único.
·	Nombre: Nombre del jugador.
·	Nivel: Nivel del jugador.
·	FechaCreacion: Fecha en que se creó el jugador.
b. Bloque
·	Propiedades:
·	Id: Identificador único.
·	Nombre: Nombre del bloque (ej. "Madera", "Hierro").
·	Tipo: Tipo de bloque (ej. "Mineral", "Decoración").
·	Rareza: Rareza del bloque (ej. "Común", "Raro").
c. Inventario
·	Propiedades:
·	Id: Identificador único.
·	JugadorId: Identificador del jugador asociado.
·	BloqueId: Identificador del bloque asociado.
·	Cantidad: Cantidad de bloques en el inventario.
·	NombreJugador: Nombre del jugador (para reportes).
·	NombreBloque: Nombre del bloque (para reportes).
---
Form1 llama a los servicios (JugadorService, BloqueService, InventarioService) para realizar operaciones.
·	Ejemplo: Al hacer clic en el botón "Buscar Ítem", Form1 llama a InventarioService.ObtenerTodos y filtra los resultados por NombreBloque.
Los servicios interactúan con la base de datos a través de DatabaseManager.
·	Ejemplo: InventarioService.ObtenerTodos usa DatabaseManager.GetConnection para ejecutar una consulta SQL.
·	Ejemplo: Los datos obtenidos por InventarioService.ObtenerTodos se asignan como fuente de datos de dgvInventario.



Proyecto WinFormsApp1

·	Clases principales:
·	Form1: Es la ventana principal de la aplicación. Contiene los controles (botones, cuadros de texto, tablas, etc.) y la lógica para manejar eventos de usuario.
·	Propósito:
·	Mostrar datos al usuario en controles como DataGridView.
·	Permitir al usuario realizar operaciones como agregar, buscar, actualizar y eliminar jugadores, bloques e ítems del inventario.
·	Conectar la interfaz gráfica con los servicios de negocio (JugadorService, BloqueService, InventarioService).
·	DataGridView (dgvJugadores, dgvBloques, dgvInventario):
·	Muestran datos tabulares de jugadores, bloques e inventario.
·	Permiten seleccionar registros para realizar operaciones como actualizar o eliminar.
·	Botones (btnAgregarJugador, btnBuscarItem, etc.):
·	Ejecutan acciones específicas como agregar, buscar o eliminar registros.
·	Cuadros de texto (txtNombreJugador, txtNombreItem, etc.):
·	Permiten al usuario ingresar datos para realizar búsquedas o agregar nuevos registros.
·	CargarJugadores, CargarBloques, CargarInventario:
·	Llaman a los servicios correspondientes para obtener datos de la base de datos.
·	Asignan los datos obtenidos como fuente de datos de los controles DataGridView.
·	Ejemplo: btnBuscarItem_Click
·	Este evento se activa cuando el usuario hace clic en el botón de buscar ítem.
·	Llama al método ObtenerTodos del servicio InventarioService y filtra los resultados por el nombre del bloque (NombreBloque).
·	Actualiza el DataGridView (dgvInventario) con los resultados filtrados.
·	Form1 utiliza instancias de JugadorService, BloqueService e InventarioService para interactuar con la base de datos.
·	Estas instancias se inicializan en el constructor de Form1 utilizando un objeto de DatabaseManager.

