# Prompt para Codex

Pega debajo de esta línea el prompt completo:

---
Hola Claude. Voy a actualizar la sección de Primeros Auxilios 
en mi PWA "Los Primeros 12". Muéstrame cada cambio ANTES de aplicarlo.

═══════════════════════════════════════════════════════════════
CONTEXTO
═══════════════════════════════════════════════════════════════
- La sección está en id="pa"
- Los números actuales están hardcodeados en HTML y en el 
  array PA (en steps y emrg)
- Usaremos una key independiente: panda_emerg_pais ('MX' o 'US')
- El modal de referencia es vac-modal-custom

═══════════════════════════════════════════════════════════════
CAMBIO 1 — CSS: estilos nuevos para emergencias
═══════════════════════════════════════════════════════════════

Agrega estos estilos después del bloque de estilos de .pa-card:

/* ── EMERGENCIAS ── */
.emerg-wrap{margin-top:6px;margin-bottom:16px}
.emerg-pais-bar{display:flex;align-items:center;
  justify-content:space-between;background:var(--sand);
  border-radius:11px;padding:9px 13px;margin-bottom:10px;
  font-size:13px;font-weight:700;color:var(--dark)}
.emerg-change{background:none;border:none;
  font-family:'Nunito',sans-serif;font-size:12px;
  color:var(--deep-sky,#5C7A9A);font-weight:700;
  cursor:pointer;text-decoration:underline;padding:0;
  touch-action:manipulation}
.emerg-selector{background:var(--warm-white);
  border:1.5px solid var(--sand);border-radius:16px;
  padding:24px 20px;text-align:center;margin:10px 0}
.emerg-sel-ttl{font-family:'Playfair Display',Georgia,serif;
  font-size:18px;color:var(--dark);margin-bottom:6px}
.emerg-sel-sub{font-size:12px;color:var(--light);
  margin-bottom:20px;line-height:1.5}
.emerg-sel-btns{display:flex;gap:12px;
  justify-content:center;flex-wrap:wrap}
.emerg-country-btn{display:flex;flex-direction:column;
  align-items:center;gap:5px;background:var(--cream);
  border:2px solid var(--sand);border-radius:14px;
  padding:16px 20px;cursor:pointer;
  touch-action:manipulation;min-width:120px;
  font-family:'Nunito',sans-serif}
.emerg-country-btn:active{border-color:var(--terra);
  background:rgba(212,132,90,.08)}
.emerg-cb-flag{font-size:32px}
.emerg-cb-name{font-size:14px;font-weight:800;color:var(--dark)}
.emerg-grid{display:grid;gap:8px;margin-bottom:10px}
.emerg-card{display:flex;align-items:center;gap:12px;
  background:#FFF0F0;border:1.5px solid #F0BFBF;
  border-radius:13px;padding:12px 14px;
  text-decoration:none}
.emerg-card:active{background:#FFE0E0}
.emerg-card-ico{font-size:22px;flex-shrink:0}
.emerg-card-info{flex:1;min-width:0}
.emerg-card-name{font-size:13px;font-weight:800;
  color:#8B2020;line-height:1.2}
.emerg-card-num{font-size:18px;font-weight:800;
  color:#D42020;letter-spacing:1px;margin-top:2px}
.emerg-card-sub{font-size:10px;color:#8B6060;margin-top:2px}
.emerg-card-call{font-size:18px;flex-shrink:0;color:#D42020}
.emerg-divider{font-size:11px;font-weight:700;
  color:var(--light);letter-spacing:.5px;
  text-transform:uppercase;margin:14px 0 8px;
  padding-left:4px}
.contact-card{display:flex;align-items:center;gap:10px;
  background:var(--warm-white);border:1.5px solid var(--sand);
  border-radius:13px;padding:11px 13px;margin-bottom:8px}
.contact-card-info{flex:1;min-width:0}
.contact-card-name{font-size:13px;font-weight:800;
  color:var(--dark);line-height:1.2}
.contact-card-num{font-size:14px;font-weight:700;
  color:var(--terra);margin-top:2px}
.contact-card-nota{font-size:11px;color:var(--light);
  margin-top:2px;line-height:1.3}
.contact-card-btns{display:flex;gap:6px;flex-shrink:0}
.contact-call-btn{background:var(--sage);color:white;
  border:none;border-radius:9px;padding:8px 11px;
  font-size:16px;cursor:pointer;touch-action:manipulation;
  text-decoration:none;display:flex;align-items:center}
.contact-edit-btn{background:var(--sand);color:var(--dark);
  border:none;border-radius:9px;padding:8px 10px;
  font-size:13px;font-weight:700;cursor:pointer;
  touch-action:manipulation}
.contact-del-btn{background:none;border:1.5px solid var(--sand);
  color:var(--light);border-radius:9px;padding:8px 10px;
  font-size:13px;cursor:pointer;touch-action:manipulation}
.contact-del-btn:active{color:#D45A5A;border-color:#D45A5A}
.emerg-add-btn{width:100%;background:none;
  border:2px dashed var(--sand);border-radius:12px;
  padding:12px;font-family:'Nunito',sans-serif;
  font-size:13px;font-weight:800;color:var(--light);
  cursor:pointer;touch-action:manipulation;margin-top:4px}
.emerg-add-btn:active{background:var(--sand);color:var(--dark)}

═══════════════════════════════════════════════════════════════
CAMBIO 2 — HTML: sección de Primeros Auxilios
═══════════════════════════════════════════════════════════════

Reemplaza el contenido completo del div id="pa" por esto:

<div class="stage" id="pa">
  <div class="pa-calm">
    <span class="pa-e">🫶</span>
    <h3>Antes de leer esto, respira.</h3>
    <p>Esta sección es solo informativa — para que nunca te 
    tome por sorpresa algo que tiene solución. No reemplaza 
    capacitación en primeros auxilios ni atención médica. 
    <strong>Todo va a estar bien.</strong></p>
  </div>

  <!-- Bloque de emergencias (país + contactos) -->
  <div class="emerg-wrap" id="emerg-wrap"></div>

  <!-- Tarjetas de primeros auxilios -->
  <div id="pa-list"></div>
</div>

═══════════════════════════════════════════════════════════════
CAMBIO 3 — JS: datos y lógica de emergencias
═══════════════════════════════════════════════════════════════

Agrega este bloque JS completo DESPUÉS de las funciones 
renderPA() y togglePA(), ANTES de la siguiente sección:

// ── EMERGENCIAS Y CONTACTOS MÉDICOS ─────────────────────────
const EMERG_PAIS_KEY    = 'panda_emerg_pais';
const EMERG_CONTACT_KEY = 'panda_contactos_v1';

const EMERG_MX = [
  { ico:'🚨', nombre:'Emergencias',   num:'911',
    sub:'Policía · Bomberos · Ambulancia', tel:'911' },
  { ico:'🏥', nombre:'Cruz Roja',     num:'065',
    sub:'Urgencias médicas gratuitas', tel:'065' },
  { ico:'🚒', nombre:'Bomberos',      num:'068',
    sub:'Incendios y rescates', tel:'068' },
  { ico:'👨‍⚕️', nombre:'IMSS Bienestar',num:'800 623 2300',
    sub:'Orientación médica 24/7', tel:'8006232300' },
  { ico:'💙', nombre:'Línea de la Vida',num:'800 911 2000',
    sub:'Crisis y orientación emocional', tel:'8009112000' },
];

const EMERG_US = [
  { ico:'🚨', nombre:'Emergencias',   num:'911',
    sub:'Police · Fire · Ambulance', tel:'911' },
  { ico:'☠️', nombre:'Poison Control', num:'1-800-222-1222',
    sub:'Intoxicaciones, 24/7', tel:'18002221222' },
  { ico:'👶', nombre:'Nurse Line',    num:'1-800-464-4000',
    sub:'Kaiser 24h (verifica tu seguro)', tel:'18004644000' },
];

function loadContactos() {
  try {
    const raw = store.get(EMERG_CONTACT_KEY);
    return raw ? JSON.parse(raw) : [];
  } catch(e) { return []; }
}

function saveContactos(arr) {
  store.set(EMERG_CONTACT_KEY, JSON.stringify(arr));
}

function renderEmergencias() {
  const wrap = document.getElementById('emerg-wrap');
  if (!wrap) return;
  const pais = store.get(EMERG_PAIS_KEY);

  if (!pais) {
    // Mostrar selector de país
    wrap.innerHTML = `
      <div class="emerg-selector">
        <div class="emerg-sel-ttl">¿Dónde estás?</div>
        <div class="emerg-sel-sub">
          Los números de emergencia varían según el país.<br>
          Puedes cambiarlo en cualquier momento.
        </div>
        <div class="emerg-sel-btns">
          <button class="emerg-country-btn" 
                  onclick="elegirPaisEmerg('MX')">
            <span class="emerg-cb-flag">🇲🇽</span>
            <span class="emerg-cb-name">México</span>
          </button>
          <button class="emerg-country-btn" 
                  onclick="elegirPaisEmerg('US')">
            <span class="emerg-cb-flag">🇺🇸</span>
            <span class="emerg-cb-name">Estados Unidos</span>
          </button>
        </div>
      </div>`;
    return;
  }

  const lista = pais === 'MX' ? EMERG_MX : EMERG_US;
  const bandera = pais === 'MX' ? '🇲🇽 México' : '🇺🇸 Estados Unidos';
  const contactos = loadContactos();

  const cardsHTML = lista.map(e => `
    <a class="emerg-card" href="tel:${e.tel}">
      <span class="emerg-card-ico">${e.ico}</span>
      <div class="emerg-card-info">
        <div class="emerg-card-name">${e.nombre}</div>
        <div class="emerg-card-num">${e.num}</div>
        <div class="emerg-card-sub">${e.sub}</div>
      </div>
      <span class="emerg-card-call">📞</span>
    </a>`).join('');

  const contactosHTML = contactos.length
    ? contactos.map((c,i) => `
        <div class="contact-card">
          <div class="contact-card-info">
            <div class="contact-card-name">${c.nombre}</div>
            <div class="contact-card-num">${c.tel}</div>
            ${c.nota 
              ? `<div class="contact-card-nota">${c.nota}</div>` 
              : ''}
          </div>
          <div class="contact-card-btns">
            <a class="contact-call-btn" href="tel:${c.tel}">📞</a>
            <button class="contact-edit-btn" 
                    onclick="editContacto(${i})">✏️</button>
            <button class="contact-del-btn" 
                    onclick="delContacto(${i})">✕</button>
          </div>
        </div>`).join('')
    : `<div style="font-size:13px;color:var(--light);
                   font-style:italic;padding:8px 4px">
         Sin contactos guardados
       </div>`;

  wrap.innerHTML = `
    <div class="emerg-pais-bar">
      <span>${bandera}</span>
      <button class="emerg-change" 
              onclick="cambiarPaisEmerg()">Cambiar país</button>
    </div>
    <div class="emerg-grid">${cardsHTML}</div>
    <div class="emerg-divider">Mis contactos médicos</div>
    ${contactosHTML}
    <button class="emerg-add-btn" 
            onclick="abrirModalContacto()">
      ＋ Agregar contacto médico
    </button>`;
}

function elegirPaisEmerg(pais) {
  store.set(EMERG_PAIS_KEY, pais);
  renderEmergencias();
}

function cambiarPaisEmerg() {
  store.del(EMERG_PAIS_KEY);
  renderEmergencias();
}

function abrirModalContacto(idx = null) {
  const contactos = loadContactos();
  const c = idx !== null ? contactos[idx] : null;

  const ov = document.createElement('div');
  ov.className = 'overlay';
  ov.id = 'contacto-ov';
  ov.onclick = e => { 
    if (e.target === ov) ov.remove(); 
  };
  ov.innerHTML = `
    <div class="overlay-box" onclick="event.stopPropagation()">
      <div class="ov-title">
        ${c ? 'Editar contacto' : 'Agregar contacto médico'}
      </div>
      <div class="ov-sub">
        Pediatra, médico familiar, enfermera, etc.
      </div>
      <input id="ct-nombre" class="ov-input" type="text"
             placeholder="Nombre y especialidad *"
             value="${c ? c.nombre : ''}">
      <input id="ct-tel" class="ov-input" type="tel"
             placeholder="Teléfono *"
             value="${c ? c.tel : ''}">
      <input id="ct-nota" class="ov-input" type="text"
             placeholder="Notas (consultorio, horario...)"
             value="${c ? (c.nota || '') : ''}">
      <button class="ov-btn" 
              onclick="guardarContacto(${idx})">
        ${c ? 'Guardar cambios' : 'Agregar'}
      </button>
      <button class="ov-skip" 
              onclick="document.getElementById('contacto-ov')
                       .remove()">
        Cancelar
      </button>
    </div>`;
  document.body.appendChild(ov);
}

function editContacto(i) {
  abrirModalContacto(i);
}

function guardarContacto(idx) {
  const nombre = document.getElementById('ct-nombre').value.trim();
  const tel    = document.getElementById('ct-tel').value.trim();
  const nota   = document.getElementById('ct-nota').value.trim();

  if (!nombre) { alert('El nombre es obligatorio'); return; }
  if (!tel)    { alert('El teléfono es obligatorio'); return; }

  const contactos = loadContactos();
  const entry = { nombre, tel, nota };

  if (idx !== null && idx >= 0) {
    contactos[idx] = entry;
  } else {
    contactos.push(entry);
  }

  saveContactos(contactos);
  document.getElementById('contacto-ov').remove();
  renderEmergencias();
}

function delContacto(i) {
  if (!confirm('¿Eliminar este contacto?')) return;
  const contactos = loadContactos();
  contactos.splice(i, 1);
  saveContactos(contactos);
  renderEmergencias();
}
// ─────────────────────────────────────────────────────────────

═══════════════════════════════════════════════════════════════
CAMBIO 4 — JS: actualizar array PA para México
═══════════════════════════════════════════════════════════════

En el array PA, los textos de emrg y steps mencionan 
"1-800-222-1222" y "EUA" de forma hardcodeada.

Actualiza cada mención a una versión neutral que funcione 
para ambos países. Reemplaza:

a) En el objeto de Atragantamiento, emrg:
   "llama al 911 inmediatamente"
   → "llama al número de emergencias inmediatamente"

b) En el objeto de Fiebre, emrg:
   "Fiebre + convulsiones... : 911."
   → "Fiebre + convulsiones... : llama a emergencias."

c) En el objeto de Caída, emrg:
   "Llama al 911 si:"
   → "Llama a emergencias si:"

d) En el objeto de Nariz tapada, emrg:
   "Llama al médico si:"
   → sin cambio (ya es neutro)

e) En el objeto de Ingesta accidental:
   - En steps: "Llama a Poison Control: 1-800-222-1222 (EUA)."
     → "Llama al centro de control de intoxicaciones de tu país."
   - En emrg: "911 primero, Poison Control después."
     → "Llama a emergencias primero, 
        luego al centro de intoxicaciones de tu país."

═══════════════════════════════════════════════════════════════
CAMBIO 5 — JS: llamar renderEmergencias() al abrir la pestaña
═══════════════════════════════════════════════════════════════

Busca el bloque donde se llama renderPA() al cambiar de tab:
  if (id==='pa') renderPA();

Reemplázalo por:
  if (id==='pa') { renderPA(); renderEmergencias(); }

═══════════════════════════════════════════════════════════════
CAMBIO 6 — JS: quitar bloque hardcodeado de "Emergencias USA"
═══════════════════════════════════════════════════════════════

En el HTML de id="pa", ya reemplazamos todo el contenido 
en el CAMBIO 2. Confirma que el div hardcodeado de 
"Emergencias USA" con el 911 y el 1-800-222-1222 ya 
no existe en el HTML — fue reemplazado por 
<div class="emerg-wrap" id="emerg-wrap"></div>

═══════════════════════════════════════════════════════════════
CAMBIO 7 — SW bump
═══════════════════════════════════════════════════════════════

En sw.js: const CACHE = 'primeros12-v9';

═══════════════════════════════════════════════════════════════
VERIFICACIÓN FINAL
═══════════════════════════════════════════════════════════════

Después de aplicar todo, confirma:
1. renderEmergencias() existe y se llama al abrir tab 'pa'
2. EMERG_MX tiene 5 entradas, EMERG_US tiene 3
3. guardarContacto(), delContacto(), editContacto() existen
4. El HTML de id="pa" ya NO contiene "1-800-222-1222" 
   hardcodeado ni el div de "Emergencias USA"
5. CACHE = 'primeros12-v9' en sw.js

NO hagas commit ni push todavía.