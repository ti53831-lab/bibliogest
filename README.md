<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>BiblioGest · Sistema de Administración de Biblioteca</title>
  <!-- Tailwind CSS para diseño responsivo y moderno -->
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Fuente Inter y Merriweather para estilo editorial limpio -->
  <link rel="preconnect" href="https://fonts.googleapis.com" />
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=Merriweather:wght@700&display=swap" rel="stylesheet" />
  <script>
    tailwind.config = {
      theme: {
        extend: {
          fontFamily: {
            sans: ['Inter', 'sans-serif'],
            serif: ['Merriweather', 'serif'],
          },
          colors: {
            editorial: {
              bg: '#F8F7F4',
              card: '#FFFFFF',
              ink: '#1E293B',
              muted: '#64748B',
              accent: '#1E3A8A',
              accentHover: '#1D4ED8',
              amber: '#B45309',
            }
          }
        }
      }
    };
  </script>
</head>
<body class="bg-editorial-bg text-editorial-ink font-sans antialiased min-h-screen flex flex-col">

  <!-- Barra superior editorial con acciones de archivos JSON -->
  <header class="bg-white border-b border-slate-200 sticky top-0 z-30 shadow-sm">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4 flex flex-col md:flex-row md:items-center md:justify-between gap-4">
      <!-- Identidad del sistema -->
      <div class="flex items-center gap-3">
        <div class="w-10 h-10 rounded-xl bg-editorial-accent text-white flex items-center justify-center shadow-sm shrink-0">
          <!-- Icono SVG de libro abierto -->
          <svg xmlns="http://www.w3.org/2000/svg" class="w-6 h-6" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
            <path stroke-linecap="round" stroke-linejoin="round" d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253" />
          </svg>
        </div>
        <div>
          <h1 class="font-serif text-xl sm:text-2xl font-bold text-slate-900 tracking-tight">BiblioGest</h1>
          <p class="text-xs sm:text-sm text-editorial-muted">Panel Editorial de Control de Catálogo y Préstamos</p>
        </div>
      </div>

      <!-- Controles de manejo de archivos (Guardar JSON / Cargar JSON / Datos de ejemplo) -->
      <div class="flex flex-wrap items-center gap-2 sm:gap-3">
        <button
          id="btnCargarDemo"
          type="button"
          class="px-3.5 py-2 rounded-xl text-xs sm:text-sm font-medium bg-amber-50 text-amber-800 border border-amber-200 hover:bg-amber-100 transition flex items-center gap-1.5"
          title="Cargar libros de muestra para pruebas"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="w-4 h-4" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
            <path stroke-linecap="round" stroke-linejoin="round" d="M19.428 15.428a2 2 0 00-1.022-.547l-2.387-.477a6 6 0 00-3.86.517l-.318.158a6 6 0 01-3.86.517L6.05 15.21a2 2 0 00-1.806.547M8 4h8l-1 1v5.172a2 2 0 00.586 1.414l5 5c1.26 1.26.367 3.414-1.415 3.414H4.828c-1.782 0-2.674-2.154-1.414-3.414l5-5A2 2 0 009 10.172V5L8 4z" />
          </svg>
          <span>Datos de ejemplo</span>
        </button>

        <button
          id="btnExportarJSON"
          type="button"
          class="px-3.5 py-2 rounded-xl text-xs sm:text-sm font-medium bg-slate-900 text-white hover:bg-slate-800 transition shadow-sm flex items-center gap-1.5"
          title="Descargar respaldo completo en archivo .json"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="w-4 h-4" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
            <path stroke-linecap="round" stroke-linejoin="round" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-4l-4 4m0 0l-4-4m4 4V4" />
          </svg>
          <span>Guardar archivo JSON</span>
        </button>

        <label
          for="inputImportarJSON"
          class="cursor-pointer px-3.5 py-2 rounded-xl text-xs sm:text-sm font-medium bg-white text-slate-700 border border-slate-300 hover:bg-slate-50 transition flex items-center gap-1.5"
          title="Cargar catálogo desde un archivo .json"
        >
          <svg xmlns="http://www.w3.org/2000/svg" class="w-4 h-4" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
            <path stroke-linecap="round" stroke-linejoin="round" d="M4 16v1a3 3 0 003 3h10a3 3 0 003-3v-1m-4-8l-4-4m0 0L8 8m4-4v12" />
          </svg>
          <span>Cargar JSON</span>
          <input id="inputImportarJSON" type="file" accept=".json,application/json" class="hidden" />
        </label>
      </div>
    </div>
  </header>

  <!-- Contenedor principal -->
  <main class="max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-6 sm:py-8 flex-1 space-y-6">

    <!-- Zona de notificaciones dinámicas (reemplaza alert/confirm) -->
    <div id="zonaNotificacion" class="hidden rounded-2xl p-4 border text-sm font-medium flex items-center justify-between gap-3 transition-all" role="alert">
      <div class="flex items-center gap-2.5">
        <span id="iconoNotificacion" class="shrink-0"></span>
        <span id="textoNotificacion"></span>
      </div>
      <button id="btnCerrarNotificacion" type="button" class="text-xs underline opacity-75 hover:opacity-100 p-1">Cerrar</button>
    </div>

    <!-- Tarjetas de resumen estadístico -->
    <section aria-label="Resumen de inventario" class="grid grid-cols-1 sm:grid-cols-3 gap-4">
      <div class="bg-white rounded-2xl p-5 border border-slate-200/80 shadow-sm flex items-center justify-between">
        <div>
          <p class="text-xs font-semibold uppercase tracking-wider text-slate-500">Total en Catálogo</p>
          <p id="statTotal" class="text-3xl font-bold text-slate-900 mt-1">0</p>
        </div>
        <div class="w-12 h-12 rounded-2xl bg-slate-100 text-slate-700 flex items-center justify-center">
          <svg xmlns="http://www.w3.org/2000/svg" class="w-6 h-6" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
            <path stroke-linecap="round" stroke-linejoin="round" d="M19 11H5m14 0a2 2 0 012 2v6a2 2 0 01-2 2H5a2 2 0 01-2-2v-6a2 2 0 012-2m14 0V9a2 2 0 00-2-2M5 11V9a2 2 0 012-2m0 0V5a2 2 0 012-2h6a2 2 0 012 2v2M7 7h10" />
          </svg>
        </div>
      </div>

      <div class="bg-white rounded-2xl p-5 border border-emerald-200/80 shadow-sm flex items-center justify-between">
        <div>
          <p class="text-xs font-semibold uppercase tracking-wider text-emerald-700">Disponibles</p>
          <p id="statDisponibles" class="text-3xl font-bold text-emerald-700 mt-1">0</p>
        </div>
        <div class="w-12 h-12 rounded-2xl bg-emerald-50 text-emerald-600 flex items-center justify-center">
          <svg xmlns="http://www.w3.org/2000/svg" class="w-6 h-6" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
            <path stroke-linecap="round" stroke-linejoin="round" d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z" />
          </svg>
        </div>
      </div>

      <div class="bg-white rounded-2xl p-5 border border-amber-200/80 shadow-sm flex items-center justify-between">
        <div>
          <p class="text-xs font-semibold uppercase tracking-wider text-amber-700">Prestados</p>
          <p id="statPrestados" class="text-3xl font-bold text-amber-700 mt-1">0</p>
        </div>
        <div class="w-12 h-12 rounded-2xl bg-amber-50 text-amber-600 flex items-center justify-center">
          <svg xmlns="http://www.w3.org/2000/svg" class="w-6 h-6" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
            <path stroke-linecap="round" stroke-linejoin="round" d="M12 8v4l3 3m6-3a9 9 0 11-18 0 9 9 0 0118 0z" />
          </svg>
        </div>
      </div>
    </section>

    <!-- Grid principal: Formularios laterales (izquierda) y Catálogo interactivo (derecha) -->
    <div class="grid grid-cols-1 lg:grid-cols-12 gap-6 items-start">

      <!-- Columna izquierda: Registro de Libros y Control de Préstamos/Devoluciones -->
      <div class="lg:col-span-4 space-y-6">

        <!-- 1. Formulario: Registrar Nuevo Libro -->
        <section class="bg-white rounded-2xl p-6 border border-slate-200/90 shadow-sm">
          <div class="flex items-center gap-2 mb-4 pb-3 border-b border-slate-100">
            <span class="w-2.5 h-2.5 rounded-full bg-editorial-accent"></span>
            <h2 class="font-serif text-lg font-bold text-slate-900">Registrar Nuevo Libro</h2>
          </div>

          <form id="formRegistrarLibro" class="space-y-4" novalidate>
            <div>
              <label for="libroTitulo" class="block text-xs font-semibold text-slate-700 uppercase tracking-wide mb-1">
                Título del libro <span class="text-rose-600">*</span>
              </label>
              <input
                id="libroTitulo"
                type="text"
                required
                placeholder="Ej. Cien años de soledad"
                class="w-full px-3.5 py-2.5 rounded-xl border border-slate-300 text-sm focus:outline-none focus:ring-2 focus:ring-blue-600 focus:border-transparent"
              />
            </div>

            <div>
              <label for="libroAutor" class="block text-xs font-semibold text-slate-700 uppercase tracking-wide mb-1">
                Autor <span class="text-rose-600">*</span>
              </label>
              <input
                id="libroAutor"
                type="text"
                required
                placeholder="Ej. Gabriel García Márquez"
                class="w-full px-3.5 py-2.5 rounded-xl border border-slate-300 text-sm focus:outline-none focus:ring-2 focus:ring-blue-600 focus:border-transparent"
              />
            </div>

            <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
              <div>
                <label for="libroAnio" class="block text-xs font-semibold text-slate-700 uppercase tracking-wide mb-1">
                  Año <span class="text-rose-600">*</span>
                </label>
                <input
                  id="libroAnio"
                  type="number"
                  min="1000"
                  max="2100"
                  required
                  placeholder="Ej. 1967"
                  class="w-full px-3.5 py-2.5 rounded-xl border border-slate-300 text-sm focus:outline-none focus:ring-2 focus:ring-blue-600 focus:border-transparent"
                />
              </div>

              <div>
                <label for="libroCategoria" class="block text-xs font-semibold text-slate-700 uppercase tracking-wide mb-1">
                  Categoría / ISBN
                </label>
                <input
                  id="libroCategoria"
                  type="text"
                  placeholder="Ej. Novela / ISBN"
                  class="w-full px-3.5 py-2.5 rounded-xl border border-slate-300 text-sm focus:outline-none focus:ring-2 focus:ring-blue-600 focus:border-transparent"
                />
              </div>
            </div>

            <button
              type="submit"
              class="w-full py-2.5 px-4 rounded-xl bg-editorial-accent hover:bg-editorial-accentHover text-white font-semibold text-sm shadow-sm transition flex items-center justify-center gap-2"
            >
              <svg xmlns="http://www.w3.org/2000/svg" class="w-4 h-4" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                <path stroke-linecap="round" stroke-linejoin="round" d="M12 4v16m8-8H4" />
              </svg>
              <span>Agregar al Catálogo</span>
            </button>
          </form>
        </section>

        <!-- 2. Panel: Registrar Préstamo y Devolución -->
        <section class="bg-white rounded-2xl p-6 border border-slate-200/90 shadow-sm space-y-6">
          <!-- Sub-bloque: Registrar Préstamo -->
          <div>
            <div class="flex items-center gap-2 mb-4 pb-3 border-b border-slate-100">
              <span class="w-2.5 h-2.5 rounded-full bg-amber-500"></span>
              <h2 class="font-serif text-lg font-bold text-slate-900">Registrar Préstamo</h2>
            </div>

            <form id="formRegistrarPrestamo" class="space-y-3.5" novalidate>
              <div>
                <label for="selectLibroPrestar" class="block text-xs font-semibold text-slate-700 uppercase tracking-wide mb-1">
                  Seleccionar libro disponible <span class="text-rose-600">*</span>
                </label>
                <select
                  id="selectLibroPrestar"
                  class="w-full px-3.5 py-2.5 rounded-xl border border-slate-300 text-sm bg-white focus:outline-none focus:ring-2 focus:ring-amber-500 focus:border-transparent"
                >
                  <option value="">-- Elige un libro disponible --</option>
                </select>
              </div>

              <div>
                <label for="prestamoLector" class="block text-xs font-semibold text-slate-700 uppercase tracking-wide mb-1">
                  Nombre del lector <span class="text-rose-600">*</span>
                </label>
                <input
                  id="prestamoLector"
                  type="text"
                  placeholder="Ej. Ana Sofía Morales"
                  class="w-full px-3.5 py-2.5 rounded-xl border border-slate-300 text-sm focus:outline-none focus:ring-2 focus:ring-amber-500 focus:border-transparent"
                />
              </div>

              <div>
                <label for="prestamoFecha" class="block text-xs font-semibold text-slate-700 uppercase tracking-wide mb-1">
                  Fecha de préstamo <span class="text-rose-600">*</span>
                </label>
                <input
                  id="prestamoFecha"
                  type="date"
                  class="w-full px-3.5 py-2.5 rounded-xl border border-slate-300 text-sm focus:outline-none focus:ring-2 focus:ring-amber-500 focus:border-transparent"
                />
              </div>

              <button
                type="submit"
                class="w-full py-2.5 px-4 rounded-xl bg-amber-600 hover:bg-amber-700 text-white font-semibold text-sm shadow-sm transition"
              >
                Confirmar Préstamo
              </button>
            </form>
          </div>

          <!-- Sub-bloque: Registrar Devolución -->
          <div class="pt-4 border-t border-slate-200">
            <div class="flex items-center gap-2 mb-3">
              <span class="w-2.5 h-2.5 rounded-full bg-emerald-600"></span>
              <h3 class="font-serif text-base font-bold text-slate-900">Registrar Devolución</h3>
            </div>

            <form id="formRegistrarDevolucion" class="space-y-3" novalidate>
              <div>
                <label for="selectLibroDevolver" class="block text-xs font-semibold text-slate-700 uppercase tracking-wide mb-1">
                  Seleccionar libro prestado
                </label>
                <select
                  id="selectLibroDevolver"
                  class="w-full px-3.5 py-2.5 rounded-xl border border-slate-300 text-sm bg-white focus:outline-none focus:ring-2 focus:ring-emerald-600 focus:border-transparent"
                >
                  <option value="">-- Elige un libro prestado --</option>
                </select>
              </div>

              <button
                type="submit"
                class="w-full py-2.5 px-4 rounded-xl bg-emerald-600 hover:bg-emerald-700 text-white font-semibold text-sm shadow-sm transition"
              >
                Marcar como Devuelto
              </button>
            </form>
          </div>
        </section>
      </div>

      <!-- Columna derecha: Buscador, Filtros rápidos y Catálogo de Libros -->
      <section class="lg:col-span-8 bg-white rounded-2xl border border-slate-200/90 shadow-sm overflow-hidden flex flex-col">

        <!-- Cabecera de búsqueda y pestañas de filtro -->
        <div class="p-5 sm:p-6 border-b border-slate-200 space-y-4">
          <div class="flex flex-col sm:flex-row sm:items-center sm:justify-between gap-3">
            <div>
              <h2 class="font-serif text-xl font-bold text-slate-900">Catálogo de la Biblioteca</h2>
              <p class="text-xs text-editorial-muted mt-0.5">Consulta, filtra o gestiona devoluciones directamente desde cada registro.</p>
            </div>

            <!-- Pestañas de filtro rápido con contadores dinámicos -->
            <div class="inline-flex p-1 rounded-xl bg-slate-100 border border-slate-200 self-start sm:self-auto" role="tablist">
              <button
                type="button"
                data-filtro="todos"
                class="filtro-btn px-3 py-1.5 rounded-lg text-xs font-semibold transition bg-white text-slate-900 shadow-sm"
              >
                Todos (<span id="badgeTodos">0</span>)
              </button>
              <button
                type="button"
                data-filtro="disponibles"
                class="filtro-btn px-3 py-1.5 rounded-lg text-xs font-semibold transition text-slate-600 hover:text-slate-900"
              >
                Disponibles (<span id="badgeDisponibles">0</span>)
              </button>
              <button
                type="button"
                data-filtro="prestados"
                class="filtro-btn px-3 py-1.5 rounded-lg text-xs font-semibold transition text-slate-600 hover:text-slate-900"
              >
                Prestados (<span id="badgePrestados">0</span>)
              </button>
            </div>
          </div>

          <!-- Buscador en tiempo real por título o autor -->
          <div class="relative">
            <span class="absolute inset-y-0 left-0 pl-3.5 flex items-center pointer-events-none text-slate-400">
              <svg xmlns="http://www.w3.org/2000/svg" class="w-4 h-4" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                <path stroke-linecap="round" stroke-linejoin="round" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" />
              </svg>
            </span>
            <input
              id="inputBuscador"
              type="search"
              placeholder="Buscar por título o autor en tiempo real..."
              class="w-full pl-10 pr-4 py-2.5 rounded-xl border border-slate-300 text-sm bg-slate-50/60 focus:bg-white focus:outline-none focus:ring-2 focus:ring-blue-600 focus:border-transparent"
            />
          </div>
        </div>

        <!-- Tabla responsiva para escritorio -->
        <div class="hidden md:block overflow-x-auto">
          <table class="w-full text-left border-collapse">
            <thead>
              <tr class="bg-slate-50 border-b border-slate-200 text-[11px] font-bold uppercase tracking-wider text-slate-500">
                <th class="py-3.5 px-5">ID</th>
                <th class="py-3.5 px-4">Obra y Autor</th>
                <th class="py-3.5 px-4">Año / Cat.</th>
                <th class="py-3.5 px-4">Estado</th>
                <th class="py-3.5 px-5 text-right">Acción</th>
              </tr>
            </thead>
            <tbody id="tablaLibrosBody" class="divide-y divide-slate-100 text-sm">
              <!-- Filas renderizadas dinámicamente por el módulo de UI -->
            </tbody>
          </table>
        </div>

        <!-- Vista de tarjetas para dispositivos móviles -->
        <div id="listaLibrosMovil" class="md:hidden divide-y divide-slate-100">
          <!-- Tarjetas renderizadas dinámicamente por el módulo de UI -->
        </div>

        <!-- Estado vacío cuando no hay coincidencias o libros -->
        <div id="estadoVacio" class="hidden p-10 text-center space-y-3">
          <div class="w-12 h-12 rounded-2xl bg-slate-100 text-slate-400 mx-auto flex items-center justify-center">
            <svg xmlns="http://www.w3.org/2000/svg" class="w-6 h-6" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
              <path stroke-linecap="round" stroke-linejoin="round" d="M12 6.253v13m0-13C10.832 5.477 9.246 5 7.5 5S4.168 5.477 3 6.253v13C4.168 18.477 5.754 18 7.5 18s3.332.477 4.5 1.253m0-13C13.168 5.477 14.754 5 16.5 5c1.747 0 3.332.477 4.5 1.253v13C19.832 18.477 18.247 18 16.5 18c-1.746 0-3.332.477-4.5 1.253" />
            </svg>
          </div>
          <h3 class="text-base font-semibold text-slate-800">No se encontraron libros</h3>
          <p class="text-xs text-slate-500 max-w-sm mx-auto">
            Registra un nuevo libro desde el panel lateral, ajusta los filtros de búsqueda o carga los datos de ejemplo desde la barra superior.
          </p>
        </div>
      </section>
    </div>
  </main>

  <!-- Pie de página -->
  <footer class="border-t border-slate-200 bg-white mt-12">
    <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4 flex flex-col sm:flex-row items-center justify-between text-xs text-slate-500 gap-2">
      <span>BiblioGest · Proyecto de Administración de Biblioteca</span>
      <span>Persistencia automática en navegador + Respaldo en archivos JSON</span>
    </div>
  </footer>

  <!-- ===================================================================== -->
  <!-- ARQUITECTURA JAVASCRIPT MODULAR POR RESPONSABILIDADES                 -->
  <!-- ===================================================================== -->
  <script>
    /**
     * =========================================================================
     * MÓDULO 1: ALMACENAMIENTO Y MANEJO DE ARCHIVOS (StorageModule)
     * Responsabilidad: Persistencia en localStorage, exportación a archivo .json
     *                  e importación/lectura desde archivos .json externos.
     * =========================================================================
     */
    const StorageModule = (() => {
      const CLAVE_STORAGE = 'bibliogest_catalogo_v1';

      /**
       * Carga la colección de libros guardada en localStorage.
       * @returns {Array<Object>} Arreglo de libros o arreglo vacío si no hay datos.
       */
      function cargarDesdeLocal() {
        try {
          const datosCrudos = localStorage.getItem(CLAVE_STORAGE);
          if (!datosCrudos) return [];
          const parsed = JSON.parse(datosCrudos);
          return Array.isArray(parsed) ? parsed : [];
        } catch (error) {
          console.error('Error al leer localStorage:', error);
          return [];
        }
      }

      /**
       * Guarda la colección completa de libros en localStorage.
       * @param {Array<Object>} libros - Lista actual de libros.
       */
      function guardarEnLocal(libros) {
        try {
          localStorage.setItem(CLAVE_STORAGE, JSON.stringify(libros));
        } catch (error) {
          console.error('Error al guardar en localStorage:', error);
        }
      }

      /**
       * Genera y descarga un archivo .json con todo el catálogo y estado de préstamos.
       * @param {Array<Object>} libros - Lista actual de libros.
       */
      function exportarArchivoJSON(libros) {
        const contenido = JSON.stringify(libros, null, 2);
        const blob = new Blob([contenido], { type: 'application/json;charset=utf-8' });
        const url = URL.createObjectURL(blob);

        const enlace = document.createElement('a');
        const fechaHoy = new Date().toISOString().slice(0, 10);
        enlace.href = url;
        enlace.download = `biblioteca_respaldo_${fechaHoy}.json`;
        document.body.appendChild(enlace);
        enlace.click();
        document.body.removeChild(enlace);
        URL.revokeObjectURL(url);
      }

      /**
       * Lee un archivo .json seleccionado por el usuario y valida su estructura.
       * @param {File} archivo - Objeto File proveniente de un input[type="file"].
       * @returns {Promise<Array<Object>>} Promesa con los libros validados.
       */
      function importarArchivoJSON(archivo) {
        return new Promise((resolve, reject) => {
          if (!archivo) {
            reject(new Error('No se seleccionó ningún archivo.'));
            return;
          }

          const lector = new FileReader();
          lector.onload = (evento) => {
            try {
              const datos = JSON.parse(evento.target.result);
              if (!Array.isArray(datos)) {
                reject(new Error('El archivo JSON debe contener una lista (arreglo) de libros.'));
                return;
              }

              // Normalizar y validar cada registro del archivo importado
              const librosValidados = datos
                .filter((item) => item && typeof item.titulo === 'string' && typeof item.autor === 'string')
                .map((item, indice) => ({
                  id: Number.isInteger(item.id) ? item.id : indice + 1,
                  titulo: item.titulo.trim(),
                  autor: item.autor.trim(),
                  anio: Number(item.anio) || new Date().getFullYear(),
                  categoria: typeof item.categoria === 'string' ? item.categoria.trim() : '',
                  disponible: typeof item.disponible === 'boolean' ? item.disponible : true,
                  prestamo: item.disponible === false && item.prestamo
                    ? {
                        lector: String(item.prestamo.lector || 'Lector registrado'),
                        fecha: String(item.prestamo.fecha || new Date().toISOString().slice(0, 10))
                      }
                    : null
                }));

              resolve(librosValidados);
            } catch (error) {
              reject(new Error('El archivo seleccionado no tiene un formato JSON válido.'));
            }
          };

          lector.onerror = () => reject(new Error('No se pudo leer el archivo seleccionado.'));
          lector.readAsText(archivo, 'UTF-8');
        });
      }

      return {
        cargarDesdeLocal,
        guardarEnLocal,
        exportarArchivoJSON,
        importarArchivoJSON
      };
    })();


    /**
     * =========================================================================
     * MÓDULO 2: GESTIÓN DE CATÁLOGO (CatalogoModule)
     * Responsabilidad: Alta de libros con ID único, búsqueda por título/autor,
     *                  filtrado por disponibilidad y datos de ejemplo.
     * =========================================================================
     */
    const CatalogoModule = (() => {
      /**
       * Calcula el siguiente ID entero único disponible en la colección.
       * @param {Array<Object>} libros
       * @returns {number}
       */
      function generarIdUnico(libros) {
        if (libros.length === 0) return 1;
        const maxId = libros.reduce((max, libro) => (libro.id > max ? libro.id : max), 0);
        return maxId + 1;
      }

      /**
       * Valida y crea un nuevo objeto libro.
       * @param {Array<Object>} libros - Catálogo actual.
       * @param {Object} datos - { titulo, autor, anio, categoria }
       * @returns {{ exito: boolean, mensaje: string, libro?: Object }}
       */
      function registrarLibro(libros, { titulo, autor, anio, categoria }) {
        const tituloLimpio = (titulo || '').trim();
        const autorLimpio = (autor || '').trim();
        const anioNumero = Number(anio);
        const categoriaLimpia = (categoria || '').trim();

        if (!tituloLimpio) {
          return { exito: false, mensaje: 'El título del libro es obligatorio.' };
        }
        if (!autorLimpio) {
          return { exito: false, mensaje: 'El nombre del autor es obligatorio.' };
        }
        if (!Number.isInteger(anioNumero) || anioNumero < 1000 || anioNumero > 2100) {
          return { exito: false, mensaje: 'Ingresa un año de publicación válido (entre 1000 y 2100).' };
        }

        const nuevoLibro = {
          id: generarIdUnico(libros),
          titulo: tituloLimpio,
          autor: autorLimpio,
          anio: anioNumero,
          categoria: categoriaLimpia || 'General',
          disponible: true,
          prestamo: null
        };

        libros.push(nuevoLibro);
        return {
          exito: true,
          mensaje: `Libro "${nuevoLibro.titulo}" registrado con ID #${nuevoLibro.id}.`,
          libro: nuevoLibro
        };
      }

      /**
       * Filtra libros combinando el estado ('todos' | 'disponibles' | 'prestados')
       * y la búsqueda textual por título o autor (insensible a mayúsculas).
       * @param {Array<Object>} libros
       * @param {string} filtroEstado
       * @param {string} terminoBusqueda
       * @returns {Array<Object>}
       */
      function filtrarCatalogo(libros, filtroEstado = 'todos', terminoBusqueda = '') {
        const consulta = terminoBusqueda.trim().toLowerCase();

        return libros.filter((libro) => {
          // 1. Filtro por estado de disponibilidad
          if (filtroEstado === 'disponibles' && !libro.disponible) return false;
          if (filtroEstado === 'prestados' && libro.disponible) return false;

          // 2. Filtro por coincidencia parcial en título o autor
          if (consulta.length > 0) {
            const coincideTitulo = libro.titulo.toLowerCase().includes(consulta);
            const coincideAutor = libro.autor.toLowerCase().includes(consulta);
            return coincideTitulo || coincideAutor;
          }

          return true;
        });
      }

      /**
       * Genera un conjunto inicial de libros de ejemplo para demostraciones.
       * @returns {Array<Object>}
       */
      function obtenerDatosEjemplo() {
        const hoy = new Date().toISOString().slice(0, 10);
        return [
          {
            id: 1,
            titulo: 'Cien años de soledad',
            autor: 'Gabriel García Márquez',
            anio: 1967,
            categoria: 'Realismo Mágico',
            disponible: true,
            prestamo: null
          },
          {
            id: 2,
            titulo: 'El principito',
            autor: 'Antoine de Saint-Exupéry',
            anio: 1943,
            categoria: 'Fábula',
            disponible: false,
            prestamo: { lector: 'Carlos Mendoza', fecha: hoy }
          },
          {
            id: 3,
            titulo: '1984',
            autor: 'George Orwell',
            anio: 1949,
            categoria: 'Ciencia Ficción',
            disponible: true,
            prestamo: null
          },
          {
            id: 4,
            titulo: 'Pedro Páramo',
            autor: 'Juan Rulfo',
            anio: 1955,
            categoria: 'Novela',
            disponible: true,
            prestamo: null
          },
          {
            id: 5,
            titulo: 'Ficciones',
            autor: 'Jorge Luis Borges',
            anio: 1944,
            categoria: 'Cuentos',
            disponible: false,
            prestamo: { lector: 'Valeria Torres', fecha: hoy }
          }
        ];
      }

      return {
        registrarLibro,
        filtrarCatalogo,
        obtenerDatosEjemplo
      };
    })();


    /**
     * =========================================================================
     * MÓDULO 3: PRÉSTAMOS Y DEVOLUCIONES (PrestamosModule)
     * Responsabilidad: Validar y ejecutar préstamos (evitando duplicados)
     *                  y registrar devoluciones restaurando disponibilidad.
     * =========================================================================
     */
    const PrestamosModule = (() => {
      /**
       * Registra el préstamo de un libro disponible a un lector.
       * @param {Array<Object>} libros
       * @param {number} idLibro
       * @param {string} lector
       * @param {string} fecha
       * @returns {{ exito: boolean, mensaje: string }}
       */
      function prestarLibro(libros, idLibro, lector, fecha) {
        const idNum = Number(idLibro);
        const lectorLimpio = (lector || '').trim();
        const fechaLimpia = (fecha || '').trim();

        if (!idNum) {
          return { exito: false, mensaje: 'Selecciona un libro disponible de la lista.' };
        }
        if (!lectorLimpio) {
          return { exito: false, mensaje: 'Ingresa el nombre del lector que solicita el libro.' };
        }
        if (!fechaLimpia) {
          return { exito: false, mensaje: 'Selecciona una fecha válida para el préstamo.' };
        }

        const libro = libros.find((item) => item.id === idNum);
        if (!libro) {
          return { exito: false, mensaje: 'No se encontró el libro seleccionado.' };
        }

        // Bloqueo explícito si el libro ya se encuentra prestado
        if (!libro.disponible) {
          return {
            exito: false,
            mensaje: `El libro "${libro.titulo}" ya está prestado a ${libro.prestamo?.lector || 'otro usuario'}.`
          };
        }

        libro.disponible = false;
        libro.prestamo = {
          lector: lectorLimpio,
          fecha: fechaLimpia
        };

        return {
          exito: true,
          mensaje: `Préstamo registrado: "${libro.titulo}" asignado a ${lectorLimpio}.`
        };
      }

      /**
       * Registra la devolución de un libro prestado y lo marca como disponible.
       * @param {Array<Object>} libros
       * @param {number} idLibro
       * @returns {{ exito: boolean, mensaje: string }}
       */
      function devolverLibro(libros, idLibro) {
        const idNum = Number(idLibro);
        if (!idNum) {
          return { exito: false, mensaje: 'Selecciona un libro prestado para devolver.' };
        }

        const libro = libros.find((item) => item.id === idNum);
        if (!libro) {
          return { exito: false, mensaje: 'No se encontró el libro indicado.' };
        }

        if (libro.disponible) {
          return { exito: false, mensaje: `El libro "${libro.titulo}" ya figura como disponible.` };
        }

        const lectorAnterior = libro.prestamo?.lector || 'el lector';
        libro.disponible = true;
        libro.prestamo = null;

        return {
          exito: true,
          mensaje: `Devolución completada: "${libro.titulo}" (devuelto por ${lectorAnterior}).`
        };
      }

      return {
        prestarLibro,
        devolverLibro
      };
    })();


    /**
     * =========================================================================
     * MÓDULO 4: INTERFAZ DE USUARIO Y CONTROLADOR (UIModule)
     * Responsabilidad: Coordinar eventos del DOM, renderizar tabla/tarjetas,
     *                  actualizar contadores, selectores y notificaciones.
     * =========================================================================
     */
    const UIModule = (() => {
      // Estado central de la aplicación en memoria
      let estado = {
        libros: [],
        filtroActual: 'todos', // 'todos' | 'disponibles' | 'prestados'
        busquedaActual: ''
      };

      // Referencias cacheadas a elementos del DOM
      const dom = {};

      /**
       * Escapa caracteres especiales HTML para evitar inyección de código en textos.
       * @param {string} texto
       * @returns {string}
       */
      function escaparHTML(texto) {
        const div = document.createElement('div');
        div.textContent = texto;
        return div.innerHTML;
      }

      /**
       * Muestra un banner de notificación visual no intrusivo.
       * @param {string} mensaje
       * @param {'exito' | 'error' | 'info'} tipo
       */
      function mostrarNotificacion(mensaje, tipo = 'exito') {
        const estilos = {
          exito: 'bg-emerald-50 border-emerald-200 text-emerald-900',
          error: 'bg-rose-50 border-rose-200 text-rose-900',
          info: 'bg-blue-50 border-blue-200 text-blue-900'
        };

        dom.zonaNotificacion.className = `rounded-2xl p-4 border text-sm font-medium flex items-center justify-between gap-3 transition-all ${estilos[tipo] || estilos.info}`;
        dom.textoNotificacion.textContent = mensaje;
        dom.zonaNotificacion.classList.remove('hidden');
      }

      /**
       * Actualiza los contadores superiores y los badges de las pestañas de filtro.
       */
      function actualizarContadores() {
        const total = estado.libros.length;
        const disponibles = estado.libros.filter((l) => l.disponible).length;
        const prestados = total - disponibles;

        dom.statTotal.textContent = total;
        dom.statDisponibles.textContent = disponibles;
        dom.statPrestados.textContent = prestados;

        dom.badgeTodos.textContent = total;
        dom.badgeDisponibles.textContent = disponibles;
        dom.badgePrestados.textContent = prestados;
      }

      /**
       * Actualiza las opciones de los selectores de Préstamo (solo disponibles)
       * y Devolución (solo prestados).
       */
      function actualizarSelectores() {
        const disponibles = estado.libros.filter((l) => l.disponible);
        const prestados = estado.libros.filter((l) => !l.disponible);

        // Selector de libros disponibles para prestar
        dom.selectLibroPrestar.innerHTML = '<option value="">-- Elige un libro disponible --</option>';
        disponibles.forEach((libro) => {
          const opcion = document.createElement('option');
          opcion.value = libro.id;
          opcion.textContent = `#${libro.id} · ${libro.titulo} (${libro.autor})`;
          dom.selectLibroPrestar.appendChild(opcion);
        });

        // Selector de libros prestados para devolver
        dom.selectLibroDevolver.innerHTML = '<option value="">-- Elige un libro prestado --</option>';
        prestados.forEach((libro) => {
          const opcion = document.createElement('option');
          opcion.value = libro.id;
          opcion.textContent = `#${libro.id} · ${libro.titulo} — ${libro.prestamo?.lector || ''}`;
          dom.selectLibroDevolver.appendChild(opcion);
        });
      }

      /**
       * Renderiza el catálogo filtrado tanto en la tabla de escritorio como
       * en las tarjetas responsivas para móvil.
       */
      function renderizarCatalogo() {
        const filtrados = CatalogoModule.filtrarCatalogo(
          estado.libros,
          estado.filtroActual,
          estado.busquedaActual
        );

        dom.tablaLibrosBody.innerHTML = '';
        dom.listaLibrosMovil.innerHTML = '';

        if (filtrados.length === 0) {
          dom.estadoVacio.classList.remove('hidden');
          return;
        }
        dom.estadoVacio.classList.add('hidden');

        filtrados.forEach((libro) => {
          const tituloSeg = escaparHTML(libro.titulo);
          const autorSeg = escaparHTML(libro.autor);
          const catSeg = escaparHTML(libro.categoria || 'General');
          const lectorSeg = libro.prestamo ? escaparHTML(libro.prestamo.lector) : '';
          const fechaSeg = libro.prestamo ? escaparHTML(libro.prestamo.fecha) : '';

          // 1. Fila para tabla en pantallas medianas/grandes
          const tr = document.createElement('tr');
          tr.className = 'hover:bg-slate-50/80 transition';
          tr.innerHTML = `
            <td class="py-4 px-5 font-mono text-xs font-semibold text-slate-500">#${libro.id}</td>
            <td class="py-4 px-4">
              <p class="font-semibold text-slate-900">${tituloSeg}</p>
              <p class="text-xs text-slate-500">${autorSeg}</p>
            </td>
            <td class="py-4 px-4 text-xs text-slate-600">
              <span class="font-medium">${libro.anio}</span>
              <span class="block text-slate-400">${catSeg}</span>
            </td>
            <td class="py-4 px-4">
              ${
                libro.disponible
                  ? `<span class="inline-flex items-center gap-1.5 px-2.5 py-1 rounded-full text-xs font-semibold bg-emerald-50 text-emerald-700 border border-emerald-200">
                       <span class="w-1.5 h-1.5 rounded-full bg-emerald-500"></span> Disponible
                     </span>`
                  : `<div>
                       <span class="inline-flex items-center gap-1.5 px-2.5 py-1 rounded-full text-xs font-semibold bg-amber-50 text-amber-800 border border-amber-200">
                         <span class="w-1.5 h-1.5 rounded-full bg-amber-500"></span> Prestado
                       </span>
                       <p class="text-[11px] text-slate-500 mt-1">A: <strong>${lectorSeg}</strong> (${fechaSeg})</p>
                     </div>`
              }
            </td>
            <td class="py-4 px-5 text-right">
              ${
                libro.disponible
                  ? `<button type="button" data-accion="seleccionar-prestar" data-id="${libro.id}"
                       class="px-3 py-1.5 rounded-lg text-xs font-semibold bg-slate-100 hover:bg-blue-50 text-slate-700 hover:text-blue-700 transition">
                       Prestar
                     </button>`
                  : `<button type="button" data-accion="devolver-rapido" data-id="${libro.id}"
                       class="px-3 py-1.5 rounded-lg text-xs font-semibold bg-emerald-600 hover:bg-emerald-700 text-white shadow-sm transition">
                       Devolver
                     </button>`
              }
            </td>
          `;
          dom.tablaLibrosBody.appendChild(tr);

          // 2. Tarjeta para pantallas móviles
          const card = document.createElement('div');
          card.className = 'p-4 space-y-2.5';
          card.innerHTML = `
            <div class="flex items-start justify-between gap-2">
              <div>
                <span class="font-mono text-[11px] font-semibold text-slate-400">#${libro.id} · ${libro.anio} · ${catSeg}</span>
                <h3 class="font-semibold text-slate-900 text-sm">${tituloSeg}</h3>
                <p class="text-xs text-slate-500">${autorSeg}</p>
              </div>
              ${
                libro.disponible
                  ? `<span class="shrink-0 px-2.5 py-1 rounded-full text-[11px] font-semibold bg-emerald-50 text-emerald-700 border border-emerald-200">Disponible</span>`
                  : `<span class="shrink-0 px-2.5 py-1 rounded-full text-[11px] font-semibold bg-amber-50 text-amber-800 border border-amber-200">Prestado</span>`
              }
            </div>
            ${
              !libro.disponible
                ? `<p class="text-xs text-amber-800 bg-amber-50/70 px-3 py-1.5 rounded-lg">
                     Prestado a <strong>${lectorSeg}</strong> el ${fechaSeg}
                   </p>`
                : ''
            }
            <div class="pt-1 flex justify-end">
              ${
                libro.disponible
                  ? `<button type="button" data-accion="seleccionar-prestar" data-id="${libro.id}"
                       class="px-3.5 py-2 rounded-xl text-xs font-semibold bg-slate-100 text-slate-800 hover:bg-blue-50 hover:text-blue-700">
                       Seleccionar para préstamo
                     </button>`
                  : `<button type="button" data-accion="devolver-rapido" data-id="${libro.id}"
                       class="px-3.5 py-2 rounded-xl text-xs font-semibold bg-emerald-600 text-white hover:bg-emerald-700">
                       Registrar devolución
                     </button>`
              }
            </div>
          `;
          dom.listaLibrosMovil.appendChild(card);
        });
      }

      /**
       * Sincroniza el estado en localStorage y refresca toda la interfaz.
       */
      function guardarYRefrescar() {
        StorageModule.guardarEnLocal(estado.libros);
        actualizarContadores();
        actualizarSelectores();
        renderizarCatalogo();
      }

      /**
       * Inicializa referencias DOM, carga datos iniciales y registra eventos.
       */
      function iniciar() {
        // Capturar elementos del DOM
        dom.zonaNotificacion = document.getElementById('zonaNotificacion');
        dom.textoNotificacion = document.getElementById('textoNotificacion');
        dom.btnCerrarNotificacion = document.getElementById('btnCerrarNotificacion');

        dom.statTotal = document.getElementById('statTotal');
        dom.statDisponibles = document.getElementById('statDisponibles');
        dom.statPrestados = document.getElementById('statPrestados');

        dom.badgeTodos = document.getElementById('badgeTodos');
        dom.badgeDisponibles = document.getElementById('badgeDisponibles');
        dom.badgePrestados = document.getElementById('badgePrestados');

        dom.formRegistrarLibro = document.getElementById('formRegistrarLibro');
        dom.libroTitulo = document.getElementById('libroTitulo');
        dom.libroAutor = document.getElementById('libroAutor');
        dom.libroAnio = document.getElementById('libroAnio');
        dom.libroCategoria = document.getElementById('libroCategoria');

        dom.formRegistrarPrestamo = document.getElementById('formRegistrarPrestamo');
        dom.selectLibroPrestar = document.getElementById('selectLibroPrestar');
        dom.prestamoLector = document.getElementById('prestamoLector');
        dom.prestamoFecha = document.getElementById('prestamoFecha');

        dom.formRegistrarDevolucion = document.getElementById('formRegistrarDevolucion');
        dom.selectLibroDevolver = document.getElementById('selectLibroDevolver');

        dom.inputBuscador = document.getElementById('inputBuscador');
        dom.tablaLibrosBody = document.getElementById('tablaLibrosBody');
        dom.listaLibrosMovil = document.getElementById('listaLibrosMovil');
        dom.estadoVacio = document.getElementById('estadoVacio');

        dom.btnCargarDemo = document.getElementById('btnCargarDemo');
        dom.btnExportarJSON = document.getElementById('btnExportarJSON');
        dom.inputImportarJSON = document.getElementById('inputImportarJSON');

        // Establecer fecha actual por defecto en el formulario de préstamo
        dom.prestamoFecha.value = new Date().toISOString().slice(0, 10);

        // Cargar libros guardados previamente en el navegador
        estado.libros = StorageModule.cargarDesdeLocal();
        guardarYRefrescar();

        // Evento: Cerrar notificación
        dom.btnCerrarNotificacion.addEventListener('click', () => {
          dom.zonaNotificacion.classList.add('hidden');
        });

        // Evento: Registrar nuevo libro
        dom.formRegistrarLibro.addEventListener('submit', (e) => {
          e.preventDefault();
          const resultado = CatalogoModule.registrarLibro(estado.libros, {
            titulo: dom.libroTitulo.value,
            autor: dom.libroAutor.value,
            anio: dom.libroAnio.value,
            categoria: dom.libroCategoria.value
          });

          if (!resultado.exito) {
            mostrarNotificacion(resultado.mensaje, 'error');
            return;
          }

          dom.formRegistrarLibro.reset();
          guardarYRefrescar();
          mostrarNotificacion(resultado.mensaje, 'exito');
        });

        // Evento: Registrar préstamo
        dom.formRegistrarPrestamo.addEventListener('submit', (e) => {
          e.preventDefault();
          const resultado = PrestamosModule.prestarLibro(
            estado.libros,
            dom.selectLibroPrestar.value,
            dom.prestamoLector.value,
            dom.prestamoFecha.value
          );

          if (!resultado.exito) {
            mostrarNotificacion(resultado.mensaje, 'error');
            return;
          }

          dom.prestamoLector.value = '';
          dom.selectLibroPrestar.value = '';
          guardarYRefrescar();
          mostrarNotificacion(resultado.mensaje, 'exito');
        });

        // Evento: Registrar devolución desde formulario
        dom.formRegistrarDevolucion.addEventListener('submit', (e) => {
          e.preventDefault();
          const resultado = PrestamosModule.devolverLibro(
            estado.libros,
            dom.selectLibroDevolver.value
          );

          if (!resultado.exito) {
            mostrarNotificacion(resultado.mensaje, 'error');
            return;
          }

          guardarYRefrescar();
          mostrarNotificacion(resultado.mensaje, 'exito');
        });

        // Delegación de eventos en tabla y tarjetas móviles (Prestar / Devolver directo)
        const manejarAccionCatalogo = (e) => {
          const boton = e.target.closest('button[data-accion]');
          if (!boton) return;

          const accion = boton.dataset.accion;
          const idLibro = Number(boton.dataset.id);

          if (accion === 'seleccionar-prestar') {
            dom.selectLibroPrestar.value = idLibro;
            dom.prestamoLector.focus();
            dom.formRegistrarPrestamo.scrollIntoView({ behavior: 'smooth', block: 'center' });
            mostrarNotificacion(`Libro #${idLibro} seleccionado. Ingresa el nombre del lector para completar el préstamo.`, 'info');
          } else if (accion === 'devolver-rapido') {
            const resultado = PrestamosModule.devolverLibro(estado.libros, idLibro);
            if (resultado.exito) {
              guardarYRefrescar();
              mostrarNotificacion(resultado.mensaje, 'exito');
            } else {
              mostrarNotificacion(resultado.mensaje, 'error');
            }
          }
        };

        dom.tablaLibrosBody.addEventListener('click', manejarAccionCatalogo);
        dom.listaLibrosMovil.addEventListener('click', manejarAccionCatalogo);

        // Evento: Búsqueda en tiempo real por título o autor
        dom.inputBuscador.addEventListener('input', (e) => {
          estado.busquedaActual = e.target.value;
          renderizarCatalogo();
        });

        // Evento: Filtros rápidos (Todos / Disponibles / Prestados)
        document.querySelectorAll('.filtro-btn').forEach((btn) => {
          btn.addEventListener('click', () => {
            estado.filtroActual = btn.dataset.filtro;

            // Actualizar estilos visuales de pestaña activa
            document.querySelectorAll('.filtro-btn').forEach((b) => {
              b.className = 'filtro-btn px-3 py-1.5 rounded-lg text-xs font-semibold transition text-slate-600 hover:text-slate-900';
            });
            btn.className = 'filtro-btn px-3 py-1.5 rounded-lg text-xs font-semibold transition bg-white text-slate-900 shadow-sm';

            renderizarCatalogo();
          });
        });

        // Evento: Cargar datos de ejemplo
        dom.btnCargarDemo.addEventListener('click', () => {
          estado.libros = CatalogoModule.obtenerDatosEjemplo();
          guardarYRefrescar();
          mostrarNotificacion('Se cargaron 5 libros de ejemplo en el catálogo.', 'info');
        });

        // Evento: Guardar/Exportar archivo JSON
        dom.btnExportarJSON.addEventListener('click', () => {
          if (estado.libros.length === 0) {
            mostrarNotificacion('No hay libros en el catálogo para exportar.', 'error');
            return;
          }
          StorageModule.exportarArchivoJSON(estado.libros);
          mostrarNotificacion('Archivo JSON generado y descargado correctamente.', 'exito');
        });

        // Evento: Cargar/Importar archivo JSON
        dom.inputImportarJSON.addEventListener('change', async (e) => {
          const archivo = e.target.files[0];
          if (!archivo) return;

          try {
            const librosImportados = await StorageModule.importarArchivoJSON(archivo);
            estado.libros = librosImportados;
            guardarYRefrescar();
            mostrarNotificacion(`Se cargaron ${librosImportados.length} libros desde "${archivo.name}".`, 'exito');
          } catch (error) {
            mostrarNotificacion(error.message, 'error');
          } finally {
            // Limpiar input para permitir recargar el mismo archivo si se desea
            dom.inputImportarJSON.value = '';
          }
        });
      }

      return { iniciar };
    })();

    // Arrancar la aplicación cuando el DOM esté listo
    window.addEventListener('DOMContentLoaded', UIModule.iniciar);
  </script>
</body>
</html>
