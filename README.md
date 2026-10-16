<!DOCTYPE html>
<html lang="es"><head><meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Libreta: organización escolar</title>
<link href="https://fonts.googleapis.com/css2?family=Bricolage+Grotesque:wght@600;800&family=Nunito+Sans:wght@400;600;700&display=swap" rel="stylesheet">
<style>
:root{--bg:#F6F8FB;--sf:#fff;--ink:#14233B;--mu:#5B6B82;--ln:#D9E1EC;--ac:#2F7FC1;--acs:#DCEBF8;--sol:#F4B63F;--red:#C8433F;color-scheme:light;box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
@media (prefers-color-scheme:dark){:root:not([data-theme="light"]){--bg:#0F1A2B;--sf:#17263B;--ink:#EAF0F8;--mu:#9BAAC0;--ln:#2A3C57;--ac:#6FB1E4;--acs:#1F3550;--red:#EE7A75;color-scheme:dark}}
:root[data-theme="dark"]{--bg:#0F1A2B;--sf:#17263B;--ink:#EAF0F8;--mu:#9BAAC0;--ln:#2A3C57;--ac:#6FB1E4;--acs:#1F3550;--red:#EE7A75;color-scheme:dark}
*,*::before,*::after{box-sizing:inherit}
html{scroll-padding-top:env(safe-area-inset-top,0px)}
body{margin:0;background:var(--bg);color:var(--ink);font:400 16px/1.45 "Nunito Sans",system-ui,sans-serif;padding-bottom:84px}
main{max-width:520px;margin:0 auto;padding:18px 16px}
h1,h2{font-family:"Bricolage Grotesque",system-ui,sans-serif;margin:0}
h1{font-size:34px;font-weight:800;letter-spacing:-.02em;line-height:1}
h2{font-size:20px;font-weight:600;margin:22px 0 10px}
.sub{color:var(--mu);margin:6px 0 14px}
.rol{display:flex;gap:4px;background:var(--acs);padding:4px;border-radius:12px;margin-bottom:6px}
.rol button{flex:1;border:0;background:none;color:var(--ink);font:600 15px inherit;font-family:inherit;padding:10px;border-radius:9px;cursor:pointer}
.rol button[aria-pressed="true"]{background:var(--sf);box-shadow:0 1px 3px rgba(20,35,59,.18)}
.mod{display:grid;grid-template-columns:54px 1fr;gap:10px;padding:12px 0;border-bottom:1px solid var(--ln)}
.mod .h{color:var(--mu);font-weight:600;font-variant-numeric:tabular-nums}
.mod b{font-weight:700}
.warn{display:block;color:var(--red);font-weight:600;font-size:14px;margin-top:2px}
.card{background:var(--sf);border:1px solid var(--ln);border-radius:12px;padding:12px 14px;margin-bottom:10px}
.card.sol{border-left:5px solid var(--sol)}
.card p{margin:4px 0 0}
.meta{color:var(--mu);font-size:14px}
.tarea{display:flex;gap:12px;align-items:flex-start}
.tarea input{width:24px;height:24px;margin:2px 0 0;accent-color:var(--ac);flex:none}
.tarea.ok .t{text-decoration:line-through;color:var(--mu)}
.due{font-weight:700;color:var(--ac)}
.due.late{color:var(--red)}
label{display:block;font-weight:600;margin:12px 0 4px;font-size:15px}
input[type=text],input[type=date],select,textarea{width:100%;font:inherit;color:var(--ink);background:var(--sf);border:1px solid var(--ln);border-radius:10px;padding:10px 12px}
textarea{min-height:84px;resize:vertical}
.seg{display:flex;gap:6px;flex-wrap:wrap}
.seg button{border:1px solid var(--ln);background:var(--sf);color:var(--ink);font:600 14px inherit;font-family:inherit;padding:8px 14px;border-radius:99px;cursor:pointer}
.seg button[aria-pressed="true"]{background:var(--ac);border-color:var(--ac);color:#fff}
.go{width:100%;margin-top:16px;border:0;background:var(--ac);color:#fff;font:700 17px inherit;font-family:inherit;padding:14px;border-radius:12px;cursor:pointer}
.msg{margin-top:12px;color:var(--ac);font-weight:700}
.empty{color:var(--mu);padding:14px 0}
nav{position:fixed;left:0;right:0;bottom:0;display:flex;background:var(--sf);border-top:1px solid var(--ln);padding-bottom:env(safe-area-inset-bottom,0px)}
nav button{flex:1;border:0;background:none;color:var(--mu);font:600 14px inherit;font-family:inherit;padding:16px 4px;cursor:pointer;border-top:3px solid transparent}
nav button[aria-pressed="true"]{color:var(--ac);border-top-color:var(--ac)}
button:focus-visible,input:focus-visible,select:focus-visible,textarea:focus-visible{outline:3px solid var(--sol);outline-offset:2px}
</style></head>
<body><main id="app"></main><nav id="nav"></nav>
<script>
const CUR=['1° A','3° B','5° C'],MY='3° B';
const MAT=['Matemática','Lengua','Historia','Geografía','Biología','Física','Inglés','Tecnología','Educación Física','Plástica','Formación Ética'];
const H={1:['Matemática','Lengua','Historia','Educación Física','Inglés'],2:['Biología','Matemática','Geografía','Lengua','Plástica'],3:['Física','Inglés','Matemática','Historia','Tecnología'],4:['Lengua','Biología','Educación Física','Geografía','Matemática'],5:['Inglés','Física','Lengua','Tecnología','Formación Ética']};
const DIAS=['domingo','lunes','martes','miércoles','jueves','viernes','sábado'];
const T=['7:30','8:10','8:50','9:50','10:30'];
const iso=n=>{const d=new Date();d.setDate(d.getDate()+n);return d.toISOString().slice(0,10)};
const fmt=s=>new Date(s+'T12:00').toLocaleDateString('es-AR',{weekday:'short',day:'numeric',month:'short'});
const esc=s=>String(s).replace(/[&<>"]/g,c=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;'}[c]));
const g=new Date().getDay(),DI=(g==0||g==6)?1:g;
let S={rol:'a',tab:{a:'hoy',d:'pub'},form:'aviso',msg:'',id:10,
tareas:[{id:1,curso:MY,mat:'Matemática',txt:'Ejercicios 4 al 9 de la guía de funciones',f:iso(1),ok:false},{id:2,curso:MY,mat:'Lengua',txt:'Leer los capítulos 3 y 4 del cuento',f:iso(3),ok:false},{id:3,curso:MY,mat:'Biología',txt:'Informe del laboratorio de células',f:iso(6),ok:false},{id:4,curso:'1° A',mat:'Historia',txt:'Línea de tiempo de la Revolución de Mayo',f:iso(2),ok:false}],
avisos:[{id:1,curso:'Toda la escuela',tit:'Acto del 17 de octubre',txt:'Los alumnos del turno mañana ingresan a las 7:15. Uniforme completo.',q:'Dirección',h:'Hoy'},{id:2,curso:MY,tit:'Reunión de padres',txt:'Jueves a las 18 h en el SUM. Se entregan las notas del segundo trimestre.',q:'Preceptoría',h:'Ayer'}],
cambios:[{curso:MY,dia:DI,mod:1,txt:'Sin profesor: módulo libre'}]};
const $=id=>document.getElementById(id);
function nav(){const t=S.rol=='a'?[['hoy','Hoy'],['tar','Tareas'],['avi','Avisos']]:[['pub','Publicar'],['env','Enviado']];
$('nav').innerHTML=t.map(([k,n])=>`<button data-tab="${k}" aria-pressed="${S.tab[S.rol]==k}">${n}</button>`).join('')}
function head(){return `<h1>Libreta</h1><p class="sub">${S.rol=='a'?'3° B · turno mañana':'Escuela de ejemplo · docentes y directivos'}</p>
<div class="rol"><button data-rol="a" aria-pressed="${S.rol=='a'}">Soy alumno</button><button data-rol="d" aria-pressed="${S.rol=='d'}">Soy docente o directivo</button></div>`}
function pend(){return S.tareas.filter(t=>t.curso==MY&&!t.ok).sort((a,b)=>a.f<b.f?-1:1)}
function tcard(t,all){const late=t.f<iso(0);return `<div class="card tarea ${t.ok?'ok':''}">${all?'':`<input type="checkbox" data-chk="${t.id}" ${t.ok?'checked':''} aria-label="Marcar como hecha">`}<div><div class="t"><b>${esc(t.mat)}</b>${all?' · '+t.curso:''}: ${esc(t.txt)}</div><div class="meta">Entrega <span class="due ${late&&!t.ok?'late':''}">${fmt(t.f)}</span></div></div></div>`}
function vHoy(){const cs=S.cambios.filter(c=>c.curso==MY&&c.dia==DI);
const rows=H[DI].map((m,i)=>{const c=cs.find(x=>x.mod==i);return `<div class="mod"><span class="h">${T[i]}</span><div><b>${m}</b>${c?`<span class="warn">${esc(c.txt)}</span>`:''}</div></div>`}).join('');
const p=pend().slice(0,3);
return `<h2>${g==0||g==6?'Lunes, tu próximo día de clase':'Hoy, '+DIAS[DI]}</h2>${rows}<h2>Para entregar pronto</h2>${p.length?p.map(t=>tcard(t,false)).join(''):'<p class="empty">No tenés entregas pendientes.</p>'}`}
function vTar(){const l=S.tareas.filter(t=>t.curso==MY).sort((a,b)=>a.f<b.f?-1:1);return `<h2>Tareas de ${MY}</h2>${l.length?l.map(t=>tcard(t,false)).join(''):'<p class="empty">Todavía no hay tareas cargadas.</p>'}`}
function vAvi(){const l=S.avisos.filter(a=>a.curso==MY||a.curso=='Toda la escuela');return `<h2>Avisos de la escuela</h2>${l.length?l.map(a=>`<div class="card sol"><b>${esc(a.tit)}</b><p>${esc(a.txt)}</p><div class="meta">${esc(a.q)} · ${esc(a.curso)} · ${a.h}</div></div>`).join(''):'<p class="empty">No hay avisos nuevos.</p>'}`}
function opts(a){return a.map(x=>`<option>${x}</option>`).join('')}
function vPub(){const f=S.form;let b='';
if(f=='aviso')b=`<label for="cu">Para</label><select id="cu">${opts(['Toda la escuela',...CUR])}</select><label for="ti">Título</label><input type="text" id="ti"><label for="tx">Mensaje</label><textarea id="tx"></textarea>`;
if(f=='tarea')b=`<label for="cu">Curso</label><select id="cu">${opts(CUR)}</select><label for="ma">Materia</label><select id="ma">${opts(MAT)}</select><label for="tx">Consigna</label><textarea id="tx"></textarea><label for="fe">Fecha de entrega</label><input type="date" id="fe" value="${iso(2)}">`;
if(f=='cambio')b=`<label for="cu">Curso</label><select id="cu">${opts(CUR)}</select><label for="mo">Módulo de hoy</label><select id="mo">${T.map((t,i)=>`<option value="${i}">${t} · ${H[DI][i]}</option>`).join('')}</select><label for="tx">Qué cambia</label><input type="text" id="tx" placeholder="Ej.: Sin profesor, módulo libre">`;
return `<h2>Publicar</h2><div class="seg">${[['aviso','Aviso'],['tarea','Tarea'],['cambio','Cambio de horario']].map(([k,n])=>`<button data-form="${k}" aria-pressed="${f==k}">${n}</button>`).join('')}</div>${b}<button class="go" data-pub="1">Publicar</button>${S.msg?`<p class="msg" role="status">${S.msg}</p>`:''}`}
function vEnv(){return `<h2>Avisos publicados</h2>${S.avisos.map(a=>`<div class="card sol"><b>${esc(a.tit)}</b><p>${esc(a.txt)}</p><div class="meta">${esc(a.q)} · ${esc(a.curso)} · ${a.h}</div></div>`).join('')}<h2>Tareas publicadas</h2>${S.tareas.map(t=>tcard(t,true)).join('')}`}
function draw(){const v={a:{hoy:vHoy,tar:vTar,avi:vAvi},d:{pub:vPub,env:vEnv}}[S.rol][S.tab[S.rol]];$('app').innerHTML=head()+v();nav()}
function pub(){const v=id=>($(id)||{}).value||'',tx=v('tx').trim(),cu=v('cu');
if(!tx){S.msg='Escribí el mensaje antes de publicar.';return draw()}
if(S.form=='aviso'){S.avisos.unshift({id:S.id++,curso:cu,tit:v('ti').trim()||'Aviso',txt:tx,q:'Dirección',h:'Ahora'})}
if(S.form=='tarea'){S.tareas.push({id:S.id++,curso:cu,mat:v('ma'),txt:tx,f:v('fe')||iso(2),ok:false})}
if(S.form=='cambio'){S.cambios.push({curso:cu,dia:DI,mod:+v('mo'),txt:tx})}
S.msg='Publicado. Los alumnos de '+(cu=='Toda la escuela'?'toda la escuela':cu)+' ya lo ven.';draw()}
document.addEventListener('click',e=>{const b=e.target.closest('[data-rol],[data-tab],[data-form],[data-pub],[data-chk]');if(!b)return;
if(b.dataset.rol){S.rol=b.dataset.rol;S.msg=''}
else if(b.dataset.tab){S.tab[S.rol]=b.dataset.tab;S.msg=''}
else if(b.dataset.form){S.form=b.dataset.form;S.msg=''}
else if(b.dataset.pub)return pub();
else if(b.dataset.chk){const t=S.tareas.find(x=>x.id==b.dataset.chk);t.ok=b.checked}
draw()});
draw();
</script></body></html>
