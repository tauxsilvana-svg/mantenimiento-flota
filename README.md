# Mantenimiento de Flota

Programa web (un solo `index.html`, sin instalación) para el mantenimiento **preventivo y correctivo** de vehículos y equipos, con trazabilidad completa.

## Roles
| Usuario inicial | PIN | Rol | Puede |
|---|---|---|---|
| `jefe` | 1234 | Jefe de Taller | Todo: cargar/cerrar OT, móviles, novedades, estadísticas, trazabilidad, usuarios y parámetros |
| `mecanico` | 1234 | Mecánico | Cargar planillas de mano de obra y repuestos en las OT |
| `administrativo` | 1234 | Administrativo | Ver alertas, órdenes, móviles y estadísticas (solo lectura) |
| `obra` | 1234 | Obra | Cargar checklists de los móviles de su obra |

Cambiá los PIN desde **Administración** apenas lo uses.

## Plan de mantenimiento preventivo
Por defecto (editable en Administración): 5.000 km cambio de aceite y revisión general · 10.000 km frenos y neumáticos · 20.000 km batería y filtros · 50.000 km sistema de refrigeración · 100.000 km correa de distribución. Cada trabajo se controla por separado en cada móvil; el aviso sale 30 días antes (estimado con el km diario del móvil) y se muestra a jefe, administrativo y obra.

## Funciones
- Botón **＋ Cargar orden de trabajo** (preventiva / correctiva, con prioridad y mecánico asignado).
- **Checklist** de 16 controles cargado desde obra; cada falla ("Mal") genera una *novedad* y una alerta hasta que se repare.
- **Alertas**: service próximo/vencido (por km y por fecha), OT sin iniciar o sin cerrar, fallas sin OT correctiva, móviles sin checklist.
- Una OT no se puede cerrar sin planilla de mano de obra; al cerrar un preventivo se recalcula el próximo service y al cerrar una correctiva se cierran las novedades vinculadas.
- **Estadísticas por móvil**: preventivos vs correctivos, horas de mano de obra, repuestos, novedades abiertas.
- **Trazabilidad**: historia por móvil y registro de cada acción (quién, cuándo, qué).
- Exportación CSV y copia de seguridad JSON.

## Modo local y modo nube
- **Local** (por defecto): los datos quedan en el navegador de cada dispositivo. Usuarios iniciales jefe / mecanico / obra, PIN 1234.
- **Nube** (Firebase, gratis): todos los usuarios comparten los mismos datos en tiempo real. Se activa pegando la configuración del proyecto en `firebase-config.js`. En este modo el acceso usa Firebase Authentication (usuario + clave de 6 o más caracteres) y la base es Firestore.

### Activar la nube
1. console.firebase.google.com → Agregar proyecto (sin Analytics).
2. Compilación → Authentication → Comenzar → Correo electrónico/contraseña → Habilitar.
3. Authentication → Usuarios → Agregar usuario: `jefe@flota.app` con una clave de 6+ caracteres (el primer ingreso lo convierte en Jefe de Taller).
4. Compilación → Firestore Database → Crear base de datos (modo producción) → pestaña Reglas:
   ```
   rules_version = "2";
   service cloud.firestore { match /databases/{db}/documents { match /{document=**} { allow read, write: if request.auth != null; } } }
   ```
5. Configuración del proyecto → Tus apps → Web (</>) → copiar el objeto `firebaseConfig` y pegarlo en `firebase-config.js` como `export default { ... };`.
6. Authentication → Configuración → Dominios autorizados: agregar `TU_USUARIO.github.io`.

El login es entre usuarios de la empresa; los roles se aplican en la aplicación. La clave de otro usuario solo la cambia él mismo o el administrador desde la consola de Firebase.

## Publicar con GitHub Pages
Settings → Pages → Branch `main` / root. Quedará en `https://<usuario>.github.io/<repo>/`.
