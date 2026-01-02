# RAG  – CTI Automation Workflow

## Descripción general

Este repositorio contiene un **workflow de automatización de Threat Intelligence (CTI)** desarrollado en **n8n**, diseñado para analizar **Indicadores de Compromiso (IoCs)** y generar **informes automáticos de inteligencia** combinando múltiples fuentes CTI y un modelo de lenguaje (LLM).

El flujo recibe un indicador (IP, hash o URL), lo consulta en **VirusTotal** y **AlienVault OTX**, normaliza y fusiona los resultados, y finalmente genera un **informe redactado automáticamente** mediante un agente de IA.

<img width="1272" height="408" alt="image" src="https://github.com/user-attachments/assets/1f84408e-a2b4-49a8-8b1a-99f6b6019351" />

---

## ¿Para qué sirve?

Este workflow sirve para:

- Automatizar el análisis de **IPs, hashes y URLs**.
- Centralizar información de **múltiples fuentes de Threat Intelligence**.
- Reducir el tiempo de análisis manual en SOCs.
- Generar **informes claros, estructurados y accionables**.
- Servir como base para un sistema **RAG (Retrieval-Augmented Generation)** aplicado a ciberseguridad.

Casos de uso habituales:
- Security Operations Center (SOC)
- Threat Hunting
- Respuesta a incidentes
- Enriquecimiento automático de alertas
- Análisis rápido de IoCs recibidos por correo, tickets o chat

---

## Flujo de trabajo (alto nivel)

1. **Recepción del indicador**
   - El workflow se activa al recibir un mensaje por chat.
   - Detecta automáticamente si el input es:
     - Dirección IP
     - Hash (MD5, SHA1 o SHA256)
     - URL

2. **Normalización del IoC**
   - Se identifica el tipo de indicador.
   - Se construyen las rutas correctas para:
     - VirusTotal API
     - AlienVault OTX API
   - Para URLs, se genera el ID Base64 requerido por VirusTotal.

3. **Consulta a fuentes CTI**
   - **VirusTotal**
     - Detecciones por motor
     - Reputación y votos
     - Geolocalización, ASN y red
     - Timestamps (first seen, last seen, análisis)
     - Tags, categorías y threat severity
   - **AlienVault OTX**
     - Pulses relacionados
     - Malware y adversarios asociados
     - Industrias objetivo
     - Referencias públicas

4. **Merge de resultados**
   - Se combinan las respuestas de ambas fuentes.
   - No se aplican heurísticas ni scoring artificial.

5. **Post-procesado (Code Node)**
   - Se genera:
     - Un `summary` estructurado en JSON.
     - Un `ai_brief` compacto y optimizado para IA.

6. **Generación del informe**
   - Un **AI Agent** redacta automáticamente un informe de Threat Intelligence en lenguaje natural.

---

## Salida del workflow

El resultado final incluye:

- Tipo de indicador y valor analizado.
- Resumen estructurado de:
  - VirusTotal
  - AlienVault OTX
- Informe CTI redactado automáticamente, listo para:
  - Tickets de incidentes
  - Informes internos
  - Compartir con otros equipos

---

## Componentes principales

- **Chat Trigger** – Entrada del indicador.
- **Code Nodes (JavaScript)**  
  - Detección y normalización del IoC.  
  - Reducción, limpieza y unificación de datos CTI.
- **HTTP Request Nodes**
  - VirusTotal API
  - AlienVault OTX API
- **Merge Node** – Unión de resultados.
- **AI Agent + OpenAI Model** – Redacción automática del informe.

---

## Requisitos

- Instancia funcional de **n8n**.
- Credenciales API válidas para:
  - VirusTotal
  - AlienVault OTX
  - OpenAI
- Conocimientos básicos de:
  - Threat Intelligence
  - n8n
  - Indicadores de compromiso

---

## Posibles extensiones

- Persistencia en base de datos o SIEM.
- Cálculo de scoring de riesgo.
- Integración con Slack, Jira o ServiceNow.
- Enriquecimiento adicional (WHOIS, Shodan, AbuseIPDB).
- Uso como backend de un chatbot SOC o plataforma RAG completa.

---

## Licencia 

Revisar los términos de uso de VirusTotal, AlienVault OTX y OpenAI antes de un uso productivo.

---
