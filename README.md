# Mantenimiento de Flota

Programa web (un solo `index.html`, sin instalación) para el mantenimiento **preventivo y correctivo** de vehículos y equipos, con trazabilidad completa.

## Roles
| Usuario inicial | PIN | Rol | Puede |
|---|---|---|---|
| `jefe` | 1234 | Jefe de Taller | Todo: cargar/cerrar OT, móviles, novedades, estadísticas, trazabilidad, usuarios y parámetros |
| `mecanico` | 1234 | Mecánico | Cargar planillas de mano de obra y repuestos en las OT |
| `obra` | 1234 | Obra | Cargar checklists de los móviles de su obra |

Cambiá los PIN desde **Administración** apenas lo uses.

## Funciones
- Botón **＋ Cargar orden de trabajo** (preventiva / correctiva, con prioridad y mecánico asignado).
- **Checklist** de 16 controles cargado desde obra; cada falla ("Mal") genera una *novedad* y una alerta hasta que se repare.
- **Alertas**: service próximo/vencido (por km y por fecha), OT sin iniciar o sin cerrar, fallas sin OT correctiva, móviles sin checklist.
- Una OT no se puede cerrar sin planilla de mano de obra; al cerrar un preventivo se recalcula el próximo service y al cerrar una correctiva se cierran las novedades vinculadas.
- **Estadísticas por móvil**: preventivos vs correctivos, horas de mano de obra, repuestos, novedades abiertas.
- **Trazabilidad**: historia por móvil y registro de cada acción (quién, cuándo, qué).
- Exportación CSV y copia de seguridad JSON.

## Importante
Los datos se guardan en el navegador (`localStorage`) de cada dispositivo; no hay servidor. Para compartir datos entre equipos usá *Administración → Descargar / Importar copia*. El login es una barrera básica, no seguridad real.
Para uso multiusuario en tiempo real habría que sumar un backend (Firebase/Supabase).

## Publicar con GitHub Pages
Settings → Pages → Branch `main` / root. Quedará en `https://<usuario>.github.io/<repo>/`.
