<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>سينيما بلس</title>
<style>
:root{--bg:#0e0b12;--fg:#f4eef6;--mut:#a79bb0;--ac:#e8b04a;--card:#1a1522;box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
@media (prefers-color-scheme:light){:root:not([data-theme="dark"]){--bg:#0e0b12}}
*{box-sizing:border-box;margin:0}
html{height:100%}
body{background:var(--bg);color:var(--fg);font-family:"Tajawal","Segoe UI",Tahoma,sans-serif;line-height:1.6}
header{position:sticky;top:env(safe-area-inset-top,0px);z-index:5;display:flex;align-items:center;gap:12px;padding:12px 4%;background:rgba(14,11,18,.92)}
.logo{font-size:22px;font-weight:800;color:var(--ac)}
header input{flex:1;max-width:320px;margin-inline-start:auto;background:var(--card);border:1px solid #2e2538;border-radius:8px;color:var(--fg);padding:8px 12px;font:inherit}
header input:focus,button:focus-visible,.card:focus-visible{outline:2px solid var(--ac)}
.hero{min-height:56vh;display:flex;flex-direction:column;justify-content:flex-end;gap:10px;padding:40px 4%;background:linear-gradient(0deg,var(--bg) 4%,transparent 60%),linear-gradient(135deg,#5a1f3a,#1d2a52 60%,#0e0b12)}
.hero h1{font-size:clamp(32px,7vw,60px);font-weight:800}
.hero p{max-width:520px;color:var(--mut)}
.btns{display:flex;gap:10px;margin-top:6px}
button{font:inherit;border:0;border-radius:8px;padding:10px 20px;cursor:pointer;font-weight:700}
.play{background:var(--ac);color:#1a1205}
.info{background:#ffffff22;color:var(--fg)}
.cats{display:flex;gap:8px;padding:16px 4% 0;overflow-x:auto}
.cats button{background:var(--card);color:var(--mut);white-space:nowrap}
.cats button.on{background:var(--ac);color:#1a1205}
section{padding:18px 0 4px}
section h2{font-size:19px;padding:0 4% 10px}
.row{display:flex;gap:12px;overflow-x:auto;padding:0 4% 8px;scroll-snap-type:x proximity}
.card{flex:0 0 140px;height:210px;border-radius:10px;padding:10px;display:flex;flex-direction:column;justify-content:flex-end;cursor:pointer;scroll-snap-align:start;transition:transform .15s}
.card:hover{transform:scale(1.05)}
.card b{font-size:15px;line-height:1.3}
.card small{color:#ffffffcc}
.modal{position:fixed;inset:0;background:#000b;display:none;align-items:center;justify-content:center;padding:20px;z-index:10}
.modal.open{display:flex}
.box{background:var(--card);border-radius:14px;max-width:460px;width:100%;overflow:hidden}
.pv{height:200px;display:flex;align-items:center;justify-content:center;font-size:56px}
.tx{padding:18px;display:grid;gap:8px}
.tx span{color:var(--mut);font-size:14px}
footer{padding:30px 4%;color:var(--mut);font-size:13px;text-align:center}
</style>
</head>
<body>
<header><div class="logo">سينيما بلس</div><input id="q" placeholder="ابحث عن فيلم أو نوع" aria-label="بحث"></header>
<div class="hero">
<h1 id="ht"></h1><p id="hd"></p>
<div class="btns"><button class="play" id="hp">▶ شاهد الآن</button><button class="info" id="hi">معلومات</button></div>
</div>
<nav class="cats" id="cats"></nav>
<main id="rows"></main>
<footer>سينيما بلس — موقع تجريبي. الأفلام والبوسترات خيالية.</footer>
<div class="modal" id="m"><div class="box"><div class="pv" id="mp"></div><div class="tx"><h3 id="mt"></h3><span id="mm"></span><p id="md"></p><div class="btns"><button class="play">▶ تشغيل</button><button class="info" id="mc">إغلاق</button></div></div></div></div>
<script>
const G=[["#7b2d26","#d9822b"],["#1f3b73","#5aa9e6"],["#2d5a3d","#9bd17a"],["#5b2a86","#e07ab5"],["#3a3a3a","#c9a227"],["#0f5257","#6fd3c6"]];
const F=[
["ظل الصحراء","أكشن","2025","2س 10د","مطاردة مثيرة عبر كثبان الصحراء المغربية."],
["ليلة مراكش","دراما","2024","1س 55د","قصة حب وأسرار في أزقة المدينة القديمة."],
["الأخير على المريخ","خيال علمي","2026","2س 20د","رائد فضاء وحيد يواجه المجهول."],
["ضحكة الحومة","كوميديا","2023","1س 40د","جيران وحكايات وفوضى لا تنتهي."],
["صرخة الغابة","رعب","2025","1س 35د","ليلة تخييم تتحول إلى كابوس."],
["رحلة الأطلس","مغامرة","2024","2س 00د","أصدقاء يبحثون عن كنز في الجبال."],
["حارس الفنار","دراما","2022","1س 50د","رجل وحيد وسر قديم على شاطئ بعيد."],
["مدينة النيون","أكشن","2026","2س 05د","محقق في مدينة لا تنام."],
["سر الواحة","مغامرة","2023","1س 45د","خريطة ممزقة وثلاث عائلات."],
["نجوم الغد","خيال علمي","2025","2س 15د","سفينة تبحث عن وطن جديد."],
["عرس في الدار","كوميديا","2024","1س 38د","تحضيرات عرس تنقلب رأساً على عقب."],
["الغرفة 13","رعب","2026","1س 42د","فندق قديم لا يغادره أحد."]
].map((a,i)=>({t:a[0],g:a[1],y:a[2],d:a[3],s:a[4],c:G[i%6]}));
const cats=["الكل",...new Set(F.map(f=>f.g))];let cur="الكل",qv="";
const $=id=>document.getElementById(id);
const bg=f=>`linear-gradient(160deg,${f.c[0]},${f.c[1]})`;
function open_(f){$("mp").style.background=bg(f);$("mp").textContent="▶";$("mt").textContent=f.t;$("mm").textContent=f.g+" • "+f.y+" • "+f.d;$("md").textContent=f.s;$("m").classList.add("open")}
function render(){
const list=F.filter(f=>(cur=="الكل"||f.g==cur)&&(f.t+f.g).includes(qv));
const groups=cur=="الكل"&&!qv?[["الأكثر مشاهدة",list.slice(0,8)],["أضيف حديثاً",list.slice().reverse().slice(0,8)]]:[["النتائج",list]];
$("rows").innerHTML="";
groups.forEach(([n,l])=>{const s=document.createElement("section");s.innerHTML=`<h2>${n}</h2><div class="row"></div>`;
const r=s.querySelector(".row");
if(!l.length)r.textContent="لا توجد نتائج. جرّب كلمة أخرى.";
l.forEach(f=>{const c=document.createElement("div");c.className="card";c.tabIndex=0;c.style.background=bg(f);c.innerHTML=`<b>${f.t}</b><small>${f.g} • ${f.y}</small>`;c.onclick=()=>open_(f);c.onkeydown=e=>{if(e.key=="Enter")open_(f)};r.appendChild(c)});
$("rows").appendChild(s)});
$("cats").innerHTML="";cats.forEach(c=>{const b=document.createElement("button");b.textContent=c;if(c==cur)b.className="on";b.onclick=()=>{cur=c;render()};$("cats").appendChild(b)})}
const H=F[0];$("ht").textContent=H.t;$("hd").textContent=H.s;
$("hp").onclick=$("hi").onclick=()=>open_(H);
$("mc").onclick=()=>$("m").classList.remove("open");
$("m").onclick=e=>{if(e.target.id=="m")$("m").classList.remove("open")};
$("q").oninput=e=>{qv=e.target.value.trim();render()};
render();
</script>
</body>
</html>
