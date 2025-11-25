# ReservasGO

Una aplicación Android moderna para gestionar reservas en restaurantes, desarrollada con Jetpack Compose y siguiendo la arquitectura MVVM.

## 📱 Características

- **Autenticación de usuarios**: Sistema de registro e inicio de sesión
- **Exploración de restaurantes**: Navega por una lista de restaurantes con información detallada
- **Sistema de reservas**: Realiza reservas en tus restaurantes favoritos
- **Gestión de favoritos**: Marca y guarda tus restaurantes preferidos
- **Notificaciones**: Recibe recordatorios sobre tus reservas
- **Filtros avanzados**: Filtra restaurantes por tipo de cocina y ubicación
- **Integración con Google Maps**: Visualiza la ubicación de los restaurantes
- **Búsqueda por proximidad**: Encuentra restaurantes cercanos a tu ubicación

## 🏗️ Arquitectura

El proyecto sigue el patrón **MVVM (Model-View-ViewModel)** con las siguientes capas:
```
app/
├── data/
│   ├── model/          # Modelos de datos
│   ├── repository/     # Repositorios para acceso a datos
│   └── responses/      # Clases de respuesta de la API
├── network/
│   ├── ReservasGoAPI   # Interface de Retrofit
│   ├── RetrofitClient  # Configuración de Retrofit
│   └── SessionManager  # Gestión de sesiones con DataStore
├── ui/
│   ├── cards/          # Componentes reutilizables
│   ├── theme/          # Configuración del tema
│   └── screens/        # Pantallas de la aplicación
├── viewmodel/          # ViewModels para la lógica de negocio
└── Workers.kt          # WorkManager para notificaciones
```

## 🛠️ Tecnologías y Librerías

### Core
- **Kotlin** - Lenguaje de programación
- **Jetpack Compose** - UI moderna y declarativa
- **Material Design 3** - Diseño de interfaz

### Arquitectura y Estado
- **ViewModel** - Gestión del estado de la UI
- **StateFlow** - Manejo de estados reactivos
- **Navigation Compose** - Navegación entre pantallas

### Networking
- **Retrofit 2.9.0** - Cliente HTTP
- **Gson** - Serialización/deserialización JSON

### Almacenamiento
- **DataStore Preferences** - Almacenamiento local de preferencias

### Mapas y Localización
- **Google Maps Compose** - Integración de mapas
- **Play Services Location** - Servicios de ubicación

### Imágenes
- **Coil 2.2.2** - Carga de imágenes asíncronas

### Background Tasks
- **WorkManager** - Programación de notificaciones

## 📋 Requisitos

- Android Studio Hedgehog o superior
- Kotlin 1.9.0
- compileSdk 35
- minSdk 24
- targetSdk 35
- JDK 17

## 🚀 Instalación

1. **Clona el repositorio**
```bash
git clone https://github.com/tu-usuario/ReservasGO_DADM.git
cd ReservasGO_DADM
```

2. **Configura la API Key de Google Maps**
   
   En `app/src/main/AndroidManifest.xml`, reemplaza la API Key:
```xml
   <meta-data
       android:name="com.google.android.geo.API_KEY"
       android:value="TU_API_KEY_AQUI" />
```

3. **Configura la URL base del servidor**
   
   En `app/src/main/java/unex/cum/reservasgo_dadm/network/RetrofitClient.kt`:
```kotlin
   private const val BASE_URL = "http://TU_IP_SERVIDOR/"
```

4. **Sincroniza el proyecto con Gradle**
   
   Abre el proyecto en Android Studio y espera a que se sincronicen las dependencias.

5. **Ejecuta la aplicación**
   
   Conecta un dispositivo Android o usa un emulador y presiona Run.

## 📱 Pantallas Principales

### Autenticación
- **LoginScreen**: Inicio de sesión de usuarios
- **RegisterScreen**: Registro de nuevos usuarios

### Principal
- **MainScreen**: Lista de restaurantes con filtros y búsqueda
- **RestauranteScreen**: Detalles del restaurante y opción de reserva

### Usuario
- **UsuarioScreen**: Perfil del usuario
- **ReservasScreen**: Historial de reservas
- **FavoritosScreen**: Restaurantes favoritos
- **NotificacionesScreen**: Notificaciones del sistema

## 🔧 Características Técnicas

### Gestión de Estado
El proyecto utiliza `StateFlow` para gestionar el estado de manera reactiva:
```kotlin
private val _restaurantes = MutableStateFlow<List<Restaurante>>(emptyList())
val restaurantes: StateFlow<List<Restaurante>> = _restaurantes
```

### Persistencia de Datos
Utiliza DataStore Preferences para almacenar datos de sesión:
```kotlin
class SessionManager(private val context: Context) {
    suspend fun saveUserId(userId: Int)
    suspend fun getUserId(): Int
}
```

### WorkManager para Notificaciones
Implementa recordatorios programados para reservas:
- Recordatorio 1 hora antes
- Recordatorio 1 día antes
- Recordatorio 3 días antes

### Filtrado y Búsqueda
- Filtrado por tipo de cocina
- Búsqueda por proximidad geográfica
- Radio de búsqueda configurable (hasta 10 km)

## 🔐 Permisos

La aplicación requiere los siguientes permisos:
```xml
<uses-permission android:name="android.permission.INTERNET" />
<uses-permission android:name="android.permission.POST_NOTIFICATIONS"/>
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
```

## 📊 Modelos de Datos

### Usuario
```kotlin
data class Usuario(
    val id_usuario: Int,
    val nombre: String,
    val email: String,
    val password: String,
    val foto_perfil: String,
    val fecha_registro: String,
    val telefono: String,
    val direccion: String
)
```

### Restaurante
```kotlin
data class Restaurante(
    val id_restaurante: Int,
    val nombre: String,
    val direccion: String,
    val telefono: String,
    val tipo_cocina: String,
    val descripcion: String,
    val foto: String,
    val rating_promedio: Float
)
```

### Reserva
```kotlin
data class Reserva(
    val id_reserva: Int,
    val id_usuario: Int,
    val id_restaurante: Int,
    val fecha_reserva: String,
    val numero_personas: Int
)
```

## 🎨 Tema y Diseño

La aplicación utiliza Material Design 3 con un color principal personalizado:
```kotlin
val colorApp = Color(0xFF4CA992)
```

## 🔄 API Endpoints

La aplicación se conecta a una API REST con los siguientes endpoints principales:

- `GET /obtenerRestaurantes` - Lista de restaurantes
- `GET /obtenerRestaurantePorId` - Detalles de un restaurante
- `POST /registrarUsuario` - Registro de usuario
- `POST /obtenerUsuario` - Login de usuario
- `GET /verReservas` - Reservas del usuario
- `POST /hacerReserva` - Crear nueva reserva
- `POST /agregarFavorito` - Añadir restaurante a favoritos
- `GET /obtenerFavoritos` - Lista de favoritos
- `POST /crearNotificacion` - Crear notificación

## 📝 Notas de Desarrollo

- El proyecto usa `usesCleartextTraffic="true"` para desarrollo. **Desactívalo en producción**.
- Las API Keys están expuestas en el código. **Usa gradle.properties y BuildConfig en producción**.
- Implementa manejo de errores más robusto antes de desplegar.

## 🤝 Contribuir

Las contribuciones son bienvenidas. Por favor:

1. Haz fork del proyecto
2. Crea una rama para tu feature (`git checkout -b feature/AmazingFeature`)
3. Commit tus cambios (`git commit -m 'Add some AmazingFeature'`)
4. Push a la rama (`git push origin feature/AmazingFeature`)
5. Abre un Pull Request

## 📄 Licencia

Este proyecto es parte de un trabajo académico para la Universidad de Extremadura.

## 👥 Autores

Proyecto desarrollado para la asignatura de Desarrollo de Aplicaciones para Dispositivos Móviles (DADM) por **Alfredo M. Okenve**.

---

**Nota**: Este proyecto está en desarrollo y es con fines educativos.
