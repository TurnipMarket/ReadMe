<!-- calidad:inicio -->
![Calidad](https://img.shields.io/badge/Calidad-sin%20c%C3%B3digo-lightgrey)

**Calidad de servicios:** sin código en `main` (no evaluable).
<!-- calidad:fin -->

# Especificación de Proyecto: TurnipMarket

**Archivo Fuente:** `PROYECTO.md`  
**Estado:** Documento Técnico / Refactorizado (v2)  

---

## 1. Descripción General

Plataforma web enfocada en permitir que usuarios oferentes publiquen sus datos de contacto y condiciones comerciales para productos o servicios.

> **Modelo Operativo y Crédito Ficticio:**  
> - La plataforma integra una **pasarela de pagos ficticia en entorno de pruebas / simulación**. El dinero cargado o manejado en las cuentas de usuario es **100% ficticio (dinero de prueba)**.  
> - La web **no procesa ni transacciona dinero real bajo ninguna circunstancia**.  
> - Para efectuar una transacción o contratación real, los compradores deben usar los **medios de contacto y preferencias de pago declarados** por el oferente (Mercado Pago, PayPal, transferencias, etc.), coordinando directamente fuera del sistema.

---

## 2. Límites del Sistema (Alcance)

### 2.1 Dentro del Alcance (In Scope)
* **Autenticación y Perfil:**
  * Registro e inicio de sesión.
  * Verificación de identidad multicanal: Email, SMS (OTP) y Discord OAuth2.
  * Muestra pública del nivel de verificación (Badge de confianza).
* **Simulación de Pasarela y Monedero Ficticio:**
  * Flujo de prueba que simula la adición de saldo ficticio al perfil del usuario.
  * Panel explicativo de saldo de demostración/prueba.
* **Gestión de Publicaciones (CRUD):**
  * Definición de título, descripción, categoría y precio de referencia.
  * Configuración de canales de contacto externos (WhatsApp, Telegram, Email, Discord, etc.).
  * Declaración de métodos de pago aceptados y preferencias/reglas de negociación externa (ej. *"Mercado Pago solo con 50% de seña"*).
  * Control del historial de publicaciones (activas, pausadas, eliminadas).
* **Búsqueda y Seguridad:**
  * Buscador con filtros por categoría, método de pago, ubicación y estado de verificación.
  * Sistema de reportes/denuncias para perfiles u ofertas sospechosas.
  * Panel de administración para moderación de contenido y usuarios.
  * Notificaciones por email ante eventos clave (verificaciones, reportes, moderación).

### 2.2 Fuera del Alcance (Out of Scope)
* Procesamiento transaccional de dinero real o integraciones bancarias con fondos verdaderos.
* Custodia de fondos reales (escrow), emisión de facturas fiscales o mediación de disputas económicas reales.
* Chat/mensajería interna en tiempo real.
* Logística, envíos o seguimiento de pedidos.
* App móvil nativa (cobertura exclusiva web responsive en esta fase).
* Sistemas avanzados de calificaciones por IA o soporte multi-idioma (planeados para fases posteriores).

---

## 3. Alcances Funcionales y No Funcionales

### 3.1 Requisitos Funcionales (RF)

| Código | Requisito | Descripción |
| :--- | :--- | :--- |
| **RF01** | Registro de Usuarios | Registro estándar mediante correo electrónico y contraseña. |
| **RF02** | Verificación Multicanal | Validación de identidad vía Email, SMS u OAuth2 con Discord. |
| **RF03** | Creación de Oferta | Publicación de bienes/servicios con condiciones de pago personalizadas. |
| **RF04** | Badge de Confianza | Despliegue público del nivel de verificación del oferente. |
| **RF05** | Navegación Pública | Exploración de la plataforma disponible para usuarios no autenticados. |
| **RF06** | Control de Publicación | Exigencia de al menos un canal verificado para publicar. |
| **RF07** | Gestión de Contenido | Edición, pausa o eliminación de publicaciones propias. |
| **RF08** | Sistema de Reportes | Módulo de denuncia de irregularidades accesible para usuarios logueados. |
| **RF09** | Panel de Moderación | Herramienta administrativa para gestión de suspensiones y reportes. |
| **RF10** | Pasarela Ficticia | Módulo interactivo que simula la recarga de saldo de prueba (dinero ficticio) en la cuenta. |

### 3.2 Requisitos No Funcionales (RNF)

| Código | Categoría | Especificación Técnica |
| :--- | :--- | :--- |
| **RNF01** | Seguridad | Contraseñas hasheadas (bcrypt/argon2), tokens JWT y rate-limiting en endpoints sensibles. |
| **RNF02** | Disponibilidad | Objetivo de uptime mensual ≥ 99.0%. |
| **RNF03** | Rendimiento | Tiempo de carga inicial < 2.5s (p95) en redes 4G. |
| **RNF04** | Escalabilidad | Arquitectura stateless orientada al crecimiento horizontal del backend. |
| **RNF05** | Usabilidad | Diseño adaptable (Responsive Web) acorde a criterios WCAG AA. |
| **RNF06** | Privacidad | Protección de datos de contacto visibles únicamente para usuarios registrados. |
| **RNF07** | Mantenibilidad | Código modular con pruebas automatizadas en flujos críticos (Auth y Publicaciones). |
| **RNF08** | Marco Legal | Inclusión explícita de aviso legal: los saldos son 100% ficticios/demo y no representan dinero real. |

---

## 4. Objetivos Específicos y Medibles

### 4.1 Entregables del MVP
1. **Módulo Auth:** Registro y verificación por al menos 1 canal activo (Email/SMS/Discord).
2. **Módulo Ofertas:** CRUD completo de publicaciones operativo al 100%.
3. **Módulo Pasarela Demo:** Sistema de carga y visualización de dinero ficticio para pruebas.
4. **Módulo Búsqueda:** Búsqueda avanzada con filtros por categoría, pago y verificación.
5. **Módulo Moderación:** Panel básico funcional para el equipo de administración.

### 4.2 Métricas de Calidad

| Métrica | Objetivo |
| :--- | :--- |
| **Tasa de verificación exitosa** | ≥ 95% de intentos válidos completados |
| **Tiempo de creación de publicación** | ≤ 3 minutos por formulario |
| **Tasa de publicaciones reportadas** | ≤ 2% del total de publicaciones |
| **Errores 5xx en producción** | ≤ 0.5% del total de solicitudes |
| **Cobertura de tests automatizados** | ≥ 70% en código crítico (Auth/Publicaciones) |

### 4.3 Criterios de Aceptación (CA)
* **CA01:** Si un usuario no verificado intenta publicar, el sistema bloquea la acción e indica qué canal de verificación le falta completar.
* **CA02:** La recepción y validación de OTP vía SMS se completa en menos de 60 segundos.
* **CA03:** Los métodos de pago reales y condiciones externas se almacenan y muestran de forma precisa cuando el comprador desea contactar al oferente.
* **CA04:** La pasarela web simula acreditar fondos ficticios correctamente a la billetera demo del usuario sin solicitar ni validar datos bancarios reales.
* **CA05:** Los visitantes no autenticados pueden explorar resultados pero los datos de contacto se mantienen ocultos hasta iniciar sesión.
* **CA06:** Los reportes de usuarios impactan en tiempo real en el panel de administración.

---

## 5. Stack Tecnológico Propuesto

| Capa | Tecnologías |
| :--- | :--- |
| **Frontend** | React + HTML/CSS (Evaluación de Next.js para SSR/SEO). |
| **Backend** | API REST / GraphQL basada en Python (FastAPI/Django) u homólogo según el equipo. |
| **Servicios Auth** | SendGrid/SES (Email), Twilio (SMS), Discord OAuth2. |
| **Base de Datos** | PostgreSQL (Relacional). |

---

## 6. Riesgos y Consideraciones Legales

* **Claridad en Entorno Demo:** Indicación visual permanente en la pasarela indicando que el saldo recargado es **dinero ficticio sin valor legal ni monetario**.
* **Deslinde de Responsabilidad:** Inclusión de Términos y Condiciones indicando el rol neutro de la plataforma respecto a los acuerdos económicos externos acordados entre usuarios.
* **Mitigación de Fraude:** Implementación de Badges de Verificación e incentivo al reporte comunitario.
* **Protección de Datos:** Cumplimiento con la normativa de protección de datos personales (ej. Ley 25.326 en Argentina).
