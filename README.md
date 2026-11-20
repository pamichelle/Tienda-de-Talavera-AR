# Talavera Dolores · tienda móvil con TDD

Catálogo · Carrito · Pagos (simulados) · Perfil de usuario · "Ver en tu espacio" con realidad aumentada.
Manual TDD · Desarrollo Móvil Integral · UTNG.

> ⚠️ La pasarela de pagos y el servidor de cuentas son **SIMULADOS**. Nunca se procesa dinero real.

## Estructura

```
lib/
  dominio/        reglas del negocio (Dart puro, sin Flutter)
    dinero producto carrito validadores direccion tarjeta
    usuario orden excepciones contratos servicio_checkout bloqueo_intentos
  datos/          catalogo_memoria almacenes pasarela_simulada auth_simulada
  presentacion/
    vm/           catalogo_vm carrito_vm sesion_vm checkout_vm
    pantallas/    catalogo, detalle, ver_en_tu_espacio, carrito, checkout, perfil
  dependencias.dart   raíz de composición
  main.dart
test/             dominio_test, servicios_test, viewmodels_test
```

## 1. Preparar (primera vez)

Abre en VS Code la carpeta `tienda_talavera` (la que tiene `pubspec.yaml`):

```bash
flutter create . --project-name tienda_talavera
del test\widget_test.dart
flutter pub get
flutter test
```

Si `pub get` se queja de alguna versión:
```bash
flutter pub add model_viewer_plus shared_preferences flutter_secure_storage crypto
```

## 2. Android

En `android/app/src/main/AndroidManifest.xml`, antes de `<application`:
```xml
<uses-permission android:name="android.permission.INTERNET"/>
```
Si al compilar sale un error de `minSdk`, en `android/app/build.gradle` (o `.kts`) pon `minSdk = 23` dentro de `defaultConfig`.

El botón de AR solo abre la cámara en un **teléfono físico** compatible con ARCore. En el emulador sí se ve el visor 3D.

## 3. Pruebas

| Archivo | Rondas | Qué prueba |
|---|---|---|
| `test/dominio_test.dart` | 2–5 | Dinero, producto, carrito, cupones, validadores, tarjeta (Luhn) |
| `test/servicios_test.dart` | 6–8 | Pasarela e idempotencia, checkout, bloqueo de intentos, cuentas |
| `test/viewmodels_test.dart` | 9 | ViewModels de catálogo, carrito, sesión y checkout |

## 4. Tarjetas de prueba

| Número | Resultado |
|---|---|
| 4242 4242 4242 4242 | Aprobada |
| 4000 0000 0000 0002 | Rechazada por el banco |
| 4000 0000 0000 9995 | Fondos insuficientes |

Cualquier fecha futura (por ejemplo 12/30) y CVV `123`.
Cupones: `DOLORES10` y `TALAVERA15`.
