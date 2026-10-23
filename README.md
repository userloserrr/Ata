<!DOCTYPE html>
<html lang="tr">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Ata</title>
<style>
:root{--bg:#f6f4ef;--sf:#fff;--tx:#1d1b16;--mu:#7a756a;--ac:#b4472b;--ac2:#fff;--bd:#e3ded2;--ub:#b4472b;--ut:#fff;color-scheme:light;box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
@media (prefers-color-scheme:dark){:root:not([data-theme="light"]){--bg:#16140f;--sf:#221f18;--tx:#f1ede3;--mu:#a29b8a;--ac:#e0704f;--ac2:#16140f;--bd:#363124;--ub:#e0704f;--ut:#16140f;color-scheme:dark}}
:root[data-theme="dark"]{--bg:#16140f;--sf:#221f18;--tx:#f1ede3;--mu:#a29b8a;--ac:#e0704f;--ac2:#16140f;--bd:#363124;--ub:#e0704f;--ut:#16140f;color-scheme:dark}
*{box-sizing:border-box}
html,body{height:100%;margin:0}
body{background:var(--bg);color:var(--tx);font:16px/1.45 system-ui,-apple-system,"Segoe UI",sans-serif;display:flex;flex-direction:column}
header{display:flex;align-items:center;gap:10px;padding:12px 16px;border-bottom:1px solid var(--bd)}
header h1{font-size:20px;margin:0;flex:1;letter-spacing:.5px}
header small{color:var(--mu)}
.chip{border:1px solid var(--bd);background:var(--sf);color:var(--tx);border-radius:999px;padding:6px 12px;font-size:13px;cursor:pointer}
.chip.on{background:var(--ac);color:var(--ac2);border-color:var(--ac)}
#log{flex:1;overflow-y:auto;padding:16px;display:flex;flex-direction:column;gap:12px}
.m{max-width:86%;padding:10px 14px;border-radius:16px;white-space:pre-wrap;word-wrap:break-word;background:var(--sf);border:1px solid var(--bd)}
.m.u{align-self:flex-end;background:var(--ub);color:var(--ut);border-color:transparent}
.m img{display:block;max-width:100%;border-radius:10px;margin-top:6px}
.m.hint{align-self:center;background:none;border:none;color:var(--mu);text-align:center;font-size:14px}
#bar{padding:10px 12px;border-top:1px solid var(--bd);background:var(--sf)}
#prev{display:none;margin-bottom:8px}
#prev img{height:56px;border-radius:8px;vertical-align:middle}
#prev button{margin-left:8px}
.row{display:flex;gap:8px;align-items:flex-end}
textarea{flex:1;resize:none;max-height:120px;border:1px solid var(--bd);background:var(--bg);color:var(--tx);border-radius:14px;padding:10px 12px;font:inherit}
.ib{width:44px;height:44px;border-radius:50%;border:1px solid var(--bd);background:var(--bg);color:var(--tx);font-size:19px;cursor:pointer;flex:none}
.ib.go{background:var(--ac);color:var(--ac2);border-color:var(--ac)}
.ib.rec{background:#c0392b;color:#fff;border-color:#c0392b}
.mb{flex:1;overflow:auto;padding:14px;display:flex;flex-direction:column;gap:12px}.row2{display:flex;gap:8px;flex-wrap:wrap;align-items:center}.mb label{font-size:14px;color:var(--mu);display:flex;gap:8px;align-items:center}
canvas{width:100%;border:1px solid var(--bd);border-radius:10px}#thumbs{display:flex;gap:8px;flex-wrap:wrap}#thumbs div{position:relative}#thumbs img{height:48px;border-radius:6px;display:block}#thumbs b{position:absolute;top:-6px;right:-6px;background:var(--ac);color:var(--ac2);border-radius:50%;width:18px;height:18px;font-size:13px;text-align:center;line-height:18px;cursor:pointer}
#prev img{margin-right:4px}
header{flex-wrap:wrap;gap:8px}header h1{min-width:90px}
#vthumbs{display:flex;gap:8px;flex-wrap:wrap}#vthumbs div{position:relative}#vthumbs img{height:48px;border-radius:6px;display:block}#vthumbs b{position:absolute;top:-6px;right:-6px;background:var(--ac);color:var(--ac2);border-radius:50%;width:18px;height:18px;font-size:13px;text-align:center;line-height:18px;cursor:pointer}
.mb select,.mb input[type=text]{border:1px solid var(--bd);background:var(--sf);color:var(--tx);border-radius:10px;padding:8px 10px;font:inherit;font-size:14px}
</style>
</head>
<body>
<header><h1>Ata <small>asistanın</small></h1><button class="chip" id="tts">🔈 Sesli yanıt</button><button class="chip" id="draw">🎨 Çizim</button><button class="chip" id="mont">🖼 Montaj</button><button class="chip" id="vid">🎬 Video</button></header>
<div id="log"><div class="m hint">Merhaba, ben Ata. Yazabilir, 🎤 ile konuşabilir, 📎 ile görsel ekleyebilirsin.<br>“Çiz: gün batımında bir kale” dersen görsel oluştururum.<br>Görsel ekleyip “video yap” dersen videonu hazırlarım.</div></div>
<div id="bar">
<div id="prev"><span id="pi"></span><button class="chip" id="px">Hepsini kaldır</button></div>
<div class="row">
<button class="ib" id="att" aria-label="Görsel ekle">📎</button>
<button class="ib" id="mic" aria-label="Konuş">🎤</button>
<textarea id="in" rows="1" placeholder="Ata’ya yaz…"></textarea>
<button class="ib go" id="send" aria-label="Gönder">➤</button>
</div>
<input type="file" id="file" accept="image/*" multiple hidden>
</div>
<div id="mp" style="display:none;position:fixed;inset:0;z-index:20;background:var(--bg);flex-direction:column;box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)">
<header><h1>🖼 Montaj</h1><button class="chip" id="mx">Kapat</button></header>
<div class="mb">
<div class="row2"><button class="chip" id="madd">➕ Görsel ekle</button><button class="chip" id="mclr">Temizle</button></div>
<div class="row2" id="lay"></div>
<div class="row2"><label>Boşluk <input type="range" id="gap" min="0" max="40" value="10"></label><label>Arka plan <input type="color" id="bgc" value="#ffffff"></label></div>
<div id="thumbs"></div>
<canvas id="cv" width="1200" height="300"></canvas>
<div class="row2"><button class="chip on" id="msend">💬 Sohbete ekle</button><button class="chip" id="mdl">⬇ İndir</button></div>
</div>
<input type="file" id="mfile" accept="image/*" multiple hidden>
</div>
<div id="vp" style="display:none;position:fixed;inset:0;z-index:30;background:var(--bg);flex-direction:column;box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)">
<header><h1>🎬 Video</h1><button class="chip" id="vx">Kapat</button></header>
<div class="mb">
<div class="row2"><button class="chip" id="vadd">➕ Görsel ekle</button><button class="chip" id="vclr">Temizle</button></div>
<div id="vthumbs"></div>
<div class="row2"><select id="vfmt"><option value="1280x720">Yatay 16:9</option><option value="720x1280">Dikey 9:16</option><option value="1080x1080">Kare 1:1</option></select><select id="vtr"><option value="fade">Yumuşak geçiş</option><option value="zoom">Yakınlaştırma</option><option value="none">Geçişsiz</option></select></div>
<div class="row2"><select id="vmus"><option value="none">Müzik yok</option><option value="calm">Sakin müzik</option><option value="happy">Neşeli müzik</option><option value="file">Kendi müziğim…</option></select><select id="vta"><option value="static">Yazı: sabit</option><option value="fade">Yazı: belirme</option><option value="type">Yazı: daktilo</option></select></div>
<div class="row2"><label>Görsel başı <input type="range" id="vsec" min="1" max="6" value="3"> <span id="vsl">3 sn</span></label></div>
<input type="text" id="vcap" placeholder="Altyazı (isteğe bağlı)">
<canvas id="vcv" width="1280" height="720" style="background:#000"></canvas>
<button class="chip on" id="vgo">🎬 Videoyu oluştur</button>
<div id="vst" style="color:var(--mu);font-size:13px">Oluşturma gerçek zamanlı sürer; bitene kadar bu ekranda kal.</div>
<video id="vout" controls playsinline style="width:100%;border-radius:10px;display:none"></video>
<button class="chip" id="vdl" style="display:none">⬇ İndir</button>
</div>
<input type="file" id="vfile" accept="image/*" multiple hidden>
<input type="file" id="vaf" accept="audio/*" hidden>
</div>
<script>
function b64(b){return new Promise(function(r){var f=new FileReader();f.onload=function(){r(String(f.result).split(",")[1])};f.readAsDataURL(b)})}
function askKey(){return new Promise(function(res){var d=document.createElement("div");d.style.cssText="position:fixed;inset:0;z-index:99;background:rgba(0,0,0,.6);display:flex;align-items:center;justify-content:center;padding:20px";d.innerHTML='<div style="background:var(--sf);color:var(--tx);padding:18px;border-radius:14px;width:100%;max-width:420px"><b>Anthropic API anahtarı</b><p style="font-size:14px;color:var(--mu)">console.anthropic.com adresinden alabilirsin. Yalnızca bu cihazda saklanır.</p><input id="kk" type="password" placeholder="sk-ant-..." style="width:100%;padding:10px;border-radius:10px;border:1px solid var(--bd);background:var(--bg);color:var(--tx);font:inherit"><div style="display:flex;gap:8px;margin-top:12px;justify-content:flex-end"><button id="kc" class="chip">Vazgeç</button><button id="ko" class="chip on">Kaydet</button></div></div>';document.body.appendChild(d);d.querySelector("#kc").onclick=function(){d.remove();res(null)};d.querySelector("#ko").onclick=function(){var v=d.querySelector("#kk").value.trim();d.remove();res(v||null)}})}
function mkSample(){
 var f=async function(input,opts){
  opts=opts||{};var key=localStorage.getItem("ata-key");
  if(!key){key=await askKey();if(!key)throw{code:"not_granted",message:"API anahtarı gerekli"};localStorage.setItem("ata-key",key)}
  var msgs=typeof input==="string"?[{role:"user",content:input}]:input.map(function(m){return{role:m.role,content:m.content}});
  if(opts.images&&opts.images.length){var parts=[];for(var i=0;i<opts.images.length;i++){var b=opts.images[i];parts.push({type:"image",source:{type:"base64",media_type:b.type||"image/jpeg",data:await b64(b)}})}
   var last=msgs[msgs.length-1];last.content=parts.concat([{type:"text",text:last.content}])}
  var r=await fetch("https://api.anthropic.com/v1/messages",{method:"POST",headers:{"content-type":"application/json","x-api-key":key,"anthropic-version":"2023-06-01","anthropic-dangerous-direct-browser-access":"true"},body:JSON.stringify({model:"claude-sonnet-5-5",max_tokens:4000,messages:msgs})});
  var d=await r.json();
  if(!r.ok){if(r.status===401)localStorage.removeItem("ata-key");throw{code:r.status===429?"rate_limited":"error",message:(d.error&&d.error.message)||("HTTP "+r.status)}}
  var text=d.content.map(function(c){return c.text||""}).join("");
  if(opts.onText)opts.onText({text:text,delta:text});
  return{text:text,truncated:d.stop_reason==="max_tokens"}};
 f.limits=async function(){return{images:true}};return f}
if(!window.claude){window.claude={use:function(n){
 if(n==="sample")return Promise.resolve(mkSample());
 if(n==="downloads")return Promise.resolve({save:function(o){var blob=o.data instanceof Blob?o.data:new Blob([o.data]);
  if(window.AndroidBridge)return b64(blob).then(function(d){AndroidBridge.save(o.filename,d,(blob.type||"application/octet-stream").split(";")[0]);return{status:"saved"}});
  var a=document.createElement("a");a.href=URL.createObjectURL(blob);a.download=o.filename;document.body.appendChild(a);a.click();a.remove();return Promise.resolve({status:"saved"})}});
 return Promise.resolve(null)}}}
if(window.AndroidBridge&&!window.SpeechRecognition&&!window.webkitSpeechRecognition){window.SpeechRecognition=function(){var s=this;s.start=function(){window.__ataVoice={res:function(t){s.onresult&&s.onresult({results:[[{transcript:t}]]})},err:function(e){s.onerror&&s.onerror({error:e})},end:function(){s.onend&&s.onend()}};AndroidBridge.listen()};s.stop=function(){AndroidBridge.stopListen()}}}
(function(){
var $=function(i){return document.getElementById(i)};
var log=$("log"),inp=$("in"),hist=[],imgs=[],drawOn=false,ttsOn=false,sample=null,busy=false,rec=null;
claude.use("sample").then(function(s){sample=s});
var SYS="Sen Ata adında, samimi ve yardımsever bir Türkçe yapay zeka asistanısın. Kısa ve net yanıt ver. Kullanıcı yazdıysa Türkçe yanıtla.";
var DSYS="Sen bir SVG çizerisin. Yalnızca tek bir geçerli <svg> öğesi döndür (viewBox=\"0 0 400 300\", xmlns ile), başka metin, açıklama veya kod çiti yazma. Script kullanma. Renkli, sade ve güzel bir çizim yap. İstek: ";
function add(t,c,src){var d=document.createElement("div");d.className="m "+(c||"");d.textContent=t;if(src){[].concat(src).forEach(function(s){var i=document.createElement("img");i.src=s;d.appendChild(i)})}log.appendChild(d);log.scrollTop=log.scrollHeight;return d}
function speak(t){if(!ttsOn||!window.speechSynthesis)return;try{speechSynthesis.cancel();var u=new SpeechSynthesisUtterance(t);u.lang="tr-TR";speechSynthesis.speak(u)}catch(e){}}
$("tts").onclick=function(){ttsOn=!ttsOn;this.classList.toggle("on",ttsOn);if(!ttsOn&&window.speechSynthesis)speechSynthesis.cancel()};
$("draw").onclick=function(){drawOn=!drawOn;this.classList.toggle("on",drawOn)};
$("att").onclick=function(){$("file").click()};
$("file").onchange=function(){[].slice.call(this.files).forEach(function(f){imgs.push(f)});this.value="";showPrev()};
function showPrev(){var p=$("pi");p.innerHTML="";imgs.forEach(function(f){var i=document.createElement("img");i.src=URL.createObjectURL(f);p.appendChild(i)});$("prev").style.display=imgs.length?"block":"none"}
$("px").onclick=function(){imgs=[];showPrev()};
inp.oninput=function(){this.style.height="auto";this.style.height=Math.min(this.scrollHeight,120)+"px"};
inp.onkeydown=function(e){if(e.key==="Enter"&&!e.shiftKey){e.preventDefault();send()}};
$("send").onclick=function(){send()};
var mi=[],lay="grid",dl=null;
claude.use("downloads").then(function(d){dl=d;if(!d)$("mdl").style.display="none"});
function toast(m){var d=document.createElement("div");d.textContent=m;d.style.cssText="position:fixed;left:12px;right:12px;bottom:calc(16px + env(safe-area-inset-bottom,0px));z-index:99;background:#222;color:#fff;padding:10px 14px;border-radius:12px;font-size:14px";document.body.appendChild(d);setTimeout(function(){d.remove()},4500)}
function ld(f){return new Promise(function(r){var i=new Image();i.onload=function(){r(i)};i.onerror=function(){r(null)};i.src=URL.createObjectURL(f)})}
[["grid","▦ Izgara"],["row","↔ Yatay"],["col","↕ Dikey"]].forEach(function(l){var b=document.createElement("button");b.className="chip"+(l[0]===lay?" on":"");b.textContent=l[1];b.onclick=function(){lay=l[0];[].forEach.call($("lay").children,function(c){c.classList.remove("on")});b.classList.add("on");paint()};$("lay").appendChild(b)});
$("mont").onclick=function(){$("mp").style.display="flex";render()};
$("mx").onclick=function(){$("mp").style.display="none"};
$("madd").onclick=function(){$("mfile").click()};
$("mfile").onchange=async function(){var fs=[].slice.call(this.files);this.value="";var bad=0;for(var k=0;k<fs.length;k++){var im=await ld(fs[k]);if(im)mi.push({f:fs[k],im:im});else bad++}if(bad)toast(bad+" görsel açılamadı (HEIC gibi desteklenmeyen biçim olabilir).");render()};
$("mclr").onclick=function(){mi=[];render()};
$("gap").oninput=paint;$("bgc").oninput=paint;
function render(){var t=$("thumbs");t.innerHTML="";mi.forEach(function(o,k){var d=document.createElement("div"),i=document.createElement("img"),x=document.createElement("b");i.src=o.im.src;x.textContent="×";x.onclick=function(){mi.splice(k,1);render()};d.appendChild(i);d.appendChild(x);t.appendChild(d)});paint()}
function paint(){var cv=$("cv"),c=cv.getContext("2d"),g=+$("gap").value,n=mi.length,W=1200;
 if(!n){cv.width=W;cv.height=300;c.fillStyle=$("bgc").value;c.fillRect(0,0,W,300);c.fillStyle="#999";c.font="32px sans-serif";c.textAlign="center";c.fillText("Görsel ekle",W/2,160);return}
 var cols=lay==="row"?n:lay==="col"?1:Math.ceil(Math.sqrt(n)),rows=Math.ceil(n/cols),cw=(W-g*(cols+1))/cols,ch=cw*.75,H=Math.round(g*(rows+1)+ch*rows);
 cv.width=W;cv.height=H;c.fillStyle=$("bgc").value;c.fillRect(0,0,W,H);
 mi.forEach(function(o,k){var r=Math.floor(k/cols),q=k%cols,inRow=Math.min(cols,n-r*cols),off=(cols-inRow)*(cw+g)/2,x=g+off+q*(cw+g),y=g+r*(ch+g),im=o.im,sr=cw/ch,sw,sh,sx,sy;
  if(im.width/im.height>sr){sh=im.height;sw=sh*sr;sx=(im.width-sw)/2;sy=0}else{sw=im.width;sh=sw/sr;sx=0;sy=(im.height-sh)/2}
  c.drawImage(im,sx,sy,sw,sh,x,y,cw,ch)})}
$("msend").onclick=function(){if(!mi.length)return;$("cv").toBlob(function(b){imgs.push(new File([b],"montaj.png",{type:"image/png"}));showPrev();$("mp").style.display="none"},"image/png")};
$("mdl").onclick=function(){if(!mi.length||!dl)return;$("cv").toBlob(function(b){dl.save({filename:"montaj.png",data:b}).catch(function(){})},"image/png")};
var vi=[],vbusy=false,vblob=null,vext="webm";
$("vid").onclick=function(){$("vp").style.display="flex";vrender()};
$("vx").onclick=function(){$("vp").style.display="none"};
$("vadd").onclick=function(){$("vfile").click()};
$("vfile").onchange=async function(){var fs=[].slice.call(this.files);this.value="";var bad=0;for(var k=0;k<fs.length;k++){var im=await ld(fs[k]);if(im)vi.push({im:im});else bad++}if(bad)toast(bad+" görsel açılamadı (HEIC gibi desteklenmeyen biçim olabilir).");vrender()};
$("vclr").onclick=function(){vi=[];vrender()};
$("vsec").oninput=function(){$("vsl").textContent=this.value+" sn"};
function vrender(){var t=$("vthumbs");t.innerHTML="";vi.forEach(function(o,k){var d=document.createElement("div"),i=document.createElement("img"),x=document.createElement("b");i.src=o.im.src;x.textContent="×";x.onclick=function(){vi.splice(k,1);vrender()};d.appendChild(i);d.appendChild(x);t.appendChild(d)})}
function cover(c,im,W,H,z,a){var s=Math.max(W/im.width,H/im.height)*z,w=im.width*s,h=im.height*s;c.globalAlpha=a;c.drawImage(im,(W-w)/2,(H-h)/2,w,h);c.globalAlpha=1}
function wrapT(c,t,mw){var l="",o=[];t.split(" ").forEach(function(x){var n=l?l+" "+x:x;if(c.measureText(n).width>mw&&l){o.push(l);l=x}else l=n});o.push(l);return o}
var vaud=null;
var K="ata-ayar",VS=["vfmt","vtr","vsec","vmus","vta","vcap","gap","bgc"];
function lsGet(){try{return JSON.parse(localStorage.getItem(K))||{}}catch(e){return{}}}
function lsSave(){try{var s={tts:ttsOn};VS.forEach(function(id){s[id]=$(id).value});localStorage.setItem(K,JSON.stringify(s))}catch(e){}}
(function(){var s=lsGet();VS.forEach(function(id){if(s[id]!=null)$(id).value=s[id]});if($("vmus").value==="file")$("vmus").value="none";$("vsl").textContent=$("vsec").value+" sn";if(s.tts){ttsOn=true;$("tts").classList.add("on")}})();
VS.forEach(function(id){$(id).addEventListener("change",lsSave);$(id).addEventListener("input",lsSave)});
$("tts").addEventListener("click",lsSave);
function note(ac,out,f,at,len,type,vol){var o=ac.createOscillator(),g=ac.createGain();o.type=type;o.frequency.value=f;g.gain.setValueAtTime(0,at);g.gain.linearRampToValueAtTime(vol,at+.05);g.gain.linearRampToValueAtTime(0,at+len);o.connect(g);g.connect(out);o.start(at);o.stop(at+len+.05)}
function gen(ac,out,kind,dur){var t0=ac.currentTime+.05,calm=kind==="calm",ch=calm?[[220,261.6,329.6],[174.6,220,261.6],[261.6,329.6,392],[196,246.9,293.7]]:[[261.6,329.6,392,523.3],[220,261.6,329.6,440],[174.6,220,261.6,349.2],[196,246.9,293.7,392]],step=calm?4:2,k=0;
 for(var t=0;t<dur;t+=step,k++){var cd=ch[k%4];
  if(calm){cd.forEach(function(f){note(ac,out,f,t0+t,step+.6,"sine",.5)})}
  else{for(var j=0;j<step/.25;j++)note(ac,out,cd[j%4],t0+t+j*.25,.3,"triangle",.7)}}}
async function mk(){if(vbusy)return;try{await mk0()}catch(e){$("vst").textContent="Hata: "+((e&&e.message)||e);$("vgo").textContent="🎬 Videoyu oluştur"}vbusy=false}
async function mk0(){
 if(!vi.length||vbusy){if(!vi.length)$("vst").textContent="Önce görsel ekle.";return}
 if(!window.MediaRecorder||!HTMLCanvasElement.prototype.captureStream){$("vst").textContent="Bu tarayıcı video oluşturmayı desteklemiyor.";return}
 vbusy=true;
 var b=$("vgo"),f=$("vfmt").value.split("x"),W=+f[0],H=+f[1],cv=$("vcv"),c=cv.getContext("2d"),S=+$("vsec").value*1000,tr=$("vtr").value,cap=$("vcap").value.trim(),n=vi.length,T=n*S;
 cv.width=W;cv.height=H;$("vout").style.display="none";$("vdl").style.display="none";
 var mt=["video/webm;codecs=vp9","video/webm;codecs=vp8","video/webm","video/mp4"].filter(function(m){return MediaRecorder.isTypeSupported(m)})[0];
 vext=mt&&mt.indexOf("mp4")>-1?"mp4":"webm";
 var ch=[],mr,ac=null,st=cv.captureStream(30),mus=$("vmus").value;
 if(mus!=="none"&&(mus!=="file"||vaud)){try{ac=new (window.AudioContext||window.webkitAudioContext)();try{await ac.resume()}catch(e){}var dst=ac.create
