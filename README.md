# FID by Lakaut — Iframe Flujo Alta

Prototipo HTML/CSS/JS del flujo completo de **Alta de Certificado Digital Jurídico** y **Firma de Documento** para el producto **FID by Lakaut**. Es un front-end estático (sin backend ni framework), pensado para validar UX y presentar los flujos a stakeholders y desarrolladores.

---

## Estructura del proyecto

```
Flujo-alta-iFrame/
├── assets/
│   ├── css/
│   │   ├── tokens.css          # Variables de diseño (colores, tipografía, spacing)
│   │   ├── base.css            # Reset y estilos globales
│   │   ├── header.css          # Header compartido
│   │   ├── buttons.css         # Sistema de botones (primary, outline, ghost, submit)
│   │   ├── inputs.css          # Campos de formulario
│   │   └── exit-modal.css      # Modal "¿Dejás el proceso?"
│   ├── icons/                  # SVGs e íconos del sistema
│   ├── images/                 # Ilustraciones y avatares
│   ├── js/
│   │   └── exit-modal.js       # Lógica compartida del modal de salida
│   ├── face-scan.json          # Animación Lottie — biometría en proceso
│   ├── card-skeleton.json      # Animación Lottie — generando certificado
│   ├── success-check.json      # Animación Lottie — validación / certificado exitoso
│   └── Verification.json       # Animación Lottie — firma exitosa
├── index.html                  # Selector de flujo (Alta o Firma)
├── inicio.html                 # Inicio del flujo Alta — selección de plan
├── otp-correo.html             # Validación de correo (OTP)
├── otp-tel.html                # Validación de teléfono (OTP)
├── paso2.html                  # Verificación de datos personales (RENAPER simulado)
├── editar-datos.html           # Corrección de datos personales
├── paso3.html                  # Carga de documentación
├── paso4.html                  # Confirmación previa a biometría
├── mail-continuacion-tramite.html # Pantalla de espera — mail de continuación enviado
├── validando-identidad.html    # Pantalla de espera — biometría en proceso (Lottie loop)
├── biometria.html              # Captura biométrica (foto)
├── validacion-exitosa.html     # Identidad validada correctamente (Lottie one-shot)
├── clave-firma.html            # Creación de clave de firma digital
├── generando-certificado.html  # Pantalla de espera — generando certificado (Lottie loop)
├── certificado-listo.html      # Certificado emitido — datos finales (Lottie one-shot)
├── pf-pago.html                # Pantalla de pago
├── firmar-otp.html             # OTP para iniciar firma de documento
├── firmar-documento.html       # Visualización del documento a firmar
├── confirmar-firma.html        # Confirmación con clave de firma
├── firmando-documento.html     # Pantalla de espera — firmando (Lottie loop)
├── firma-exitosa.html          # Firma completada (Lottie one-shot)
├── modal-tyc.html              # Modal de Términos y Condiciones
└── guia-botones.html           # Referencia interna del sistema de botones
```

---

## Flujos

### Flujo 1 — Alta de Certificado Digital

```
index.html (Selector de flujo)
    └─▶ inicio.html (Selección de plan)
            └─▶ otp-correo.html (Validar correo)
                    └─▶ otp-tel.html (Validar teléfono)
                            └─▶ paso2.html (Verificar datos RENAPER)
                                    ├─▶ editar-datos.html (Corregir datos) ──▶ paso2.html
                                    └─▶ paso3.html (Carga de documentación)
                                            └─▶ paso4.html (Confirmación)
                                                    └─▶ mail-continuacion-tramite.html
                                                    └─▶ validando-identidad.html (Lottie loop)
                                                            └─▶ biometria.html (Captura foto)
                                                                    └─▶ validacion-exitosa.html (Lottie)
                                                                            └─▶ clave-firma.html
                                                                                    └─▶ generando-certificado.html (Lottie loop)
                                                                                            └─▶ certificado-listo.html (Lottie)
                                                                                                    └─▶ pf-pago.html
```

### Flujo 2 — Firma de Documento

```
index.html (Selector de flujo)
    └─▶ firmar-otp.html (OTP de acceso)
            └─▶ firmar-documento.html (Ver documento)
                    └─▶ confirmar-firma.html (Ingresar clave de firma)
                            └─▶ firmando-documento.html (Lottie loop)
                                    └─▶ firma-exitosa.html (Lottie)
```

---

## Animaciones Lottie

El proyecto usa **lottie-web (bodymovin)** con renderer SVG. Las animaciones se cargan vía `lottie.loadAnimation()`.

| Pantalla | Archivo | Contenedor | Loop |
|---|---|---|---|
| `validando-identidad.html` | `assets/face-scan.json` | 200 × 200 px | ✅ Infinito |
| `generando-certificado.html` | `assets/card-skeleton.json` | 200 × 200 px | ✅ Infinito |
| `firmando-documento.html` | `assets/card-skeleton.json` | 200 × 200 px | ✅ Infinito |
| `validacion-exitosa.html` | `assets/success-check.json` | 160 × 160 px | ❌ Una vez |
| `firma-exitosa.html` | `assets/Verification.json` | 160 × 160 px | ❌ Una vez |
| `certificado-listo.html` | `assets/success-check.json` | 80 × 80 px | ❌ Una vez |

> Los tamaños del contenedor son iguales en desktop y mobile. Lo que varía entre breakpoints es el padding y layout del card que los contiene.

---

## Persistencia de datos (sessionStorage)

El prototipo usa `sessionStorage` para pasar datos entre pantallas sin backend.

| Clave | Se guarda en | Se lee en | Descripción |
|---|---|---|---|
| `flow` | `inicio.html` | Múltiples pantallas | Identifica el flujo activo: `'pf'` = Alta |
| `planKey` | `inicio.html` | `pf-pago.html` | Plan seleccionado por el usuario |
| `edit_nombre` | `paso2.html` | `editar-datos.html` | Nombre previo para edición |
| `edit_dni` | `paso2.html` | `editar-datos.html` | DNI previo para edición |
| `edit_genero` | `paso2.html` | `editar-datos.html` | Género previo para edición |
| `renaper_nombre` | `editar-datos.html` | `paso2.html` | Nombre actualizado tras corrección |
| `renaper_dni` | `editar-datos.html` | `paso2.html` | DNI actualizado tras corrección |
| `renaper_cuil` | `editar-datos.html` | `paso2.html` | CUIL actualizado tras corrección |
| `renaper_genero` | `editar-datos.html` | `paso2.html` | Género actualizado tras corrección |

---

## Cómo correr el proyecto

Es HTML estático — cualquier servidor local funciona.

```bash
# Con Node.js
npx serve .

# Con Python
python3 -m http.server 3000
```

Luego abrí `http://localhost:3000/index.html` en el navegador.

---

## Sistema de diseño

- **Tokens**: `assets/css/tokens.css` — colores, radios, sombras, tipografía.
- **Tipografía**: Inter (UI general), Open Sans (helpers y notas), Montserrat (destacados).
- **Botones**: ver `guia-botones.html` para referencia visual de todos los estados.
- **Modal de salida**: componente compartido (`exit-modal.js` + `exit-modal.css`) inyectado automáticamente. Muestra "¿Dejás el proceso?" con botón primario (continuar) y secundario (salir).

---

## Notas de desarrollo

- El prototipo **simula** la consulta a RENAPER con datos aleatorios según DNI y género ingresados. No hay llamadas reales a APIs externas.
- Los OTP de correo y teléfono son simulados: cualquier código de 6 dígitos avanza el flujo.
- La biometría es una pantalla de espera con temporizador, sin integración real.
- `guia-botones.html` es una pantalla interna de referencia, no forma parte del flujo de usuario.
- `modal-tyc.html` contiene los Términos y Condiciones embebidos como modal.
