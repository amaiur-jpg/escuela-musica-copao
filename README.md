# Escuela Musical Clave de Copao — Sistema de Fichas y Control de Cupos

Sistema CRM y panel de gestión de secretaría para la **Escuela Musical Clave de Copao** (La Serena, Chile). Permite gestionar las fichas de alumnos con alta, edición y borrado, control automático de cupos por instrumento con visualización de pentagrama, cálculo en tiempo real de ingresos mensuales estimados y persistencia local completa.

---

## 🎹 Resumen del Proyecto

* **Cliente:** Escuela Musical Clave de Copao
* **Ubicación:** La Serena, Chile
* **Moneda:** Pesos Chilenos (CLP `$`)
* **Stack:** React 18, Tailwind CSS, Lucide Icons, `localStorage`
* **Identidad Visual:**
  * Fondo crema pergamino: `#FBF5EE`
  * Azul tinta: `#1F3A5F`
  * Mostaza cálido: `#E2A93B`
  * Carbón oscuro: `#27302F`
  * Rojo terracota / alerta: `#B5453C`
  * Tipografías: **Fraunces** (títulos, números y acentos musicales) y **Karla** (cuerpo de texto y tablas).

---

## 🔑 Acceso al Panel de Secretaría

El acceso al panel se realiza mediante una clave compartida definida en código, sin verificación de identidad externa ni cuentas individuales:

* **Clave de acceso predefinida:** `copao2026`
* **Cómo cambiar la clave:**
  En el archivo `index.html`, localiza la constante en la cabecera del script:
  ```javascript
  const CLAVE_PANEL = "copao2026"; // Cambia esta cadena por la nueva clave deseada
  ```

---

## 🎼 Reglas de Negocio y Control de Cupos

1. **Cálculo de cupo disponible:**
   $$\text{Cupos Libres} = \text{Cupo Total del Instrumento} - (\text{Alumnos Activos} + \text{Alumnos Congelados})$$
2. **Estados del alumno:**
   * **Activo:** Ocupa plaza y suma su mensualidad al ingreso mensual estimado de la escuela.
   * **Congelado:** Conserva su plaza reservada en el instrumento pero **aporta $ 0** a la facturación mensual mientras dure la pausa.
   * **Retirado:** Libera la vacante de inmediato en el momento en que se cambia su estado o se borra su ficha.
3. **Bloqueo por falta de cupo:**
   Cuando un instrumento llega a 0 lugares libres, la barra se tiñe de color de alerta (`#B5453C`) con la etiqueta **«Sin cupo»**. El formulario rechaza inmediatamente dar de alta o reactivar a un alumno como Activo mostrando el mensaje oficial: *«No queda cupo en este instrumento»*.
4. **Semáforo de cupos:**
   * $\le 3$ lugares: Alerta ámbar **«Últimos lugares»**.
   * $0$ lugares: Alerta roja **«Sin cupo»**.
5. **Regla de menor de edad (18 años):**
   A partir de la fecha de nacimiento ingresada, el sistema calcula la edad exacta. Si el alumno tiene **menos de 18 años**, se despliega de forma obligatoria el campo **«Persona adulta responsable»** para registrar a la madre, padre o tutor. Si tiene 18 años o más, el campo permanece oculto.
6. **Horarios de 45 minutos:**
   * Lunes a Viernes (15:00 a 21:00 hs): `15:00`, `15:45`, `16:30`, `17:15`, `18:00`, `18:45`, `19:30`, `20:15`.
   * Sábados (09:30 a 13:30 hs): `09:30`, `10:15`, `11:00`, `11:45`, `12:30`.
7. **Control de duplicados:**
   Los correos se comparan tras eliminar espacios (*trim*) y convertir a minúsculas, impidiendo fichas duplicadas con el mismo correo del dominio `@example.com`.

---

## 📦 Semilla de Datos Sintéticos

La aplicación cuenta con una semilla de **30 fichas precargadas** con datos sintéticos y realistas que cubren casos extremos:
* **Batería:** 8 de 8 alumnos (100% llena, cupo cero).
* **Violín:** 8 de 10 alumnos (últimos 2 lugares).
* **Congelados:** Al menos 3 alumnos en pausa reservando plaza.
* **Retirados:** Al menos 2 alumnos retirados con plazas liberadas.
* **Rango de edades:** Desde los 7 hasta los 54 años.

### Política estricta de `localStorage`:
* La semilla se inyecta **únicamente si no existe la clave propia en el navegador** (`clave_copao_alumnos_v1`).
* Una lista vacía guardada a propósito **no equivale a una primera visita** (si la secretaría borra a todos los alumnos, la pantalla permanece vacía tras recargar la página).
* Se incluye un botón de **«Restablecer datos de ejemplo»** con confirmación modal para reiniciar la demo a su estado inicial en cualquier momento.

---

## 🚀 Cómo ejecutar la aplicación

1. Haz doble clic en el archivo `index.html` para abrirlo en cualquier navegador moderno (Google Chrome, Microsoft Edge, Safari o Mozilla Firefox).
2. No requiere Node.js, compiladores ni configuración de servidores externos.
3. Totalmente adaptable y optimizado para pantallas móviles desde **360 px de ancho** (*mobile-first*). Incluye explícitamente `<meta name="viewport" content="width=device-width, initial-scale=1" />` en el encabezado HTML para garantizar proporciones y adaptabilidad visual impecable en cualquier celular.

---

## 📋 Lista de Comprobación de Criterios de Aceptación (Kodarvia)

- [x] **1. Crear, editar y borrar un alumno** actualiza el listado y las barras de cupo sin recargar la página.
- [x] **2. Adulto responsable** se solicita exclusivamente cuando el alumno es menor de 18 años.
- [x] **3. Bloqueo con cupo cero:** Rechaza guardar un alumno nuevo como «Activo» con el mensaje *«No queda cupo en este instrumento»*.
- [x] **4. Liberación de cupo:** Pasar a «Retirado» o borrar una ficha libera el lugar en el momento.
- [x] **5. Alumno Congelado:** Ocupa vacante pero aporta $ 0 al ingreso mensual estimado.
- [x] **6. Reactivación:** No permite reactivar a un retirado si su instrumento está lleno.
- [x] **7. Ingreso mensual:** Suma únicamente las mensualidades de los alumnos en estado Activo.
- [x] **8. Buscador y filtros combinables:** Búsqueda por texto + filtro de instrumento + filtro de estado funcionan de forma simultánea.
- [x] **9. Persistencia y diseño responsive:** Datos persisten en `localStorage` y la interfaz se maneja con soltura en 360 px.
- [x] **10. Documentación completa:** Clave documentada, explicación de datos sintéticos y entrega sin dependencias externas.
