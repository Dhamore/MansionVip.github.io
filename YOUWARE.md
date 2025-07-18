# YOUWARE.md - Sistema Financiero Mansión Plast C.A

## Descripción del Proyecto
Sistema financiero completo desarrollado como Single Page Application (SPA) para la gestión de transacciones, proveedores, reportes financieros, entradas/salidas de efectivo y balance general. Diseñado específicamente para Mansión Plast C.A.

## Arquitectura del Sistema

### Estructura Principal
- **Aplicación SPA**: Implementada en vanilla JavaScript con HTML/CSS
- **Almacenamiento**: LocalStorage para persistencia de datos
- **UI Framework**: TailwindCSS para estilos y diseño responsivo
- **Patrones de Diseño**: Modular con separación de funciones por área

### Componentes Principales

#### 1. Sistema de Autenticación
- Login con usuarios predefinidos
- Roles: master, admin, usuario
- Sesión persistente con localStorage

#### 2. Gestión de Transacciones
- **Tipos**: Por Pagar, Por Cobrar, Entradas, Salidas
- **Estados**: Pendiente, Pagada, Anulada
- **Monedas**: USD y VES con conversión automática
- **Métodos de Pago**: Efectivo, Transferencia, Pago Móvil

#### 3. Sistema de Proveedores
- CRUD completo de proveedores
- Asociación con transacciones

#### 4. Sistema de Pagos
- **Abonos Parciales**: Permite pagos fraccionados
- **Pagos Completos**: Liquidación total de facturas
- **Conversión de Moneda**: Automática con tasa configurable
- **Historial**: Registro completo de todos los pagos

#### 5. Reportes Financieros
- Reportes por moneda (USD/VES)
- Balances generales
- Filtros por fecha y tipo
- Exportación a PDF

#### 6. Entradas y Salidas Directas
- Registro de movimientos de efectivo
- Tipos: Físico y Digital
- Métodos: Efectivo, Transferencia, Pago Móvil

## Funcionalidades Específicas Implementadas

### Tasa de Cambio Dinámica
- **Ubicación Global**: Header del sistema
- **Actualización Desde Pagos**: Botones de "Actualizar" en ventanas de abonar/pagar completo
- **Campos en Entradas/Salidas**: Campo de tasa editable en ambas ventanas
- **Propagación**: Actualización automática en todos los formularios del sistema

### Gestión de Balances
- **Cálculo Real**: Los reportes ahora usan datos reales de transacciones
- **Separación por Moneda**: USD y VES independientes
- **Conversión**: Totales combinados usando tasa actual
- **Estados**: Diferenciación entre pendiente, pagado y anulado

### Validación de Pagos
- **Efectivo VES**: Corrección del problema de validación
- **Métodos Habilitados**: Todos los métodos de pago funcionan correctamente
- **Referencias**: Obligatorias para transferencias y pagos móviles

## Estructura de Datos

### Transacciones
```javascript
{
  id: string,
  invoiceNumber: string,
  type: 'payable' | 'receivable' | 'income' | 'expense',
  providerId: string,
  amount: number,
  currency: 'USD' | 'VES',
  date: string,
  description: string,
  status: 'pending' | 'paid' | 'annulled',
  paymentType: string,
  createdAt: string,
  user: string
}
```

### Historial de Pagos
```javascript
{
  id: string,
  transactionId: string,
  invoiceNumber: string,
  amount: number,
  currency: 'USD' | 'VES',
  method: string,
  reference: string,
  date: string,
  exchangeRate: number,
  paymentType: 'partial_payment' | 'full_payment'
}
```

## Flujos de Trabajo

### Flujo de Pago Parcial (Abono)
1. Seleccionar factura pendiente
2. Configurar moneda y monto del abono
3. Elegir método de pago
4. Si es necesario, actualizar tasa de cambio
5. Confirmar con modal de resumen
6. Procesar y actualizar balance

### Flujo de Pago Completo
1. Seleccionar factura pendiente
2. El sistema calcula automáticamente el monto pendiente
3. Elegir moneda de pago (con conversión automática)
4. Confirmar método y procesar
5. Marcar factura como pagada

### Flujo de Entradas/Salidas
1. Especificar monto y moneda
2. Configurar tasa de cambio si es necesario
3. Seleccionar medio (físico/digital)
4. Si es digital, especificar tipo y banco
5. Procesar y actualizar balance general

## Usuarios del Sistema

### Master
- Acceso completo a todas las funciones
- Gestión de usuarios
- Configuración del sistema
- Migración de datos

### Admin
- Gestión de transacciones y proveedores
- Reportes financieros
- Procesamiento de pagos

### Usuario
- Consulta de transacciones
- Reportes básicos
- Sin capacidad de edición

## Consideraciones Técnicas

### Responsividad
- Diseño mobile-first
- Breakpoints optimizados para tablets y desktop
- Modales adaptativos

### Seguridad
- Validaciones client-side
- Sanitización de inputs
- Control de acceso por roles

### Performance
- Lazy loading de datos
- Filtros optimizados
- Animaciones suaves con CSS

### Mantenibilidad
- Código modular y comentado
- Funciones reutilizables
- Estructura clara de archivos

## Comandos de Desarrollo

### Estructura del Proyecto
```
/
├── index.html          # Archivo principal SPA
├── logo.png           # Logo de la empresa
└── YOUWARE.md         # Esta documentación
```

### Testing
- Pruebas manuales en navegador
- Validación de flujos críticos
- Testing de conversión de monedas

## Notas Importantes

### Datos de Prueba
El sistema incluye datos de ejemplo para testing. En producción se debe:
1. Limpiar localStorage
2. Configurar usuarios reales
3. Establecer tasa de cambio inicial

### Backup y Migración
- Funciones de exportación integradas
- Migración de datos entre versiones
- Respaldo automático en localStorage

### Restricciones del Código Base
- **No tocar funcionalidad existente** sin autorización
- **Respetar estructura modular** del código
- **Mantener compatibilidad** con datos existentes
- **Solo modificar** lo específicamente solicitado

## Últimas Modificaciones Realizadas

### Corrección de Problemas (2024)
1. **Botón de Pago en Efectivo VES**: Eliminación de validaciones que bloqueaban pagos en VES con efectivo
2. **Campo Tasa en Entradas/Salidas**: Agregado campo editable de tasa de cambio
3. **Actualización de Tasa desde Pagos**: Botones para actualizar tasa global desde ventanas de pago
4. **Reportes con Datos Reales**: Corrección de cálculos para mostrar montos correctos desde transacciones reales

### Funciones Agregadas
- `updateGlobalExchangeRateFromPayment()`
- `updateGlobalExchangeRateFromFullPayment()`
- `updateAllExchangeRateFields()`
- `calculateReportData()`
- `validateAndEnablePayment()`