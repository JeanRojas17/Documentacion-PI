# Diagrama de Clases (Mermaid) — Transportes RBL

Este diagrama representa la arquitectura de clases del sistema de información para la empresa Transportes RBL, incluyendo entidades, servicios y sus relaciones.

```mermaid
classDiagram
    class Usuario {
        -int idUsuario
        -String nombre
        -String apellido
        -String email
        -String password
        -int idRol
        -String telefono
        -String estado
        -datetime fechaRegistro
        -datetime ultimaConexion
        +Usuario(nombre, apellido, email, password, idRol)
        +login(email, password) boolean
        +actualizarUltimaConexion() void
        +cambiarEstado(nuevoEstado) void
        +getNombreCompleto() String
        +validarCredenciales(password) boolean
        +getRol() Rol
    }
    
    class Rol {
        -int idRol
        -String nombreRol
        -String descripcion
        -datetime fechaCreacion
        +Rol(nombreRol, descripcion)
        +getPermisos() array
        +asignarPermiso(permiso) void
        +revocarPermiso(permiso) void
    }
    
    class Chofer {
        -int idChofer
        -int idUsuario
        -String licencia
        -String estado
        -date fechaVencimientoLicencia
        -int aniosExperiencia
        +Chofer(idUsuario, licencia, fechaVencimiento)
        +validarLicencia() boolean
        +estaDisponible() boolean
        +getAsignaciones() array
        +actualizarExperiencia() void
        +renovarLicencia(nuevaFecha) void
    }
    
    class Auxiliar {
        -int idAuxiliar
        -int idUsuario
        -String estado
        -String especialidad
        +Auxiliar(idUsuario, especialidad)
        +estaDisponible() boolean
        +getAsignaciones() array
        +cambiarEspecialidad(nuevaEspecialidad) void
    }
    
    class Camion {
        -int idCamion
        -String placa
        -String marca
        -String modelo
        -int anio
        -double capacidad
        -int idEstadoCamion
        -int idUsuario
        -int kilometraje
        -datetime fechaRegistro
        +Camion(placa, marca, modelo, anio, capacidad)
        +estaDisponible() boolean
        +cambiarEstado(nuevoEstado) void
        +actualizarKilometraje(km) void
        +getCapacidadDisponible() double
        +asignarResponsable(usuario) void
        +necesitaMantenimiento() boolean
        +getHistorialAsignaciones() array
    }
    
    class EstadoCamion {
        -int idEstadoCamion
        -String estado
        -String descripcion
        +EstadoCamion(estado, descripcion)
        +getCamiones() array
        +esDisponible() boolean
    }
    
    class Ruta {
        -int idRuta
        -String origen
        -String destino
        -double distancia
        -String tiempoEstimado
        -decimal peaje
        +Ruta(origen, destino, distancia)
        +calcularCosto() double
        +calcularTiempoEstimado() String
        +getDescripcion() String
        +esDisponible() boolean
        +getAsignaciones() array
    }
    
    class Producto {
        -int idProducto
        -String nombre
        -String tipo
        -String descripcion
        -double peso
        -int cantidad
        -int idUsuario
        -datetime fechaRegistro
        +Producto(nombre, tipo, peso)
        +actualizarCantidad(cantidad) void
        +estaDisponible() boolean
        +getPesoTotal() double
        +getStock() int
        +reducirStock(cantidad) boolean
        +aumentarStock(cantidad) void
    }
    
    class Cliente {
        -int idCliente
        -int idUsuario
        -String nombreEmpresa
        -String nit
        -String contactoNombre
        -String contactoTelefono
        -String direccion
        -String ciudad
        +Cliente(nombreEmpresa, nit, contactoNombre)
        +getEntregas() array
        +getDireccionCompleta() String
        +actualizarContacto(nombre, telefono) void
        +getHistorialPedidos() array
    }
    
    class Asignacion {
        -int idAsignacion
        -int idCamion
        -int idChofer
        -int idAuxiliar
        -int idRuta
        -int idUsuario
        -String estado
        -datetime fecha
        -datetime fechaSalida
        -datetime fechaLlegada
        -String observaciones
        +Asignacion(idCamion, idChofer, idRuta, fechaSalida)
        +crear() boolean
        +actualizar() boolean
        +eliminar() boolean
        +cambiarEstado(nuevoEstado) void
        +agregarProducto(producto, cantidad) void
        +eliminarProducto(producto) void
        +getProductos() array
        +getCamion() Camion
        +getChofer() Chofer
        +getRuta() Ruta
        +calcularPesoTotal() double
        +validarCapacidad() boolean
        +iniciar() void
        +completar() void
        +cancelar() void
    }
    
    class AsignacionProducto {
        -int idAsignacionProducto
        -int idAsignacion
        -int idProducto
        -int cantidad
        +AsignacionProducto(idAsignacion, idProducto, cantidad)
        +getPesoTotal() double
        +validarCantidad() boolean
    }
    
    class Entrega {
        -int idEntrega
        -int idAsignacion
        -int idCliente
        -int idEstadoEntrega
        -datetime fechaProgramada
        -datetime fechaEntrega
        -String observaciones
        +Entrega(idAsignacion, idCliente, fechaProgramada)
        +programar() boolean
        +entregar() boolean
        +rechazar(motivo) void
        +cancelar(motivo) void
        +getEstado() EstadoEntrega
        +esEntregada() boolean
        +estaPendiente() boolean
        +registrarEntrega() void
    }
    
    class EstadoEntrega {
        -int idEstadoEntrega
        -String estado
        +EstadoEntrega(estado)
        +getEntregas() array
        +esEstadoFinal() boolean
    }
    
    class AutenticacionService {
        -Connection db_connection
        +login(email, password) Usuario
        +logout(usuario) void
        +validarSesion(token) boolean
        +cambiarPassword(usuario, nuevaPassword) boolean
        +recuperarPassword(email) void
    }
    
    class AsignacionService {
        -Connection db_connection
        +crear(asignacionData) Asignacion
        +actualizar(id, asignacionData) boolean
        +eliminar(id) boolean
        +listar(filtros) array
        +obtenerPorId(id) Asignacion
        +cambiarEstado(id, estado) boolean
        +validarDisponibilidad(camion, chofer, fecha) boolean
    }
    
    class CamionService {
        -Connection db_connection
        +crear(camionData) Camion
        +actualizar(id, camionData) boolean
        +eliminar(id) boolean
        +listar(filtros) array
        +obtenerDisponibles() array
        +cambiarEstado(id, estado) boolean
        +registrarMantenimiento(id, tipo) void
    }
    
    class ProductoService {
        -Connection db_connection
        +crear(productoData) Producto
        +actualizar(id, productoData) boolean
        +eliminar(id) boolean
        +listar(filtros) array
        +actualizarStock(id, cantidad) boolean
        +getDisponibles() array
    }
    
    class ReporteService {
        -Connection db_connection
        +generarReporteAsignaciones(fechaInicio, fechaFin) array
        +generarReporteProductos() array
        +generarReporteCamiones() array
        +generarReporteChoferes() array
        +getEstadisticasDashboard() array
        +exportarPDF(tipoReporte, datos) file
        +exportarExcel(tipoReporte, datos) file
    }
    
    class DashboardService {
        -Connection db_connection
        +getEstadisticasGenerales() array
        +getEntregasActivas() int
        +getEntregasCompletadas() int
        +getEntregasPendientes() int
        +getCamionesDisponibles() int
        +getCapacidadDisponible() double
    }
    
    %% Relaciones
    Usuario "n" --> "1" Rol : tiene
    Chofer "1" --> "1" Usuario : es un
    Auxiliar "1" --> "1" Usuario : es un
    Camion "n" --> "1" EstadoCamion : tiene
    Camion "n" --> "0..1" Usuario : asignado a
    Asignacion "n" --> "1" Camion : usa
    Asignacion "n" --> "1" Chofer : conducida por
    Asignacion "n" --> "0..1" Auxiliar : asistida por
    Asignacion "n" --> "1" Ruta : recorre
    Asignacion "n" --> "1" Usuario : creada por
    AsignacionProducto "n" --> "1" Asignacion : pertenece a
    AsignacionProducto "n" --> "1" Producto : contiene
    Asignacion "n" -- "n" Producto : transporta
    Entrega "n" --> "1" Asignacion : corresponde a
    Entrega "n" --> "1" Cliente : para
    Entrega "n" --> "1" EstadoEntrega : tiene
    Cliente "n" --> "0..1" Usuario : asociado a
    Producto "n" --> "0..1" Usuario : registrado por
    
    %% Servicios
    AutenticacionService ..> Usuario : gestiona
    AsignacionService ..> Asignacion : gestiona
    CamionService ..> Camion : gestiona
    ProductoService ..> Producto : gestiona
    ReporteService ..> Asignacion : analiza
    DashboardService ..> Asignacion : consulta
```

<div align="center">

[← Volver al README](README.md)

</div>