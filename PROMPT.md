# Prompt para Codex

Pega debajo de esta línea el prompt completo:

---

Hola Claude. Voy a hacer tres mejoras a la sección de Siestas 
más una corrección de bug. Todo en index.html. 

Muéstrame cada cambio ANTES de aplicarlo.

═══════════════════════════════════════════════════════════════
CORRECCIÓN DE BUG — finalizarSiesta()
═══════════════════════════════════════════════════════════════

En finalizarSiesta(), busca esta línea:
  store.remove ? store.remove(SIESTA_KEY) 
               : localStorage.removeItem(SIESTA_KEY);

Reemplázala por:
  store.del(SIESTA_KEY);

═══════════════════════════════════════════════════════════════
CAMBIO 1 — Input manual mejorado (horas + minutos separados)
═══════════════════════════════════════════════════════════════

Reemplaza el bloque del registro manual (el div con s-hora, 
s-dur y el botón + Agregar) por esto:

<div style="display:flex;gap:8px;align-items:center;flex-wrap:wrap">
  <input id="s-hora-ini" class="reg-input" type="time"
         style="flex:1;min-width:100px">
  <div style="display:flex;gap:4px;align-items:center">
    <input id="s-dur-h" class="reg-input" type="number"
           placeholder="h" min="0" max="23"
           style="width:52px;text-align:center">
    <span style="color:var(--light);font-size:12px;
                 font-weight:700">h</span>
    <input id="s-dur-m" class="reg-input" type="number"
           placeholder="min" min="0" max="59"
           style="width:60px;text-align:center">
    <span style="color:var(--light);font-size:12px;
                 font-weight:700">m</span>
  </div>
  <button class="reg-btn" onclick="addSiesta()">
    + Agregar
  </button>
</div>
<div style="font-size:10px;color:var(--light);
            margin-top:4px;text-align:right">
  Hora de inicio · duración manual
</div>

También actualiza addSiesta() para leer los nuevos inputs:

function addSiesta() {
  const horaIni = document.getElementById('s-hora-ini').value 
                  || nowTime();
  const horas   = parseInt(document.getElementById('s-dur-h').value) || 0;
  const minutos = parseInt(document.getElementById('s-dur-m').value) || 0;
  const dur     = horas * 60 + minutos;
  if (dur < 1) { 
    alert('Ingresa la duración (horas y/o minutos)'); 
    return; 
  }
  const key  = dk(dayOff), data = loadReg(key);
  data.siestas.push({ hora: horaIni, dur });
  data.siestas.sort((a,b) => a.hora.localeCompare(b.hora));
  saveReg(key, data);
  document.getElementById('s-hora-ini').value = '';
  document.getElementById('s-dur-h').value    = '';
  document.getElementById('s-dur-m').value    = '';
  renderReg();
}

═══════════════════════════════════════════════════════════════
CAMBIO 2 — Editar registro de siesta
═══════════════════════════════════════════════════════════════

A) En renderReg(), en el map() que genera cada fila de siesta,
agrega un botón de editar junto al botón de eliminar:

Reemplaza el return del map() por esto:

return `
  <div class="reg-entry">
    <div class="reg-edet">
      <strong>${rowTexto}</strong>
    </div>
    <div style="display:flex;gap:6px">
      <button class="reg-edel" 
              style="background:none;border:1.5px solid var(--sand);
                     border-radius:7px;color:var(--light);
                     font-size:11px;padding:3px 8px;cursor:pointer"
              onclick="editSiesta('${key}',${i})">✎</button>
      <button class="reg-edel" 
              onclick="delSiesta('${key}',${i})">✕</button>
    </div>
  </div>`;

B) Agrega esta función editSiesta() después de delSiesta():

function editSiesta(key, i) {
  const data  = loadReg(key);
  const s     = data.siestas[i];
  const horas = Math.floor((s.dur || 0) / 60);
  const mins  = (s.dur || 0) % 60;

  // Reutilizar el sistema de overlay existente
  const ov = document.createElement('div');
  ov.className = 'overlay';
  ov.id = 'edit-siesta-ov';
  ov.innerHTML = `
    <div class="overlay-box">
      <div class="ov-title">Editar siesta</div>
      <div class="ov-sub">Corrige la hora de inicio, fin o duración</div>

      <label style="display:block;font-size:12px;font-weight:700;
                    color:var(--mid);margin:14px 0 5px">
        Hora de inicio
      </label>
      <input id="es-hora-ini" type="time" class="ov-input"
             value="${s.hora || ''}">

      <label style="display:block;font-size:12px;font-weight:700;
                    color:var(--mid);margin:10px 0 5px">
        Hora de fin <span style="font-weight:400;color:var(--light)">(opcional)</span>
      </label>
      <input id="es-hora-fin" type="time" class="ov-input"
             value="${s.horaFin || ''}">

      <label style="display:block;font-size:12px;font-weight:700;
                    color:var(--mid);margin:10px 0 5px">
        Duración
      </label>
      <div style="display:flex;gap:8px;align-items:center;
                  margin-bottom:16px">
        <input id="es-dur-h" type="number" class="ov-input"
               placeholder="h" min="0" max="23"
               value="${horas || ''}"
               style="width:70px;text-align:center">
        <span style="color:var(--light);font-weight:700">h</span>
        <input id="es-dur-m" type="number" class="ov-input"
               placeholder="min" min="0" max="59"
               value="${mins || ''}"
               style="width:70px;text-align:center">
        <span style="color:var(--light);font-weight:700">m</span>
      </div>

      <button class="ov-btn" onclick="guardarEditSiesta('${key}',${i})">
        Guardar cambios
      </button>
      <button class="ov-skip" 
              onclick="document.getElementById('edit-siesta-ov')
                       .remove()">
        Cancelar
      </button>
    </div>`;
  document.body.appendChild(ov);
}

function guardarEditSiesta(key, i) {
  const horaIni = document.getElementById('es-hora-ini').value;
  const horaFin = document.getElementById('es-hora-fin').value || null;
  const horas   = parseInt(document.getElementById('es-dur-h').value) || 0;
  const mins    = parseInt(document.getElementById('es-dur-m').value) || 0;
  const dur     = horas * 60 + mins;

  if (!horaIni) { alert('La hora de inicio es obligatoria'); return; }
  if (dur < 1)  { alert('Ingresa una duración válida'); return; }

  const data = loadReg(key);
  data.siestas[i] = { 
    hora: horaIni, 
    horaFin: horaFin || undefined,
    dur 
  };
  data.siestas.sort((a,b) => a.hora.localeCompare(b.hora));
  saveReg(key, data);

  document.getElementById('edit-siesta-ov').remove();
  renderReg();
}

═══════════════════════════════════════════════════════════════
CAMBIO 3 — Alerta de 4 horas en siesta activa
═══════════════════════════════════════════════════════════════

En la función iniciarTimerBanner(), después de actualizar el 
texto del timer, agrega la verificación de 4 horas:

Reemplaza el setInterval completo por esto:

_siestaTimer = setInterval(() => {
  const diff = Math.floor((new Date() - inicio) / 1000);
  const h = Math.floor(diff / 3600);
  const m = Math.floor((diff % 3600) / 60);
  const s = diff % 60;
  const el = document.getElementById('siesta-banner-timer');
  if (el) el.textContent =
    `${h}:${String(m).padStart(2,'0')}:${String(s).padStart(2,'0')}`;

  // Alerta a las 4 horas exactas (14400 segundos)
  // Solo una vez — después de la alerta no vuelve a disparar
  if (diff === 14400) {
    const confirmar = window.confirm(
      '⚠️ La siesta lleva 4 horas.\n\n' +
      '¿Deseas finalizarla ahora?\n\n' +
      'Si el cronómetro quedó corriendo por error, ' +
      'podrás editar la duración después.'
    );
    if (confirmar) finalizarSiesta();
  }
}, 1000);

═══════════════════════════════════════════════════════════════
CAMBIO 4 — SW bump
═══════════════════════════════════════════════════════════════

En sw.js: const CACHE = 'primeros12-v7';

═══════════════════════════════════════════════════════════════
VERIFICACIÓN FINAL
═══════════════════════════════════════════════════════════════

Después de aplicar todo, confirma:
1. store.remove ya no existe en finalizarSiesta() — 
   usa store.del(SIESTA_KEY)
2. addSiesta() lee s-hora-ini, s-dur-h, s-dur-m 
   (no s-hora ni s-dur)
3. editSiesta() y guardarEditSiesta() existen
4. El setInterval tiene el check de diff === 14400
5. CACHE = 'primeros12-v7' en sw.js

NO hagas commit ni push. Yo lo hago después de revisar.