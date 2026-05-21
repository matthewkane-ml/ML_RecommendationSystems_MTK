# FlikPik AI — Plataforma de Descubrimiento de Entretenimiento con IA

> Un motor de recomendación híbrido que ayuda a las personas a decidir qué ver — combinando filtrado colaborativo, búsqueda impulsada por IA, señales de tendencias e integración de tráilers en una sola app.

**Demo en vivo:** [flikpik-ai.onrender.com](https://flikpik-ai.onrender.com/)

---

## Problema

Elegir qué ver es más difícil de lo que debería ser — las recomendaciones están dispersas entre servicios de streaming y la mayoría de los sistemas recomendadores dependen de una sola señal. FlikPik AI reúne recomendaciones híbridas, búsqueda semántica, señales sociales y de tendencias, tráilers y disponibilidad en streaming en un solo lugar, para que el usuario pueda pasar de "no sé qué ver" a una elección segura en un par de clics.

## Mi Rol

Desarrollado como proyecto final de 4 personas para el programa de Data Science & Machine Learning de 4Geeks Academy. Actué como **Ingeniero de Datos y Líder de Preparación de Datos**, responsable de:

- Diseñar e implementar la metodología de división entrenamiento / validación / prueba basada en tiempo para prevenir la fuga de datos
- Construir la matriz dispersa de usuario-elemento 610 × 9.724 (98,3% de dispersión) sobre la que el equipo de modelado trabajó
- Limpieza de datos y análisis exploratorio sobre el dataset de valoraciones MovieLens
- Ingeniería de características (valoración promedio, conteo de valoraciones, puntuación de popularidad bayesiana, recencia)
- Validación de la integridad referencial en las cuatro tablas fuente
- Entrega de seis archivos de salida limpios y validados a los compañeros, y alineación en los esquemas de datos

## Conjunto de datos

- **Fuente:** [MovieLens 100K](https://grouplens.org/datasets/movielens/) — referencia estándar de investigación en ML mantenida por la Universidad de Minnesota
- **Tamaño:** 100.836 valoraciones en 9.742 películas y 610 usuarios
- **Enriquecimiento:** Integración con la API de TMDB para metadatos de películas en tiempo real, pósters y tráilers
- **Características clave:** IDs de usuario, IDs de película, valoraciones (0,5–5,0), marcas de tiempo, géneros codificados one-hot, más metadatos de TMDB

## Metodología

El pipeline va desde las valoraciones crudas hasta una app desplegada:

1. **Limpieza de datos y EDA** — gestión de valores nulos, verificación de cero duplicados, validación de integridad referencial en todas las tablas, conversión de timestamps Unix a datetime, y codificación one-hot de strings de géneros separados por barras verticales en 20 columnas binarias.
2. **Ingeniería de características** — construcción de valoración promedio, conteo de valoraciones, puntuación de popularidad bayesiana (equilibrando calidad y tamaño de muestra), y características de recencia por película. También se calcularon estadísticas de valoración a nivel de usuario.
3. **División entrenamiento/validación/prueba** — división 80/10/10 basada en tiempo ordenada por timestamp para que el modelo entrene con valoraciones históricas y evalúe en valoraciones futuras, previniendo la fuga de datos.
4. **Construcción de la matriz dispersa** — matriz de usuario-elemento 610 × 9.724 con 98,3% de dispersión, almacenada como matriz dispersa CSR para eficiencia de memoria.
5. **Modelado** — recomendador híbrido que combina filtrado colaborativo ítem-ítem (FC), factorización matricial via TruncatedSVD (50 factores latentes), filtrado de contenido por género y una referencia de popularidad. Ensemble ponderado: FC 30%, FM 45%, Género 15%, Popularidad 10%.
6. **Despliegue** — empaquetado con interfaz Streamlit y desplegado en vivo en Render.

## Resultados

La app desplegada devuelve recomendaciones personalizadas híbridas, resultados de búsqueda en lenguaje natural impulsados por IA, títulos en tendencia y tráilers para cualquier título del catálogo. El componente de factorización matricial (45% de peso en el ensemble) aportó la señal individual más fuerte, superando consistentemente a la referencia de popularidad en las valoraciones del conjunto de prueba. La app en vivo gestiona solicitudes de recomendación de extremo a extremo en tiempo real con el sistema de recomendación completo en línea.

## Funcionalidades

**Descubrimiento**
- Motor de recomendación híbrido (FC + FM + Género + Popularidad)
- Búsqueda semántica impulsada por IA ("Ask FlikPik")
- Buscador de películas similares
- Búsqueda por Actor / Director

**Social y Tendencias**
- Inteligencia de tendencias — qué es popular esta semana
- Señales de popularidad social

**Medios enriquecidos**
- Pósters y tráilers reales de películas via TMDB
- Disponibilidad en streaming (Netflix, Disney+, Hulu y más)

**Personalización**
- Perspectivas de gustos personalizados
- Sistema de valoración de usuario

## Tecnologías utilizadas

`Python` · `pandas` · `NumPy` · `scikit-learn` · `SciPy (matrices dispersas)` · `Streamlit` · `API de TMDB` · `Render` · `Git`

## Ejecución local

```bash
git clone https://github.com/matthewkane-ml/ML_RecommendationSystems_MTK.git
cd ML_RecommendationSystems_MTK
pip install -r requirements.txt
```

FlikPik AI necesita una clave API de TMDB para obtener pósters y metadatos. Consigue una clave gratuita en [themoviedb.org/settings/api](https://www.themoviedb.org/settings/api), luego crea un archivo `.env` en la raíz del proyecto:

```
TMDB_API_KEY=tu_clave_aqui
```

```bash
streamlit run app.py
```

## Capturas de pantalla

<!-- Añade capturas de pantalla a una carpeta /screenshots en el repositorio y actualiza estas rutas -->
![Página principal de FlikPik AI con títulos en tendencia](screenshots/home.png)
![Recomendaciones personalizadas](screenshots/recommendations.png)

## Próximos pasos

- **Escalar el dataset** — actualizar de MovieLens 100K al dataset completo MovieLens 25M para recomendaciones más ricas y personalizadas con dispersión significativamente reducida.
- **Pulir el frontend** — ajustar la UI, corregir problemas de renderizado (algunas tarjetas mostraban HTML sin formato en las pruebas) y mejorar la respuesta en móviles.
- **Añadir manejo de arranque en frío** — construir un flujo de incorporación donde los nuevos usuarios valoren un puñado de películas antes de recibir recomendaciones personalizadas, en lugar de recurrir a la referencia de popularidad.
- **Cachear respuestas de TMDB** — reducir llamadas a la API externa y mejorar los tiempos de carga cacheando localmente las respuestas de pósters y metadatos.

## Equipo

Proyecto final de 4 personas de 4Geeks Academy.

| Nombre | GitHub |
|--------|--------|
| Matthew Kane | [matthewkane-ml](https://github.com/matthewkane-ml) |
| Tomeka L. German | [tomekagerman](https://github.com/tomekagerman) |
| Vishal Desai | [vdd11](https://github.com/vdd11) |
| Brandon Henry | — |

---

**Autor:** Matthew Kane — [LinkedIn](https://www.linkedin.com/in/thomas-k-392094410/) · [GitHub](https://github.com/matthewkane-ml)
