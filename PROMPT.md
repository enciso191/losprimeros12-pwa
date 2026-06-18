# Prompt para Codex

Pega debajo de esta línea el prompt completo:

---

Confirmado. Procede con la implementación. Aquí están los cambios 
exactos, en orden. Muéstrame cada bloque ANTES de aplicarlo.

═══════════════════════════════════════════════════════════════
CONTEXTO IMPORTANTE
═══════════════════════════════════════════════════════════════
- Cada siesta hoy tiene: { hora, dur }
- Vamos a agregar campos opcionales: { hora, dur, horaFin, 
  enCurso } sin romper entradas viejas
- El reporte médico usa data.siestas[i].dur — debe seguir 
  funcionando igual
- No existe banner flotante — hay que crearlo desde cero
- El overlay existente bloquea toda la pantalla — NO lo usamos 
  para el banner de siesta en curso

═══════════════════════════════════════════════════════════════
CAMBIO 1 — CSS: banner de siesta en curso
═══════════════════════════════════════════════════════════════

Agrega estos estilos después del bloque de estilos de siestas 
(líneas 204–224):

/* ── BANNER SIESTA EN CURSO ── */
#siesta-banner{
  position:fixed;
  bottom:72px; /* encima del BottomNav si existe, ajustar si hace falta */
  left:50%;
  transform:translateX(-50%);
  width:calc(100% - 32px);
  max-width:480px;
  background:var(--dark);
  color:var(--cream);
  border-radius:16px;
  padding:13px 16px;
  display:flex;
  align-items:center;
  gap:12px;
  z-index:900;
  box-shadow:0 4px 20px rgba(0,0,0,.25);
  animation:slideUp .3s ease;
}
#siesta-banner.hidden{display:none}
#siesta-banner-ico{font-size:22px;flex-shrink:0}
#siesta-banner-info{flex:1;min-width:0}
#siesta-banner-lbl{font-size:12px;font-weight:700;
  color:rgba(255,255,255,.6);letter-spacing:.3px}
#siesta-banner-timer{font-size:20px;font-weight:800;
  font-variant-numeric:tabular-nums;letter-spacing:1px}
#siesta-banner-fin{
  background:var(--sage);
  color:white;
  border:none;
  border-radius:10px;
  padding:9px 14px;
  font-family:'Nunito',sans-serif;
  font-size:12px;
  font-weight:800;
  cursor:pointer;
  touch-action:manipulation;
  flex-shrink:0;
  white-space:nowrap;
}
#siesta-banner-fin:active{background:var(--deep-sage)}
@keyframes slideUp{
  from{transform:translateX(-50%) translateY(20px);opacity:0}
  to{transform:translateX(-50%) translateY(0);opacity:1}
}

═══════════════════════════════════════════════════════════════
CAMBIO 2 — HTML: sección de siestas
═══════════════════════════════════════════════════════════════

Reemplaza el bloque HTML de siestas (líneas 744–765) con esto:

<div class="reg-sec" id="sec-siestas">
  <div class="reg-sec-head">
    <span class="reg-sec-ico">😴</span>
    <span class="reg-sec-name">Siestas</span>
    <span class="reg-sec-total" id="s-total">0m</span>
  </div>
  <div id="s-list">
    <p style="text-align:center;color:var(--light);
       font-style:italic;font-size:13px;padding:10px 0">
      Sin siestas registradas
    </p>
  </div>

  <!-- Controles: iniciar siesta o registro manual -->
  <div id="s-controls">

    <!-- Botón principal: iniciar siesta -->
    <div id="s-iniciar-wrap">
      <button class="reg-btn" id="s-btn-iniciar" 
              onclick="iniciarSiesta()"
              style="width:100%;background:var(--sage);
                     color:white;font-size:14px;
                     padding:12px;margin-top:6px">
        ▶ Iniciar siesta
      </button>
    </div>

    <!-- Separador -->
    <div style="display:flex;align-items:center;gap:8px;
                margin:10px 0;color:var(--light);font-size:11px">
      <div style="flex:1;height:1px;background:var(--sand)"></div>
      o registrar manualmente
      <div style="flex:1;height:1px;background:var(--sand)"></div>
    </div>

    <!-- Registro manual (igual que antes) -->
    <div style="display:flex;gap:8px;align-items:center">
      <select id="s-hora" class="reg-input" style="flex:1">
      </select>
      <input id="s-dur" class="reg-input" type="number"
             placeholder="Min" min="1" max="300"
             style="width:80px">
      <button class="reg-btn" onclick="addSiesta()">
        + Agregar
      </button>
    </div>
    <div style="font-size:10px;color:var(--light);
                margin-top:4px;text-align:right">
      Registra hora de inicio y duración en minutos
    </div>

  </div>
</div>

<!-- Banner flotante: siesta en curso -->
<div id="siesta-banner" class="hidden">
  <span id="siesta-banner-ico">🌙</span>
  <div id="siesta-banner-info">
    <div id="siesta-banner-lbl">SIESTA EN CURSO</div>
    <div id="siesta-banner-timer">0:00:00</div>
  </div>
  <button id="siesta-banner-fin" onclick="finalizarSiesta()">
    ⏹ Finalizar
  </button>
</div>

═══════════════════════════════════════════════════════════════
CAMBIO 3 — JS: lógica de siesta en curso
═══════════════════════════════════════════════════════════════

Agrega estas funciones DESPUÉS de delSiesta() y ANTES de 
cualquier función de biberón. No modifiques addSiesta() 
ni delSiesta() — solo agrégalas nuevas:

// ── SIESTA EN CURSO ──────────────────────────────────────────
const SIESTA_KEY = 'panda_siesta_activa';
let _siestaTimer = null;

function iniciarSiesta() {
  // Si ya hay una en curso, no hacer nada
  if (store.get(SIESTA_KEY)) return;

  const ahora = new Date();
  const datos = {
    inicio: ahora.toISOString(),
    horaStr: ahora.toTimeString().slice(0,5) // "HH:mm"
  };
  store.set(SIESTA_KEY, JSON.stringify(datos));
  mostrarBannerSiesta();
}

function finalizarSiesta() {
  const raw = store.get(SIESTA_KEY);
  if (!raw) return;

  const datos = JSON.parse(raw);
  const inicio = new Date(datos.inicio);
  const fin = new Date();
  const durMin = Math.round((fin - inicio) / 60000);

  // Guardar en el registro diario con horaFin
  const data = loadReg();
  data.siestas.push({
    hora: datos.horaStr,
    horaFin: fin.toTimeString().slice(0,5),
    dur: Math.max(1, durMin)
  });
  // Ordenar por hora de inicio
  data.siestas.sort((a,b) => a.hora.localeCompare(b.hora));
  saveReg(data);

  // Limpiar estado
  store.remove ? store.remove(SIESTA_KEY) 
               : localStorage.removeItem(SIESTA_KEY);
  clearInterval(_siestaTimer);
  _siestaTimer = null;

  ocultarBannerSiesta();
  renderReg();
}

function mostrarBannerSiesta() {
  const banner = document.getElementById('siesta-banner');
  const btnIniciar = document.getElementById('s-btn-iniciar');
  if (banner) banner.classList.remove('hidden');
  if (btnIniciar) {
    btnIniciar.textContent = '🌙 Siesta en curso...';
    btnIniciar.disabled = true;
    btnIniciar.style.opacity = '0.5';
    btnIniciar.style.cursor = 'default';
  }
  iniciarTimerBanner();
}

function ocultarBannerSiesta() {
  const banner = document.getElementById('siesta-banner');
  const btnIniciar = document.getElementById('s-btn-iniciar');
  if (banner) banner.classList.add('hidden');
  if (btnIniciar) {
    btnIniciar.textContent = '▶ Iniciar siesta';
    btnIniciar.disabled = false;
    btnIniciar.style.opacity = '1';
    btnIniciar.style.cursor = 'pointer';
  }
}

function iniciarTimerBanner() {
  const raw = store.get(SIESTA_KEY);
  if (!raw) return;
  const inicio = new Date(JSON.parse(raw).inicio);

  clearInterval(_siestaTimer);
  _siestaTimer = setInterval(() => {
    const diff = Math.floor((new Date() - inicio) / 1000);
    const h = Math.floor(diff / 3600);
    const m = Math.floor((diff % 3600) / 60);
    const s = diff % 60;
    const el = document.getElementById('siesta-banner-timer');
    if (el) el.textContent =
      `${h}:${String(m).padStart(2,'0')}:${String(s).padStart(2,'0')}`;
  }, 1000);
}

function checkSiestaActiva() {
  // Llamar al init: si hay siesta en curso al abrir la app,
  // restaurar el banner y el timer
  if (store.get(SIESTA_KEY)) {
    mostrarBannerSiesta();
  }
}
// ─────────────────────────────────────────────────────────────

═══════════════════════════════════════════════════════════════
CAMBIO 4 — JS: actualizar renderReg() para mostrar horaFin
═══════════════════════════════════════════════════════════════

En la función renderReg(), en la parte donde genera cada fila 
de siesta, actualiza el texto para mostrar hora de fin si existe.

Busca el map() que genera las filas de siestas y reemplaza 
la línea que muestra hora y duración por esto:

const horaFin = s.horaFin 
  ? ` → ${s.horaFin}` 
  : '';
const durTexto = s.dur >= 60 
  ? `${Math.floor(s.dur/60)}h ${s.dur%60}m` 
  : `${s.dur}m`;
// Texto de la fila: "14:30 → 16:23 · 1h 53m"
// o si es manual: "14:30 · 45m"
const rowTexto = `${s.hora}${horaFin} · ${durTexto}`;

═══════════════════════════════════════════════════════════════
CAMBIO 5 — JS: llamar checkSiestaActiva() en el init
═══════════════════════════════════════════════════════════════

En la función de inicialización (donde se llaman renderReg(), 
renderChecklist(), etc.), agrega esta llamada:

checkSiestaActiva();

═══════════════════════════════════════════════════════════════
CAMBIO 6 — SW: bump de versión
═══════════════════════════════════════════════════════════════

En sw.js: const CACHE = 'primeros12-v6';

═══════════════════════════════════════════════════════════════
VERIFICACIÓN FINAL
═══════════════════════════════════════════════════════════════

Después de aplicar todo, confirma:
1. Que addSiesta() y delSiesta() no fueron modificadas
2. Que el reporte médico (líneas ~1474–1499) sigue usando 
   data.siestas[i].dur sin cambios
3. Que checkSiestaActiva() está en el init
4. Que el banner tiene id="siesta-banner" con clase "hidden"
5. Cache en sw.js = 'primeros12-v6'

NO hagas commit ni push. Yo lo hago después de revisar.