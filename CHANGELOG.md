# Changelog - CHEMES Dashboard de Ventas

## [1.1.0] - 2026-02-15

### Nuevas funcionalidades

#### 1. Descarga de Ranking a Excel (.xlsx)
- Se agrego un boton **"Excel"** en la seccion "Top Articulos (SKU)" que permite descargar el ranking completo en formato `.xlsx`.
- La descarga incluye **todos los articulos** (no solo la pagina visible), con las columnas: `#`, `SKU`, `Descripcion`, `Unidades`, `Importe`.
- El archivo se genera con nombre automatico: `ranking_articulos_YYYY-MM-DD.xlsx`.
- Se incorporo la libreria **SheetJS (xlsx v0.20.3)** via CDN para la generacion del archivo Excel.

#### 2. Paginacion por secciones en el Ranking
- La tabla de ranking ahora muestra **100 articulos por pagina** (antes se mostraban todos en una sola lista).
- Se agregaron botones de navegacion (`<<` anterior / `>>` siguiente) para moverse entre secciones.
- Se muestra un indicador de pagina actual (ej: `1 / 5`).
- La paginacion se reinicia automaticamente al cambiar filtros (periodo, canal, criterio de orden).

---

### Archivos modificados

| Archivo      | Cambios |
|--------------|---------|
| `index.html` | +151 lineas, -16 lineas modificadas |

---

### Detalle tecnico de los cambios en `index.html`

#### HTML - Nuevos elementos
- **Linea 9**: Se agrego el tag `<script>` para cargar la libreria SheetJS desde CDN.
- **Lineas 692-702**: Se agregaron los controles de paginacion (botones `rankPrev`, `rankNext`, indicador `rankPageInfo`) y el boton de descarga Excel (`btnRankXlsx`) dentro del header de la tabla de ranking.

#### CSS - Nuevos estilos (lineas 457-521)
- `.rank-pagination`: Contenedor flex para los controles de paginacion.
- `.rank-pagination .page-info`: Indicador de pagina con fuente monoespaciada.
- `.rank-btn`: Estilos para botones de paginacion (28x28px, bordes redondeados, hover verde).
- `.rank-btn:disabled`: Estado deshabilitado con opacidad reducida.
- `.btn-download-xlsx`: Estilos para el boton de descarga Excel con icono SVG.

#### JavaScript - Nuevas variables de estado
- `rankPage` (linea 794): Indice de la seccion/pagina actual del ranking (base 0).
- `rankAllRows` (linea 795): Cache de todas las filas del ranking ya ordenadas.

#### JavaScript - Funciones modificadas
- **`updateRanking()`** (lineas 1299-1333): Reescrita para soportar paginacion por secciones de 100 items. Calcula total de paginas, actualiza indicadores, habilita/deshabilita botones de navegacion, y renderiza solo la seccion actual.
- **`applyFilters()`** (linea 1090): Se agrego `rankPage = 0` para reiniciar la paginacion al cambiar filtros.

#### JavaScript - Nuevas funciones
- **`downloadRankingXlsx()`** (lineas 1335-1371): Genera y descarga un archivo Excel con el ranking completo usando SheetJS. Incluye validaciones y anchos de columna configurados.

#### JavaScript - Nuevos event listeners (lineas 1426-1437)
- `rankSort.change`: Reinicia paginacion y actualiza ranking al cambiar criterio de orden.
- `rankPrev.click`: Navega a la seccion anterior.
- `rankNext.click`: Navega a la seccion siguiente.
- `btnRankXlsx.click`: Ejecuta la descarga Excel.

---

### Dependencias agregadas

| Dependencia | Version | Tipo | URL |
|-------------|---------|------|-----|
| SheetJS (xlsx) | 0.20.3 | CDN | `https://cdn.sheetjs.com/xlsx-0.20.3/package/dist/xlsx.full.min.js` |

---

### Branch de desarrollo

- **Branch**: `claude/ranking-excel-pagination-AJAIa`
- **Commit**: `8a1b785` - *feat: add Excel download and section-based pagination to ranking*
- **Base**: `master` (commit `f088242`)
