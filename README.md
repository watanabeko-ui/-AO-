<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>三島 AO生 進捗ダッシュボード</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Zen+Kaku+Gothic+New:wght@400;500;700&family=Zen+Old+Mincho:wght@600;700&family=IBM+Plex+Mono:wght@400;500&display=swap">
<style>
:root{
  --paper:#F4F6F3; --surface:#FFFFFF; --ink:#1E2940; --muted:#5E6878; --line:#D9DED8;
  --accent:#1F7A6B; --accent-soft:#DCEEE9; --plan:#8A93A3;
  --good:#2E7D4F; --warn:#B7791F; --bad:#B4433A; --good-bg:#E3F2E8; --warn-bg:#F8EDD6; --bad-bg:#F6E1DE;
  --display:"Zen Old Mincho", "Hiragino Mincho ProN", serif;
  --body:"Zen Kaku Gothic New", "Hiragino Sans", "Yu Gothic", sans-serif;
  --mono:"IBM Plex Mono", ui-monospace, Menlo, monospace;
}
@media (prefers-color-scheme: dark){
  :root:not([data-theme="light"]){
    color-scheme:dark;
    --paper:#131A24; --surface:#1B2431; --ink:#E6EAF0; --muted:#9AA4B3; --line:#2E3A4A;
    --accent:#4FB8A4; --accent-soft:#1D3A37; --plan:#7C879A;
    --good:#6CC08E; --warn:#E0A94A; --bad:#E57A6F; --good-bg:#1D3326; --warn-bg:#3A2F1A; --bad-bg:#3D2322;
  }
}
:root[data-theme="dark"]{
  color-scheme:dark;
  --paper:#131A24; --surface:#1B2431; --ink:#E6EAF0; --muted:#9AA4B3; --line:#2E3A4A;
  --accent:#4FB8A4; --accent-soft:#1D3A37; --plan:#7C879A;
  --good:#6CC08E; --warn:#E0A94A; --bad:#E57A6F; --good-bg:#1D3326; --warn-bg:#3A2F1A; --bad-bg:#3D2322;
}
*{box-sizing:border-box} html,body{margin:0}
body{background:var(--paper);color:var(--ink);font-family:var(--body);font-size:15px;line-height:1.7;padding:0 16px}
.wrap{max-width:1080px;margin:0 auto;padding-block:28px 56px;display:grid;gap:28px}
header{display:grid;gap:6px}
.eyebrow{font-size:12px;letter-spacing:.12em;color:var(--muted)}
h1{font-family:var(--display);font-size:clamp(24px,4vw,34px);line-height:1.3;margin:0;text-wrap:balance}
h2{font-family:var(--display);font-size:20px;margin:0 0 12px;text-wrap:balance}
.sub{color:var(--muted);margin:0}
.num{font-family:var(--mono);font-variant-numeric:tabular-nums}
.kpis{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:12px}
.kpi{background:var(--surface);border:1px solid var(--line);border-radius:10px;padding:16px 18px;display:grid;gap:4px;align-content:start}
.kpi .label{font-size:12px;color:var(--muted);letter-spacing:.06em}
.kpi .big{font-family:var(--mono);font-size:30px;font-weight:500;line-height:1.2}
.kpi .big small{font-size:15px;color:var(--muted)}
.bar{height:8px;background:var(--line);border-radius:4px;position:relative;margin-block:4px}
.bar i{position:absolute;inset:0 auto 0 0;background:var(--accent);border-radius:4px}
.bar b{position:absolute;top:-3px;bottom:-3px;width:2px;background:var(--ink)}
.note{font-size:12px;color:var(--muted)}
.pill{display:inline-block;font-size:12px;padding:1px 9px;border-radius:999px;font-weight:500;white-space:nowrap}
.pill.good{background:var(--good-bg);color:var(--good)} .pill.warn{background:var(--warn-bg);color:var(--warn)} .pill.bad{background:var(--bad-bg);color:var(--bad)} .pill.none{background:var(--line);color:var(--muted)}
section.panel{background:var(--surface);border:1px solid var(--line);border-radius:10px;padding:20px;min-width:0}
.grid2{display:grid;grid-template-columns:3fr 2fr;gap:20px}
@media (max-width:820px){.grid2{grid-template-columns:1fr}}
.chart{width:100%;height:auto;display:block}
.legend{display:flex;gap:16px;flex-wrap:wrap;font-size:12px;color:var(--muted)}
.legend span::before{content:"";display:inline-block;width:14px;height:3px;margin-right:6px;vertical-align:middle;background:currentColor}
.legend .l-act{color:var(--accent)} .legend .l-plan{color:var(--plan)}
.tbl{overflow-x:auto}
table{border-collapse:collapse;width:100%;min-width:640px;font-size:14px}
th,td{padding:8px 10px;border-bottom:1px solid var(--line);text-align:right;white-space:nowrap}
th:first-child,td:first-child{text-align:left}
thead th{font-size:12px;color:var(--muted);font-weight:500;letter-spacing:.04em}
td.cell{font-family:var(--mono)}
td small{font-size:11px;opacity:.85}
td.c-good{background:var(--good-bg);color:var(--good)} td.c-warn{background:var(--warn-bg);color:var(--warn)} td.c-bad{background:var(--bad-bg);color:var(--bad)} td.c-none{color:var(--muted)}
tfoot td{font-weight:700;border-bottom:none}
form{display:grid;gap:12px}
.row{display:grid;grid-template-columns:repeat(auto-fit,minmax(120px,1fr));gap:10px}
label{display:grid;gap:4px;font-size:12px;color:var(--muted)}
input,select,textarea{font:inherit;color:var(--ink);background:var(--paper);border:1px solid var(--line);border-radius:6px;padding:7px 9px;width:100%}
textarea{min-height:70px;resize:vertical}
input:focus-visible,select:focus-visible,textarea:focus-visible,button:focus-visible{outline:2px solid var(--accent);outline-offset:1px}
button{font:inherit;font-weight:700;background:var(--accent);color:#fff;border:0;border-radius:6px;padding:9px 16px;cursor:pointer;justify-self:start}
button:disabled{opacity:.5;cursor:default}
.status{font-size:13px;color:var(--muted);min-height:1.4em}
.deliv{list-style:none;margin:0;padding:0;display:grid}
.deliv li{display:grid;grid-template-columns:28px 1fr auto;gap:8px;align-items:baseline;padding:8px 0;border-bottom:1px solid var(--line);font-size:14px}
.deliv .no{font-family:var(--mono);color:var(--accent);font-weight:500}
.deliv .m{font-size:12px;color:var(--muted)}
.insights{display:grid;gap:12px;margin:0;padding:0;list-style:none}
.insights li{display:grid;grid-template-columns:56px 1fr;gap:12px;font-size:14px}
.insights .mo{font-family:var(--mono);color:var(--muted);font-size:13px}
.callout{border-left:3px solid var(--warn);background:var(--warn-bg);padding:10px 14px;border-radius:0 6px 6px 0;font-size:14px}
@media (prefers-reduced-motion:no-preference){.bar i{transition:width .6s ease}}
</style>
</head>
<body>


<div class="wrap">
  <header>
    <div class="eyebrow">成果物① ／ SL三島BASE ／ DLP 渡辺 こてつ</div>
    <h1>三島 AO生 進捗ダッシュボード</h1>
    <p class="sub">目標：担当195世帯のうちAO生 71世帯（普及率36.4%）を6ヶ月で達成。月間 8世帯（一般聴講生4・Life&amp;Works 2・商店主DX 2）。</p>
  </header>

  <div class="kpis" id="kpis"></div>

  <div class="callout" id="pace"></div>

  <div class="grid2">
    <section class="panel">
      <h2>累計AO生の推移（計画と実績）</h2>
      <svg class="chart" id="chart" viewBox="0 0 560 280" role="img" aria-label="累計AO生の計画と実績の推移"></svg>
      <div class="legend"><span class="l-plan">計画（月8世帯）</span><span class="l-act">実績</span></div>
    </section>
    <section class="panel">
      <h2>月次実績を入力</h2>
      <form id="form">
        <label for="f-month">対象月
          <select id="f-month"></select>
        </label>
        <div class="row">
          <label for="f-general">一般聴講生（目標4）<input id="f-general" type="number" min="0" step="1" inputmode="numeric"></label>
          <label for="f-lw">Life&amp;Works（目標2）<input id="f-lw" type="number" min="0" step="1" inputmode="numeric"></label>
          <label for="f-shop">商店主DX（目標2）<input id="f-shop" type="number" min="0" step="1" inputmode="numeric"></label>
        </div>
        <label for="f-memo">要因・気づきメモ<textarea id="f-memo" placeholder="例：成果物⑦を持参した個別相談から1件獲得"></textarea></label>
        <button type="submit" id="f-save">保存する</button>
        <div class="status" id="f-status">共有データに接続中…</div>
      </form>
    </section>
  </div>

  <section class="panel">
    <h2>ターゲット層別 月次実績</h2>
    <div class="tbl"><table id="tbl"></table></div>
    <p class="note">色：達成率 100%以上＝緑、50〜99%＝黄、50%未満＝赤。普及率は担当195世帯を分母に算出（気づきメモ2〜4ヶ月目と同じ基準）。</p>
  </section>

  <div class="grid2">
    <section class="panel">
      <h2>成果物の進捗（①〜⑫）</h2>
      <ul class="deliv" id="deliv"></ul>
    </section>
    <section class="panel">
      <h2>月ごとの気づき</h2>
      <ul class="insights" id="insights"></ul>
    </section>
  </div>
</div>

<script>
(function(){
  var TOTAL=195, BASE=23, GOAL=71, TARGET={general:4,lw:2,shop:2};
  var MONTHS=[
    {id:"m1",label:"5月度",n:1},{id:"m2",label:"6月度",n:2},{id:"m3",label:"7月度",n:3},
    {id:"m4",label:"8月度",n:4},{id:"m5",label:"9月度",n:5},{id:"m6",label:"10月度",n:6}
  ];
  // 気づきメモ1〜4ヶ月目の報告値（共有データが読めない時にも表示できるよう保持）
  var REPORTED={
    m1:{general:1,lw:2,shop:2,memo:"判断軸の訴求が現役世代・商店主に響いた。シニア層は目の前の不便さ解消を入口にしないと伝わりにくい。"},
    m2:{general:0,lw:0,shop:1,memo:"拠点開拓とマップ作成に偏り、現場の対話量が不足。理念先行の営業に陥ったと内省。"},
    m3:{general:0,lw:1,shop:1,memo:"共感型対話に立ち戻り2件。道具の完成度より、道具を携えて現場に出る行動量が価値を証明する。"},
    m4:{general:0,lw:0,shop:0,memo:"成果物⑦⑧の制作に集中し獲得0。作る時間が使う時間を上回る構造が再発。"}
  };
  var data={}; Object.keys(REPORTED).forEach(function(k){data[k]=REPORTED[k];});
  var DELIV=[
    ["①","月間進捗管理ダッシュボード",1],["②","「共感と役割転換」個別伴走記録ノート",1],
    ["③","AO校（学習拠点）利用可能施設マップ",2],["④","店舗を地域の学びの場にする提案書",2],
    ["⑤","アナログ啓発スクラップブック",3],["⑥","「街のくらし安全入門」個別サポートプラン",3],
    ["⑦","家内安全プロトコル",4],["⑧","「お茶の間対話」伝播ケーススタディ",4],
    ["⑨","「お茶の間対話」実践事例集",5],["⑩","「明日の先生」成長度評価チェックリスト",5],
    ["⑪","71世帯達成報告・活動軌跡レポート",6],["⑫","三島地域 SROI算出書",6]
  ];
  var db=null, $=function(id){return document.getElementById(id);};

  function sum(d){return d?(+d.general||0)+(+d.lw||0)+(+d.shop||0):null;}
  function has(d){return !!d && [d.general,d.lw,d.shop].some(function(v){return v!==null&&v!==undefined&&v!=="";});}
  function lastIdx(){var li=-1;MONTHS.forEach(function(m,i){if(has(data[m.id]))li=i;});return li;}
  function cls(v,t){if(v==null)return"none";var r=v/t;return r>=1?"good":r>=.5?"warn":"bad";}
  function pct(v,t){return (v/t*100).toFixed(1)+"%";}
  function esc(s){var d=document.createElement("div");d.textContent=s==null?"":String(s);return d.innerHTML;}
  function segTotal(k,li){var s=0;MONTHS.forEach(function(m,i){if(i<=li&&has(data[m.id]))s+=+data[m.id][k]||0;});return s;}

  function render(){
    var li=lastIdx(), cum=BASE, cums=[BASE];
    MONTHS.forEach(function(m,i){ if(i<=li){cum+=sum(data[m.id])||0;} cums.push(i<=li?cum:null); });
    var now=li>=0?cums[li+1]:BASE, gained=now-BASE, planNow=BASE+8*(li+1), planGain=8*(li+1);
    var left=MONTHS.length-(li+1), need=Math.max(0,GOAL-now);
    $("kpis").innerHTML=
      kpi("累計AO生", now+"<small> / "+GOAL+" 世帯</small>", now/GOAL*100, planNow/GOAL*100, "計画上の現時点："+planNow+"世帯（黒線）")+
      kpi("普及率", pct(now,TOTAL)+"<small> / 36.4%</small>", (now/TOTAL)/(GOAL/TOTAL)*100, null, "開始時 12.4%（23/185世帯）")+
      kpi("新規獲得（累計）", gained+"<small> / "+planGain+" 世帯</small>", planGain?gained/planGain*100:0, null, li>=0?MONTHS[li].label+"までの計画比 "+(gained/planGain*100).toFixed(1)+"%":"実績未入力")+
      kpi("残り必要数", need+"<small> 世帯</small>", null, null, left>0?"残り"+left+"ヶ月で月平均 "+(need/left).toFixed(1)+"世帯が必要":"期間終了");
    $("pace").innerHTML= left>0
      ? "<strong>ペース確認：</strong>計画比 "+(now-planNow)+"世帯。目標71世帯には残り"+left+"ヶ月で月 <span class='num'>"+Math.ceil(need/left)+"</span> 世帯が必要です（計画の月8世帯の約"+(need/left/8).toFixed(1)+"倍）。一般聴講生（シニア層）は累計"+segTotal("general",li)+"世帯で、最も遅れているターゲット層です。"
      : "<strong>期間終了：</strong>最終累計 "+now+"世帯（目標比 "+pct(now,GOAL)+"）。";
    drawChart(cums);
    drawTable(li);
    $("deliv").innerHTML=DELIV.map(function(d){
      var st=d[2]<=4?["good","作成済"]:d[2]===5?["warn","5ヶ月目"]:["none","6ヶ月目"];
      return "<li><span class='no'>"+d[0]+"</span><span>"+esc(d[1])+"<br><span class='m'>"+MONTHS[d[2]-1].n+"ヶ月目（"+MONTHS[d[2]-1].label+"）</span></span><span class='pill "+st[0]+"'>"+st[1]+"</span></li>";
    }).join("");
    $("insights").innerHTML=MONTHS.filter(function(m){return data[m.id]&&data[m.id].memo;}).map(function(m){
      return "<li><span class='mo'>"+m.label+"</span><span>"+esc(data[m.id].memo)+"</span></li>";}).join("")||"<li>メモはまだありません。</li>";
  }
  function kpi(label,big,p,mark,note){
    var bar=p==null?"":"<div class='bar'><i style='width:"+Math.min(100,Math.max(0,p)).toFixed(1)+"%'></i>"+(mark!=null?"<b style='left:"+Math.min(100,mark).toFixed(1)+"%'></b>":"")+"</div>";
    return "<div class='kpi'><div class='label'>"+label+"</div><div class='big'>"+big+"</div>"+bar+"<div class='note'>"+note+"</div></div>";
  }
  function drawTable(li){
    var rows=[["一般聴講生","general"],["Life&amp;Works","lw"],["商店主DX","shop"]];
    var h="<thead><tr><th>ターゲット層（月間目標）</th>"+MONTHS.map(function(m){return"<th>"+m.label+"</th>";}).join("")+"<th>累計／計画</th></tr></thead><tbody>";
    rows.forEach(function(r){
      h+="<tr><td>"+r[0]+"（"+TARGET[r[1]]+"）</td>";
      MONTHS.forEach(function(m){var d=data[m.id];var v=has(d)?(+d[r[1]]||0):null;
        h+="<td class='cell c-"+cls(v,TARGET[r[1]])+"'>"+(v==null?"—":v+" <small>"+Math.round(v/TARGET[r[1]]*100)+"%</small>")+"</td>";});
      h+="<td class='num'>"+segTotal(r[1],li)+" / "+TARGET[r[1]]*(li+1)+"</td></tr>";
    });
    h+="</tbody><tfoot><tr><td>合計（8）</td>";
    MONTHS.forEach(function(m){var d=data[m.id];var v=has(d)?sum(d):null;
      h+="<td class='cell c-"+cls(v,8)+"'>"+(v==null?"—":v+" <small>"+(v/8*100).toFixed(1)+"%</small>")+"</td>";});
    var tot=segTotal("general",li)+segTotal("lw",li)+segTotal("shop",li), run=BASE;
    h+="<td class='num'>"+tot+" / "+(8*(li+1))+"</td></tr><tr><td>累計AO生（普及率）</td>";
    MONTHS.forEach(function(m,i){ if(i<=li){run+=sum(data[m.id])||0; h+="<td class='num'>"+run+" <small>"+pct(run,TOTAL)+"</small></td>";} else h+="<td class='c-none'>—</td>";});
    h+="<td class='num'>目標 71</td></tr></tfoot>";
    $("tbl").innerHTML=h;
  }
  function drawChart(cums){
    var W=560,H=280,L=40,R=20,T=16,B=36, x=function(i){return L+i*(W-L-R)/6;}, y=function(v){return T+(1-v/80)*(H-T-B);};
    var s="";
    [0,20,40,60,80].forEach(function(v){s+="<line x1='"+L+"' x2='"+(W-R)+"' y1='"+y(v)+"' y2='"+y(v)+"' stroke='var(--line)' stroke-width='1'/><text x='"+(L-8)+"' y='"+(y(v)+4)+"' text-anchor='end' font-size='11' fill='var(--muted)' font-family='IBM Plex Mono, monospace'>"+v+"</text>";});
    s+="<line x1='"+L+"' x2='"+(W-R)+"' y1='"+y(71)+"' y2='"+y(71)+"' stroke='var(--accent)' stroke-dasharray='2 4' stroke-width='1'/><text x='"+(L+6)+"' y='"+(y(71)-6)+"' text-anchor='start' font-size='11' fill='var(--accent)'>目標 71世帯</text>";
    ["開始"].concat(MONTHS.map(function(m){return m.label;})).forEach(function(l,i){s+="<text x='"+x(i)+"' y='"+(H-12)+"' text-anchor='middle' font-size='11' fill='var(--muted)'>"+l+"</text>";});
    var plan=[];for(var i=0;i<=6;i++)plan.push(x(i)+","+y(BASE+8*i));
    s+="<polyline points='"+plan.join(" ")+"' fill='none' stroke='var(--plan)' stroke-width='2' stroke-dasharray='6 5'/>";
    var pts=[];cums.forEach(function(v,i){if(v!=null)pts.push([x(i),y(v),v]);});
    if(pts.length){
      var area="M"+pts[0][0]+","+y(0)+" "+pts.map(function(p){return"L"+p[0]+","+p[1];}).join(" ")+" L"+pts[pts.length-1][0]+","+y(0)+"Z";
      s+="<path d='"+area+"' fill='var(--accent-soft)' opacity='.7'/>";
      s+="<polyline points='"+pts.map(function(p){return p[0]+","+p[1];}).join(" ")+"' fill='none' stroke='var(--accent)' stroke-width='2.5'/>";
      pts.forEach(function(p,i){var last=i===pts.length-1;
        s+="<circle cx='"+p[0]+"' cy='"+p[1]+"' r='"+(last?5:3)+"' fill='"+(last?"var(--accent)":"var(--surface)")+"' stroke='var(--accent)' stroke-width='2'/>";
        s+="<text x='"+p[0]+"' y='"+(p[1]+18)+"' text-anchor='middle' font-size='11' font-family='IBM Plex Mono, monospace' fill='var(--ink)'>"+p[2]+"</text>";});
    }
    $("chart").innerHTML=s;
  }

  var sel=$("f-month");
  sel.innerHTML=MONTHS.map(function(m){return"<option value='"+m.id+"'>"+m.label+"（"+m.n+"ヶ月目）</option>";}).join("");
  function fill(){var d=data[sel.value]||{};$("f-general").value=d.general!=null?d.general:"";$("f-lw").value=d.lw!=null?d.lw:"";$("f-shop").value=d.shop!=null?d.shop:"";$("f-memo").value=d.memo||"";}
  sel.addEventListener("change",fill);
  sel.value=MONTHS[Math.min(lastIdx()+1,5)].id; fill();
  $("form").addEventListener("submit",function(e){
    e.preventDefault();
    var toN=function(v){return v===""?null:Math.max(0,parseInt(v,10)||0);};
    var m=MONTHS.filter(function(x){return x.id===sel.value;})[0];
    var rec={month:m.n,general:toN($("f-general").value),lw:toN($("f-lw").value),shop:toN($("f-shop").value),memo:$("f-memo").value.trim(),updatedAt:new Date().toISOString()};
    data[sel.value]=rec; render();
    if(!db){try{localStorage.setItem("mishima-ao-months",JSON.stringify(data));$("f-status").textContent=m.label+"の実績をこのブラウザに保存しました。";}catch(e){$("f-status").textContent="保存できませんでした（ブラウザの保存機能が無効です）。";}return;}
    $("f-save").disabled=true; $("f-status").textContent="保存中…";
    Promise.resolve(db.doc("months/"+sel.value).set(rec)).then(function(){
      $("f-status").textContent=m.label+"の実績を保存しました。";
    }).catch(function(err){
      $("f-status").textContent="保存できませんでした（"+((err&&err.code)||"権限なし")+"）。編集権限のあるアカウントで開いてください。";
    }).then(function(){$("f-save").disabled=false;});
  });

  try{var saved=JSON.parse(localStorage.getItem("mishima-ao-months")||"null");if(saved&&typeof saved==="object"){Object.keys(saved).forEach(function(k){if(saved[k])data[k]=saved[k];});sel.value=MONTHS[Math.min(lastIdx()+1,5)].id;fill();}}catch(e){}
  render();

  function unwrap(r){if(!r)return null;if(typeof r.data==="function")return r.data();if(r.data!==undefined)return r.data;return r;}
  var off=function(){$("f-status").textContent="入力内容はこのブラウザに保存されます。";};
  if(window.claude&&window.claude.use){
    window.claude.use("db").then(function(d){
      db=d; if(!db){off();return;}
      $("f-status").textContent="共有データに接続しました。";
      return Promise.all(MONTHS.map(function(m){
        return Promise.resolve(db.doc("months/"+m.id).get()).then(function(r){var v=unwrap(r);if(v&&typeof v==="object"&&("general" in v||"lw" in v||"shop" in v))data[m.id]=v;}).catch(function(){});
      })).then(function(){render();sel.value=MONTHS[Math.min(lastIdx()+1,5)].id;fill();});
    }).catch(off);
  } else off();
})();
</script>

</body>
</html>
