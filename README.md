# PROYECTO.md — Plataforma de Publicación de Contactos para Servicios/Productos
## 1. Descripción General

Plataforma web donde usuarios (oferentes) publican sus datos de contacto y condiciones de pago para servicios y/o productos que ofrecen. La plataforma no procesa pagos ni actúa como pasarela: solo intermedia el descubrimiento y contacto entre oferente y comprador. El pago y la negociación final ocurren fuera de la app, entre las partes, usando el medio que el oferente haya declarado (Mercado Pago, PayPal, billetera personalizada, etc.).

## 2. Límites del Sistema (Alcance)
2.1 Dentro del alcance (In Scope)
Registro y autenticación de usuarios.
Verificación de identidad vía uno o más de estos canales: email, SMS (teléfono), Discord OAuth.
Creación, edición y baja de "publicaciones" (oferta de servicio/producto).
Cada publicación incluye:
Título, descripción, categoría, precio de referencia (informativo, no transaccional).
Medios de contacto del oferente (WhatsApp, email, Discord, Telegram, etc.).
Métodos de pago aceptados, cada uno con condiciones libres definidas por el usuario (texto/reglas, ej: "Mercado Pago solo con seña del 50%", "PayPal solo compras >$50 USD").
Búsqueda y filtrado de publicaciones (por categoría, método de pago, ubicación, verificación del oferente).
Perfil público del usuario con su nivel de verificación visible (badge).
Sistema de reportes/denuncias de publicaciones o usuarios.
Panel de administración básico (moderación de publicaciones, gestión de reportes, baja de usuarios).
Historial simple de publicaciones propias (activas/pausadas/eliminadas).
Notificaciones básicas (email) ante eventos clave (verificación, reporte recibido, publicación aprobada/rechazada).
2.2 Fuera del alcance (Out of Scope)
Procesamiento de pagos dentro de la plataforma (no se integra Mercado Pago SDK, PayPal Checkout, ni ninguna pasarela transaccional real; solo se muestran como "método aceptado" declarado por el usuario).
Custodia de dinero, escrow, o mediación de disputas económicas.
Chat/mensajería en tiempo real dentro de la app (la comunicación ocurre por los medios de contacto externos publicados).
Facturación, emisión de comprobantes fiscales.
Logística de envíos o seguimiento de pedidos.
App móvil nativa (solo web responsive en esta fase).
Sistema de reputación/calificaciones avanzado con IA o detección automática de fraude (fase futura).
Multi-idioma (fase futura; MVP en español).
## 3. Alcances Funcionales y No Funcionales
3.1 Funcionales
Requisito
RF01	El usuario puede registrarse con email y contraseña.
RF02	El usuario puede verificar su cuenta por email (link/código), SMS (OTP) o vinculando su cuenta de Discord (OAuth2).
RF03	El usuario puede crear una publicación con datos de contacto y métodos de pago con condiciones.
RF04	El sistema muestra públicamente qué canal(es) de verificación tiene aprobados el usuario (badge de confianza).
RF05	Los visitantes pueden buscar/filtrar publicaciones sin necesidad de estar logueados (solo lectura).
RF06	Solo usuarios verificados en al menos un canal pueden publicar.
RF07	El usuario puede editar o eliminar sus propias publicaciones.
RF08	Cualquier usuario logueado puede reportar una publicación o perfil.
RF09	Un administrador puede revisar reportes y suspender publicaciones/usuarios.
RF10	El sistema nunca solicita ni almacena datos de tarjetas o credenciales de pasarelas de pago.
3.2 No Funcionales
###	Requisito
RNF01	Seguridad: contraseñas hasheadas (bcrypt/argon2), tokens JWT con expiración, rate limiting en login y envío de OTP.
RNF02	Disponibilidad: uptime objetivo ≥ 99% mensual.
RNF03	Rendimiento: tiempo de carga inicial < 2.5s (p95) en conexión 4G.
RNF04	Escalabilidad: arquitectura que soporte crecimiento horizontal del backend (stateless API).
RNF05	Usabilidad: diseño responsive, accesible (WCAG AA básico).
RNF06	Privacidad: datos de contacto visibles solo tras registro/login (evitar scraping masivo de bots).
RNF07	Mantenibilidad: código documentado, separación clara frontend/backend, tests automatizados en flujos críticos (auth, publicación).
RNF08	Cumplimiento legal: aviso legal claro de que la plataforma no es responsable de transacciones económicas entre usuarios (deslinde de responsabilidad).
## 4. Objetivos Específicos y Medibles
4.1 Funcionalidades (MVP)
Registro + verificación por al menos 1 de los 3 canales (email/SMS/Discord) — operativo antes del lanzamiento.
CRUD completo de publicaciones — funcional al 100% en MVP.
Buscador con al menos 3 filtros (categoría, método de pago, verificación) — funcional en MVP.
Panel de moderación básico — funcional en MVP.
4.2 Métricas de Calidad
Métrica	Objetivo
Tasa de verificación exitosa (OTP/email/Discord)	≥ 95% de intentos válidos completados sin error
Tiempo promedio de creación de publicación	≤ 3 minutos
Tasa de publicaciones reportadas sobre el total	≤ 2%
Errores 5xx en producción	≤ 0.5% de requests
Cobertura de tests en módulos de auth y publicaciones	≥ 70%
4.3 Criterios de Aceptación
CA01: Un usuario no verificado intenta publicar → el sistema bloquea la acción y muestra mensaje claro indicando qué verificación falta.
CA02: Un usuario completa verificación por SMS → recibe OTP en < 60 segundos y puede validarlo en un intento correcto.
CA03: Un usuario crea una publicación con 2 métodos de pago y condiciones distintas → ambos se guardan y se muestran correctamente en el detalle público.
CA04: Un visitante no logueado busca por categoría → obtiene resultados sin ver datos de contacto directos (debe loguearse para verlos).
CA05: Un usuario reporta una publicación → el reporte aparece en el panel de administración en tiempo real (o próxima carga, según arquitectura elegida).
CA06: Ningún endpoint ni componente del sistema procesa, solicita o almacena números de tarjeta, CVV, o credenciales de billeteras — verificado por auditoría de código antes de cada release.
## 5. Stack Tecnológico Propuesto
Frontend: React + HTML/CSS (posible uso de un framework como Next.js si se requiere SSR/SEO para las publicaciones públicas).
Backend: a definir por el equipo responsable; compatible con Python (ej. FastAPI/Django) u otra alternativa, expuesto vía API REST o GraphQL consumida por el frontend.
Autenticación/Verificación: proveedor de email transaccional (ej. SendGrid/SES), proveedor SMS (ej. Twilio), Discord OAuth2.
Base de datos: relacional (PostgreSQL sugerido) por la naturaleza estructurada de usuarios/publicaciones/reportes.

Nota: el stack de backend queda sujeto a confirmación del equipo/infraestructura que lo administrará, fuera del control directo de este documento.

##6. Riesgos y Consideraciones Legales
Al no mediar en pagos, la plataforma debe dejar explícito en Términos y Condiciones que no garantiza la buena fe de las transacciones entre usuarios.
Riesgo de uso de la plataforma para estafas (falsos oferentes) → mitigado con verificación obligatoria + sistema de reportes.
Posible requerimiento de cumplimiento con normativa de protección de datos personales (ej. Ley 25.326 en Argentina) por almacenar teléfonos/emails.
