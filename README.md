# ⚡ Canales de Información del Sector Eléctrico (ENReGE)
> *Mapa interactivo y dinámico de flujos de datos, canales de comunicación y bases de información técnica/comercial.*

---

## 🗺️ Mapa General de Navegación

```
                     ┌── [ 🔌 Conexión Punto a Punto ] ──► ( Cortes WS | Reclamos WS | Obras Res 64 | SIDyAA | Facturación )
                     │
 [ CANALES ENReGE ] ─┼── [ 🌐 Plataformas Lotus / Web ] ─► ( Obras Transporte DAIT | Ambiental Generadoras )
                     │
                     ├── [ ☁️ APIs Externas ] ──────────► ( OpenWeather API | SAISTE CAMMESA )
                     │
                     └── [ 🗄️ Área Distribución Interna ] ► ( Res 2/98 VB5 | Tabla 21 | SINTYS | RESEF S.E. )
```

---

## 🔌 1. Conexión Punto a Punto (Distribuidoras: EDENOR / EDESUR)
*Intercambio directo y privado de datos críticos en tiempo real y diferido a través de la red dedicada.*

<details>
  <summary><b>🟢 Cortes de Baja, Media y Alta Tensión (Real-Time)</b></summary>

  ### 📊 Flujo y Monitoreo de Contingencias
  * **Mecanismo:** Consumo automatizado de un Web Service (WWSS) publicado por las distribuidoras en la Punto a Punto.
  * **Frecuencia de actualización:** Cada `5 minutos` (aproximadamente).
  * **Uso operativo:** Alimentación de tableros de control e históricos para el mapeo inmediato de interrupciones de suministro.

  | Variable Crítica | Detalle Técnico / Estado |
  | :--- | :--- |
  | **Métricas de BD** | 📦 *Tamaño actual:* `[Completar]` | 📈 *Tasa de crecimiento:* `[Completar]` |
  | **Documentación** | 📄 [Especificación Técnica de Interfaz WWSS](https://github.com/enrege/docs/cortes-ws "Especificación WWSS Cortes") |

</details>

<details>
  <summary><b>🔵 Reclamos Técnicos y Comerciales (Flujo Bidireccional)</b></summary>

  ### 🔄 Sincronización y Asignación Automática
  * **Mecanismo:** Consumo de múltiples Web Services parametrizados según la distribuidora y el tipo de servicio técnico/comercial.
  * **Flujo de Entrada (Novedades):** Captura en tiempo real de apertura de reclamos, novedades operativas, reiteraciones y cierres informados por las empresas.
  * **Flujo de Salida (Falta de Suministro):** El ENReGE numera automáticamente los reclamos de usuarios que ingresan por nuestros canales de atención al público y los inserta en los sistemas de las distribuidoras.
  * **Auditoría:** Servicio específico parametrizado para consultar a demanda si un reclamo sigue abierto o ya fue cerrado formalmente.

  | Variable Crítica | Detalle Técnico / Estado |
  | :--- | :--- |
  | **Métricas de BD** | 📦 *Tamaño actual:* `[Completar]` | 📈 *Tasa de crecimiento:* `[Completar]` |
  | **Documentación** | 📄 [Manual de Servicios Parametrizados de Reclamos](https://github.com/enrege/docs/reclamos-ws "Manual WWSS Reclamos") |

</details>

<details>
  <summary><b>🏗️ Obras Distribución (Resolución 64/2017)</b></summary>

  ### 🚦 Flujo Híbrido: Validación Automatizada y Procesamiento de Adjuntos
  * **Datos Declarados:** Información estructurada correspondiente a los Anexos 1, 2, 3 y 4 de la **Res. 64/2017**.
  * **Mecanismo Diario (WWSS):** Consumo automatizado diario en ventanas horarias diferenciadas para cada anexo.
  * **Motor de Reglas de Negocio:** La información atraviesa una serie de validaciones automáticas estrictas. Si se detecta un desvío o error, el lote se marca como **Rechazado** y el sistema dispara una notificación automática de inmediato hacia la distribuidora.
  * **Gestión de Adjuntos (FTP):** Las distribuidoras depositan en un servidor FTP dedicado archivos comprimidos con documentación de respaldo de las obras.
  * **Agente de Automatización:** Un agente interno desarrollado en Lotus Notes monitorea el FTP, descomprime los archivos de forma automática, los almacena en el file-server y asienta la ruta absoluta en la base de datos SQL.

  | Variable Crítica | Detalle Técnico / Estado |
  | :--- | :--- |
  | **Métricas de BD** | 📦 *Tamaño actual:* `[Completar]` | 📈 *Tasa de crecimiento:* `[Completar]` |
  | **Documentación** | 📄 [Reglas de Negocio y Estructura Res 64/17](https://github.com/enrege/docs/res64-2017 "Resolución 64/2017") |

</details>

<details>
  <summary><b>📊 Sistema de Información de Atención y Auditoría (SIDyAA)</b></summary>

  ### 🏬 Monitoreo de Canales y Sucursales Comerciales
  * **Mecanismo:** Consumo de Web Service en la Punto a Punto con una frecuencia crítica de `5 minutos`.
  * **Datos Capturados:** Variables operativas de los salones de atención al público de EDENOR y EDESUR.
  * **Indicadores Clave:** Cantidad de usuarios en espera, cantidad de boxes comerciales operativos, tiempos promedio de espera y tiempos de resolución de trámites presenciales.

  | Variable Crítica | Detalle Técnico / Estado |
  | :--- | :--- |
  | **Métricas de BD** | 📦 *Tamaño actual:* `[Completar]` | 📈 *Tasa de crecimiento:* `[Completar]` |
  | **Documentación** | 📄 [Diccionario de Datos y Modelo de Tablas SIDyAA](https://github.com/enrege/docs/sidyaa "Diccionario SIDyAA") |

</details>

<details>
  <summary><b>🩺 Reclamos de Usuarios Electrodependientes (Canal Inverso)</b></summary>

  ### 🛡️ Recepción Pasiva y Protección de Usuarios Vulnerables
  * **Mecanismo:** **Publicación de un Web Service** por parte del ENReGE. En este canal, el Ente actúa de manera pasiva y no realiza búsquedas activas de información; son las distribuidoras las encargadas de empujar los datos hacia nuestra infraestructura.
  * **Validación en Puerta:** El sistema recibe el reclamo, corre validaciones lógicas inmediatas y, en caso de no cumplir con las reglas de negocio establecidas, emite un mensaje de rechazo directo al servicio de la distribuidora.

  | Variable Crítica | Detalle Técnico / Estado |
  | :--- | :--- |
  | **Métricas de BD** | 📦 *Tamaño actual:* `[Completar]` | 📈 *Tasa de crecimiento:* `[Completar]` |
  | **Documentación** | 📄 [Protocolo Técnico de Recepción Electrodependientes](https://github.com/enrege/docs/electrodependientes-ws "Protocolo Electrodependientes") |

</details>

<details>
  <summary><b>🧾 Control Diario de Facturación (Réplica y Descompresión)</b></summary>

  ### 🗄️ Circuito de Auditoría de Lotes Diarios
  ```
  [Distribuidoras] ──(Adjuntan ZIP vía Web/Cliente)──► [Servidor NOTES_DIST (Punto a Punto)]
                                                                  │
                                                                  ▼ (Réplica Automatizada)
  [Fin del Proceso] ◄──(Asiento de Estado de Lote)─── [Servidor RUGOR (LAN Interna)]
  (Procesado / Rechazado en Lotus)                       (Descompresión y Procesamiento SQL)
  ```
  * **Operación:** Las distribuidoras cargan de forma diaria archivos comprimidos `.zip` con datos de facturación mediante formularios de Lotus (utilizando la interfaz web o clientes Lotus Notes dedicados).
  * **Tránsito de Datos:** Los archivos ingresan a nuestro servidor perimetral `NOTES_DIST` mediante la punto a punto y se replican automáticamente hacia el servidor `RUGOR`, ubicado de forma segura en la LAN interna del Ente.
  * **Cierre de Auditoría:** El Área de Distribución procesa los archivos internamente. Una vez finalizado el análisis, se asienta de manera definitiva el estado del lote (`Procesado` o `Rechazado`) en el documento original de Lotus.

  | Variable Crítica | Detalle Técnico / Estado |
  | :--- | :--- |
  | **Métricas de BD** | 📦 *Tamaño actual:* `[Completar]` | 📈 *Tasa de crecimiento:* `[Completar]` |
  | **Documentación** | 📄 [Procedimiento Interno de Control de Facturación](https://github.com/enrege/docs/control-facturacion "Manual Control Facturación") |

</details>

---

## 🌐 2. Plataformas Web y Formularios (Entorno Lotus Notes / Web)
*Sistemas basados en formularios avanzados orientados a la declaración jurada y fiscalización de Transportistas y Generadoras.*

<details>
  <summary><b>🏗️ Obras de Empresas Transportistas (Módulo DAIT)</b></summary>

  ### ⏱️ Control de Ventanas Temporales de Declaración
  * **Administración:** El Departamento de Análisis e Inspección Técnica (DAIT) gestiona de forma centralizada un menú de configuración dentro del ecosistema de Lotus Notes.
  * **Gobernanza:** A través de este menú se definen y habilitan de manera estricta los periodos cronológicos específicos en los cuales las empresas Transportistas quedan autorizadas para proporcionar información de sus obras.
  * **Carga de Datos:** Se realiza bajo un esquema híbrido; determinados usuarios autorizados ingresan directamente mediante el Cliente Lotus Notes de escritorio, mientras que otros utilizan formularios Web expuestos de manera segura.

  | Variable Crítica | Detalle Técnico / Estado |
  | :--- | :--- |
  | **Métricas de BD** | 📦 *Tamaño actual:* `[Completar]` | 📈 *Tasa de crecimiento:* `[Completar]` |
  | **Documentación** | 📄 [Guía de Configuración de Ventanas DAIT](https://github.com/enrege/docs/dait-obras "Guía DAIT") |

</details>

<details>
  <summary><b>🌱 Control Ambiental de Generadoras Eléctricas</b></summary>

  ### 🏭 Monitoreo y Procesamiento de Emisiones Gaseosas
  * **Mecanismo:** Las Generadoras de energía eléctrica acceden mediante entorno web o a través de un Cliente Lotus dedicado para completar formularios de declaración jurada ambiental.
  * **Insumos Técnicos:** Además del formulario, adjuntan archivos estructurados de datos con extensión `.DAT` que contienen los registros crudos de emisiones gaseosas de sus plantas.
  * **Pipelines de Datos:** El sistema descarga las declaraciones de forma automatizada, las procesa e impacta dentro de un servidor relacional SQL Server, generando reportes consolidados listos para ser auditados por los especialistas de la **Unidad de Seguridad Pública y Ambiente**.

  | Variable Crítica | Detalle Técnico / Estado |
  | :--- | :--- |
  | **Métricas de BD** | 📦 *Tamaño actual:* `[Completar]` | 📈 *Tasa de crecimiento:* `[Completar]` |
  | **Documentación** | 📄 [Especificación de Estructura de Archivos .DAT](https://github.com/enrege/docs/ambiental-dat "Especificación DAT Ambiental") |

</details>

---

## ☁️ 3. Integraciones de Datos Externos (APIs de Terceros)
*Consumo e ingesta automatizada de plataformas de terceros para enriquecimiento analítico y validación cruzada.*

<details>
  <summary><b>🌤️ Datos Meteorológicos e Históricos (OpenWeather API)</b></summary>

  ### 🌦️ Correlación Operativa Clima-Cortes
  * **Mecanismo:** Consumo automatizado vía REST API consumiendo el servicio de [OpenWeatherMap](https://openweathermap.org/api "OpenWeather API").
  * **Frecuencia:** Cada `5 minutos`.
  * **Finalidad Analítica:** Los datos climáticos actuales se persisten y se cruzan en reportes específicos con la base de datos histórica de cortes de suministro. Esto permite auditar el comportamiento de las redes ante contingencias meteorológicas severas (picos de temperatura, tormentas, etc.).

  | Variable Crítica | Detalle Técnico / Estado |
  | :--- | :--- |
  | **Métricas de BD** | 📦 *Tamaño actual:* `[Completar]` | 📈 *Tasa de crecimiento:* `[Completar]` |
  | **Documentación** | 📄 [Estrategia de Persistencia de Variables Climáticas](https://github.com/enrege/docs/weather-api "Estrategia API Clima") |

</details>

<details>
  <summary><b>⚙️ Sistema SAISTE (API CAMMESA & Resoluciones)</b></summary>

  ### 🛠️ Análisis de Indisponibilidades y Sanciones (En Desarrollo)
  * **Estado del Proyecto:** Actualmente en etapa activa de desarrollo de software.
  * **Objetivo:** Automatizar la recopilación de datos para el *Sistema para el Análisis de Indisponibilidades y Sanciones a las Transportistas Eléctricas* (SAISTE).
  * **Pipeline de Datos (API):** El sistema consume a demanda la API expuesta por CAMMESA, extrayendo información estructurada sobre equipamientos críticos e indisponibilidades registradas en el Transporte Eléctrico.
  * **Ingesta de Archivos:** Descarga de forma directa documentos en formato `.xlsx` (Excel), procesando sus filas de manera transaccional para guardarlas en tablas dedicadas de un servidor SQL Server.
  * **Procesamiento de Legado (PDF):** El sistema integra un motor de extracción para recuperar históricos de equipamientos y sus respectivos atributos analizando resoluciones normativas publicadas originalmente en formato PDF.

  | Variable Crítica | Detalle Técnico / Estado |
  | :--- | :--- |
  | **Métricas de BD** | 📦 *Fase de pruebas:* `[Estimado Desarrollo]` | 📈 *Tasa proyectada:* `[Estimado Desarrollo]` |
  | **Documentación** | 📄 [Documento de Arquitectura y Endpoints SAISTE-CAMMESA](https://github.com/enrege/docs/saiste-cammesa "Arquitectura SAISTE") |

</details>

---

## 🗄️ 4. Procesos Internos y Consolidación (Área Distribución)
*Bases históricas, regímenes consolidados y modelos de exportación institucional administrados internamente por los sectores técnicos.*

<details>
  <summary><b>📜 Régimen Informativo de Interrupciones (Resolución 2/98)</b></summary>

  ### 💾 Ingesta del Modelo de Datos Relacional Legado
  * **Periodicidad:** Envíos masivos estructurados con frecuencia trimestral y semestral.
  * **Alcance de Datos:** Universo completo de usuarios, altas, bajas, interrupciones generales de red, registros detallados de cortes en BT, MT y AT, así como el consolidado de reclamos.
  * **Disparador del Proceso:** El ingreso formal se inicia mediante una nota física o digital que ingresa formalmente por la Mesa de Entradas del Ente.
  * **Procesamiento de Software:** El equipo técnico realiza el mantenimiento y soporte de una aplicación legada desarrollada en **Visual Basic 5 (VB5)**. Los agentes del Área de Distribución configuran dentro de la app la ruta de un directorio local o de red desde el cual se importan los archivos planos provistos, ejecutando las rutinas de validación e inserción en la base de datos histórica. *(Nota: El canal de transporte exacto por el cual las distribuidoras entregan los archivos al área previo a la importación es externo al sistema).*

  | Variable Crítica | Detalle Técnico / Estado |
  | :--- | :--- |
  | **Métricas de BD** | 📦 *Tamaño actual:* `[Completar]` | 📈 *Tasa de crecimiento:* `[Completar]` |
  | **Documentación** | 📄 [Diccionario y Estructuras de Archivos Res 2/98](https://github.com/enrege/docs/res2-98 "Modelo Res 2/98") |

</details>

<details>
  <summary><b>👥 Universo Total de Usuarios (Tabla 21 Mensual)</b></summary>

  ### 🗄️ Padrón Consolidador de Clientes
  * **Periodicidad:** Actualización mensual estricta.
  * **Contenido:** Proporciona de forma detallada el universo total de usuarios residenciales, comerciales e industriales de las áreas de concesión de EDENOR y EDESUR.
  * **Gobernanza:** Su gestión, descarga y consistencia recae directamente bajo la responsabilidad exclusiva del Área de Distribución del Ente. *(Nota: Históricamente la información era recibida mediante un servidor FTP dedicado; actualmente se asume que continúa por dicha vía pero debe ser validado con el sector operativo).*

  | Variable Crítica | Detalle Técnico / Estado |
  | :--- | :--- |
  | **Métricas de BD** | 📦 *Tamaño actual:* `[Completar]` | 📈 *Tasa de crecimiento:* `[Completar]` |
  | **Documentación** | 📄 [Estructura de Registros e Índices de Tabla 21](https://github.com/enrege/docs/tabla21 "Estructura Tabla 21") |

</details>

<details>
  <summary><b>🏥 Cruzamiento de Padrón Social y Sanitario (SINTYS & Salud)</b></summary>

  ### 🤝 Identificación de Población Vulnerable y Subsidiada
  * **Gobernanza:** Operado de forma permanente por la **División de Tarifa Social** dependiente del Sector de Distribución del ENReGE.
  * **Misión Crítica:** Ejecución de procesos de mantenimiento, depuración y actualización sistemática de las bases de datos encargadas de identificar de forma inequívoca a los usuarios beneficiarios de la Tarifa Social y a aquellos registrados bajo la condición médica de **Electrodependientes**.
  * **Intercambio Dinámico:** Integración y cruzamiento periódico de datos con los registros del **SINTYS** (Sistema de Identificación Nacional Tributario y Social) y los padrones oficiales provistos por las carteras de Salud.

  | Variable Crítica | Detalle Técnico / Estado |
  | :--- | :--- |
  | **Métricas de BD** | 📦 *Tamaño actual:* `[Completar]` | 📈 *Tasa de crecimiento:* `[Completar]` |
  | **Documentación** | 📄 [Convenio y Protocolo de Intercambio de Datos SINTYS](https://github.com/enrege/docs/sintys-protocol "Protocolo SINTYS") |

</details>

<details>
  <summary><b>📊 Régimen Informativo RESEF (Salida hacia Secretaría de Energía)</b></summary>

  ### 📈 Consolidación Macroeconómica del Sector Eléctrico
  * **Destino:** Toda la información procesada y estructurada es empaquetada y remitida de forma oficial hacia la **Secretaría de Energía** de la Nación.
  * **Variables Reportadas:** Datos macro de Suministros, Usuarios Registrados y Curvas de Consumo Eléctrico.
  * **Estrategia de Compilación:**
    * **Usuarios y Suministros:** Se genera a partir de la explotación de la *Tabla 21 mensual* enriquecida con los padrones consolidados de *Tarifa Social y Electrodependientes*.
    * **Consumos:** Se ejecuta un pipeline y algoritmo de procesamiento de datos desarrollado a medida por el Sector de Distribución, encargado de procesar grandes volúmenes de registros históricos para generar las matrices consolidadas finales de demanda energética.

  | Variable Crítica | Detalle Técnico / Estado |
  | :--- | :--- |
  | **Métricas de BD** | 📦 *Tamaño actual:* `[Completar]` | 📈 *Tasa de crecimiento:* `[Completar]` |
  | **Documentación** | 📄 [Especificación de Formatos de Exportación RESEF](https://github.com/enrege/docs/resef-format "Formatos RESEF") |

</details>
