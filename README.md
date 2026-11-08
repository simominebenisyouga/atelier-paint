<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>AtelierPaint</title>
<link rel="icon" href="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'%3E%3Ctext y='.9em' font-size='90'%3E%F0%9F%8E%A8%3C/text%3E%3C/svg%3E">
<style>
:root{--bg:#25272c;--pn:#33363c;--ln:#4a4e57;--tx:#e8eaee;--sub:#a3a9b5;--ac:#4d8dff;--acbg:#27395e}
*{box-sizing:border-box}
html,body{height:100%;margin:0}
body{background:var(--bg);color:var(--tx);font:14px/1.4 "Hiragino Sans","Yu Gothic",system-ui,sans-serif;display:flex;flex-direction:column;overflow:hidden}
header{background:var(--pn);border-bottom:1px solid var(--ln);padding:6px 8px;display:flex;flex-wrap:wrap;gap:6px;align-items:center}
header i{width:1px;height:22px;background:var(--ln);margin:0 4px}
button{font:inherit;color:var(--tx);background:#3d4149;border:1px solid var(--ln);border-radius:6px;padding:5px 10px;cursor:pointer;min-height:32px}
button:hover{background:#464b55}
button.on{background:var(--acbg);border-color:var(--ac);color:#fff}
button:focus-visible,input:focus-visible{outline:2px solid var(--ac);outline-offset:1px}
#mid{flex:1;min-height:0;display:flex}
#tools{width:66px;background:var(--pn);border-right:1px solid var(--ln);padding:6px;display:flex;flex-direction:column;gap:4px;overflow:auto}
#tools button{width:100%;padding:6px 2px;font-size:12px}
#view{flex:1;position:relative;overflow:hidden;background:#1b1c20;touch-action:none;cursor:crosshair}
#stage{position:absolute;left:0;top:0;transform-origin:0 0;background:#fff;box-shadow:0 2px 14px #0009}
#stage canvas{position:absolute;left:0;top:0}
#side{width:240px;background:var(--pn);border-left:1px solid var(--ln);padding:10px;overflow:auto;display:flex;flex-direction:column;gap:10px}
label{display:block;color:var(--sub);font-size:12px}
label span{float:right;color:var(--tx)}
input[type=range]{width:100%;accent-color:var(--ac);margin:2px 0 0}
#col{width:100%;height:42px;padding:0;border:1px solid var(--ln);border-radius:6px;background:none;cursor:pointer}
h3{margin:4px 0 0;font-size:13px;font-weight:700;display:flex;justify-content:space-between;align-items:center}
h3 div{display:flex;gap:3px}
h3 button{padding:2px 7px;min-height:26px}
#ll{display:flex;flex-direction:column;gap:3px}
.lr{display:grid;grid-template-columns:30px 1fr 58px;gap:3px 6px;align-items:center;background:#3d4149;border:1px solid var(--ln);border-radius:6px;padding:4px;cursor:pointer}
.lr.on{border-color:var(--ac);background:var(--acbg)}
.lr button{min-height:26px;padding:0}
.lr2{grid-column:1/4;display:flex;gap:6px;align-items:center}.lr2>*{flex:1;min-width:0}
.lr select{background:#2d3036;color:var(--tx);border:1px solid var(--ln);border-radius:5px;font:inherit;font-size:12px;padding:2px}
.lr.cl{margin-left:14px;border-left:3px solid var(--ac)}
.lr .cp{font-size:11px}.lr .cp.on{background:var(--acbg);border-color:var(--ac);color:#fff}
.r2.w{flex-wrap:wrap}.r2.w button{font-size:12px;padding:4px 6px;min-height:28px}
.cpt{display:flex;gap:8px;align-items:center}
#chips{position:relative;width:54px;height:46px;flex:none}
#cm,#cs{position:absolute;width:32px;height:32px;min-height:0;padding:0;border-radius:9px;border:2px solid #fff;box-shadow:0 0 0 1px #0007}
#cm{left:0;top:0;z-index:2}#cs{left:20px;top:13px;background:#fff}
#hx{flex:1;min-width:0;background:#3d4149;color:var(--tx);border:1px solid var(--ln);border-radius:6px;padding:6px 8px;font:13px ui-monospace,Menlo,Consolas,monospace}
#sv{position:relative;height:140px;border-radius:8px;overflow:hidden;touch-action:none;cursor:crosshair;border:1px solid var(--ln)}
#svc{display:block;width:100%;height:100%}
#svk{position:absolute;width:16px;height:16px;margin:-8px 0 0 -8px;border-radius:50%;border:2px solid #fff;box-shadow:0 0 0 1px #000a,inset 0 0 0 1px #0006;pointer-events:none}
#hue{-webkit-appearance:none;appearance:none;height:14px;border-radius:7px;border:1px solid var(--ln);background:linear-gradient(90deg,#f00,#ff0,#0f0,#0ff,#00f,#f0f,#f00)}
#hue::-webkit-slider-thumb{-webkit-appearance:none;width:18px;height:18px;border-radius:50%;background:#fff;border:2px solid #0008}
#hue::-moz-range-thumb{width:14px;height:14px;border-radius:50%;background:#fff;border:2px solid #0008}
.lh{display:flex;justify-content:space-between;align-items:center;margin-bottom:4px}
.lh button{padding:2px 8px;min-height:24px;font-size:11px}
.pt{display:flex;gap:4px;flex-wrap:wrap;margin-bottom:6px}
.pt button{padding:2px 9px;min-height:24px;font-size:11px;border-radius:12px}
.sw8{display:grid;grid-template-columns:repeat(8,1fr);gap:4px}
.sw8:empty::after{content:attr(data-e);color:var(--sub);font-size:11px;grid-column:1/-1}
.sq{aspect-ratio:1;min-height:0;padding:0;border-radius:6px;border:1px solid #0005;cursor:pointer;transition:transform .08s}
.sq:hover{transform:scale(1.15);background-blend-mode:normal}
.sq.on{outline:2px solid var(--ac);outline-offset:1px}
#md{width:min(98vw,1020px)}
.mh{display:flex;justify-content:space-between;align-items:center;margin-bottom:8px}
.mg{display:grid;grid-template-columns:minmax(0,1fr) 290px;gap:14px}
.mcw{position:relative;min-width:0}
.seg{display:flex;gap:4px;margin-bottom:8px}.seg button{flex:1}
#mcbox{display:flex;justify-content:center;background:#aab3c2;border-radius:8px;padding:8px}
#mc{display:block;background:#fff;box-shadow:0 2px 10px #0005;touch-action:none;cursor:grab}
#mcap{font-size:13px;margin-top:8px;min-height:2.8em;line-height:1.5}
#mleg{font-size:12px;color:var(--sub)}#mleg i{display:inline-block;width:10px;height:10px;border-radius:50%;margin:0 3px 0 10px;vertical-align:-1px}
#mhint{position:absolute;left:50%;top:70px;transform:translateX(-50%);background:var(--ac);color:#fff;padding:6px 14px;border-radius:16px;font-size:13px;pointer-events:none;box-shadow:0 2px 8px #0006;white-space:nowrap}
.mt{font-size:12px;color:var(--sub);line-height:1.5}
.mside{display:flex;flex-direction:column;gap:10px;max-height:74vh;overflow:auto;padding-right:4px}
.ms1{display:flex;flex-direction:column;gap:6px;padding-bottom:10px;border-bottom:1px solid var(--ln)}
.ms1>b{font-size:14px}
.mf{display:flex;gap:14px;align-items:center;flex-wrap:wrap;margin-top:12px;border-top:1px solid var(--ln);padding-top:10px}
.mf label{margin:0}
@media (max-width:760px){.mg{grid-template-columns:1fr}.mside{max-height:none}#mhint{white-space:normal;width:80%;text-align:center}}
#ad{width:min(92vw,420px)}
#ad h3{margin:0 0 6px}
#ai{width:100%;margin-top:8px;background:#3d4149;color:var(--tx);border:1px solid var(--ln);border-radius:6px;padding:7px 9px;font:inherit}
#toast{position:fixed;left:50%;bottom:24px;transform:translateX(-50%);background:#14171c;color:#fff;border:1px solid var(--ln);padding:10px 16px;border-radius:10px;font-size:13px;max-width:92vw;z-index:99999;opacity:0;pointer-events:none;transition:opacity .2s;text-align:center;line-height:1.5}
#toast.on{opacity:1}
#rg{position:absolute;left:0;top:0;pointer-events:none;z-index:5}
#rhd{position:absolute;left:0;top:0;right:0;bottom:0;pointer-events:none;z-index:6}
.vph{position:absolute;width:30px;height:30px;margin:-15px 0 0 -15px;border-radius:50%;border:3px solid #4d8dff;background:rgba(20,24,32,.72);color:#fff;font:bold 13px sans-serif;display:none;place-items:center;pointer-events:none;touch-action:none;user-select:none}
#rhd.edit .vph:not(.out){pointer-events:auto;cursor:grab;box-shadow:0 0 0 4px rgba(255,255,255,.35)}
.vph.out{opacity:.55;border-style:dashed}
#pbar{position:absolute;left:50%;bottom:16px;transform:translateX(-50%);display:flex;gap:8px;align-items:center;flex-wrap:wrap;justify-content:center;background:var(--pn);border:1px solid var(--ln);border-radius:12px;padding:8px 10px;box-shadow:0 4px 16px #0009;z-index:20;max-width:96%}
#pbar button{min-height:40px;padding:6px 14px}
#gl{display:grid;grid-template-columns:repeat(auto-fill,minmax(190px,1fr));gap:12px;margin-top:10px}
.gc{background:#3d4149;border:1px solid var(--ln);border-radius:10px;padding:8px;display:flex;flex-direction:column;gap:6px}
.gc.cur{border-color:var(--ac);box-shadow:0 0 0 1px var(--ac)}
.gc img{width:100%;aspect-ratio:4/3;object-fit:contain;background:#fff;border-radius:6px;cursor:pointer}
.gc .gn{font-weight:700;font-size:13px;overflow:hidden;white-space:nowrap;text-overflow:ellipsis}
.gc .gm{font-size:11px;color:var(--sub)}
.gc .gb{display:flex;gap:4px}.gc .gb button{flex:1;min-height:28px;padding:2px 4px;font-size:12px}
#gst{max-width:210px;overflow:hidden;white-space:nowrap;text-overflow:ellipsis}
#gd{width:min(96vw,880px)}
.brand{font-weight:700;font-size:16px;margin-right:8px;align-self:center;white-space:nowrap}
.hg{display:flex;flex-direction:column;gap:2px}.hg small{font-size:10px;color:var(--sub)}.hr{display:flex;gap:6px;align-items:center}
#tools small{font-size:10px;color:var(--sub);text-align:center;margin-top:6px}
.sec{border-bottom:1px solid var(--ln);padding-bottom:10px}
.sec>summary{cursor:pointer;font-weight:700;font-size:14px;padding:2px 0;list-style:none}
.sec>summary::-webkit-details-marker{display:none}
.sec>summary::before{content:"▾ ";color:var(--sub)}
.sec:not([open])>summary::before{content:"▸ "}
.in{display:flex;flex-direction:column;gap:10px;margin-top:8px}
#bd{font-size:12px;background:#2d3036;border-radius:6px;padding:6px 8px;line-height:1.5}
#tip{position:absolute;left:10px;bottom:10px;max-width:calc(100% - 20px);background:#000b;color:#fff;font-size:12px;padding:5px 10px;border-radius:6px;pointer-events:none}
#hp h4{margin:12px 0 2px;font-size:13px}
#hp ul{margin:0;padding-left:18px;color:var(--sub);font-size:13px;line-height:1.6}
@media (max-width:760px){#tools small{display:none}}
#epw{position:sticky;top:-14px;z-index:3;background:var(--pn);padding:4px 0 8px}
#epv{width:100%;border-radius:6px;background:#fff;display:block}
#ed{width:min(96vw,520px)}
#pj{width:100%;background:#3d4149;color:var(--tx);border:1px solid var(--ln);border-radius:6px;padding:8px;font:12px ui-monospace,Menlo,Consolas,monospace;margin-top:8px}
#bg,#bg2,#cb{display:grid;grid-template-columns:repeat(3,1fr);gap:4px;margin-top:4px}
#cb{margin:0 0 6px}#cb:empty{display:none}
#bg button,#bg2 button,#cb button{padding:6px 2px;font-size:12px;overflow:hidden;white-space:nowrap;text-overflow:ellipsis}
.r2{display:flex;gap:6px}.r2 button{flex:1}
button:disabled{opacity:.4;cursor:default}
dialog{background:var(--pn);color:var(--tx);border:1px solid var(--ln);border-radius:10px;padding:14px;width:min(92vw,360px);max-height:92vh;overflow:auto}
dialog::backdrop{background:#0008}
dialog h3{margin:0 0 4px;font-size:15px}
dialog label{margin-top:8px}
dialog input[type=text],dialog select{width:100%;background:#3d4149;color:var(--tx);border:1px solid var(--ln);border-radius:6px;padding:5px;font:inherit}
#epv{width:100%;border-radius:6px;margin:10px 0;background:#fff;display:block}
@media (max-width:760px){#mid{flex-direction:column}#tools{width:auto;flex-direction:row;border-right:0;border-bottom:1px solid var(--ln)}#tools button{width:auto;flex:1}#side{width:auto;height:270px;border-left:0;border-top:1px solid var(--ln)}}
</style>
</head>
<body>
<header>
  <div class="brand">🎨 AtelierPaint</div>
  <div class="hg"><small id="gst">作品</small><div class="hr">
    <button id="bgal" title="描いた絵の一覧を開きます">ギャラリー</button>
    <button id="bsvg" title="いまの絵をギャラリーに保存します(Ctrl+S)">保存</button>
  </div></div>
  <div class="hg"><small>ファイル</small><div class="hr">
    <button id="bn" title="新しいキャンバスを作ります">新規</button>
    <button id="bo" title="画像を新しいレイヤーとして読み込みます">画像を読み込む</button><input type="file" id="fi" accept="image/*" hidden>
    <button id="bs" title="絵・レイヤー・オリジナルペンを1つのHTMLファイルに保存します。開くと続きから描けます">HTMLで保存</button>
    <button id="bp" title="完成した絵を画像(PNG)で保存します">PNG書き出し</button>
  </div></div>
  <div class="hg"><small>編集</small><div class="hr">
    <button id="bu" title="Ctrl+Z">元に戻す</button>
    <button id="br" title="Ctrl+Y">やり直す</button>
  </div></div>
  <div class="hg"><small>キャンバス全体</small><div class="hr"><button id="cfx" title="絵全体(全レイヤー)を左右反転します。元に戻せます">左右反転</button><button id="cfy" title="絵全体(全レイヤー)を上下反転します。元に戻せます">上下反転</button></div></div>
  <div class="hg"><small>表示</small><div class="hr">
    <button id="zo">−</button><b id="zl" style="min-width:44px;text-align:center"></b><button id="zi">＋</button><button id="zf">全体表示</button><button id="vmx" title="描いた絵は変えずに、見え方だけを左右反転します(Fキー)。バランスの確認に便利です">鏡 左右</button><button id="vmy" title="描いた絵は変えずに、見え方だけを上下反転します">鏡 上下</button>
  </div></div>
  <div class="hg"><small>挿入</small><div class="hr"><button id="b3" title="ポーズを取れる3Dの人形を呼び出します">3Dモデル</button></div></div>
  <div class="hg" style="margin-left:auto"><small>&nbsp;</small><div class="hr"><button id="bh">使い方</button></div></div>
</header>
<div id="mid">
  <div id="tools">
    <small>描く</small>
    <button data-t="eraser">消しゴム</button>
    <button data-t="fill">塗りつぶし</button>
    <small>図形</small>
    <button data-t="line">直線</button>
    <button data-t="rect">四角</button>
    <button data-t="ellipse">楕円</button>
    <small>選択</small>
    <button data-t="rsel">四角選択</button>
    <button data-t="lasso">投げ縄</button>
    <button data-t="wand">自動選択</button>
    <button data-t="move">移動</button>
    <small>その他</small>
    <button data-t="eye">スポイト</button>
    <button data-t="hand">手のひら</button>
    <button data-t="rcol">色置換</button>
    <button data-t="ruler">定規</button>
  </div>
  <div id="view"><div id="stage"><canvas id="so" style="z-index:999;pointer-events:none;transform-origin:0 0"></canvas><canvas id="ov" style="z-index:1000;pointer-events:none"></canvas></div><div id="tip"></div>
    <canvas id="rg"></canvas><div id="rhd"></div>
    <div id="pbar" style="display:none"><span class="mt" id="pbt">クリックで点を打ちます</span><button id="pok" class="on" disabled>✓ 閉じて確定 (Enter)</button><button id="pbk">↶ 1つ戻す</button><button id="pcn">✕ 取り消し (Esc)</button></div></div>
  <div id="side">
    <details class="sec" open><summary>ブラシ</summary><div class="in">
      <div><label>標準のペン</label><div id="bg">
        <button data-t="pen" class="on">ペン</button>
        <button data-t="gpen">Gペン</button>
        <button data-t="brush">筆</button>
        <button data-t="pencil">鉛筆</button>
        <button data-t="crayon">クレヨン</button>
        <button data-t="marker">マーカー</button>
        <button data-t="airbrush">エアブラシ</button>
        <button data-t="watercolor">水彩</button>
        <button data-t="spray">スプレー</button>
      </div></div>
      <div><label>特殊ペン(ぼかし・指先・スタンプなど)</label><div id="bg2"></div></div>
      <div><label>オリジナルのペン</label><div id="cb"></div><div class="r2"><button id="bm">＋ ペンを作る</button><button id="be" disabled>編集・コピー</button></div><div class="r2 w" style="margin-top:6px"><button id="bt" title="いまの選択範囲を、スタンプのペン先にします">選択範囲からペン先を作る</button><button id="bpx">書き出し・読み込み</button></div></div>
      <div id="bd"></div>
      <label>太さ<span id="szv"></span><input type="range" id="sz" min="1" max="120" value="8"></label>
      <label>不透明度(濃さ)<span id="opv"></span><input type="range" id="op" min="5" max="100" value="100"></label>
      <label>手ぶれ補正<span id="stv"></span><input type="range" id="st" min="0" max="90" value="30"></label>
    </div></details>
    <details class="sec" open><summary>色</summary><div class="in">
      <div class="cpt">
        <div id="chips"><button id="cs" title="サブカラー(クリックかXキーでメインと入れ替え)"></button><button id="cm" title="メインカラー(いま描く色)"></button></div>
        <input type="text" id="hx" maxlength="7" spellcheck="false" aria-label="色コード">
        <button id="ey" title="画面の色をスポイトで拾います">スポイト</button>
      </div>
      <div id="sv"><canvas id="svc" width="240" height="140"></canvas><i id="svk"></i></div>
      <input type="range" id="hue" min="0" max="360" value="210" aria-label="色相">
      <div><div class="lh"><label>パレット</label></div><div class="pt" id="pt"></div><div class="sw8" id="pl"></div></div>
      <div><div class="lh"><label>最近使った色</label></div><div class="sw8" id="hs" data-e="描くとここに並びます"></div></div>
      <div><div class="lh"><label>お気に入り</label><button id="fa" title="いまの色を登録">＋ 登録</button></div><div class="sw8" id="fv" data-e="「＋ 登録」で色を保存。ダブルクリックで削除"></div></div>
    </div></details>
    <details class="sec" open><summary>選択範囲・色の変更</summary><div class="in">
      <div class="r2"><button data-sm="new" class="on">新規</button><button data-sm="add">追加</button><button data-sm="sub">引く</button></div>
      <div class="r2"><button data-lm="free" class="on" title="ドラッグで自由に囲みます">投げ縄: フリーハンド</button><button data-lm="poly" title="クリックで点を打って、多角形で囲みます">投げ縄: 多角形</button></div>
      <div class="mt">多角形は、クリックで点を打っていきます。終わるときは、画面の下の「閉じて確定」を押すか、オレンジの輪(最初の点)をクリック、ダブルクリック、右クリック、Enterのどれかです。</div>
      <div class="r2 w"><button id="sa">全選択</button><button id="sd" disabled>解除</button><button id="si" disabled>反転</button></div>
      <div class="r2 w"><button id="sg" disabled title="選択範囲を少し広げます">広げる</button><button id="ss" disabled title="選択範囲を少し縮めます">縮める</button><button id="sm" disabled title="ギザギザをなめらかにします">なめらか</button></div>
      <div class="r2 w"><button id="sc" disabled>範囲を消去</button><button id="sf" disabled>描画色で塗る</button><button id="sl" disabled>新規レイヤーに</button></div>
      <label>「移動」の拡大縮小<span id="tzv"></span><input type="range" id="tz" min="10" max="300" value="100" disabled></label>
      <label>「移動」の回転(向きの変更)<span id="trv"></span><input type="range" id="tr" min="-180" max="180" value="0" disabled></label>
      <label>色の許容範囲(自動選択・色置換)<span id="tolv"></span><input type="range" id="tol" min="0" max="100" value="20"></label>
      <label>色を変える(選択範囲があればその中だけ)</label>
      <div class="r2 w"><button id="tc" title="線画などの色を、透明度はそのままで描画色に変えます">描画色に染める</button><button id="ch" title="色相・彩度・明るさなどを調整します">色相・彩度・明るさ…</button></div>
    </div></details>
    <details class="sec"><summary>パース定規</summary><div class="in">
      <div class="r2"><button data-rm="0" class="on">なし</button><button data-rm="1">1点</button><button data-rm="2">2点</button><button data-rm="3">3点</button></div>
      <div class="mt" id="rlh"></div>
      <label style="display:flex;gap:8px;align-items:center"><input type="checkbox" id="rsn" checked>定規に沿って描く(Altキーで一時的に外す)</label>
      <label style="display:flex;gap:8px;align-items:center"><input type="checkbox" id="rsh" checked>ガイド線を表示</label>
      <label>線の数<span id="rnv"></span><input type="range" id="rn" min="6" max="80" value="24"></label>
      <label>線の濃さ<span id="rov"></span><input type="range" id="ro" min="10" max="100" value="55"></label>
      <label>水平線の高さ<span id="rhrv"></span><input type="range" id="rhr" min="-50" max="150" value="45"></label>
      <label id="rrx">消失点の横位置<span id="rx1v"></span><input type="range" id="rx1" min="-50" max="150" value="50"></label>
      <label id="rrs2">消失点の間隔(広いほど、ゆるやかなパース)<span id="rspv"></span><input type="range" id="rsp" min="10" max="400" value="80"></label>
      <label id="rrv">縦の消失点の高さ(下なら見下ろし、上なら見上げ)<span id="rv3v"></span><input type="range" id="rv3" min="-300" max="400" value="250"></label>
      <label id="rrg" style="display:flex;gap:8px;align-items:center"><input type="checkbox" id="rgd">横線のグリッドも出す(1点のみ)</label>
      <div class="r2 w"><button id="rtl" title="消失点の丸い点をドラッグして動かします">消失点を動かす</button><button id="rrs">初期位置に戻す</button></div>
      <div class="mt">消失点が画面の外にあるときは、画面を縮小(0キー・「全体表示」)すると見えます。</div>
    </div></details>
    <details class="sec" open><summary>レイヤー</summary><div class="in">
      <div class="r2 w"><button id="la">＋ 追加</button><button id="ld">複製</button><button id="lu">↑ 上へ</button><button id="lw">↓ 下へ</button><button id="lx">削除</button></div>
      <div class="r2 w"><button id="lf">加工…</button><button id="lh" title="選んでいるレイヤーだけを左右反転します">レイヤー左右反転</button><button id="lv" title="選んでいるレイヤーだけを上下反転します">レイヤー上下反転</button><button id="lm">下と結合</button></div>
      <div id="ll"></div>
    </div></details>
  </div>
</div>
<dialog id="ed">
  <h3>ペン工房</h3>
  <div id="epw"><canvas id="epv" width="420" height="110"></canvas></div>
  <label>名前<input type="text" id="en" maxlength="12"></label>
  <label>ひな形から始める<select id="et"></select></label>
  <details class="sec" open><summary>① 動き・形・大きさ</summary><div class="in">
    <label>動き<select id="emd"><option value="paint">描く</option><option value="blur">ぼかす</option><option value="smudge">指でのばす(色を引きずる)</option><option value="erase">消す</option></select></label>
    <label>形<select id="esh"><option value="nib">丸・平ペン</option><option value="dots">粒</option><option value="star">星</option><option value="heart">ハート</option><option value="leaf">葉</option><option value="square">四角</option><option value="diamond">ひし形</option><option value="line">線(ひとすじ)</option><option value="image">自分の絵(ペン先)</option></select></label>
    <div class="r2 w"><button id="etp" title="いまの選択範囲(なければレイヤーの絵)を、ペン先にします">選択範囲からペン先を作る</button></div>
    <div class="mt" id="etn"></div>
    <div id="ef1"></div>
  </div></details>
  <details class="sec"><summary>② ゆらぎ・向き</summary><div class="in">
    <div id="ef2"></div>
    <label style="display:flex;gap:8px;align-items:center"><input type="checkbox" id="efo">描く方向にペン先を向ける</label>
  </div></details>
  <details class="sec"><summary>③ 色の変化</summary><div class="in">
    <div id="ef3"></div>
    <label style="display:flex;gap:8px;align-items:center"><input type="checkbox" id="eti">ペン先を描画色で塗る(自分の絵のとき)</label>
  </div></details>
  <details class="sec"><summary>④ 質感・重ね方</summary><div class="in">
    <label>質感<select id="etx"><option value="none">なし</option><option value="noise">ざらざら</option><option value="paper">紙</option><option value="canvas">キャンバス</option><option value="dots">網点</option></select></label>
    <div id="ef4"></div>
    <label style="display:flex;gap:8px;align-items:center"><input type="checkbox" id="eov">一筆の中で重ねても濃くならない</label>
  </div></details>
  <div class="r2 w" style="margin-top:12px"><button id="ek" class="on">保存</button><button id="ek2">別名で保存</button><button id="ec">キャンセル</button><button id="ex">削除</button></div>
</dialog>
<dialog id="fd">
  <h3>レイヤーの加工</h3>
  <div id="fnote" style="font-size:12px;color:var(--sub)"></div>
  <div id="ff"></div>
  <label style="display:flex;gap:8px;align-items:center"><input type="checkbox" id="f_inv">色を反転</label>
  <label style="display:flex;gap:8px;align-items:center"><input type="checkbox" id="f_gray">モノクロにする</label>
  <div class="r2" style="margin-top:12px"><button id="fk" class="on">適用</button><button id="fr">リセット</button><button id="fc">キャンセル</button></div>
</dialog>
<dialog id="md">
  <div class="mh"><h3>3Dモデル(ポーズ人形)</h3><button id="mcl">閉じる</button></div>
  <div class="mg">
    <div class="mcw">
      <div class="seg" id="mmd"><button data-mm="pose" class="on">ポーズを付ける</button><button data-mm="view">向きを変える</button><button data-mm="move">位置を動かす</button></div>
      <div id="mcbox"><canvas id="mc" width="640" height="480"></canvas></div>
      <div id="mcap"></div>
      <div id="mleg">関節の色:<i style="background:#4d8dff"></i>腕<i style="background:#2fb57a"></i>脚<i style="background:#9a6bff"></i>体・頭<i style="background:#ff9f1c"></i>選択中</div>
      <div id="mhint">青い点をドラッグして、腕や脚を動かしてみよう</div>
    </div>
    <div class="mside">
      <div class="ms1"><b>① ポーズを選ぶ</b>
        <div class="pt" id="mpc"></div><div class="pt" id="mpr"></div><div class="pt" id="mup"></div>
        <div class="r2 w"><button id="mun">ひとつ戻す</button><button id="mmi">左右反転</button><button id="mre">リセット</button></div>
        <button id="msv">いまのポーズを保存</button></div>
      <div class="ms1"><b>② 向きと大きさを決める</b>
        <div class="pt" id="mvw"></div>
        <label>人形の大きさ<span id="mzv"></span><input type="range" id="mzm" min="30" max="140" value="80"></label>
        <div class="mt">左の画面が、そのまま絵に貼り付けられます。位置は「位置を動かす」で決めます。</div></div>
      <details class="sec" id="mdt"><summary>くわしい設定(必要なときだけ)</summary><div class="in">
        <label>調整する関節<select id="mj"></select></label>
        <div class="mt">左右は、人形から見た左右です。</div>
        <label>前後に曲げる<span id="mrvx"></span><input type="range" id="mrx" min="-180" max="180" value="0"></label>
        <label>ひねる<span id="mrvy"></span><input type="range" id="mry" min="-180" max="180" value="0"></label>
        <label>横に開く<span id="mrvz"></span><input type="range" id="mrz" min="-180" max="180" value="0"></label>
        <button id="mjr">この関節をもとに戻す</button>
        <label>頭の大きさ<span id="mhsv"></span><input type="range" id="mhs" min="70" max="140" value="100"></label>
        <label>体の太さ<span id="mbtv"></span><input type="range" id="mbt" min="70" max="140" value="100"></label>
        <label>腰の位置: 高さ<span id="mpyv"></span><input type="range" id="mpy" min="-60" max="60" value="0"></label>
        <label>腰の位置: 左右<span id="mpxv"></span><input type="range" id="mpx" min="-60" max="60" value="0"></label>
        <label>腰の位置: 前後<span id="mpzv"></span><input type="range" id="mpz" min="-60" max="60" value="0"></label>
        <label>光の向き<input type="range" id="mli" min="0" max="360" value="300"></label>
        <label>色<select id="mst"><option value="skin">マネキン(肌色)</option><option value="gray">グレー</option><option value="white">白</option><option value="sil">シルエット(黒)</option></select></label>
        <label style="display:flex;gap:8px;align-items:center"><input type="checkbox" id="mjo" checked>関節の点を表示(貼り付けには入りません)</label>
        <label style="display:flex;gap:8px;align-items:center"><input type="checkbox" id="msh" checked>地面の影を表示(貼り付けには入りません)</label>
      </div></details>
    </div>
  </div>
  <div class="mf"><b>③ 絵に貼り付ける</b>
    <label style="display:flex;gap:8px;align-items:center"><input type="checkbox" id="mtr" checked>下絵用に半透明にする</label>
    <button id="mok" class="on">貼り付ける(新しいレイヤー)</button>
  </div>
</dialog>
<dialog id="pd">
  <h3>ペンの書き出し・読み込み</h3>
  <div class="mt">自作ペンを、文字として書き出します。コピーして、メモなどに保存したり、友だちに送れます。読み込むときは、ここに貼り付けて「読み込む」を押します。</div>
  <textarea id="pj" rows="8" spellcheck="false"></textarea>
  <div class="r2 w" style="margin-top:10px"><button id="pcp">書き出してコピー</button><button id="pim" class="on">読み込む</button><button id="pcl">閉じる</button></div>
</dialog>
<dialog id="ad">
  <h3 id="at"></h3>
  <div class="mt" id="am"></div>
  <input type="text" id="ai" maxlength="40">
  <div class="r2" id="ab" style="margin-top:12px"></div>
</dialog>
<div id="toast"></div>
<dialog id="gd">
  <div class="mh"><h3>ギャラリー</h3><button id="gcl">閉じる</button></div>
  <div class="r2 w" style="margin-bottom:6px"><button id="gnw">＋ 新しい絵を描く</button><button id="gsv" class="on">いまの絵を保存</button></div>
  <div class="mt" id="gus"></div>
  <div id="gl"></div>
</dialog>
<dialog id="hp">
  <h3>AtelierPaintの使い方</h3>
  <h4>描く</h4><ul><li>右の「ブラシ」でペンを選び、太さと濃さを決めて描きます。</li><li>左の列に、消しゴム・塗りつぶし・図形・スポイトがあります。</li><li>「手ぶれ補正」を上げると、線がなめらかになります。</li><li>色は、四角と色相バー、パレット、最近使った色、お気に入りから選べます。サブカラーとはXキーで入れ替えできます。</li></ul>
  <h4>オリジナルのペン</h4><ul><li>「＋ ペンを作る」で、ひな形から自分のペンを作れます。</li><li>作ったペンは一覧に並び、「編集」でいつでも直せます。</li></ul>
  <h4>レイヤー</h4><ul><li>透明なシートを重ねて描けます。クリックで選択、ダブルクリックで名前の変更。</li><li>「クリップ」: 下のレイヤーの形の中にだけ表示します(影や色塗りに便利)。</li><li>プルダウンで合成モード(乗算など)、スライダーで透明度を変えます。</li><li>「加工…」で、ぼかし・色調整・移動・回転・拡大縮小ができます。</li></ul>
  <h4>画面の操作</h4><ul><li>拡大縮小: ホイール、または二本指。</li><li>移動: 手のひらツール、スペース+ドラッグ、または二本指。</li></ul>
  <h4>選択範囲と色の変更</h4><ul><li>左の「四角選択」「投げ縄」「自動選択」で範囲を作ります。Shiftで追加、Altで引きます。</li><li>「移動」で、選択範囲の中の絵を動かします(範囲がなければレイヤー全体)。まわりの四角い点で拡大縮小(Shiftで縦横比を自由に)、上の丸い点で向きの変更(回転)ができます。</li><li>「描画色に染める」で線画の色をまとめて変更、「色置換」でクリックした色を描画色に置き換えます。</li><li>Ctrl+D 選択解除 / Delete 範囲を消去</li><li>多角形の投げ縄: 点を打ったあと、「閉じて確定」・最初の点(オレンジの輪)・ダブルクリック・右クリック・Enterのどれでも確定できます。Escで取り消し、Backspaceで1つ戻ります。</li></ul>
  <h4>パース定規</h4><ul><li>右の「パース定規」で、1点・2点・3点を選ぶと、消失点に向かうガイド線が出ます。</li><li>「定規に沿って描く」をオンにすると、ペンや直線の線が、消失点に向かう線(1点と2点では、水平・垂直の線)に吸い付きます。描き始めて少し動かした向きに近い線が選ばれ、そのまま引けます。Altキーを押している間は、自由に描けます。</li><li>左の「定規」(Pキー)を選ぶと、消失点の丸い点をドラッグして動かせます。スライダーでも調整できます。</li></ul>
  <h4>反転</h4><ul><li>「鏡 左右」「鏡 上下」(Fキーで左右)は、絵は変えずに、見え方だけを反転します。左右のバランスや、形のゆがみを確認するときに使います。もう一度押すと元に戻ります。</li><li>「キャンバス全体」の「左右反転」「上下反転」は、全レイヤーを実際に反転します。「元に戻す」で戻せます。</li><li>「レイヤー左右反転」「レイヤー上下反転」は、選んでいるレイヤーだけを反転します。</li></ul>
  <h4>ギャラリー</h4><ul><li>「保存」(Ctrl+S)で、いまの絵をギャラリーに保存します。初めて保存するときは、名前をつけます。</li><li>一度保存した絵は、変更があると45秒ごとに自動で保存されます。</li><li>「ギャラリー」を開くと、保存した絵が一覧で出ます。「開く」で続きから描けます。名前の変更、複製、削除もできます。</li><li>保存場所は、このブラウザの中です。大事な絵は、「HTMLで保存」でもファイルに残しておくと安心です。</li></ul>
  <h4>ぼかしペンと自作ペン</h4><ul><li>「特殊ペン」に、ぼかし、指先(色を引きずる)、混色、点描、油絵、チョーク、墨、虹、星、ハート、きらきら、葉、飛沫があります。</li><li>「＋ ペンを作る」の「ペン工房」で、動き(描く・ぼかす・指でのばす・消す)、形、ゆらぎ、色の変化、質感を組み合わせて、自分のペンを作れます。特殊ペンを選んで「編集・コピー」を押すと、それを元に作り直せます。</li><li>「選択範囲からペン先を作る」で、自分の絵をスタンプのペン先にできます。</li><li>「書き出し・読み込み」で、自作ペンを文字として共有できます。</li></ul>
  <h4>3Dモデル</h4><ul><li>上の「3Dモデル」で人形が開きます。順番は、①ポーズを選ぶ → ②向きと大きさを決める → ③絵に貼り付ける、です。</li><li>ポーズは、青い点(関節)をドラッグして付けます。何もない所をドラッグすると人形が回ります。</li><li>ひな形は、「立つ」「動く」「座る・寝る」「腕・手」の4つの分類から選べます(全部で28種類)。</li><li>左の画面は、そのまま絵に貼り付けられます。人形は輪郭線つきで、半透明にして下絵として使えます。</li><li>よく使うポーズは「ポーズを保存」で残せます(HTMLで保存したファイルにも入ります)。</li></ul>
  <h4>保存</h4><ul><li>「HTMLで保存」: 絵・レイヤー・オリジナルペンがそのまま入ります。開くと続きから描けます。</li><li>「PNG書き出し」: 完成した絵を画像で保存します。</li></ul>
  <h4>ショートカット</h4><ul><li>Ctrl+Z 元に戻す / Ctrl+Y やり直し</li><li>[ ] 太さの変更 / スペース 一時的に手のひら / X メインとサブの色を入れ替え</li><li>B ペン / E 消しゴム / G 塗りつぶし / I スポイト / H 手のひら / V 移動 / M 四角選択 / L 投げ縄 / W 自動選択</li><li>0 全体表示 / + − 拡大縮小</li></ul>
  <div class="r2" style="margin-top:12px"><button id="hc" class="on">閉じる</button></div>
</dialog>
<script id="proj" type="application/json"></script>
<script>
(function(){
var pristine='<!DOCTYPE html>\n'+document.documentElement.outerHTML;
var $=function(i){return document.getElementById(i)};
var W=1200,H=800,layers=[],cur=null,tool='pen',undoS=[],redoS=[];
var st=$('stage'),view=$('view'),ov=$('ov'),octx=ov.getContext('2d');
var Z=1,PX=20,PY=20,color='#1d2733',size=8,op=1,stab=.3,mode=null,ps,a,sm,lastP,sp=false,tp={},pz,spd=0,bx;

function applyT(){st.style.transform='translate('+(PX+(VMX?W*Z:0))+'px,'+(PY+(VMY?H*Z:0))+'px) scale('+(VMX?-Z:Z)+','+(VMY?-Z:Z)+')';$('zl').textContent=Math.round(Z*100)+'%';drawRuler()}
function fitView(){var r=view.getBoundingClientRect();Z=Math.min(1,(r.width-40)/W,(r.height-40)/H);PX=(r.width-W*Z)/2;PY=(r.height-H*Z)/2;applyT()}
function zoom(f,cx,cy){var r=view.getBoundingClientRect();cx=cx==null?r.width/2:cx;cy=cy==null?r.height/2:cy;var n=Math.max(.1,Math.min(8,Z*f));PX=cx-(cx-PX)*n/Z;PY=cy-(cy-PY)*n/Z;Z=n;applyT()}
function mk(name,url){var c=document.createElement('canvas');c.width=W;c.height=H;st.appendChild(c);
  var l={c:c,x:c.getContext('2d',{willReadFrequently:true}),n:name,v:true,o:1,m:'normal',clip:false};
  if(url){var im=new Image();im.onload=function(){l.x.drawImage(im,0,0);dirty()};im.src=url}return l}
function init(w,h,data){W=w;H=h;layers.forEach(function(l){st.removeChild(l.c);if(l.d)st.removeChild(l.d)});layers=[];undoS=[];redoS=[];
  st.style.width=W+'px';st.style.height=H+'px';ov.width=W;ov.height=H;var bf=document.createElement('canvas');bf.width=W;bf.height=H;bx=bf.getContext('2d');initSel();
  if(data)data.forEach(function(d){var l=mk(d.n,d.u);l.v=d.v;l.o=d.o;l.m=d.m||'normal';l.clip=!!d.k;layers.push(l)});else layers.push(mk('レイヤー 1'));
  cur=layers[layers.length-1];rl();fitView()}

var BM=[['normal','通常'],['multiply','乗算'],['screen','スクリーン'],['overlay','オーバーレイ'],['darken','比較(暗)'],['lighten','比較(明)'],['color-dodge','覆い焼き'],['color-burn','焼き込み'],['hard-light','ハードライト'],['soft-light','ソフトライト'],['difference','差の絶対値'],['exclusion','除外'],['hue','色相'],['saturation','彩度'],['color','カラー'],['luminosity','輝度']];
function baseOf(i){var j=i-1;while(j>=0&&layers[j].clip)j--;return j>=0?layers[j]:null}
function isClip(l,i){return l.clip&&baseOf(i)}
function ensureD(l){if(l.d)return;var d=document.createElement('canvas');d.width=W;d.height=H;st.appendChild(d);l.d=d;l.dx=d.getContext('2d')}
var dq=false;
function dirty(){if(dq)return;dq=true;requestAnimationFrame(function(){dq=false;refreshAll()})}
function refreshAll(){if(!layers.some(function(l){return l.clip}))return;
  layers.forEach(function(l,i){var b=isClip(l,i);if(!b)return;ensureD(l);var c=l.dx;
    c.globalCompositeOperation='source-over';c.clearRect(0,0,W,H);c.drawImage(l.c,0,0);
    c.globalCompositeOperation='destination-in';c.drawImage(b.c,0,0);c.globalCompositeOperation='source-over'})}
function src(l,i){if(isClip(l,i)){refreshAll();return l.d}return l.c}
function rl(){mdirty();var ll=$('ll');ll.innerHTML='';
  layers.forEach(function(l,i){var cl=!!isClip(l,i),b=baseOf(i);if(l.clip)ensureD(l);
    var showC=l.v&&!cl,showD=l.v&&cl&&b&&b.v;
    [l.c,l.d].forEach(function(el,k){if(!el)return;el.style.zIndex=i+1;el.style.opacity=l.o;el.style.mixBlendMode=l.m;el.style.display=(k?showD:showC)?'block':'none'})});
  refreshAll();
  for(var i=layers.length-1;i>=0;i--)(function(l,i){
    var d=document.createElement('div');d.className='lr'+(l===cur?' on':'')+(isClip(l,i)?' cl':'');
    var e=document.createElement('button');e.textContent=l.v?'表':'非';e.onclick=function(ev){ev.stopPropagation();l.v=!l.v;rl()};
    var n=document.createElement('span');n.textContent=l.n;
    var cp=document.createElement('button');cp.className='cp'+(l.clip?' on':'');cp.textContent='クリップ';cp.title='下のレイヤーの形で切り抜いて表示';
    cp.onclick=function(ev){ev.stopPropagation();l.clip=!l.clip;rl()};
    var w=document.createElement('div');w.className='lr2';
    var sel=document.createElement('select');BM.forEach(function(m){var o=document.createElement('option');o.value=m[0];o.textContent=m[1];sel.appendChild(o)});
    sel.value=l.m;sel.onchange=function(){l.m=sel.value;rl()};sel.onclick=function(ev){ev.stopPropagation()};
    var r=document.createElement('input');r.type='range';r.min=0;r.max=100;r.value=l.o*100;
    r.oninput=function(){mdirty();l.o=r.value/100;[l.c,l.d].forEach(function(el){if(el)el.style.opacity=l.o})};r.onclick=function(ev){ev.stopPropagation()};
    d.onclick=function(){flt=null;cur=l;rl()};d.ondblclick=function(){askText('レイヤーの名前',l.n,function(s){if(s){l.n=s;rl()}})};
    w.appendChild(sel);w.appendChild(r);
    d.appendChild(e);d.appendChild(n);d.appendChild(cp);d.appendChild(w);ll.appendChild(d)})(layers[i],i);moveUI()}

function pushU(){mdirty();undoS.push({l:cur,d:cur.x.getImageData(0,0,W,H)});if(undoS.length>25)undoS.shift();redoS=[]}
function snap(){flt=null;pushU()}
function swap(from,to){var s=from.pop();if(!s)return;
  if(s.all){var inv=s.all.filter(function(e){return layers.indexOf(e.l)>=0});if(!inv.length)return swap(from,to);
    to.push({all:inv.map(function(e){return{l:e.l,d:e.l.x.getImageData(0,0,W,H)}}),rf:s.rf});inv.forEach(function(e){e.l.x.putImageData(e.d,0,0)});if(s.rf){rlFlip(s.rf);maskFlip(s.rf)}return}
  if(layers.indexOf(s.l)<0)return swap(from,to);
  to.push({l:s.l,d:s.l.x.getImageData(0,0,W,H)});s.l.x.putImageData(s.d,0,0)}
function undo(){if(flt)deselect();swap(undoS,redoS);dirty();moveUI()}function redo(){if(flt)deselect();swap(redoS,undoS);dirty();moveUI()}

function rgb(h){return[parseInt(h.substr(1,2),16),parseInt(h.substr(3,2),16),parseInt(h.substr(5,2),16)]}
function rgba(h,al){var c=rgb(h);return'rgba('+c[0]+','+c[1]+','+c[2]+','+al+')'}
function pt(e){var r=view.getBoundingClientRect();return{x:X1(e.clientX-r.left),y:Y1(e.clientY-r.top),p:e.pointerType==='pen'?(e.pressure||.5):1}}

function refresh(){var c=cur.x;c.putImageData(undoS[undoS.length-1].d,0,0);c.save();var cb1=CBo();c.globalAlpha=op*(cb1?cb1.flow/100:.6);c.drawImage(bx.canvas,0,0);c.restore()}
function stamp(x,y,p){var c=cur.x,s=size*(.3+.7*p);
  var cb0=CBo();if(cb0){var sp0=cb0.mode==='blur'||cb0.mode==='smudge',ovp=cb0.ov&&!sp0;STK.src=cur.x;stampC(ovp?bx:c,cb0,x,y,p,ovp,color,size,op,STK,STK.has?STK.dir:null);return}
  if(tool==='marker'){c=bx;s=size*1.4;c.save();c.fillStyle=color;c.translate(x,y);c.rotate(-.6);c.fillRect(-s/2,-s/5,s,s*.4);c.restore();return}
  if(tool==='crayon'){c.save();c.fillStyle=color;c.globalAlpha=op*.8;var n=Math.max(6,s*1.6|0),r=s/2;
    for(var i=0;i<n;i++){var a2=Math.random()*6.283,d2=Math.sqrt(Math.random())*r;c.fillRect(x+Math.cos(a2)*d2,y+Math.sin(a2)*d2,1.6,1.6)}c.restore();return}
  if(tool==='spray'){c.save();c.fillStyle=color;c.globalAlpha=op*.9;var n2=Math.max(8,s*.8|0),r2=s*.9;
    for(var j=0;j<n2;j++){var a3=Math.random()*6.283,d3=r2*Math.random();c.fillRect(x+Math.cos(a3)*d3,y+Math.sin(a3)*d3,1.2,1.2)}c.restore();return}
  if(tool==='watercolor'){var R=s*.75*(.85+Math.random()*.3),ox=x+(Math.random()-.5)*s*.2,oy=y+(Math.random()-.5)*s*.2,al=op*.07,g2=c.createRadialGradient(ox,oy,0,ox,oy,R);
    g2.addColorStop(0,rgba(color,al*.7));g2.addColorStop(.7,rgba(color,al));g2.addColorStop(1,rgba(color,0));c.fillStyle=g2;c.fillRect(ox-R,oy-R,R*2,R*2);return}
  if(tool==='gpen')s=size*(.12+.88*p);
  if(tool==='brush')s=size*(.1+.9*p);
  c.save();
  if(tool==='eraser'){c.globalCompositeOperation='destination-out';c.fillStyle='#000';c.globalAlpha=op}
  else if(tool==='airbrush'){s*=1.8;var g=c.createRadialGradient(x,y,0,x,y,s/2);g.addColorStop(0,rgba(color,op*.22));g.addColorStop(1,rgba(color,0));c.fillStyle=g;c.fillRect(x-s/2,y-s/2,s,s);c.restore();return}
  else if(tool==='pencil'){c.fillStyle=color;c.globalAlpha=op*.5;x+=(Math.random()-.5)*s*.25;y+=(Math.random()-.5)*s*.25;s*=.85}
  else{c.fillStyle=color;c.globalAlpha=op}
  c.beginPath();c.arc(x,y,s/2,0,7);c.fill();c.restore()}
var STK={dist:0,dir:0,has:false,src:null,carry:0,len:0};
function seg(A,B){var d=Math.hypot(B.x-A.x,B.y-A.y),cb=CBo(),sp=Math.max(.8,cb?size*cb.sz/100*cb.gap/100:size*({pencil:.2,crayon:.12,spray:.22,watercolor:.09,marker:.07}[tool]||.1));
  if(d<1e-6)return;
  if(d>.6){var nd=Math.atan2(B.y-A.y,B.x-A.x);if(!STK.has){STK.dir=nd;STK.has=true}else{var df=nd-STK.dir;while(df>Math.PI)df-=2*Math.PI;while(df<-Math.PI)df+=2*Math.PI;STK.dir+=df*.45}}
  /* 間隔は、マウスの動きの細かさに関係なく、道のりで測る */
  var pos=sp-(STK.carry||0),n=0,d0=STK.len||0;
  while(pos<=d){var t=pos/d;STK.dist=d0+pos;stamp(A.x+(B.x-A.x)*t,A.y+(B.y-A.y)*t,A.p+(B.p-A.p)*t);pos+=sp;n++}
  STK.carry=(STK.carry||0)+d-n*sp;STK.len=d0+d}
function shape(c,A,B,al){c.save();c.globalAlpha=al;c.strokeStyle=color;c.lineWidth=size;c.lineCap='round';c.beginPath();
  if(tool==='line'){c.moveTo(A.x,A.y);c.lineTo(B.x,B.y)}
  else if(tool==='rect')c.rect(A.x,A.y,B.x-A.x,B.y-A.y);
  else c.ellipse((A.x+B.x)/2,(A.y+B.y)/2,Math.abs(B.x-A.x)/2,Math.abs(B.y-A.y)/2,0,0,7);
  c.stroke();c.restore()}

function fill(x0,y0){x0|=0;y0|=0;var im=cur.x.getImageData(0,0,W,H),d=im.data,i0=(y0*W+x0)*4,
  tr=d[i0],tg=d[i0+1],tb=d[i0+2],ta=d[i0+3],f=rgb(color),fa=Math.round(op*255);
  function m(i){return Math.abs(d[i]-tr)+Math.abs(d[i+1]-tg)+Math.abs(d[i+2]-tb)+Math.abs(d[i+3]-ta)<=90}
  var seen=new Uint8Array(W*H),stk=[x0,y0];
  while(stk.length){var y=stk.pop(),x=stk.pop();if(seen[y*W+x]||!m((y*W+x)*4))continue;
    var xl=x,xr=x;while(xl>0&&!seen[y*W+xl-1]&&m((y*W+xl-1)*4))xl--;while(xr<W-1&&!seen[y*W+xr+1]&&m((y*W+xr+1)*4))xr++;
    for(var xx=xl;xx<=xr;xx++){var q=y*W+xx,k=q*4;seen[q]=1;d[k]=f[0];d[k+1]=f[1];d[k+2]=f[2];d[k+3]=fa;
      if(y>0&&!seen[q-W])stk.push(xx,y-1);if(y<H-1&&!seen[q+W])stk.push(xx,y+1)}}
  cur.x.putImageData(im,0,0)}
function comp(){var t=document.createElement('canvas');t.width=W;t.height=H;var c=t.getContext('2d');c.fillStyle='#fff';c.fillRect(0,0,W,H);
  layers.forEach(function(l,i){var b=baseOf(i);if(!l.v||(isClip(l,i)&&!b.v))return;c.globalAlpha=l.o;c.globalCompositeOperation=l.m==='normal'?'source-over':l.m;c.drawImage(src(l,i),0,0);c.globalAlpha=1;c.globalCompositeOperation='source-over'});return t}
function hex(n){return('0'+n.toString(16)).slice(-2)}
function inb(p){return p.x>=0&&p.y>=0&&p.x<W&&p.y<H}

view.addEventListener('pointerdown',function(e){
  view.setPointerCapture(e.pointerId);
  if(e.pointerType==='touch'){tp[e.pointerId]={x:e.clientX,y:e.clientY};
    if(Object.keys(tp).length>=2){
      if(mode==='sel')octx.clearRect(0,0,W,H);
      if(mode==='brush'||mode==='shape'){var s0=undoS.pop();if(s0)s0.l.x.putImageData(s0.d,0,0);octx.clearRect(0,0,W,H)}
      var ks=Object.keys(tp),A1=tp[ks[0]],B1=tp[ks[1]];
      pz={d:Math.hypot(B1.x-A1.x,B1.y-A1.y)||1,mx:(A1.x+B1.x)/2,my:(A1.y+B1.y)/2,Z:Z,PX:PX,PY:PY};mode='pinch';return}}
  if(!e.isPrimary&&e.pointerType!=='touch')return;
  var p=pt(e);
  if(e.button===1||tool==='hand'||sp){mode='pan';ps={x:e.clientX-PX,y:e.clientY-PY};return}
  if(!cur.v)return;if(tool==='ruler')return;
  if(tool==='eye'){if(inb(p)){var px=comp().getContext('2d').getImageData(p.x|0,p.y|0,1,1).data;color='#'+hex(px[0])+hex(px[1])+hex(px[2]);setColor(color)}return}
  if(tool==='fill'){if(inb(p)){snap();fill(p.x,p.y)}return}
  if(tool==='lasso'&&lmode==='poly'){if(e.button===2)return;polyClick(p,e);return}
  if(tool==='rsel'||tool==='lasso'){mode='sel';a=p;lastP=p;lpts=[p];return}
  if(tool==='wand'){wand(p,e);return}
  if(tool==='rcol'){recolorAt(p);return}
  if(tool==='move'){var hh=hitHandle(p);if(startMove(p,hh))mode=hh===8?'rotate':hh>=0?'scale':'move';return}
  snap();a=p;lastP=p;sm={x:p.x,y:p.y,p:p.p};spd=0;bx.clearRect(0,0,W,H);STK={dist:0,dir:0,has:false,src:cur.x,carry:0,len:0};if(RL)RL.lock=null;
  if(tool==='line'||tool==='rect'||tool==='ellipse')mode='shape';else{mode='brush';stamp(p.x,p.y,p.p);if(tool==='marker'||(CBo()&&CBo().ov))refresh()}});
view.addEventListener('pointermove',function(e){
  if(tp[e.pointerId])tp[e.pointerId]={x:e.clientX,y:e.clientY};
  if(mode==='pinch'){var ks=Object.keys(tp);if(ks.length<2)return;
    var A1=tp[ks[0]],B1=tp[ks[1]],r=view.getBoundingClientRect(),d=Math.hypot(B1.x-A1.x,B1.y-A1.y)||1,
    mx=(A1.x+B1.x)/2,my=(A1.y+B1.y)/2,nz=Math.max(.1,Math.min(8,pz.Z*d/pz.d)),
    cx=(pz.mx-r.left-pz.PX)/pz.Z,cy=(pz.my-r.top-pz.PY)/pz.Z;
    Z=nz;PX=mx-r.left-cx*nz;PY=my-r.top-cy*nz;applyT();return}
  if(!mode)return;
  if(mode==='pan'){PX=e.clientX-ps.x;PY=e.clientY-ps.y;applyT();return}
  var p=pt(e);if((mode==='brush'||(mode==='shape'&&tool==='line'))&&RL&&RL.m&&RL.snap&&!e.altKey)p=snapP(p);lastP=p;
  if(mode==='sel'){selPrev(p);return}
  if(mode==='move'){moveTo(p);return}
  if(mode==='scale'){scaleTo(p,e.shiftKey);return}
  if(mode==='rotate'){rotateTo(p,e.shiftKey);return}
  if(mode==='shape'){octx.clearRect(0,0,W,H);shape(octx,a,p,op);return}
  var k=1-stab,nx=sm.x+(p.x-sm.x)*k,ny=sm.y+(p.y-sm.y)*k;
  spd=spd*.7+Math.hypot(nx-sm.x,ny-sm.y)*.3;
  var pe=p.p;if(e.pointerType!=='pen'&&(tool==='gpen'||tool==='brush'))pe=Math.max(.2,1-spd/(tool==='brush'?22:45));
  var cbo=CBo();if(cbo)pe=e.pointerType==='pen'?1-cbo.pr/100*(1-p.p):1-Math.min(.85,spd*cbo.sv/100/22);
  var n={x:nx,y:ny,p:sm.p+(pe-sm.p)*.35};seg(sm,n);sm=n;if(tool==='marker'||(cbo&&cbo.ov))refresh()});
function up(e){delete tp[e.pointerId];
  if(mode==='pinch'){if(!Object.keys(tp).length)mode=null;return}
  if(mode==='sel'){finishSel(lastP,e);mode=null;return}
  if(mode==='move'||mode==='scale'||mode==='rotate'){endMove();mode=null;return}
  if(mode==='shape'){octx.clearRect(0,0,W,H);shape(cur.x,a,lastP,op)}mode=null}
view.addEventListener('pointerup',up);view.addEventListener('pointercancel',up);
view.addEventListener('pointermove',function(){if(mode)dirty()});view.addEventListener('pointerdown',function(){dirty()});view.addEventListener('pointerup',function(){dirty()});view.addEventListener('pointercancel',function(){dirty()});
view.addEventListener('wheel',function(e){e.preventDefault();var r=view.getBoundingClientRect();zoom(e.deltaY<0?1.15:1/1.15,e.clientX-r.left,e.clientY-r.top)},{passive:false});

document.addEventListener('keydown',function(e){
  if(e.target.tagName==='INPUT'&&e.target.type!=='range'&&e.target.type!=='color')return;
  var k=e.key.toLowerCase();
  if((e.ctrlKey||e.metaKey)&&k==='z'){e.preventDefault();e.shiftKey?redo():undo()}
  else if((e.ctrlKey||e.metaKey)&&k==='y'){e.preventDefault();redo()}
  else if(k===' '){e.preventDefault();sp=true}
  else if(k==='['){setSize(Math.max(1,size-(size>20?5:1)))}
  else if(k===']'){setSize(Math.min(120,size+(size>=20?5:1)))}});
document.addEventListener('keyup',function(e){if(e.key===' ')sp=false});

document.querySelectorAll('[data-t]').forEach(function(b){b.onclick=function(){selTool(b.dataset.t)}});
function setSize(v){size=+v;$('sz').value=size;$('szv').textContent=size+'px'}
$('sz').oninput=function(){setSize(this.value)};
$('op').oninput=function(){op=this.value/100;$('opv').textContent=this.value+'%'};
$('st').oninput=function(){stab=this.value/100;$('stv').textContent=this.value+'%'};

setSize(8);$('opv').textContent='100%';$('stv').textContent='30%';

$('bu').onclick=undo;$('br').onclick=redo;
$('zi').onclick=function(){zoom(1.25)};$('zo').onclick=function(){zoom(.8)};$('zf').onclick=fitView;
$('la').onclick=function(){var l=mk('レイヤー '+(layers.length+1));layers.splice(layers.indexOf(cur)+1,0,l);cur=l;rl()};
$('ld').onclick=function(){var l=mk(cur.n+' のコピー');l.x.drawImage(cur.c,0,0);l.o=cur.o;l.m=cur.m;l.clip=cur.clip;layers.splice(layers.indexOf(cur)+1,0,l);cur=l;rl()};
$('lu').onclick=function(){var i=layers.indexOf(cur);if(i<layers.length-1){layers.splice(i,1);layers.splice(i+1,0,cur);rl()}};
$('lw').onclick=function(){var i=layers.indexOf(cur);if(i>0){layers.splice(i,1);layers.splice(i-1,0,cur);rl()}};
$('lx').onclick=function(){if(layers.length<2)return;var i=layers.indexOf(cur);st.removeChild(cur.c);if(cur.d)st.removeChild(cur.d);layers.splice(i,1);cur=layers[Math.max(0,i-1)];rl()};
$('bn').onclick=function(){askText('新しいキャンバスの大きさ(幅x高さ)','1200x800',function(s){var m=s.match(/(\d+)\s*[x×*,]\s*(\d+)/i);if(!m){toast('「1200x800」のように入力してください');return}
  gLeave(function(){init(Math.min(4000,+m[1]),Math.min(4000,+m[2]));curId=null;curName='無題';gDirty=false;gHDirty=false})},'いまの絵は、保存していないと消えます。')};
$('bo').onclick=function(){$('fi').click()};
$('fi').onchange=function(){var f=this.files[0];if(!f)return;var r=new FileReader();r.onload=function(){
  var im=new Image();im.onload=function(){var l=mk(f.name);var s=Math.min(1,W/im.width,H/im.height);l.x.drawImage(im,0,0,im.width*s,im.height*s);layers.push(l);cur=l;rl()};im.src=r.result};r.readAsDataURL(f);this.value=''};
function dl(blob,name){var u=URL.createObjectURL(blob),a2=document.createElement('a');a2.href=u;a2.download=name;document.body.appendChild(a2);a2.click();a2.remove();setTimeout(function(){URL.revokeObjectURL(u)},4000);toast(name+' を書き出しました。ダウンロードフォルダを確認してください。(保存されないときは、ファイルをダウンロードして、ブラウザで直接開いてお使いください)',6000)}
$('bp').onclick=function(){comp().toBlob(function(b){dl(b,'atelier-paint.png')})};
$('bs').onclick=function(){
  var j=JSON.stringify({w:W,h:H,b:CB,f:FAV,m:MS,u:UPO,rl:rlPlain(),l:layers.map(function(l){return{n:l.n,v:l.v,o:l.o,m:l.m,k:l.clip?1:0,u:l.c.toDataURL()}})}).replace(/</g,'\\u003c');
  var re=new RegExp('(<scr'+'ipt id="proj"[^>]*>)[\\s\\S]*?(<\\/scr'+'ipt>)');
  dl(new Blob([pristine.replace(re,function(m,x,y){return x+j+y})],{type:'text/html'}),'atelier-paint.html');gHDirty=false};

/* ---- オリジナルペン ---- */
var CB={};
var BI={};
function CBo(){return CB[tool]||BI[tool]||null}
function saveCB(){try{localStorage.setItem('ps_brushes',JSON.stringify(CB))}catch(err){}}
function loadCB(){try{var s=JSON.parse(localStorage.getItem('ps_brushes')||'{}');for(var k in s)CB[k]=s[k]}catch(err){}}
function selTool(t){tool=t;document.querySelectorAll('[data-t]').forEach(function(x){x.classList.toggle('on',x.dataset.t===t)});
  view.style.cursor=t==='hand'?'grab':t==='move'?'move':'crosshair';$('be').disabled=!(CB[t]||BI[t]);var cbT=CB[t]||BI[t];if(cbT&&cbT.tip)tipOf(cbT);showTip(t);moveUI();polyCancel();rlTool()}
function renderCB(){var el=$('cb');el.innerHTML='';
  Object.keys(CB).forEach(function(k){var b=document.createElement('button');b.dataset.t=k;b.textContent=CB[k].n;b.title=CB[k].n;
    b.className=tool===k?'on':'';b.onclick=function(){selTool(k)};el.appendChild(b)});
  $('be').disabled=!(CB[tool]||BI[tool]);showTip(tool)}
/* ---- ペンの部品(形・色の変化・質感・ぼかし・指でのばす) ---- */
var DEF={mode:'paint',shape:'nib',sz:100,hard:100,flow:100,gap:10,pr:70,sv:0,jit:0,ang:0,asp:100,dens:50,ov:0,follow:0,rj:0,sj:0,oj:0,hj:0,sa:0,lj:0,rb:0,mx:0,tx:0,tex:'none',tp:0,tint:0,bl:50,tip:null};
function nB(B){for(var k in DEF)if(B[k]===undefined)B[k]=DEF[k];return B}
var SCRS=[];
function scratch(i,w,h){var c=SCRS[i]||(SCRS[i]=document.createElement('canvas'));c.width=Math.max(1,Math.round(w));c.height=Math.max(1,Math.round(h));return c}
function hsl2rgb(h,sa,l){h=((h%360)+360)%360;sa=Math.max(0,Math.min(1,sa));l=Math.max(0,Math.min(1,l));
  var c=(1-Math.abs(2*l-1))*sa,x=c*(1-Math.abs((h/60)%2-1)),m=l-c/2,t=[[c,x,0],[x,c,0],[0,c,x],[0,x,c],[x,0,c],[c,0,x]][Math.floor(h/60)%6];
  return[Math.round((t[0]+m)*255),Math.round((t[1]+m)*255),Math.round((t[2]+m)*255)]}
function rgb2hsl(r,g,b){r/=255;g/=255;b/=255;var mx=Math.max(r,g,b),mn=Math.min(r,g,b),l=(mx+mn)/2,d=mx-mn,h=0,sa=0;
  if(d){sa=d/(1-Math.abs(2*l-1));if(mx===r)h=((g-b)/d+6)%6;else if(mx===g)h=(b-r)/d+2;else h=(r-g)/d+4;h*=60}return[h,sa,l]}
var TEXC={};
function texTile(kind){if(TEXC[kind])return TEXC[kind];var t=document.createElement('canvas');t.width=t.height=128;var c=t.getContext('2d'),im=c.createImageData(128,128),d=im.data;
  for(var y=0;y<128;y++)for(var x=0;x<128;x++){var h,dx,dy;
    if(kind==='noise')h=Math.random();
    else if(kind==='paper')h=Math.pow(Math.random(),2)*.9;
    else if(kind==='canvas'){h=((((x>>1)+(y>>1))&1)?.5:.12)+Math.random()*.22}
    else{dx=(x%8)-3.5;dy=(y%8)-3.5;h=Math.sqrt(dx*dx+dy*dy)<2.7?0:1}
    var i=(y*128+x)*4;d[i]=d[i+1]=d[i+2]=0;d[i+3]=Math.round(h*255)}
  c.putImageData(im,0,0);TEXC[kind]=t;return t}
var TIPC={};
function tipOf(B,cb){if(!B.tip)return null;var e=TIPC[B.tip];if(e&&e.c)return e.c;
  if(!e){e=TIPC[B.tip]={c:null,w:[]};var im=new Image();im.onload=function(){var k=document.createElement('canvas');k.width=im.width;k.height=im.height;k.getContext('2d').drawImage(im,0,0);e.c=k;e.w.forEach(function(f){f()});e.w=[]};im.src=B.tip}
  if(cb)e.w.push(cb);return null}
function shapePath(g,kind,r){g.beginPath();
  if(kind==='square')g.rect(-r,-r,2*r,2*r);
  else if(kind==='diamond'){g.moveTo(0,-r*1.2);g.lineTo(r*1.2,0);g.lineTo(0,r*1.2);g.lineTo(-r*1.2,0);g.closePath()}
  else if(kind==='star'){for(var i=0;i<10;i++){var a=-Math.PI/2+i*Math.PI/5,rr=i%2?r*.46:r*1.05;if(i)g.lineTo(Math.cos(a)*rr,Math.sin(a)*rr);else g.moveTo(Math.cos(a)*rr,Math.sin(a)*rr)}g.closePath()}
  else if(kind==='heart'){g.moveTo(0,r*.9);g.bezierCurveTo(-r*1.55,-r*.05,-r*.9,-r*1.1,0,-r*.42);g.bezierCurveTo(r*.9,-r*1.1,r*1.55,-r*.05,0,r*.9);g.closePath()}
  else if(kind==='leaf'){g.moveTo(0,-r*1.3);g.quadraticCurveTo(r*1.1,-r*.2,0,r*1.3);g.quadraticCurveTo(-r*1.1,-r*.2,0,-r*1.3);g.closePath()}
  else if(kind==='line')g.rect(-r,-r*.16,2*r,r*.32);
  else g.arc(0,0,r,0,7)}
function paintShape(g,B,cc,rr,al){var col='rgb('+cc[0]+','+cc[1]+','+cc[2]+')';
  if(B.shape==='dots'){g.fillStyle=col;g.globalAlpha=al;var n=Math.max(4,(rr*2*B.dens/100*2)|0);
    for(var i=0;i<n;i++){var t=Math.random()*6.283,d=Math.sqrt(Math.random())*rr;g.fillRect(Math.cos(t)*d-.8,Math.sin(t)*d-.8,1.6,1.6)}return}
  if(B.shape==='image'){var im=tipOf(B);if(!im)return;var k=2*rr/Math.max(im.width,im.height),w=im.width*k,h=im.height*k,src=im;
    if(B.tint){var T=scratch(3,im.width,im.height),t2=T.getContext('2d');t2.drawImage(im,0,0);t2.globalCompositeOperation='source-in';t2.fillStyle=col;t2.fillRect(0,0,im.width,im.height);src=T}
    g.globalAlpha=al;g.drawImage(src,-w/2,-h/2,w,h);return}
  if(B.shape==='nib'){
    if(B.hard>=97){g.fillStyle=col;g.globalAlpha=al}
    else{var gr=g.createRadialGradient(0,0,0,0,0,rr),hd=B.hard/100,rg='rgba('+cc[0]+','+cc[1]+','+cc[2]+',';gr.addColorStop(0,rg+al+')');gr.addColorStop(hd,rg+al+')');gr.addColorStop(1,rg+'0)');g.fillStyle=gr;g.globalAlpha=1}
    g.beginPath();g.arc(0,0,rr,0,7);g.fill();return}
  g.fillStyle=col;g.globalAlpha=al;shapePath(g,B.shape,rr);g.fill()}
function sampleC(c,x,y){try{var d=c.getImageData(Math.round(x),Math.round(y),1,1).data;return[d[0],d[1],d[2],d[3]]}catch(e){return null}}
function softMask(t,R,r,hard){t.globalCompositeOperation='destination-in';var g=t.createRadialGradient(R,R,0,R,R,Math.max(1,r)),hd=Math.min(.97,Math.max(0,hard/100*.9));
  g.addColorStop(0,'rgba(0,0,0,1)');g.addColorStop(hd,'rgba(0,0,0,1)');g.addColorStop(1,'rgba(0,0,0,0)');t.fillStyle=g;t.fillRect(0,0,R*2,R*2);t.globalCompositeOperation='source-over'}
function blurAt(c,x,y,r,str,B){var b=Math.min(160,B.bl/100*r*.8+.6),pad=Math.ceil(b*2.2),R=Math.min(520,Math.ceil(r)+pad),S=R*2,x0=Math.round(x-R),y0=Math.round(y-R),T=scratch(0,S,S),t=T.getContext('2d'),
  k=Math.max(1,Math.floor(b/5)),sw=Math.max(1,Math.ceil(S/k)),hasF='filter' in t;
  if(k===1&&hasF){t.filter='blur('+b+'px)';t.drawImage(c.canvas,x0,y0,S,S,0,0,S,S);t.filter='none'}
  else{var sm=scratch(1,sw,sw),s2=sm.getContext('2d');s2.imageSmoothingQuality='high';s2.drawImage(c.canvas,x0,y0,S,S,0,0,sw,sw);
    var sb=sm;if(hasF){sb=scratch(4,sw,sw);var s4=sb.getContext('2d');s4.filter='blur('+Math.max(.6,b/k)+'px)';s4.drawImage(sm,0,0);s4.filter='none'}
    t.imageSmoothingQuality='high';t.drawImage(sb,0,0,sw,sw,0,0,S,S)}
  softMask(t,R,r,B.hard);c.save();c.globalAlpha=str;c.drawImage(T,x0,y0);c.restore()}
function smudgeAt(c,st,x,y,r,str){var R=Math.max(2,Math.ceil(r)),S=R*2,x0=Math.round(x-R),y0=Math.round(y-R);
  if(!st.sm||st.smR!==R){var K=document.createElement('canvas');K.width=K.height=S;K.getContext('2d').drawImage(c.canvas,x0,y0,S,S,0,0,S,S);st.sm=K;st.smR=R;return}
  var N=scratch(1,S,S),n=N.getContext('2d');n.drawImage(c.canvas,x0,y0,S,S,0,0,S,S);
  var T=scratch(0,S,S),t=T.getContext('2d');t.drawImage(st.sm,0,0);softMask(t,R,r,60);
  c.save();c.globalAlpha=str;c.drawImage(T,x0,y0);c.restore();
  var q=.6+.35*str,K2=document.createElement('canvas');K2.width=K2.height=S;var k2=K2.getContext('2d');k2.drawImage(N,0,0);k2.globalAlpha=q;k2.drawImage(st.sm,0,0);st.sm=K2}
function stampC(c,B,x,y,m,ov,col,sz0,opa,st,dir){
  nB(B);st=st||{};var PI=Math.PI,s=sz0*B.sz/100*Math.max(.08,m);
  if(B.sj)s*=1-B.sj/100*Math.random()*.85;
  if(B.tp&&st.dist!=null){var L=Math.max(10,sz0*B.sz/100*5*B.tp/50);s*=Math.min(1,.12+.88*Math.min(1,st.dist/L))}
  var md=B.mode;
  if(md==='blur'){blurAt(c,x,y,s/2,Math.min(1,opa*B.flow/100),B);return}
  if(md==='smudge'){smudgeAt(c,st,x,y,s/2,Math.min(1,opa*B.flow/100));return}
  var al=ov?1:opa*B.flow/100;if(B.oj)al*=1-B.oj/100*Math.random();
  var cc=rgb(col);
  if(B.mx){var sp=sampleC(st.src||c,x,y);st.carry=st.carry||cc.slice();
    if(sp&&sp[3]>30){var kk=B.mx/100*.2;st.carry=[st.carry[0]+(sp[0]-st.carry[0])*kk,st.carry[1]+(sp[1]-st.carry[1])*kk,st.carry[2]+(sp[2]-st.carry[2])*kk]}
    var rk=.02*(1-B.mx/100);st.carry=[st.carry[0]+(cc[0]-st.carry[0])*rk,st.carry[1]+(cc[1]-st.carry[1])*rk,st.carry[2]+(cc[2]-st.carry[2])*rk];
    cc=[Math.round(st.carry[0]),Math.round(st.carry[1]),Math.round(st.carry[2])]}
  if(B.hj||B.sa||B.lj||B.rb){var hs=rgb2hsl(cc[0],cc[1],cc[2]),H=hs[0]+(B.hj?(Math.random()-.5)*2*B.hj:0)+(B.rb&&st.dist!=null?st.dist*B.rb/100*.6:0),
      S2=hs[1]+(B.sa?(Math.random()-.5)*2*B.sa/100:0),Lq=hs[2]+(B.lj?(Math.random()-.5)*2*B.lj/100:0);
    if(B.rb){if(hs[1]<.3)S2=.85;if(Lq>.85||Lq<.15)Lq=.55}
    cc=hsl2rgb(H,S2,Lq)}
  if(B.jit){var j=s*B.jit/100;x+=(Math.random()-.5)*j*2;y+=(Math.random()-.5)*j*2}
  var rot=B.ang*PI/180+(B.rj?(Math.random()-.5)*2*PI*B.rj/100:0);
  if(B.follow&&dir!=null)rot+=dir+((B.shape==='nib'||B.shape==='leaf'||B.shape==='square'||B.shape==='diamond')?PI/2:0);
  var rr=s/2,erase=md==='erase',tx=B.tex!=='none'&&B.tx>0;
  if(!tx){c.save();c.translate(x,y);c.rotate(rot);c.scale(1,B.asp/100);if(erase)c.globalCompositeOperation='destination-out';paintShape(c,B,cc,rr,al);c.restore();return}
  var hh=Math.ceil(rr*1.5)+3,T=scratch(2,hh*2,hh*2),g=T.getContext('2d');
  g.translate(hh,hh);g.rotate(rot);g.scale(1,B.asp/100);paintShape(g,B,cc,rr,1);
  g.setTransform(1,0,0,1,0,0);g.globalCompositeOperation='destination-out';g.globalAlpha=B.tx/100;g.translate(-(x-hh),-(y-hh));
  g.fillStyle=g.createPattern(texTile(B.tex),'repeat');g.fillRect(x-hh,y-hh,hh*2,hh*2);
  c.save();c.globalAlpha=al;if(erase)c.globalCompositeOperation='destination-out';c.drawImage(T,x-hh,y-hh);c.restore()}

/* ---- ペン工房 ---- */
var FL=[['sz','サイズ倍率',10,300,'%',1],['hard','硬さ(ふちのぼかし)',0,100,'%',1],['flow','濃さ(ぼかす・のばす時は強さ)',3,100,'%',1],['gap','間隔',3,200,'%',1],
  ['pr','筆圧の影響',0,100,'%',1],['sv','速く描くと細くなる',0,100,'%',1],['tp','描き始めの細さ(入り)',0,100,'%',1],
  ['jit','位置のばらつき',0,150,'%',2],['sj','大きさのばらつき',0,100,'%',2],['oj','濃さのばらつき',0,100,'%',2],['rj','向きのばらつき',0,100,'%',2],
  ['ang','角度',-180,180,'°',2],['asp','平たさ(100で真円)',10,100,'%',2],['dens','粒の多さ(粒のみ)',5,100,'%',2],
  ['hj','色相のゆらぎ',0,180,'°',3],['sa','彩度のゆらぎ',0,100,'%',3],['lj','明るさのゆらぎ',0,60,'%',3],['rb','虹色(色が変わる速さ)',0,100,'%',3],
  ['mx','混色(下の色を拾う)',0,100,'%',3],['bl','ぼかしの強さ(ぼかす時)',0,400,'%',3],['tx','質感の強さ',0,100,'%',4]];
var PRE={
  pen:{sz:100,hard:100,flow:100,gap:10,pr:70},
  brush:{sz:110,hard:90,flow:100,gap:8,pr:90,sv:80},
  pencil:{sz:85,hard:100,flow:50,gap:20,pr:60,jit:12},
  crayon:{shape:'dots',sz:100,flow:80,gap:12,pr:50,dens:80},
  marker:{sz:140,hard:100,flow:60,gap:7,pr:0,ang:-34,asp:40,ov:1},
  airbrush:{sz:180,hard:0,flow:22,gap:10,pr:60},
  watercolor:{sz:75,hard:70,flow:7,gap:9,pr:50,jit:10},
  spray:{shape:'dots',sz:180,flow:90,gap:22,pr:30,dens:80},
  blur:{mode:'blur',sz:140,hard:60,flow:70,gap:8,pr:30,bl:100},
  blurS:{mode:'blur',sz:170,hard:40,flow:90,gap:8,pr:30,bl:260},
  smudge:{mode:'smudge',sz:120,hard:70,flow:70,gap:6,pr:40},
  mixer:{sz:110,hard:55,flow:70,gap:8,pr:50,mx:75,jit:4},
  stipple:{shape:'dots',sz:170,flow:90,gap:22,pr:40,dens:65,sj:45,jit:30},
  oil:{sz:120,hard:92,flow:92,gap:6,pr:50,asp:34,follow:1,sj:8,lj:6,tex:'canvas',tx:55,mx:30},
  chalk:{sz:110,hard:100,flow:88,gap:10,pr:60,asp:80,jit:6,tex:'paper',tx:80},
  sumi:{sz:110,hard:88,flow:95,gap:6,pr:90,sv:70,tp:60,tex:'paper',tx:22},
  rainbow:{sz:100,hard:100,flow:100,gap:8,pr:60,rb:55},
  star:{shape:'star',sz:110,flow:100,gap:130,pr:30,rj:70,sj:35,hj:25},
  heart:{shape:'heart',sz:110,flow:100,gap:140,pr:30,rj:25,sj:30,hj:12},
  sparkle:{shape:'star',sz:60,flow:100,gap:90,pr:20,jit:140,rj:100,sj:70,hj:180,lj:18},
  leaf:{shape:'leaf',sz:100,flow:100,gap:100,pr:20,follow:1,rj:20,sj:35,hj:30,lj:10,jit:40},
  splatter:{shape:'dots',sz:230,flow:90,gap:50,pr:30,dens:90,jit:100,sj:70}};
var PNAME={pen:'ペン',brush:'筆',pencil:'鉛筆',crayon:'クレヨン',marker:'マーカー',airbrush:'エアブラシ',watercolor:'水彩',spray:'スプレー',blur:'ぼかし',blurS:'ぼかし(強)',smudge:'指先',mixer:'混色',stipple:'点描',oil:'油絵',chalk:'チョーク',sumi:'墨',rainbow:'虹',star:'星',heart:'ハート',sparkle:'きらきら',leaf:'葉',splatter:'飛沫'};
var BIL=[['blur','ぼかし','ぼかします。同じところをなぞるほど、ぼやけます。強さは、ペン工房の「ぼかしの強さ」で変えられます。'],['blurS','ぼかし(強)','とても強くぼかします。広い範囲を、なめらかにするときに。'],['smudge','指先','絵の具を指でのばしたように、色を引きずります。'],
  ['mixer','混色','下の色を拾いながら、混ぜて描きます。'],['stipple','点描','細かい点を散らして描きます。'],['oil','油絵','描く方向に沿った、毛先の跡が出る厚塗りです。'],
  ['chalk','チョーク','かすれた、粉っぽい線です。'],['sumi','墨','入りと抜きのある、墨のような線です。'],['rainbow','虹','なぞるほど、色が虹のように変わります。'],
  ['star','星','星のスタンプを並べます。'],['heart','ハート','ハートのスタンプを並べます。'],['sparkle','きらきら','色とりどりの星が散ります。'],['leaf','葉','葉っぱを、描く方向に並べます。'],['splatter','飛沫','絵の具が飛び散ったような点を散らします。']];
function ext(a,b){var r={};for(var k in a)r[k]=a[k];for(var k2 in b)r[k2]=b[k2];return r}
BIL.forEach(function(b){BI['bi:'+b[0]]=ext(nB(ext(PRE[b[0]],{})),{n:b[1],d:b[2]})});
function renderBI(){var el=$('bg2');el.innerHTML='';BIL.forEach(function(b){var k='bi:'+b[0],bt=document.createElement('button');bt.dataset.t=k;bt.textContent=b[1];bt.title=b[2];
  bt.className=tool===k?'on':'';bt.onclick=function(){selTool(k)};el.appendChild(bt)})}
var eid=null,etip=null,ptmp=document.createElement('canvas');ptmp.width=420;ptmp.height=110;
function readEd(){var P={mode:$('emd').value,shape:$('esh').value,ov:$('eov').checked?1:0,follow:$('efo').checked?1:0,tint:$('eti').checked?1:0,tex:$('etx').value,tip:etip};
  FL.forEach(function(f){P[f[0]]=+$('e_'+f[0]).value});return P}
function fillEd(P){P=nB(ext(P,{}));$('emd').value=P.mode;$('esh').value=P.shape;$('eov').checked=!!P.ov;$('efo').checked=!!P.follow;$('eti').checked=!!P.tint;$('etx').value=P.tex;etip=P.tip||null;
  $('etn').textContent=etip?'ペン先: 自分の絵が入っています':'';
  FL.forEach(function(f){$('e_'+f[0]).value=P[f[0]];$('v_'+f[0]).textContent=P[f[0]]+f[4]});preview()}
function preview(){var P=readEd(),cv=$('epv'),c=cv.getContext('2d'),w=cv.width,h=cv.height,sp0=P.mode!=='paint',useBuf=P.ov&&!sp0,tg=useBuf?ptmp.getContext('2d'):c;
  c.clearRect(0,0,w,h);c.fillStyle='#fff';c.fillRect(0,0,w,h);
  if(sp0){var cols=['#e5383b','#f2c200','#2e9e5b','#2b5cff','#9a6bff'],bw=w/cols.length;cols.forEach(function(k,i){c.fillStyle=k;c.fillRect(i*bw,0,bw+1,h)});c.fillStyle='rgba(255,255,255,.55)';for(var i=0;i<h;i+=16)c.fillRect(0,i,w,3)}
  if(useBuf)tg.clearRect(0,0,w,h);
  if(P.shape==='image')tipOf(P,preview);
  var base=sp0?30:22,sp=Math.max(.8,base*P.sz/100*P.gap/100),st={dist:0,src:c},prev=null,acc=0;
  for(var i=0;i<=700;i++){var t=i/700,q={x:24+t*(w-48),y:h/2+Math.sin(t*7.5)*(h*.28),m:1-P.pr/100*(1-(.25+.75*Math.sin(t*3.1416)))};
    if(prev){var dd=Math.hypot(q.x-prev.x,q.y-prev.y);acc+=dd;st.dist+=dd;if(dd>.3)st.dir=Math.atan2(q.y-prev.y,q.x-prev.x)}
    if(!prev||acc>=sp){acc=0;stampC(tg,P,q.x,q.y,q.m,useBuf,'#1d2733',base,1,st,st.dir==null?null:st.dir)}prev=q}
  if(useBuf){c.save();c.globalAlpha=P.flow/100;c.drawImage(ptmp,0,0);c.restore()}}
function openEd(key,base){eid=key;var P=key?CB[key]:(base?ext(base,{}):ext(PRE.pen,{}));
  $('en').value=key?P.n:(base&&base.n?base.n+'のコピー':'マイペン '+(Object.keys(CB).length+1));$('et').value='pen';$('ex').style.display=key?'':'none';
  delete P.d;fillEd(P);$('ed').showModal()}
(function(){var h=['','','','',''];FL.forEach(function(f){h[f[5]]+='<label>'+f[1]+'<span id="v_'+f[0]+'"></span><input type="range" id="e_'+f[0]+'" min="'+f[2]+'" max="'+f[3]+'"></label>'});
  for(var i=1;i<=4;i++)$('ef'+i).innerHTML=h[i];
  FL.forEach(function(f){$('e_'+f[0]).oninput=function(){$('v_'+f[0]).textContent=this.value+f[4];preview()}});
  ['emd','esh','etx'].forEach(function(id){$(id).onchange=preview});['eov','efo','eti'].forEach(function(id){$(id).onchange=preview});
  var o='';Object.keys(PRE).forEach(function(k){o+='<option value="'+k+'">'+PNAME[k]+'</option>'});$('et').innerHTML=o})();
$('et').onchange=function(){var keep=etip;fillEd(PRE[this.value]);if(keep&&PRE[this.value].shape==='image'){etip=keep}};
$('bm').onclick=function(){openEd(null)};
$('be').onclick=function(){if(CB[tool])openEd(tool);else if(BI[tool])openEd(null,BI[tool])};
$('ec').onclick=function(){$('ed').close()};
function savePen(asNew){var P=readEd();P.n=($('en').value||'マイペン').slice(0,12);
  if(P.shape==='image'&&!P.tip){toast('「自分の絵」のペン先がありません。「選択範囲からペン先を作る」を押してください');return}
  var key=(!asNew&&eid)||('c:'+Date.now().toString(36)+Math.random().toString(36).slice(2,5));CB[key]=P;saveCB();renderCB();selTool(key);$('ed').close();toast('「'+P.n+'」を保存しました')}
$('ek').onclick=function(){savePen(false)};$('ek2').onclick=function(){savePen(true)};
$('ex').onclick=function(){if(!eid)return;askYes('このペンを削除しますか?',function(){delete CB[eid];saveCB();var was=tool===eid;renderCB();if(was)selTool('pen');$('ed').close()})};
/* 選択範囲から、自分の絵のペン先を作る */
function makeTip(){var B=boxOf();if(!B){toast('ペン先にしたい絵を、選択範囲にするか、レイヤーに描いてください');return null}
  var src=hasSel?selContent():cur.c,w=B.x1-B.x0,h=B.y1-B.y0,k=Math.min(1,128/Math.max(w,h)),c=document.createElement('canvas');
  c.width=Math.max(1,Math.round(w*k));c.height=Math.max(1,Math.round(h*k));c.getContext('2d').drawImage(src,B.x0,B.y0,w,h,0,0,c.width,c.height);return c.toDataURL('image/png')}
$('bt').onclick=function(){var tip=makeTip();if(!tip)return;openEd(null,{shape:'image',tip:tip,sz:150,gap:100,pr:0,tint:0});$('en').value='スタンプ '+(Object.keys(CB).length+1)};
$('etp').onclick=function(){var tip=makeTip();if(!tip)return;etip=tip;$('esh').value='image';$('etn').textContent='ペン先: 自分の絵が入っています';preview()};
/* 自作ペンの書き出し・読み込み(文字として、コピー&貼り付けで共有できる) */
$('bpx').onclick=function(){$('pj').value='';$('pd').showModal()};
$('pcl').onclick=function(){$('pd').close()};
$('pcp').onclick=function(){var t=JSON.stringify({atelierpaint:'pens',pens:CB});$('pj').value=t;$('pj').select();var done=function(){toast('書き出しました。コピーできます')};
  try{navigator.clipboard.writeText(t).then(done,function(){try{document.execCommand('copy')}catch(e){}done()})}catch(e){try{document.execCommand('copy')}catch(e2){}done()}};
$('pim').onclick=function(){var o;try{o=JSON.parse($('pj').value)}catch(e){toast('読み込めませんでした。書き出した文字を、そのまま貼り付けてください');return}
  var pens=o&&(o.pens||o);if(!pens||typeof pens!=='object'){toast('読み込めるペンがありません');return}
  var n=0;Object.keys(pens).forEach(function(k){var P=pens[k];if(!P||typeof P!=='object'||!P.n)return;
    if(P.tip&&String(P.tip).indexOf('data:image/')!==0)P.tip=null;
    CB['c:'+Date.now().toString(36)+Math.random().toString(36).slice(2,5)+n]=nB(P);n++});
  if(!n){toast('読み込めるペンがありません');return}saveCB();renderCB();toast(n+'個のペンを読み込みました')};
renderBI();
loadCB();

/* ---- レイヤー加工 ---- */
var FF=[['bl','ぼかし',0,40,'px',0],['br','明るさ',0,200,'%',100],['ct','コントラスト',0,200,'%',100],['sa','彩度',0,300,'%',100],['hu','色相',-180,180,'°',0],
  ['tx','移動 横',-1000,1000,'px',0],['ty','移動 縦',-1000,1000,'px',0],['ro','回転',-180,180,'°',0],['sc','拡大縮小',10,300,'%',100]];
(function(){var h='';FF.forEach(function(f){h+='<label>'+f[1]+'<span id="fv_'+f[0]+'"></span><input type="range" id="f_'+f[0]+'" min="'+f[2]+'" max="'+f[3]+'" value="'+f[5]+'"></label>'});
  $('ff').innerHTML=h;FF.forEach(function(f){$('f_'+f[0]).oninput=fPrev});$('f_inv').onchange=$('f_gray').onchange=fPrev})();
function fEl(){return isClip(cur,layers.indexOf(cur))?cur.d:cur.c}
function fRead(){var P={inv:$('f_inv').checked,gray:$('f_gray').checked};FF.forEach(function(f){P[f[0]]=+$('f_'+f[0]).value});return P}
function fStr(P){return'blur('+P.bl+'px) brightness('+P.br+'%) contrast('+P.ct+'%) saturate('+P.sa+'%) hue-rotate('+P.hu+'deg)'+(P.inv?' invert(1)':'')+(P.gray?' grayscale(1)':'')}
function fPrev(){var P=fRead();if(hasSel&&fsrc){fPrevSel(P);FF.forEach(function(f){$('fv_'+f[0]).textContent=P[f[0]]+f[4]});return}var el=fEl();el.style.filter=fStr(P);
  el.style.transform='translate('+P.tx+'px,'+P.ty+'px) rotate('+P.ro+'deg) scale('+P.sc/100+')';
  FF.forEach(function(f){$('fv_'+f[0]).textContent=P[f[0]]+f[4]})}
function fClear(){octx.clearRect(0,0,W,H);[cur.c,cur.d].forEach(function(el){if(el){el.style.filter='';el.style.transform=''}})}
function fReset(){FF.forEach(function(f){$('f_'+f[0]).value=f[5]});$('f_inv').checked=false;$('f_gray').checked=false;fPrev()}
$('lf').onclick=function(){fsrc=hasSel?selContent():null;$('fnote').textContent=hasSel?'選択範囲の中だけに適用します(プレビューは目安です)。':'レイヤー全体に適用します。';fReset();$('fd').showModal()};
$('fr').onclick=fReset;
$('fc').onclick=function(){$('fd').close()};
$('fd').addEventListener('close',fClear);
$('fk').onclick=function(){var P=fRead();fClear();snap();
  var t=document.createElement('canvas');t.width=W;t.height=H;var c=t.getContext('2d');
  if('filter' in c)c.filter=fStr(P);else if(!window._fw){window._fw=1;toast('このブラウザは色やぼかしの加工に対応していません。変形だけ適用します。')}
  c.translate(W/2+P.tx,H/2+P.ty);c.rotate(P.ro*Math.PI/180);c.scale(P.sc/100,P.sc/100);c.translate(-W/2,-H/2);c.drawImage(hasSel&&fsrc?fsrc:cur.c,0,0);
  if(hasSel&&fsrc){cur.x.save();cur.x.globalCompositeOperation='destination-out';cur.x.drawImage(sel,0,0);cur.x.restore()}else cur.x.clearRect(0,0,W,H);
  cur.x.drawImage(t,0,0);$('fd').close();dirty()};
function flip(h){snap();var t=document.createElement('canvas');t.width=W;t.height=H;t.getContext('2d').drawImage(cur.c,0,0);
  var c=cur.x;c.save();c.clearRect(0,0,W,H);if(h){c.translate(W,0);c.scale(-1,1)}else{c.translate(0,H);c.scale(1,-1)}c.drawImage(t,0,0);c.restore();dirty()}
$('lh').onclick=function(){flip(true)};$('lv').onclick=function(){flip(false)};
$('lm').onclick=function(){var i=layers.indexOf(cur);if(i<1)return;var b=layers[i-1];
  b.x.save();b.x.globalAlpha=cur.o;b.x.globalCompositeOperation=cur.m==='normal'?'source-over':cur.m;b.x.drawImage(src(cur,i),0,0);b.x.restore();
  st.removeChild(cur.c);if(cur.d)st.removeChild(cur.d);layers.splice(i,1);cur=b;undoS=[];redoS=[];rl()};

/* ---- 説明 ---- */
var TIPS={pen:'ペン — くっきりした基本の線。筆圧で太さが変わります。',gpen:'Gペン — 筆圧や速さで強弱がつく、マンガ向けの線。',
  brush:'筆 — 速く動かすほど細くなる、はらいのある線。',pencil:'鉛筆 — ざらっとした、少し薄い線。',crayon:'クレヨン — かすれた粒状の質感。',
  marker:'マーカー — 半透明の平たい線。重ねても濃くなりません。',airbrush:'エアブラシ — ふんわりぼかした線。',
  watercolor:'水彩 — にじんだ縁。重ねるほど濃くなります。',spray:'スプレー — 点が広い範囲に散ります。',
  eraser:'消しゴム — 選んでいるレイヤーの絵を消します。大きさは「太さ」で変えます。',fill:'塗りつぶし — 囲まれた範囲を描画色で塗ります(選んでいるレイヤーの中だけ)。',
  line:'直線 — ドラッグで直線を引きます。',rect:'四角 — ドラッグで四角の枠を描きます。',ellipse:'楕円 — ドラッグで楕円の枠を描きます。',
  eye:'スポイト — クリックした場所の色を描画色にします。',hand:'手のひら — ドラッグで画面を動かします(スペース+ドラッグでも可)。'};
function showTip(t){var s=CB[t]?CB[t].n+' — オリジナルのペン。「編集・コピー」で調整できます。':BI[t]?BI[t].n+' — '+BI[t].d:(TIPS[t]||'');$('bd').textContent=s;$('tip').textContent=s}
document.querySelectorAll('[data-t]').forEach(function(b){if(TIPS[b.dataset.t])b.title=TIPS[b.dataset.t]});
$('bh').onclick=function(){$('hp').showModal()};$('hc').onclick=function(){$('hp').close()};

/* ---- カラーパレット ---- */
var hH=210,hS=.7,hV=.9,subC='#ffffff',PAL=0,HIST=[],FAV=[];
var PALS=[
 ['基本',['#000000','#4b4b4b','#8c8c8c','#c8c8c8','#ffffff','#e5383b','#f77f00','#f2c200','#2e9e5b','#1fa7c9','#2b5cff','#7b4fd6','#e0569b','#8a5a3c','#ff8fa3','#7fd1ae']],
 ['パステル',['#ffd6e0','#ffe5b4','#fff3b0','#d4f5c9','#c7f0ee','#cfe2ff','#e0d4ff','#f5d5f0','#ffb3c1','#ffcf9f','#fbe38e','#b9e8a3','#a0e4df','#a9c8ff','#c9b6ff','#eab4e6']],
 ['くすみ',['#5b6b73','#8a8d91','#b8a99a','#c9b8a4','#9a7b6b','#a5594f','#cc8a6f','#d6b98c','#8a9a5b','#6f8f72','#5f8a8b','#6b7ea1','#7c6f9b','#a1799d','#c68b9a','#4a4e57']],
 ['肌・髪',['#ffe0cc','#fbd0b4','#f3bd9b','#e6a57f','#cf8a62','#a8683f','#7a4a2d','#4e2f1e','#2b1d14','#5a3b24','#8b5a2b','#c28f4c','#e8c872','#f1ebe0','#9aa0a8','#1b1b1f']],
 ['自然',['#0b1d3a','#123e6b','#2c7bb6','#6bb7e6','#bfe3f5','#e9f6fb','#20502f','#3c7a3f','#6fae4a','#b5d66b','#f4e285','#e8a33d','#c4642f','#8a3b2a','#5c3d2e','#3a2a22']]];
try{HIST=JSON.parse(localStorage.getItem('ps_hist')||'[]');FAV=JSON.parse(localStorage.getItem('ps_fav')||'[]')}catch(err){}
function saveH(){try{localStorage.setItem('ps_hist',JSON.stringify(HIST))}catch(err){}}
function saveFav(){try{localStorage.setItem('ps_fav',JSON.stringify(FAV))}catch(err){}}
function h2v(h){var r=parseInt(h.substr(1,2),16)/255,g=parseInt(h.substr(3,2),16)/255,b=parseInt(h.substr(5,2),16)/255,
  mx=Math.max(r,g,b),mn=Math.min(r,g,b),d=mx-mn,hh=0;
  if(d){if(mx===r)hh=((g-b)/d+6)%6;else if(mx===g)hh=(b-r)/d+2;else hh=(r-g)/d+4;hh*=60}
  return[hh,mx?d/mx:0,mx]}
function v2h(h,sa,v){var c=v*sa,x=c*(1-Math.abs((h/60)%2-1)),m=v-c,i=Math.floor(h/60)%6,
  t=[[c,x,0],[x,c,0],[0,c,x],[0,x,c],[x,0,c],[c,0,x]][i];
  return'#'+t.map(function(n){return hex(Math.round((n+m)*255))}).join('')}
function drawSV(){var cv2=$('svc'),c=cv2.getContext('2d'),w=cv2.width,h=cv2.height;
  c.fillStyle='hsl('+hH+',100%,50%)';c.fillRect(0,0,w,h);
  var g=c.createLinearGradient(0,0,w,0);g.addColorStop(0,'#fff');g.addColorStop(1,'rgba(255,255,255,0)');c.fillStyle=g;c.fillRect(0,0,w,h);
  g=c.createLinearGradient(0,0,0,h);g.addColorStop(0,'rgba(0,0,0,0)');g.addColorStop(1,'#000');c.fillStyle=g;c.fillRect(0,0,w,h)}
function markSel(){document.querySelectorAll('.sq').forEach(function(b){b.classList.toggle('on',b.title===color)})}
function setColor(hx,fromPicker){hx=hx.toLowerCase();color=hx;
  if(!fromPicker){var a=h2v(hx);if(a[1]>0&&a[2]>0)hH=a[0];hS=a[1];hV=a[2]}
  $('cm').style.background=color;if(document.activeElement!==$('hx'))$('hx').value=color;
  $('svk').style.left=hS*100+'%';$('svk').style.top=(1-hV)*100+'%';$('hue').value=Math.round(hH);drawSV();markSel()}
function swapC(){var t=color;setColor(subC);subC=t;$('cs').style.background=subC}
function sq(c,fn,del){var b=document.createElement('button');b.className='sq';b.style.background=c;b.title=c;b.onclick=function(){fn(c)};
  if(del)b.ondblclick=b.oncontextmenu=function(e){e.preventDefault();del(c)};return b}
function renderPal(){var pt=$('pt'),pl=$('pl');pt.innerHTML='';pl.innerHTML='';
  PALS.forEach(function(P,i){var b=document.createElement('button');b.textContent=P[0];b.className=i===PAL?'on':'';b.onclick=function(){PAL=i;renderPal();markSel()};pt.appendChild(b)});
  PALS[PAL][1].forEach(function(c){pl.appendChild(sq(c,function(x){setColor(x)}))})}
function renderLists(){var hs=$('hs'),fv=$('fv');hs.innerHTML='';fv.innerHTML='';
  HIST.forEach(function(c){hs.appendChild(sq(c,function(x){setColor(x)}))});
  FAV.forEach(function(c){fv.appendChild(sq(c,function(x){setColor(x)},function(x){askYes('お気に入りからこの色を消しますか?',function(){FAV=FAV.filter(function(y){return y!==x});saveFav();renderLists()})}))});
  markSel()}
function pushHist(c){if(HIST[0]===c)return;HIST=HIST.filter(function(x){return x!==c});HIST.unshift(c);HIST=HIST.slice(0,16);saveH();renderLists()}
(function(){var box=$('sv'),dn=false;
  function mv(e){var r=box.getBoundingClientRect();hS=Math.max(0,Math.min(1,(e.clientX-r.left)/r.width));hV=1-Math.max(0,Math.min(1,(e.clientY-r.top)/r.height));setColor(v2h(hH,hS,hV),true)}
  box.addEventListener('pointerdown',function(e){dn=true;box.setPointerCapture(e.pointerId);mv(e)});
  box.addEventListener('pointermove',function(e){if(dn)mv(e)});
  box.addEventListener('pointerup',function(){dn=false});box.addEventListener('pointercancel',function(){dn=false})})();
$('hue').oninput=function(){hH=+this.value;setColor(v2h(hH,hS,hV),true)};
$('hx').oninput=function(){var m=this.value.trim().match(/^#?([0-9a-f]{6})$/i);if(m)setColor('#'+m[1])};
$('hx').onblur=function(){this.value=color};
$('cs').onclick=swapC;
$('ey').onclick=function(){selTool('eye')};
$('fa').onclick=function(){if(FAV.indexOf(color)<0){FAV.unshift(color);FAV=FAV.slice(0,16);saveFav();renderLists()}};
view.addEventListener('pointerup',function(){if(tool!=='hand'&&tool!=='eye'&&tool!=='eraser'&&!sp)pushHist(color)});
document.addEventListener('keydown',function(e){if(e.target.tagName==='INPUT'&&e.target.type==='text')return;if((e.key==='x'||e.key==='X')&&!e.ctrlKey&&!e.metaKey)swapC()});
renderPal();renderLists();setColor('#1d2733');$('cs').style.background=subC;

/* ---- 選択範囲 ---- */
var sel=document.createElement('canvas'),sx=sel.getContext('2d',{willReadFrequently:true}),hasSel=false,flt=null,selM='new',lpts=[],mv0=null,fsrc=null;
function initSel(){rlOnInit();var o=$('so');o.width=W;o.height=H;sel.width=W;sel.height=H;hasSel=false;flt=null;drawSel()}
function selMode(e){return e&&e.shiftKey?'add':e&&e.altKey?'sub':selM}
function drawSel(){var o=$('so'),c=o.getContext('2d');c.clearRect(0,0,W,H);o.style.transform='';
  var d=sx.getImageData(0,0,W,H).data,im=c.createImageData(W,H),b=im.data,any=false,rw=W*4;
  for(var y=0;y<H;y++)for(var x=0;x<W;x++){var i=(y*W+x)*4+3;if(d[i]>127){any=true;var k=i-3,
    edge=x===0||y===0||x===W-1||y===H-1||d[i-4]<128||d[i+4]<128||d[i-rw]<128||d[i+rw]<128;
    if(edge){var v=((x+y)>>2&1)?255:0;b[k]=b[k+1]=b[k+2]=v;b[k+3]=255}else{b[k]=77;b[k+1]=141;b[k+2]=255;b[k+3]=46}}}
  c.putImageData(im,0,0);hasSel=any;
  ['sd','si','sc','sf','sl','sg','ss','sm'].forEach(function(id){$(id).disabled=!any});moveUI()}
function applyMask(fn,m){flt=null;if(m==='new')sx.clearRect(0,0,W,H);sx.save();sx.fillStyle='#fff';
  sx.globalCompositeOperation=m==='sub'?'destination-out':'source-over';fn(sx);sx.restore();drawSel()}
function deselect(){flt=null;sx.clearRect(0,0,W,H);drawSel()}
var lmode='free',poly=[],polyT=0,ptype='mouse',cursOn=false;
function drawPoly(pts,curP,hint,near){octx.clearRect(0,0,W,H);cursOn=false;if(!pts.length)return;var u=1/Z;octx.save();
  octx.beginPath();pts.forEach(function(q,i){i?octx.lineTo(q.x,q.y):octx.moveTo(q.x,q.y)});if(curP)octx.lineTo(curP.x,curP.y);octx.closePath();
  octx.fillStyle='rgba(77,141,255,.22)';octx.fill();octx.lineWidth=1.5*u;octx.setLineDash([6*u,4*u]);octx.strokeStyle='#fff';octx.stroke();
  octx.strokeStyle='#000';octx.lineDashOffset=5*u;octx.stroke();octx.setLineDash([]);
  if(lmode==='poly'){pts.forEach(function(q){octx.beginPath();octx.arc(q.x,q.y,3.5*u,0,7);octx.fillStyle='#fff';octx.fill();octx.lineWidth=u;octx.strokeStyle='#000';octx.stroke()})}
  if(hint){var f=pts[0],rr=(near?20:13)*u;octx.beginPath();octx.arc(f.x,f.y,rr,0,7);octx.fillStyle=near?'rgba(255,159,28,.55)':'rgba(255,159,28,.3)';octx.fill();octx.lineWidth=2.5*u;octx.strokeStyle='#ff9f1c';octx.stroke();
    if(near&&!VMX&&!VMY){octx.font='bold '+(13*u)+'px sans-serif';octx.textBaseline='bottom';var tx='ここで閉じる',w=octx.measureText(tx).width;octx.fillStyle='rgba(20,24,32,.9)';octx.fillRect(f.x+rr,f.y-rr-16*u,w+10*u,18*u);octx.fillStyle='#fff';octx.fillText(tx,f.x+rr+5*u,f.y-rr+0*u)}}
  octx.restore()}
function selPrev(p){var u=1/Z;
  if(tool==='rsel'){octx.clearRect(0,0,W,H);octx.save();octx.fillStyle='rgba(77,141,255,.18)';octx.fillRect(a.x,a.y,p.x-a.x,p.y-a.y);
    octx.lineWidth=1.5*u;octx.setLineDash([6*u,4*u]);octx.strokeStyle='#fff';octx.strokeRect(a.x,a.y,p.x-a.x,p.y-a.y);octx.strokeStyle='#000';octx.lineDashOffset=5*u;octx.strokeRect(a.x,a.y,p.x-a.x,p.y-a.y);octx.restore();return}
  var l=lpts[lpts.length-1];if(!l||Math.hypot(p.x-l.x,p.y-l.y)>=2*u)lpts.push(p);drawPoly(lpts,null,false)}
function closeR(){return(ptype==='touch'?32:20)/Z}
function updPoly(){var on=poly.length>0;$('pbar').style.display=on?'flex':'none';$('pok').disabled=poly.length<3;
  $('pbt').textContent=poly.length<3?'あと'+(3-poly.length)+'点、クリックしてください':'最初の点(オレンジの輪)をクリックしても閉じます'}
function polyCancel(){if(poly.length){poly=[];octx.clearRect(0,0,W,H)}updPoly()}
function polyFinish(e){if(poly.length>=3){var pts=poly.slice();poly=[];octx.clearRect(0,0,W,H);updPoly();
    applyMask(function(c){c.beginPath();pts.forEach(function(q,i){i?c.lineTo(q.x,q.y):c.moveTo(q.x,q.y)});c.closePath();c.fill()},selMode(e))}else polyCancel()}
function polyBack(){if(!poly.length)return;poly.pop();if(poly.length)drawPoly(poly,null,poly.length>=3,false);else octx.clearRect(0,0,W,H);updPoly()}
function polyClick(p,e){var now=Date.now();
  if(poly.length>=3){var f=poly[0],l=poly[poly.length-1];
    if(Math.hypot(p.x-f.x,p.y-f.y)<=closeR()||(now-polyT<400&&Math.hypot(p.x-l.x,p.y-l.y)<=10/Z)){polyFinish(e);return}}
  poly.push(p);polyT=now;drawPoly(poly,null,poly.length>=3,false);updPoly()}
function smoothPts(P){var q=P;for(var k=0;k<2;k++){if(q.length<4)break;var r=[q[0]];for(var i=1;i<q.length-1;i++)r.push({x:(q[i-1].x+2*q[i].x+q[i+1].x)/4,y:(q[i-1].y+2*q[i].y+q[i+1].y)/4});r.push(q[q.length-1]);q=r}return q}
function finishSel(p,e){octx.clearRect(0,0,W,H);var m=selMode(e);
  if(tool==='rsel'){var x=Math.min(a.x,p.x),y=Math.min(a.y,p.y),w=Math.abs(p.x-a.x),h=Math.abs(p.y-a.y);
    if(w<2||h<2){if(m==='new')deselect();return}applyMask(function(c){c.fillRect(x,y,w,h)},m)}
  else{if(lpts.length<3){if(m==='new')deselect();return}
    var pp=smoothPts(lpts);applyMask(function(c){c.beginPath();pp.forEach(function(q,i){i?c.lineTo(q.x,q.y):c.moveTo(q.x,q.y)});c.closePath();c.fill()},m)}}
function wand(p,e){if(!inb(p))return;var im=cur.x.getImageData(0,0,W,H),d=im.data,x0=p.x|0,y0=p.y|0,i0=(y0*W+x0)*4,
  tr=d[i0],tg=d[i0+1],tb=d[i0+2],ta=d[i0+3],T=+$('tol').value*4;
  function m(i){return Math.abs(d[i]-tr)+Math.abs(d[i+1]-tg)+Math.abs(d[i+2]-tb)+Math.abs(d[i+3]-ta)<=T}
  var seen=new Uint8Array(W*H),stk=[x0,y0];
  while(stk.length){var y=stk.pop(),x=stk.pop();if(seen[y*W+x]||!m((y*W+x)*4))continue;
    var xl=x,xr=x;while(xl>0&&!seen[y*W+xl-1]&&m((y*W+xl-1)*4))xl--;while(xr<W-1&&!seen[y*W+xr+1]&&m((y*W+xr+1)*4))xr++;
    for(var xx=xl;xx<=xr;xx++){var q=y*W+xx;seen[q]=1;if(y>0&&!seen[q-W])stk.push(xx,y-1);if(y<H-1&&!seen[q+W])stk.push(xx,y+1)}}
  var o=new ImageData(W,H),b=o.data;for(var j=0;j<seen.length;j++)if(seen[j]){b[j*4]=b[j*4+1]=b[j*4+2]=255;b[j*4+3]=255}
  var t=document.createElement('canvas');t.width=W;t.height=H;t.getContext('2d').putImageData(o,0,0);
  applyMask(function(c){c.drawImage(t,0,0)},selMode(e))}
function selContent(){var t=document.createElement('canvas');t.width=W;t.height=H;var c=t.getContext('2d');c.drawImage(cur.c,0,0);c.globalCompositeOperation='destination-in';c.drawImage(sel,0,0);return t}
/* 選択範囲の移動・拡大縮小・回転 */
var handlesOn=false,mbox=null,mv0=null,tzs=false,HPL=[[-1,-1],[0,-1],[1,-1],[1,0],[1,1],[0,1],[-1,1],[-1,0]],
  HCUR=['nwse-resize','ns-resize','nesw-resize','ew-resize','nwse-resize','ns-resize','nesw-resize','ew-resize'];
function boxOf(){var d=(hasSel?sx:cur.x).getImageData(0,0,W,H).data,x0=W,y0=H,x1=-1,y1=-1;
  for(var y=0;y<H;y++){var row=y*W*4;for(var x=0;x<W;x++){if(d[row+x*4+3]>8){if(x<x0)x0=x;if(x>x1)x1=x;if(y<y0)y0=y;if(y>y1)y1=y}}}
  return x1<0?null:{x0:x0,y0:y0,x1:x1+1,y1:y1+1}}
/* 変換: 元の中心 c0 を (cx,cy) へ動かし、拡大縮小(kx,ky)と回転(th)をかける */
function xf(f){var c=Math.cos(f.th),s=Math.sin(f.th),a=c*f.kx,b=s*f.kx,cc=-s*f.ky,d=c*f.ky,c0x=(f.B0.x0+f.B0.x1)/2,c0y=(f.B0.y0+f.B0.y1)/2;
  return[a,b,cc,d,f.cx-(a*c0x+cc*c0y),f.cy-(b*c0x+d*c0y)]}
function mmul(m,n){return[m[0]*n[0]+m[2]*n[1],m[1]*n[0]+m[3]*n[1],m[0]*n[2]+m[2]*n[3],m[1]*n[2]+m[3]*n[3],m[0]*n[4]+m[2]*n[5]+m[4],m[1]*n[4]+m[3]*n[5]+m[5]]}
function minv(m){var det=m[0]*m[3]-m[1]*m[2]||1e-9,a=m[3]/det,b=-m[1]/det,c=-m[2]/det,d=m[0]/det;return[a,b,c,d,-(a*m[4]+c*m[5]),-(b*m[4]+d*m[5])]}
function geom(){var f=flt&&flt.l===cur?flt:null,B=f?f.B0:mbox;if(!B)return null;
  var w0=B.x1-B.x0,h0=B.y1-B.y0,kx=f?f.kx:1,ky=f?f.ky:1,th=f?f.th:0,cx=f?f.cx:(B.x0+B.x1)/2,cy=f?f.cy:(B.y0+B.y1)/2,c=Math.cos(th),s=Math.sin(th);
  return{cx:cx,cy:cy,u:[c,s],v:[-s,c],hw:w0*kx/2,hh:h0*ky/2,w0:w0,h0:h0,th:th}}
function hpos(g,i){var h=HPL[i],lx=h[0]*g.hw,ly=h[1]*g.hh;return{x:g.cx+g.u[0]*lx+g.v[0]*ly,y:g.cy+g.u[1]*lx+g.v[1]*ly}}
function rotPos(g){var off=Math.abs(g.hh)+34/Z;return{x:g.cx-g.v[0]*off,y:g.cy-g.v[1]*off}}
function hitHandle(p){var g=geom();if(!g)return-1;var r=(ptype==='touch'?26:13)/Z,rp=rotPos(g);
  if((rp.x-p.x)*(rp.x-p.x)+(rp.y-p.y)*(rp.y-p.y)<r*r)return 8;
  var best=-1,bd=r*r;for(var i=0;i<8;i++){var h=hpos(g,i),d=(h.x-p.x)*(h.x-p.x)+(h.y-p.y)*(h.y-p.y);if(d<bd){bd=d;best=i}}return best}
function drawHandles(){octx.clearRect(0,0,W,H);cursOn=false;var g=geom();if(!g){handlesOn=false;return}handlesOn=true;
  var u=1/Z,k=ptype==='touch'?1.7:1,corners=[0,2,4,6].map(function(i){return hpos(g,i)});octx.save();
  octx.beginPath();corners.forEach(function(q,i){i?octx.lineTo(q.x,q.y):octx.moveTo(q.x,q.y)});octx.closePath();
  octx.lineWidth=1.5*u;octx.setLineDash([6*u,4*u]);octx.strokeStyle='#fff';octx.stroke();octx.strokeStyle='#000';octx.lineDashOffset=5*u;octx.stroke();octx.setLineDash([]);
  var rp=rotPos(g),tm=hpos(g,1);octx.beginPath();octx.moveTo(tm.x,tm.y);octx.lineTo(rp.x,rp.y);octx.lineWidth=1.5*u;octx.strokeStyle='#4d8dff';octx.stroke();
  octx.beginPath();octx.arc(rp.x,rp.y,6*u*k,0,7);octx.fillStyle='#fff';octx.fill();octx.lineWidth=2*u;octx.strokeStyle='#4d8dff';octx.stroke();
  for(var i=0;i<8;i++){var h=hpos(g,i),sz=9*u*k;octx.fillStyle='#fff';octx.fillRect(h.x-sz/2,h.y-sz/2,sz,sz);octx.lineWidth=1.5*u;octx.strokeStyle='#4d8dff';octx.strokeRect(h.x-sz/2,h.y-sz/2,sz,sz)}
  octx.restore()}
function syncTz(){var f=flt&&flt.l===cur?flt:null;$('tz').value=Math.round(Math.abs(f?f.kx:1)*100);$('tzv').textContent=$('tz').value+'%';
  var dg=f?Math.round(f.th*180/Math.PI):0;$('tr').value=dg;$('trv').textContent=dg+'°'}
function moveUI(){var on=tool==='move';$('tz').disabled=!on;$('tr').disabled=!on;
  if(!on){if(handlesOn||cursOn){octx.clearRect(0,0,W,H);handlesOn=false;cursOn=false}return}
  if(!flt||flt.l!==cur)mbox=boxOf();syncTz();drawHandles()}
function ensureFloat(){if(flt&&flt.l===cur)return true;var B0=boxOf();if(!B0)return false;
  var f=document.createElement('canvas');f.width=W;f.height=H;var fc=f.getContext('2d');fc.drawImage(cur.c,0,0);
  var m0=null;if(hasSel){fc.globalCompositeOperation='destination-in';fc.drawImage(sel,0,0);m0=document.createElement('canvas');m0.width=W;m0.height=H;m0.getContext('2d').drawImage(sel,0,0)}
  var c=cur.x;c.save();if(hasSel){c.globalCompositeOperation='destination-out';c.drawImage(sel,0,0)}else c.clearRect(0,0,W,H);c.restore();
  flt={f:f,base:c.getImageData(0,0,W,H),B0:B0,m0:m0,kx:1,ky:1,th:0,cx:(B0.x0+B0.x1)/2,cy:(B0.y0+B0.y1)/2,l:cur};return true}
function startMove(p,hh){pushU();if(!ensureFloat()){undoS.pop();return false}
  mv0={x:p.x,y:p.y,kx:flt.kx,ky:flt.ky,th:flt.th,cx:flt.cx,cy:flt.cy,h:hh,M:xf(flt),g:geom()};return true}
function mvRender(){var c=cur.x,M=xf(flt);c.putImageData(flt.base,0,0);c.save();c.imageSmoothingQuality='high';c.setTransform(M[0],M[1],M[2],M[3],M[4],M[5]);c.drawImage(flt.f,0,0);c.restore();
  var rel=mmul(M,minv(mv0.M));$('so').style.transform='matrix('+rel.join(',')+')';
  drawHandles();syncTz();dirty()}
function moveTo(p){flt.cx=mv0.cx+(p.x-mv0.x);flt.cy=mv0.cy+(p.y-mv0.y);mvRender()}
function clampK(k,w){return Math.abs(k)<.02?(k<0?-.02:.02):k}
function scaleTo(p,free){var g=mv0.g,h=HPL[mv0.h],hx=h[0],hy=h[1],
  A={x:g.cx-g.u[0]*hx*g.hw-g.v[0]*hy*g.hh,y:g.cy-g.u[1]*hx*g.hw-g.v[1]*hy*g.hh},
  du=(p.x-A.x)*g.u[0]+(p.y-A.y)*g.u[1],dv=(p.x-A.x)*g.v[0]+(p.y-A.y)*g.v[1],
  kx=hx?du/(hx*g.w0):mv0.kx,ky=hy?dv/(hy*g.h0):mv0.ky;
  if(hx&&hy&&!free){var r=Math.max(.02,(Math.abs(kx/mv0.kx)+Math.abs(ky/mv0.ky))/2);kx=(mv0.kx<0?-1:1)*Math.abs(mv0.kx)*r;ky=(mv0.ky<0?-1:1)*Math.abs(mv0.ky)*r}
  kx=clampK(kx);ky=clampK(ky);
  flt.kx=kx;flt.ky=ky;flt.cx=A.x+g.u[0]*hx*g.w0*kx/2+g.v[0]*hy*g.h0*ky/2;flt.cy=A.y+g.u[1]*hx*g.w0*kx/2+g.v[1]*hy*g.h0*ky/2;mvRender()}
function rotateTo(p,snap){var a0=Math.atan2(mv0.y-mv0.cy,mv0.x-mv0.cx),a1=Math.atan2(p.y-mv0.cy,p.x-mv0.cx),th=mv0.th+(a1-a0);
  if(snap){var st=Math.PI/12;th=Math.round(th/st)*st}
  while(th>Math.PI)th-=2*Math.PI;while(th<-Math.PI)th+=2*Math.PI;flt.th=th;mvRender()}
function endMove(){if(!flt)return;
  if(hasSel&&flt.m0){var M=xf(flt);sx.save();sx.clearRect(0,0,W,H);sx.setTransform(M[0],M[1],M[2],M[3],M[4],M[5]);sx.drawImage(flt.m0,0,0);sx.restore();drawSel()}
  else moveUI()}
function tBegin(){if(tool!=='move'||tzs)return;pushU();if(!ensureFloat()){undoS.pop();return}
  mv0={x:0,y:0,kx:flt.kx,ky:flt.ky,th:flt.th,cx:flt.cx,cy:flt.cy,h:-1,M:xf(flt),g:geom()};tzs=true}
['tz','tr'].forEach(function(id){$(id).addEventListener('pointerdown',tBegin);$(id).addEventListener('keydown',tBegin);
  $(id).addEventListener('change',function(){if(tzs){tzs=false;endMove()}})});
$('tz').addEventListener('input',function(){if(!tzs||!flt)return;var k=this.value/100;flt.kx=flt.ky=k;mvRender()});
$('tr').addEventListener('input',function(){if(!tzs||!flt)return;flt.th=this.value*Math.PI/180;mvRender()});
view.addEventListener('pointerdown',function(e){ptype=e.pointerType},true);
view.addEventListener('pointermove',function(e){if(tool!=='move'||mode)return;var h=hitHandle(pt(e));view.style.cursor=h<0?'move':h===8?'grab':HCUR[h]});
/* 多角形の投げ縄: 動かしている間の線を見せる */
view.addEventListener('pointermove',function(e){if(tool==='lasso'&&lmode==='poly'&&poly.length&&!mode){var p=pt(e),f=poly[0],near=poly.length>=3&&Math.hypot(p.x-f.x,p.y-f.y)<=closeR();
  drawPoly(poly,p,poly.length>=3,near);view.style.cursor=near?'pointer':'crosshair'}});
view.addEventListener('contextmenu',function(e){if(tool==='lasso'&&lmode==='poly'&&poly.length){e.preventDefault();polyFinish(e)}});
view.addEventListener('dblclick',function(e){if(tool==='lasso'&&lmode==='poly'&&poly.length>=3)polyFinish(e)});
$('pok').onclick=function(e){polyFinish(e)};$('pbk').onclick=polyBack;$('pcn').onclick=polyCancel;
['pointerdown','pointermove','pointerup','dblclick','contextmenu'].forEach(function(t){$('pbar').addEventListener(t,function(e){e.stopPropagation()})});
document.querySelectorAll('[data-lm]').forEach(function(b){b.onclick=function(){lmode=b.dataset.lm;lsSet('ps_lmode',lmode);polyCancel();
  document.querySelectorAll('[data-lm]').forEach(function(x){x.classList.toggle('on',x===b)})}});
(function(){var m=lsGet('ps_lmode');if(m==='poly'){lmode='poly';document.querySelectorAll('[data-lm]').forEach(function(x){x.classList.toggle('on',x.dataset.lm==='poly')})}})();
/* 選択範囲を広げる・縮める・なめらかにする */
function morph(kind){if(!hasSel)return;if(!('filter' in sx)){toast('このブラウザでは使えません');return}
  flt=null;var t=document.createElement('canvas');t.width=W;t.height=H;var c=t.getContext('2d');c.filter='blur('+(kind==='m'?2.6:2.2)+'px)';c.drawImage(sel,0,0);c.filter='none';
  var im=c.getImageData(0,0,W,H),d=im.data,th=kind==='g'?8:kind==='s'?247:127;
  for(var i=3;i<d.length;i+=4)d[i]=d[i]>th?255:0;
  c.putImageData(im,0,0);sx.clearRect(0,0,W,H);sx.drawImage(t,0,0);drawSel()}
$('sg').onclick=function(){morph('g')};$('ss').onclick=function(){morph('s')};$('sm').onclick=function(){morph('m')};
/* ブラシの大きさが見える丸いカーソル */
function isBrushTool(){return['rsel','lasso','wand','move','rcol','fill','eye','hand','line','rect','ellipse','ruler'].indexOf(tool)<0}
view.addEventListener('pointermove',function(e){if(e.pointerType==='touch'||(mode&&mode!=='brush'))return;
  if(!isBrushTool()){if(cursOn){octx.clearRect(0,0,W,H);cursOn=false}return}
  var p=pt(e),cb=CBo(),r=Math.max(1,(cb?size*cb.sz/100:size*(tool==='marker'?1.4:1))/2),u=1/Z;
  octx.clearRect(0,0,W,H);octx.save();octx.beginPath();octx.arc(p.x,p.y,r,0,7);octx.lineWidth=2.2*u;octx.strokeStyle='rgba(255,255,255,.9)';octx.stroke();
  octx.lineWidth=1*u;octx.strokeStyle='rgba(0,0,0,.85)';octx.stroke();octx.restore();cursOn=true});
view.addEventListener('pointerleave',function(){if(cursOn){octx.clearRect(0,0,W,H);cursOn=false}});
/* ショートカットキー */
(function(){var KM={b:'pen',e:'eraser',g:'fill',i:'eye',h:'hand',v:'move',m:'rsel',l:'lasso',w:'wand',p:'ruler'};
  document.addEventListener('keydown',function(e){if(e.ctrlKey||e.metaKey||e.altKey||document.querySelector('dialog[open]'))return;
    var t=e.target.tagName;if(t==='INPUT'&&e.target.type==='text'||t==='TEXTAREA'||t==='SELECT')return;
    var k=e.key.toLowerCase();if(KM[k]){selTool(KM[k]);return}
    if(k==='0')fitView();else if(k==='='||k==='+')zoom(1.25);else if(k==='-')zoom(.8)});
  setTimeout(function(){Object.keys(KM).forEach(function(k){var b=document.querySelector('[data-t="'+KM[k]+'"]');if(b&&TIPS[KM[k]])b.title=TIPS[KM[k]]+' ['+k.toUpperCase()+']'})},0)})();
/* 色を変える */
function recolorAt(p){if(!inb(p))return;var im=cur.x.getImageData(0,0,W,H),d=im.data,i0=((p.y|0)*W+(p.x|0))*4;if(d[i0+3]<10)return;
  snap();var tr=d[i0],tg=d[i0+1],tb=d[i0+2],cc=rgb(color),T=+$('tol').value*3+10,mk=hasSel?sx.getImageData(0,0,W,H).data:null;
  for(var i=0;i<d.length;i+=4){if(!d[i+3])continue;var w=mk?mk[i+3]/255:1;if(!w)continue;
    var dist=Math.abs(d[i]-tr)+Math.abs(d[i+1]-tg)+Math.abs(d[i+2]-tb);if(dist>T*1.6)continue;
    var k=(dist<=T?1:1-(dist-T)/(T*.6))*w;d[i]+=(cc[0]-d[i])*k;d[i+1]+=(cc[1]-d[i+1])*k;d[i+2]+=(cc[2]-d[i+2])*k}
  cur.x.putImageData(im,0,0);dirty()}
function tintC(){snap();var im=cur.x.getImageData(0,0,W,H),d=im.data,cc=rgb(color),mk=hasSel?sx.getImageData(0,0,W,H).data:null;
  for(var i=0;i<d.length;i+=4){if(!d[i+3])continue;var w=mk?mk[i+3]/255:1;if(!w)continue;
    d[i]+=(cc[0]-d[i])*w;d[i+1]+=(cc[1]-d[i+1])*w;d[i+2]+=(cc[2]-d[i+2])*w}
  cur.x.putImageData(im,0,0);dirty()}
function fPrevSel(P){octx.clearRect(0,0,W,H);octx.save();if('filter' in octx)octx.filter=fStr(P);
  octx.translate(W/2+P.tx,H/2+P.ty);octx.rotate(P.ro*Math.PI/180);octx.scale(P.sc/100,P.sc/100);octx.translate(-W/2,-H/2);
  octx.drawImage(fsrc,0,0);octx.restore()}
document.querySelectorAll('[data-sm]').forEach(function(b){b.onclick=function(){selM=b.dataset.sm;
  document.querySelectorAll('[data-sm]').forEach(function(x){x.classList.toggle('on',x===b)})}});
$('sa').onclick=function(){applyMask(function(c){c.fillRect(0,0,W,H)},'new')};
$('sd').onclick=deselect;
$('si').onclick=function(){var t=document.createElement('canvas');t.width=W;t.height=H;var c=t.getContext('2d');c.fillStyle='#fff';c.fillRect(0,0,W,H);
  c.globalCompositeOperation='destination-out';c.drawImage(sel,0,0);flt=null;sx.clearRect(0,0,W,H);sx.drawImage(t,0,0);drawSel()};
$('sc').onclick=function(){if(!hasSel)return;snap();cur.x.save();cur.x.globalCompositeOperation='destination-out';cur.x.drawImage(sel,0,0);cur.x.restore();dirty()};
$('sf').onclick=function(){if(!hasSel)return;snap();var t=document.createElement('canvas');t.width=W;t.height=H;var c=t.getContext('2d');
  c.fillStyle=color;c.fillRect(0,0,W,H);c.globalCompositeOperation='destination-in';c.drawImage(sel,0,0);
  cur.x.save();cur.x.globalAlpha=op;cur.x.drawImage(t,0,0);cur.x.restore();dirty()};
$('sl').onclick=function(){if(!hasSel)return;var i=layers.indexOf(cur),l=mk('選択範囲');l.x.drawImage(selContent(),0,0);layers.splice(i+1,0,l);cur=l;rl()};
$('tc').onclick=tintC;
$('ch').onclick=function(){$('lf').onclick()};
$('tol').oninput=function(){$('tolv').textContent=this.value};$('tolv').textContent=$('tol').value;
document.addEventListener('keydown',function(e){if(document.querySelector('dialog[open]'))return;if(e.target.tagName==='INPUT'&&e.target.type==='text')return;
  if((e.ctrlKey||e.metaKey)&&e.key.toLowerCase()==='d'){e.preventDefault();deselect()}
  else if(e.key==='Delete'&&hasSel){e.preventDefault();$('sc').onclick()}
  else if(e.key==='Escape'&&mode==='sel'){mode=null;lpts=[];octx.clearRect(0,0,W,H)}
  else if(e.key==='Escape'&&poly.length)polyCancel();
  else if((e.key==='Enter'||e.key===' ')&&poly.length>=3){e.preventDefault();polyFinish(e)}
  else if(e.key==='Backspace'&&poly.length){e.preventDefault();polyBack()}
  else if(e.key==='Escape'&&hasSel)deselect()});
TIPS.rsel='四角選択 — ドラッグで四角い範囲を選びます。Shiftで追加、Altで引きます。';
TIPS.lasso='投げ縄 — 囲んだ範囲を選びます。フリーハンドと、クリックで点を打つ多角形が選べます。';
TIPS.wand='自動選択 — クリックした場所と近い色の範囲を選びます(許容範囲はスライダーで調整)。';
TIPS.move='移動 — 選択範囲(なければレイヤー全体)をドラッグで動かします。四角い点で拡大縮小(Shiftで縦横比を自由に)、上の丸い点で回転(Shiftで15度ずつ)できます。';
TIPS.ruler='定規 — 消失点の丸い点をドラッグして動かします。パースの種類や、定規に沿って描く設定は、右の「パース定規」にあります。';
TIPS.rcol='色置換 — クリックした色を描画色に置き換えます(選択範囲があればその中だけ)。';
['rsel','lasso','wand','move','rcol'].forEach(function(k){var b=document.querySelector('[data-t="'+k+'"]');if(b)b.title=TIPS[k]});

/* ---- 3Dモデルの計算 ---- */
/* ---- 3Dモデル(人形)の計算部分: 立体メッシュ + ソフトウェア描画 ---- */
var MQ=(function(){
var D2R=Math.PI/180;
var J=[
 ['root','腰',-1,[0,92,0]],['spine','腹',0,[0,10,0]],['chest','胸',1,[0,18,0]],['neck','首',2,[0,24,0]],['head','頭',3,[0,8,0]],
 ['shL','左肩',2,[17,21,0]],['elL','左ひじ',5,[0,-29,0]],['wrL','左手首',6,[0,-25,0]],
 ['shR','右肩',2,[-17,21,0]],['elR','右ひじ',8,[0,-29,0]],['wrR','右手首',9,[0,-25,0]],
 ['hpL','左股関節',0,[9,-4,0]],['knL','左ひざ',11,[0,-42,0]],['anL','左足首',12,[0,-40,0]],
 ['hpR','右股関節',0,[-9,-4,0]],['knR','右ひざ',14,[0,-42,0]],['anR','右足首',15,[0,-40,0]]];
var N=J.length,JI={};J.forEach(function(j,i){JI[j[0]]=i});
var PIV={wrL:'elL',wrR:'elR',elL:'shL',elR:'shR',anL:'knL',anR:'knR',knL:'hpL',knR:'hpR',head:'neck',neck:'chest'};
var STY={skin:[238,208,178],gray:[196,198,206],white:[248,248,248],sil:null};

function rx(a){var c=Math.cos(a),s=Math.sin(a);return[1,0,0,0,c,-s,0,s,c]}
function ry(a){var c=Math.cos(a),s=Math.sin(a);return[c,0,s,0,1,0,-s,0,c]}
function rz(a){var c=Math.cos(a),s=Math.sin(a);return[c,-s,0,s,c,0,0,0,1]}
function mul(a,b){var r=[];for(var i=0;i<3;i++)for(var j=0;j<3;j++)r[i*3+j]=a[i*3]*b[j]+a[i*3+1]*b[3+j]+a[i*3+2]*b[6+j];return r}
function tr(a){return[a[0],a[3],a[6],a[1],a[4],a[7],a[2],a[5],a[8]]}
function mv(a,v){return[a[0]*v[0]+a[1]*v[1]+a[2]*v[2],a[3]*v[0]+a[4]*v[1]+a[5]*v[2],a[6]*v[0]+a[7]*v[1]+a[8]*v[2]]}
function eul2m(e){return mul(rz(e[2]*D2R),mul(ry(e[1]*D2R),rx(e[0]*D2R)))}
function m2eul(m){var y=-Math.asin(Math.max(-1,Math.min(1,m[6]))),x,z;
  if(Math.abs(m[6])<.9999){x=Math.atan2(m[7],m[8]);z=Math.atan2(m[3],m[0])}else{x=Math.atan2(-m[5],m[4]);z=0}
  return[x/D2R,y/D2R,z/D2R]}
function rod(ax,ang){var l=Math.hypot(ax[0],ax[1],ax[2]);if(l<1e-9)return[1,0,0,0,1,0,0,0,1];
  var x=ax[0]/l,y=ax[1]/l,z=ax[2]/l,c=Math.cos(ang),s=Math.sin(ang),t=1-c;
  return[t*x*x+c,t*x*y-s*z,t*x*z+s*y,t*x*y+s*z,t*y*y+c,t*y*z-s*x,t*x*z-s*y,t*y*z+s*x,t*z*z+c]}
function cross(a,b){return[a[1]*b[2]-a[2]*b[1],a[2]*b[0]-a[0]*b[2],a[0]*b[1]-a[1]*b[0]]}
function norm3(a){var l=Math.hypot(a[0],a[1],a[2])||1;return[a[0]/l,a[1]/l,a[2]/l]}

function newState(){var r=[];for(var i=0;i<N;i++)r.push([0,0,0]);return{rot:r,rp:[0,0,0],hs:1,bt:1}}
function fk(S){var R=[],P=[];
  for(var i=0;i<N;i++){var p=J[i][2],l=eul2m(S.rot[i]);
    if(p<0){R[i]=l;P[i]=[J[i][3][0]+S.rp[0],J[i][3][1]+S.rp[1],J[i][3][2]+S.rp[2]]}
    else{R[i]=mul(R[p],l);var o=mv(R[p],J[i][3]);P[i]=[P[p][0]+o[0],P[p][1]+o[1],P[p][2]+o[2]]}}
  return{R:R,P:P}}

/* ===== 人形のパーツ(関節ごとの座標系で定義) ===== */
var NU=14,NR=8,NC=4;
function smooth(t){return t*t*(3-2*t)}
function cr(p0,p1,p2,p3,u){return .5*((2*p1)+(-p0+p2)*u+(2*p0-5*p1+4*p2-p3)*u*u+(-p0+3*p1-3*p2+p3)*u*u*u)}
function prof(pr,t){var n=pr.length;if(n===1||t<=pr[0][0])return[pr[0][1],pr[0][2]];
  for(var i=1;i<n;i++){if(t<=pr[i][0]){var a=pr[i-1],b=pr[i],p0=pr[i>1?i-2:0],p3=pr[i<n-1?i+1:n-1],u=(t-a[0])/((b[0]-a[0])||1);
    return[Math.max(.2,cr(p0[1],a[1],b[1],p3[1],u)),Math.max(.2,cr(p0[2],a[2],b[2],p3[2],u))]}}
  var l=pr[n-1];return[l[1],l[2]]}
var PARTS=[],GID={};
function gid(g){if(!GID[g])GID[g]=Object.keys(GID).length+1;return GID[g]}
function addTube(sp){
  var a=norm3(sp.d),u0=Math.abs(a[0])<.9?[1,0,0]:[0,0,1],dp=u0[0]*a[0]+u0[1]*a[1]+u0[2]*a[2],
  u=norm3([u0[0]-a[0]*dp,u0[1]-a[1]*dp,u0[2]-a[2]*dp]),v=cross(a,u),p0=prof(sp.pr,0),p1=prof(sp.pr,1),
  ha=sp.ha==null?(p0[0]+p0[1])/2:sp.ha,hb=sp.hb==null?(p1[0]+p1[1])/2:sp.hb,rings=[];
  for(var i=0;i<NC;i++){var an=Math.PI/2*(1-i/NC);rings.push([-ha*Math.sin(an),p0[0]*Math.cos(an),p0[1]*Math.cos(an)])}
  var nt=sp.len<1e-6?0:NR;
  for(var i=0;i<=nt;i++){var t=nt?i/nt:0,pp=prof(sp.pr,t);rings.push([sp.len*t,pp[0],pp[1]])}
  for(var i=1;i<=NC;i++){var an=Math.PI/2*(i/NC);rings.push([(nt?sp.len:0)+hb*Math.sin(an),p1[0]*Math.cos(an),p1[1]*Math.cos(an)])}
  var R=rings.length,nv=R*NU,V=new Float32Array(nv*3),Nn=new Float32Array(nv*3),ct=Math.cos,st=Math.sin;
  for(var r=0;r<R;r++)for(var k=0;k<NU;k++){var th=2*Math.PI*k/NU,c=ct(th)*rings[r][1],s=st(th)*rings[r][2],q=(r*NU+k)*3;
    V[q]=sp.o[0]+a[0]*rings[r][0]+u[0]*c+v[0]*s;V[q+1]=sp.o[1]+a[1]*rings[r][0]+u[1]*c+v[1]*s;V[q+2]=sp.o[2]+a[2]*rings[r][0]+u[2]*c+v[2]*s}
  for(var r=0;r<R;r++)for(var k=0;k<NU;k++){
    var i1=((r+1<R?r+1:r)*NU+k)*3,i0=((r>0?r-1:r)*NU+k)*3,k1=((k+1)%NU+r*NU)*3,k0=((k+NU-1)%NU+r*NU)*3,q=(r*NU+k)*3,
    t1=[V[k1]-V[k0],V[k1+1]-V[k0+1],V[k1+2]-V[k0+2]],t2=[V[i1]-V[i0],V[i1+1]-V[i0+1],V[i1+2]-V[i0+2]],n=cross(t1,t2),l=Math.hypot(n[0],n[1],n[2]);
    if(l<1e-6){n=r<R/2?[-a[0],-a[1],-a[2]]:[a[0],a[1],a[2]]}
    else{n=[n[0]/l,n[1]/l,n[2]/l];var cx=sp.o[0]+a[0]*rings[r][0],cy=sp.o[1]+a[1]*rings[r][0],cz=sp.o[2]+a[2]*rings[r][0];
      if(n[0]*(V[q]-cx)+n[1]*(V[q+1]-cy)+n[2]*(V[q+2]-cz)<0)n=[-n[0],-n[1],-n[2]]}
    Nn[q]=n[0];Nn[q+1]=n[1];Nn[q+2]=n[2]}
  var T=[];for(var r=0;r<R-1;r++)for(var k=0;k<NU;k++){var k2=(k+1)%NU,i00=r*NU+k,i01=r*NU+k2,i10=(r+1)*NU+k,i11=(r+1)*NU+k2;T.push(i00,i10,i01,i01,i10,i11)}
  PARTS.push({j:JI[sp.j],V:V,N:Nn,T:new Uint16Array(T),nv:nv,k:sp.k||'n',tint:sp.tint||1,id:gid(sp.g||('p'+PARTS.length))})}
function spec(j,o,d,len,pr,x){var sp={j:j,o:o,d:d,len:len,pr:pr};for(var k in x)sp[k]=x[k];
  var side=j.slice(-1)==='L';addTube(sp);
  if(side||sp.mir){var m={};for(var k in sp)m[k]=sp[k];m.o=[-sp.o[0],sp.o[1],sp.o[2]];m.d=[-sp.d[0],sp.d[1],sp.d[2]];
    if(side){m.j=j.slice(0,-1)+'R';if(sp.g)m.g=sp.g.replace(/L$/,'R')}addTube(m)}}
function ball(j,r,g){spec(j,[0,0,0],[0,1,0],0,[[0,r,r]],{ha:r,hb:r,k:'b',tint:.93,g:g})}
var DN=[0,-1,0],UP=[0,1,0];
/* 胴体 */
spec('root',[0,-1,0],UP,0,[[0,14.8,10.3]],{ha:9.5,hb:9.5,g:'pelvis'});
spec('spine',[0,-6,0],UP,24,[[0,12.5,9],[.4,11,8],[1,13,9.2]],{ha:0,hb:0,g:'abdomen'});
spec('chest',[0,-1,0],UP,25,[[0,13,9.5],[.4,15.8,11.2],[.85,14.4,9.8],[1,10.5,7.5]],{ha:0,hb:4.5,g:'chest'});
spec('neck',[0,-2,0],UP,12,[[0,5.2,5],[1,4.4,4.4]],{ha:0,hb:0,g:'neck'});
/* 頭(目鼻はなく、向きがわかる小さな鼻と耳だけ) */
spec('head',[0,6.5,0],UP,4,[[0,8.6,9.4],[1,10,10.4]],{ha:8.5,hb:10.5,k:'h',g:'H'});
spec('head',[0,6.8,9.3],[0,-.3,1],1.6,[[0,1.7,1.3],[1,1,.8]],{ha:.9,hb:.7,k:'h',g:'H'});
spec('head',[9.6,9,0],[1,0,0],1.2,[[0,3.2,2.6],[1,2.8,2.2]],{ha:.8,hb:.8,k:'h',g:'H',mir:1});
/* 腕 */
ball('shL',6.6,'shL');
spec('shL',[0,0,0],DN,29,[[0,5.5,5.4],[.5,4.9,4.8],[1,4.5,4.4]],{ha:0,hb:0,k:'v',g:'uaL'});
ball('elL',5.1,'elL');
spec('elL',[0,0,0],DN,25,[[0,4.4,4.3],[.3,4.7,4.6],[1,3.3,3.1]],{ha:0,hb:0,k:'v',g:'faL'});
ball('wrL',3.5,'wrL');
spec('wrL',[0,-1.5,0],DN,8,[[0,3.4,2.3],[.2,4.4,2.2],[.7,4.3,1.9],[1,3.5,1.5]],{ha:0,hb:3.2,g:'handL'});
spec('wrL',[-2.8,-2.2,1.2],[-.35,-.9,.5],5,[[0,1.8,1.6],[1,1.1,1]],{ha:1.4,hb:1.2,g:'handL'});
/* 脚 */
ball('hpL',6.9,'hpL');
spec('hpL',[0,0,0],DN,42,[[0,7.8,8.3],[.3,7.7,8.1],[1,5.6,5.8]],{ha:0,hb:0,k:'v',g:'thL'});
ball('knL',5.7,'knL');
spec('knL',[0,0,0],DN,40,[[0,5.3,5.4],[.25,5.5,6.1],[.7,4.2,4.2],[1,3.4,3.4]],{ha:0,hb:0,k:'v',g:'shnL'});
ball('anL',3.9,'anL');
spec('anL',[0,-2.2,-3.6],[0,-.08,1],19.5,[[0,3.5,3.8],[.35,4.3,2.9],[.75,4.1,2.1],[1,3.1,1.6]],{ha:3.5,hb:2.4,g:'footL'});

/* ===== 描画(ソフトウェアのZバッファ) ===== */
var TG=[0,88,0];
function newCam(){return{yaw:0,pitch:8,frac:.8,dist:560,ox:0,oy:0}}
function camM(c){return mul(rx(c.pitch*D2R),ry(c.yaw*D2R))}
function project(S,cam,w,h){var F=fk(S),Rc=camM(cam),k=h*cam.frac/185;
  function pc(p){var q=mv(Rc,[p[0]-TG[0],p[1]-TG[1],p[2]-TG[2]]),s=k*cam.dist/Math.max(40,cam.dist-q[2]);return{x:w/2+(cam.ox||0)*w+q[0]*s,y:h/2+(cam.oy||0)*h-q[1]*s,z:q[2],s:s,q:q}}
  return{jt:J.map(function(j,i){return pc(F.P[i])}),Rc:Rc,k:k,F:F,pc:pc}}
function render(S,cam,w,h,o){
  var F=fk(S),Rc=camM(cam),k=h*cam.frac/185,D=cam.dist,ox=(cam.ox||0)*w,oy=(cam.oy||0)*h,sil=o.style==='sil',base=STY[o.style||'skin']||STY.skin;
  var an=(o.light==null?300:o.light)*D2R,Lx=Math.cos(an)*.6,Ly=Math.sin(an)*.6+.25,Lz=.75,ll=Math.hypot(Lx,Ly,Lz);Lx/=ll;Ly/=ll;Lz/=ll;
  var zb=new Float32Array(w*h).fill(-1e9),ib=new Uint8Array(w*h),px=new Uint8ClampedArray(w*h*4),hs=S.hs,bt=S.bt;
  for(var pi=0;pi<PARTS.length;pi++){var p=PARTS[pi],R=F.R[p.j],P=F.P[p.j],M=mul(Rc,R),tq=mv(Rc,[P[0]-TG[0],P[1]-TG[1],P[2]-TG[2]]),nv=p.nv,
    sx=new Float32Array(nv),sy=new Float32Array(nv),sz=new Float32Array(nv),nx=new Float32Array(nv),ny=new Float32Array(nv),nz=new Float32Array(nv),
    kx=1,ky=1,kz=1,ix=1,iy=1,iz=1;
    if(p.k==='h'){kx=ky=kz=hs}else if(p.k==='v'){kx=kz=bt;ix=iz=1/bt}else if(p.k==='b'){kx=ky=kz=bt}
    for(var i=0;i<nv;i++){var q=i*3,x=p.V[q]*kx,y=p.V[q+1]*ky,z=p.V[q+2]*kz,
      cx=tq[0]+M[0]*x+M[1]*y+M[2]*z,cy=tq[1]+M[3]*x+M[4]*y+M[5]*z,cz=tq[2]+M[6]*x+M[7]*y+M[8]*z,s=k*D/Math.max(40,D-cz);
      sx[i]=w/2+ox+cx*s;sy[i]=h/2+oy-cy*s;sz[i]=cz;
      var lx=p.N[q]*ix,ly=p.N[q+1],lz=p.N[q+2]*iz,l=Math.hypot(lx,ly,lz)||1;lx/=l;ly/=l;lz/=l;
      nx[i]=M[0]*lx+M[1]*ly+M[2]*lz;ny[i]=M[3]*lx+M[4]*ly+M[5]*lz;nz[i]=M[6]*lx+M[7]*ly+M[8]*lz}
    var T=p.T,id=p.id,tint=p.tint,b0=sil?29:base[0]*tint,b1=sil?39:base[1]*tint,b2=sil?51:base[2]*tint;
    for(var t=0;t<T.length;t+=3){var i0=T[t],i1=T[t+1],i2=T[t+2],x0=sx[i0],y0=sy[i0],x1=sx[i1],y1=sy[i1],x2=sx[i2],y2=sy[i2],
      area=(x1-x0)*(y2-y0)-(x2-x0)*(y1-y0);if(area>-1e-6&&area<1e-6)continue;var inv=1/area,
      minx=Math.max(0,Math.floor(Math.min(x0,x1,x2))),maxx=Math.min(w-1,Math.ceil(Math.max(x0,x1,x2))),
      miny=Math.max(0,Math.floor(Math.min(y0,y1,y2))),maxy=Math.min(h-1,Math.ceil(Math.max(y0,y1,y2)));
      for(var y=miny;y<=maxy;y++){var py=y+.5;for(var x=minx;x<=maxx;x++){var qx=x+.5,
        w0=((x1-qx)*(y2-py)-(x2-qx)*(y1-py))*inv,w1=((x2-qx)*(y0-py)-(x0-qx)*(y2-py))*inv,w2=1-w0-w1;
        if(w0<-.002||w1<-.002||w2<-.002)continue;
        var z=w0*sz[i0]+w1*sz[i1]+w2*sz[i2],ix2=y*w+x;if(z<=zb[ix2])continue;zb[ix2]=z;ib[ix2]=id;var o4=ix2*4;
        if(sil){px[o4]=b0;px[o4+1]=b1;px[o4+2]=b2;px[o4+3]=255;continue}
        var ax=w0*nx[i0]+w1*nx[i1]+w2*nx[i2],ay=w0*ny[i0]+w1*ny[i1]+w2*ny[i2],az=w0*nz[i0]+w1*nz[i1]+w2*nz[i2],al=Math.sqrt(ax*ax+ay*ay+az*az)||1;
        ax/=al;ay/=al;az/=al;var d=ax*Lx+ay*Ly+az*Lz;if(d<0)d=0;
        var sh=.34+.56*d+.14*(.5+.5*ay);if(az<.55)sh*=.8+.2*Math.max(0,az)/.55;
        px[o4]=b0*sh;px[o4+1]=b1*sh;px[o4+2]=b2*sh;px[o4+3]=255}}}}
  if(!sil){var ed=new Uint8Array(w*h);
    for(var y=0;y<h;y++)for(var x=0;x<w;x++){var i=y*w+x,id=ib[i];if(!id)continue;var e=0,nb=[x<w-1?i+1:-1,y<h-1?i+w:-1,x>0?i-1:-1,y>0?i-w:-1];
      for(var q=0;q<4;q++){var j=nb[q];if(j<0)continue;var nid=ib[j];if(nid!==id){if(nid===0)e=2;else if(zb[i]>=zb[j]&&e<1)e=1}}
      ed[i]=e}
    for(var i=0;i<ed.length;i++){if(!ed[i])continue;var o4=i*4;
      if(ed[i]===2){px[o4]=px[o4]*.25+36;px[o4+1]=px[o4+1]*.25+28;px[o4+2]=px[o4+2]*.25+25}
      else{px[o4]*=.58;px[o4+1]*=.58;px[o4+2]*=.58}}}
  return{px:px,F:F}}
var OFF=null;
function draw(ctx,w,h,S,cam,o){var ss=o.ss||1,W2=Math.round(w*ss),H2=Math.round(h*ss),r=render(S,cam,W2,H2,o),pr=project(S,cam,w,h);
  OFF=OFF||document.createElement('canvas');OFF.width=W2;OFF.height=H2;OFF.getContext('2d').putImageData(new ImageData(r.px,W2,H2),0,0);
  ctx.clearRect(0,0,w,h);
  if(o.shadow){var g=pr.pc([S.rp[0],0,S.rp[2]]),rr=44*g.s,ry2=rr*Math.max(.1,Math.sin(Math.abs(cam.pitch)*D2R)+.1);
    ctx.save();ctx.fillStyle='rgba(0,0,0,.18)';ctx.beginPath();ctx.ellipse(g.x,g.y,rr,ry2,0,0,7);ctx.fill();ctx.restore()}
  ctx.save();ctx.imageSmoothingEnabled=true;ctx.imageSmoothingQuality='high';ctx.drawImage(OFF,0,0,W2,H2,0,0,w,h);ctx.restore();
  if(o.joints){
    var GC=function(i){var n=J[i][0];return/^(sh|el|wr)/.test(n)?'#4d8dff':/^(hp|kn|an)/.test(n)?'#2fb57a':'#9a6bff'};
    ctx.save();ctx.lineCap='round';
    [[5,'rgba(255,255,255,.9)'],[1.6,'rgba(30,45,90,.75)']].forEach(function(st){ctx.lineWidth=st[0];ctx.strokeStyle=st[1];
      J.forEach(function(j,i){if(j[2]>=0){var a1=pr.jt[j[2]],b1=pr.jt[i];ctx.beginPath();ctx.moveTo(a1.x,a1.y);ctx.lineTo(b1.x,b1.y);ctx.stroke()}})});
    pr.jt.forEach(function(j,i){var act=i===o.sel||i===o.hover;
      if(i===o.hover){ctx.beginPath();ctx.arc(j.x,j.y,15,0,7);ctx.fillStyle='rgba(255,255,255,.45)';ctx.fill()}
      ctx.beginPath();ctx.arc(j.x,j.y,act?10:7,0,7);ctx.fillStyle=i===o.sel?'#ff9f1c':GC(i);ctx.fill();ctx.lineWidth=2.5;ctx.strokeStyle='#fff';ctx.stroke();
      ctx.lineWidth=1;ctx.strokeStyle='rgba(0,0,0,.45)';ctx.stroke()});
    [o.sel,o.hover].filter(function(v,k,arr){return v>=0&&arr.indexOf(v)===k}).forEach(function(i){var j=pr.jt[i],t=J[i][1];
      ctx.font='bold 15px sans-serif';var bw=ctx.measureText(t).width+16,x=Math.min(w-bw-4,Math.max(4,j.x+14)),y=Math.max(4,j.y-34);
      ctx.fillStyle='rgba(20,24,32,.9)';ctx.fillRect(x,y,bw,24);ctx.fillStyle='#fff';ctx.textBaseline='middle';ctx.fillText(t,x+8,y+12)});
    ctx.restore()}
  return pr}
function hit(S,cam,w,h,x,y){var pr=project(S,cam,w,h),best=-1,bd=18*18;
  pr.jt.forEach(function(j,i){var d=(j.x-x)*(j.x-x)+(j.y-y)*(j.y-y);if(d<bd){bd=d;best=i}});return best}
function applyCam(S,cam,i,dRcam){var F=fk(S),Rc=camM(cam),dRw=mul(tr(Rc),mul(dRcam,Rc)),p=J[i][2],
  Rn=mul(dRw,F.R[i]),Rl=p<0?Rn:mul(tr(F.R[p]),Rn);S.rot[i]=m2eul(Rl)}
function dragRot(S,cam,i,dx,dy){applyCam(S,cam,i,mul(rx(dy*.012),ry(dx*.012)))}
function dragFollow(S,cam,w,h,i,px,py){var pv=JI[PIV[J[i][0]]],pr=project(S,cam,w,h),Pq=pr.jt[pv],Jq=pr.jt[i],
  v=[Jq.q[0]-Pq.q[0],Jq.q[1]-Pq.q[1],Jq.q[2]-Pq.q[2]],L=Math.hypot(v[0],v[1],v[2]);if(L<1e-6)return;
  var tx=(px-Pq.x)/Pq.s,ty=-(py-Pq.y)/Pq.s,r2=tx*tx+ty*ty,vz;
  if(r2>=L*L*.9996){var f=L*.9998/Math.sqrt(r2);tx*=f;ty*=f;r2=tx*tx+ty*ty}
  vz=Math.sqrt(Math.max(0,L*L-r2))*(v[2]<0?-1:1);var n=[tx,ty,vz];
  var ax=[v[1]*n[2]-v[2]*n[1],v[2]*n[0]-v[0]*n[2],v[0]*n[1]-v[1]*n[0]],
  ang=Math.atan2(Math.hypot(ax[0],ax[1],ax[2]),v[0]*n[0]+v[1]*n[1]+v[2]*n[2]);
  if(ang<1e-5)return;applyCam(S,cam,pv,rod(ax,ang))}
function mirror(S){var T=newState();T.hs=S.hs;T.bt=S.bt;T.rp=[-S.rp[0],S.rp[1],S.rp[2]];
  J.forEach(function(j,i){var n=j[0],m=n.slice(-1)==='L'?n.slice(0,-1)+'R':n.slice(-1)==='R'?n.slice(0,-1)+'L':n,k=JI[m],e=S.rot[i];T.rot[k]=[e[0],-e[1],-e[2]]});return T}
function pose(o,g){var S=newState();for(var k in o){if(k==='rp')S.rp=o[k].slice();else S.rot[JI[k]]=o[k].slice()}
  if(g===1){var F=fk(S),y=Math.min(F.P[JI.anL][1],F.P[JI.anR][1]);S.rp[1]+=6-y}else if(g===3){S.rp[1]=11-92}return S}
var PRESETS=[
 /* 立つ */
 ['立ち',pose({shL:[0,0,8],shR:[0,0,-8],elL:[-8,0,0],elR:[-8,0,0]}),'立つ'],
 ['手を腰に',pose({shL:[0,0,38],shR:[0,0,-38],elL:[-20,0,-105],elR:[-20,0,105],hpL:[0,0,3],hpR:[0,0,-3]},1),'立つ'],
 ['腕組み',pose({shL:[-22,0,8],shR:[-22,0,-8],elL:[-125,-78,0],elR:[-125,78,0],head:[4,0,0]},1),'立つ'],
 ['後ろで手を組む',pose({shL:[38,0,6],shR:[38,0,-6],elL:[-15,0,-22],elR:[-15,0,22]},1),'立つ'],
 ['手を広げる',pose({shL:[0,0,78],shR:[0,0,-78],elL:[-12,0,0],elR:[-12,0,0],head:[-6,0,0]},1),'立つ'],
 ['おじぎ',pose({spine:[20,0,0],chest:[22,0,0],neck:[10,0,0],shL:[-8,0,6],shR:[-8,0,-6],elL:[-12,0,0],elR:[-12,0,0]},1),'立つ'],
 ['振り向き',pose({spine:[0,18,0],chest:[0,22,0],neck:[0,25,0],head:[0,20,0],shL:[0,0,10],shR:[0,0,-10]},1),'立つ'],
 ['見上げる',pose({neck:[-22,0,0],head:[-16,0,0],spine:[-4,0,0],shL:[0,0,10],shR:[0,0,-10]},1),'立つ'],
 ['モデル立ち',pose({root:[0,0,6],spine:[0,0,-5],chest:[0,0,-4],hpL:[-12,0,-6],knL:[10,0,0],hpR:[8,0,4],knR:[4,0,0],shL:[0,0,30],elL:[-20,0,-95],shR:[0,0,-10],elR:[-10,0,0],head:[0,0,5]},1),'立つ'],
 /* 動く */
 ['歩く',pose({hpL:[-26,0,0],knL:[8,0,0],hpR:[18,0,0],knR:[30,0,0],shL:[22,0,6],shR:[-26,0,-6],elL:[-18,0,0],elR:[-26,0,0],spine:[3,0,0]}),'動く'],
 ['走る',pose({spine:[14,0,0],chest:[6,0,0],hpL:[-72,0,0],knL:[78,0,0],anL:[-12,0,0],hpR:[28,0,0],knR:[100,0,0],shL:[52,0,12],elL:[-95,0,0],shR:[-62,0,-12],elR:[-95,0,0],rp:[0,-4,0]}),'動く'],
 ['ジャンプ',pose({hpL:[-60,0,10],knL:[85,0,0],hpR:[-35,0,-10],knR:[95,0,0],shL:[0,0,140],shR:[0,0,-140],elL:[-20,0,0],elR:[-20,0,0],spine:[-4,0,0],rp:[0,40,0]}),'動く'],
 ['キック',pose({root:[0,0,-4],spine:[-8,0,0],hpR:[-85,0,-4],knR:[25,0,0],anR:[-20,0,0],hpL:[4,0,3],shL:[0,0,55],elL:[-20,0,0],shR:[-30,0,-70],elR:[-30,0,0]},1),'動く'],
 ['パンチ',pose({spine:[6,22,0],chest:[0,12,0],hpL:[-22,0,14],knL:[22,0,0],hpR:[18,0,-12],knR:[24,0,0],shR:[-88,0,-6],elR:[0,0,0],shL:[-46,0,18],elL:[-110,0,-8]},1),'動く'],
 ['構え',pose({spine:[8,-14,0],hpL:[-18,0,12],knL:[24,0,0],hpR:[16,0,-10],knR:[28,0,0],shL:[-50,0,14],elL:[-115,0,-10],shR:[-60,0,-16],elR:[-115,0,10]},1),'動く'],
 ['投げる',pose({spine:[4,-24,0],chest:[0,-14,0],hpL:[-24,0,6],knL:[16,0,0],hpR:[14,0,-6],knR:[14,0,0],shR:[40,0,-95],elR:[-95,0,-5],shL:[-70,0,20],elL:[-20,0,0]},1),'動く'],
 /* 座る・寝る */
 ['座る',pose({hpL:[-90,0,3],hpR:[-90,0,-3],knL:[90,0,0],knR:[90,0,0],shL:[-20,0,8],shR:[-20,0,-8],elL:[-60,0,0],elR:[-60,0,0],rp:[0,-42,0]}),'座る・寝る'],
 ['あぐら',pose({hpL:[-129,0,-90],hpR:[-129,0,90],knL:[127,0,0],knR:[124,0,0],spine:[4,0,0],shL:[-22,0,16],shR:[-22,0,-16],elL:[-75,0,0],elR:[-75,0,0]},3),'座る・寝る'],
 ['体育座り',pose({hpL:[-135,0,8],hpR:[-135,0,-8],knL:[160,0,0],knR:[160,0,0],spine:[10,0,0],shL:[-62,0,10],shR:[-62,0,-10],elL:[-105,0,-40],elR:[-105,0,40]},1),'座る・寝る'],
 ['しゃがむ',pose({hpL:[-120,0,10],hpR:[-120,0,-10],knL:[145,0,0],knR:[145,0,0],spine:[18,0,0],chest:[10,0,0],anL:[-8,0,0],anR:[-8,0,0],shL:[-50,0,12],shR:[-50,0,-12],elL:[-40,0,0],elR:[-40,0,0]},1),'座る・寝る'],
 ['片ひざ立ち',pose({hpL:[-90,0,3],knL:[90,0,0],hpR:[18,0,-3],knR:[72,0,0],anR:[40,0,0],spine:[6,0,0],shL:[-30,0,8],elL:[-60,0,0],shR:[-20,0,-8],elR:[-30,0,0]},1),'座る・寝る'],
 ['寝る',pose({root:[-90,0,0],shL:[0,0,12],shR:[0,0,-12],elL:[-6,0,0],elR:[-6,0,0],rp:[0,-81,-12]}),'座る・寝る',{yaw:25,pitch:55}],
 /* 腕・手 */
 ['手を振る',pose({shR:[0,0,-150],elR:[-30,0,0],shL:[0,0,8],elL:[-8,0,0],head:[0,0,-6]}),'腕・手'],
 ['万歳',pose({shL:[0,0,168],shR:[0,0,-168],elL:[0,0,0],elR:[0,0,0]}),'腕・手'],
 ['敬礼',pose({shR:[0,0,-88],elR:[-12,0,-140],shL:[0,0,6],elL:[-6,0,0]},1),'腕・手'],
 ['ガッツポーズ',pose({shL:[-32,0,26],shR:[-32,0,-26],elL:[-128,0,0],elR:[-128,0,0],spine:[-5,0,0],head:[-10,0,0],hpL:[-6,0,4],hpR:[4,0,-4],knL:[8,0,0],knR:[6,0,0]},1),'腕・手'],
 ['指さし',pose({shR:[-86,0,-8],elR:[0,0,0],shL:[0,0,10],elL:[-10,0,0],head:[0,-8,0]},1),'腕・手'],
 ['祈り',pose({shL:[-28,0,4],shR:[-28,0,-4],elL:[-118,0,-30],elR:[-118,0,30],neck:[8,0,0]},1),'腕・手']];
return{J:J,N:N,JI:JI,newState:newState,newCam:newCam,draw:draw,render:render,project:project,hit:hit,dragRot:dragRot,dragFollow:dragFollow,mirror:mirror,PRESETS:PRESETS,STY:STY,PIV:PIV,m2eul:m2eul,eul2m:eul2m,fk:fk,PARTS:PARTS}
})();

/* ---- 3Dモデルの画面 ---- */
var MS=MQ.newState(),MC=MQ.newCam(),MO={style:'skin',shadow:true,joints:true,sel:4,hover:-1,light:300,ss:1},MM='pose',mcv=$('mc'),mctx=mcv.getContext('2d'),mdrag=null,mqf=false,UPO=[],MH=[];
try{var sv0=JSON.parse(localStorage.getItem('ps_pose')||'null');if(sv0&&sv0.rot&&sv0.rot.length===MQ.N)MS=sv0;UPO=JSON.parse(localStorage.getItem('ps_poses')||'[]')}catch(err){}
function mSave(){try{localStorage.setItem('ps_pose',JSON.stringify(MS))}catch(err){}}
function mSaveU(){try{localStorage.setItem('ps_poses',JSON.stringify(UPO))}catch(err){}}
function clone(o){return JSON.parse(JSON.stringify(o))}
function mpush(){MH.push(clone(MS));if(MH.length>30)MH.shift()}
var MHINT={pose:'青い点(関節)をドラッグすると、腕や脚が動きます。何もない所をドラッグすると、人形が回ります。',view:'ドラッグで、人形の向きを変えます。',move:'ドラッグで、人形の位置を動かします。この位置のまま絵に貼り付けます。'};
function mCap(i){var t;if(i>=0&&MM==='pose'){var n=MQ.J[i][0],pv=MQ.PIV[n];
  t='「'+MQ.J[i][1]+'」: '+(pv?'ドラッグすると、「'+MQ.J[MQ.JI[pv]][1]+'」を支点にして動きます。':'ドラッグすると、この関節が回ります。')}else t=MHINT[MM];$('mcap').textContent=t}
function mFit(){var lg=640,r=W/H,cw=r>=1?lg:Math.round(lg*r),ch=r>=1?Math.round(lg/r):lg;
  if(mcv.width!==cw||mcv.height!==ch){mcv.width=cw;mcv.height=ch}
  var bw=($('mcbox').clientWidth-16)||500,mh=Math.max(240,window.innerHeight*.58),sc=Math.min(bw/cw,mh/ch,1.6);
  mcv.style.width=Math.round(cw*sc)+'px';mcv.style.height=Math.round(ch*sc)+'px'}
function mRender(){if(mqf)return;mqf=true;requestAnimationFrame(function(){mqf=false;MQ.draw(mctx,mcv.width,mcv.height,MS,MC,MO)})}
function mSync(){var e=MS.rot[MO.sel];$('mj').value=MO.sel;
  ['x','y','z'].forEach(function(a,i){$('mr'+a).value=Math.round(e[i]);$('mrv'+a).textContent=Math.round(e[i])+'°'});
  $('mhs').value=Math.round(MS.hs*100);$('mhsv').textContent=Math.round(MS.hs*100)+'%';
  $('mbt').value=Math.round(MS.bt*100);$('mbtv').textContent=Math.round(MS.bt*100)+'%';
  [['mpx',0],['mpy',1],['mpz',2]].forEach(function(a){$(a[0]).value=Math.round(MS.rp[a[1]]);$(a[0]+'v').textContent=Math.round(MS.rp[a[1]])});
  $('mzm').value=Math.round(MC.frac*100);$('mzv').textContent=Math.round(MC.frac*100)+'%'}
function mpos(e){var r=mcv.getBoundingClientRect();return{x:(e.clientX-r.left)*mcv.width/r.width,y:(e.clientY-r.top)*mcv.height/r.height}}
function setPose(T,cam){mpush();T.hs=MS.hs;T.bt=MS.bt;MS=T;if(cam){MC.yaw=cam.yaw;MC.pitch=cam.pitch}mSave();mSync();mRender()}
(function(){var h='';MQ.J.forEach(function(j,i){h+='<option value="'+i+'">'+j[1]+'</option>'});$('mj').innerHTML=h;
  var cats=[];MQ.PRESETS.forEach(function(P){if(cats.indexOf(P[2])<0)cats.push(P[2])});
  var pc=$('mpc'),pr=$('mpr');
  function showCat(c){pc.innerHTML='';pr.innerHTML='';
    cats.forEach(function(n){var t=document.createElement('button');t.textContent=n;t.className=n===c?'on':'';t.onclick=function(){showCat(n)};pc.appendChild(t)});
    MQ.PRESETS.forEach(function(P){if(P[2]!==c)return;var b=document.createElement('button');b.textContent=P[0];b.onclick=function(){setPose(clone(P[1]),P[3])};pr.appendChild(b)})}
  showCat(cats[0]);
  var vw=$('mvw');[['正面',0,8],['斜め',35,10],['横',90,8],['後ろ',180,8],['上から',0,70]].forEach(function(v){var b=document.createElement('button');b.textContent=v[0];
    b.onclick=function(){MC.yaw=v[1];MC.pitch=v[2];mRender()};vw.appendChild(b)})})();
function renderUP(){var el=$('mup');el.innerHTML='';
  UPO.forEach(function(P,i){var b=document.createElement('button');b.textContent=P.n;b.title='ダブルクリックで削除';
    b.onclick=function(){setPose(clone(P.s))};
    b.ondblclick=function(){askYes('「'+P.n+'」を削除しますか?',function(){UPO.splice(i,1);mSaveU();renderUP()})};el.appendChild(b)})}
document.querySelectorAll('[data-mm]').forEach(function(b){b.onclick=function(){MM=b.dataset.mm;
  document.querySelectorAll('[data-mm]').forEach(function(x){x.classList.toggle('on',x===b)});MO.hover=-1;mCap(-1);mcv.style.cursor=MM==='move'?'move':'grab';mRender()}});
$('msv').onclick=function(){askText('ポーズの名前','ポーズ '+(UPO.length+1),function(n){if(!n)return;UPO.push({n:n.slice(0,10),s:clone(MS)});UPO=UPO.slice(-24);mSaveU();renderUP();toast('ポーズを保存しました')})};
$('mun').onclick=function(){if(!MH.length)return;MS=MH.pop();mSave();mSync();mRender()};
$('mmi').onclick=function(){mpush();MS=MQ.mirror(MS);mSave();mSync();mRender()};
$('mre').onclick=function(){mpush();MS=MQ.newState();mSave();mSync();mRender()};
$('mjr').onclick=function(){mpush();MS.rot[MO.sel]=[0,0,0];mSave();mSync();mRender()};
$('mj').onchange=function(){MO.sel=+this.value;mSync();mRender()};
['x','y','z'].forEach(function(a,i){$('mr'+a).oninput=function(){MS.rot[MO.sel][i]=+this.value;$('mrv'+a).textContent=this.value+'°';mSave();mRender()}});
$('mhs').oninput=function(){MS.hs=this.value/100;$('mhsv').textContent=this.value+'%';mSave();mRender()};
$('mbt').oninput=function(){MS.bt=this.value/100;$('mbtv').textContent=this.value+'%';mSave();mRender()};
[['mpx',0],['mpy',1],['mpz',2]].forEach(function(a){$(a[0]).oninput=function(){MS.rp[a[1]]=+this.value;$(a[0]+'v').textContent=this.value;mSave();mRender()}});
['mrx','mry','mrz','mhs','mbt','mpx','mpy','mpz'].forEach(function(id){$(id).addEventListener('pointerdown',mpush)});
$('mzm').oninput=function(){MC.frac=this.value/100;$('mzv').textContent=this.value+'%';mRender()};
$('mli').oninput=function(){MO.light=+this.value;mRender()};
$('mst').onchange=function(){MO.style=this.value;mRender()};
$('mjo').onchange=function(){MO.joints=this.checked;mRender()};
$('msh').onchange=function(){MO.shadow=this.checked;mRender()};
mcv.addEventListener('pointerdown',function(e){mcv.setPointerCapture(e.pointerId);$('mhint').style.display='none';
  var p=mpos(e),j=MM==='pose'?MQ.hit(MS,MC,mcv.width,mcv.height,p.x,p.y):-1;
  if(j>=0){mpush();MO.sel=j;mdrag={t:'j',j:j,x:p.x,y:p.y};mSync()}
  else if(MM==='move')mdrag={t:'m',x:p.x,y:p.y,ox:MC.ox,oy:MC.oy};
  else mdrag={t:'c',x:p.x,y:p.y,yaw:MC.yaw,pitch:MC.pitch};
  MO.ss=.6;mRender()});
mcv.addEventListener('pointermove',function(e){var p=mpos(e);
  if(!mdrag){if(MM!=='pose')return;var h=MQ.hit(MS,MC,mcv.width,mcv.height,p.x,p.y);
    if(h!==MO.hover){MO.hover=h;mcv.style.cursor=h>=0?'pointer':'grab';mCap(h);mRender()}return}
  if(mdrag.t==='j'){var nm=MQ.J[mdrag.j][0];
    if(MQ.PIV[nm])MQ.dragFollow(MS,MC,mcv.width,mcv.height,mdrag.j,p.x,p.y);else MQ.dragRot(MS,MC,mdrag.j,p.x-mdrag.x,p.y-mdrag.y);
    mdrag.x=p.x;mdrag.y=p.y;mSync()}
  else if(mdrag.t==='m'){MC.ox=mdrag.ox+(p.x-mdrag.x)/mcv.width;MC.oy=mdrag.oy+(p.y-mdrag.y)/mcv.height}
  else{MC.yaw=mdrag.yaw+(p.x-mdrag.x)*.5;MC.pitch=Math.max(-80,Math.min(80,mdrag.pitch+(p.y-mdrag.y)*.4))}
  mRender()});
function mend(){if(mdrag){mSave();mdrag=null;MO.ss=1;mRender()}}
mcv.addEventListener('pointerup',mend);mcv.addEventListener('pointercancel',mend);
mcv.addEventListener('pointerleave',function(){if(!mdrag&&MO.hover>=0){MO.hover=-1;mCap(-1);mRender()}});
mcv.addEventListener('wheel',function(e){e.preventDefault();MC.frac=Math.max(.3,Math.min(1.4,MC.frac*(e.deltaY<0?1.08:1/1.08)));mSync();mRender()},{passive:false});
$('b3').onclick=function(){$('mhint').style.display='';MM='pose';document.querySelectorAll('[data-mm]').forEach(function(x){x.classList.toggle('on',x.dataset.mm==='pose')});
  $('md').showModal();mFit();mSync();renderUP();MO.hover=-1;mCap(-1);mcv.style.cursor='grab';mRender()};
window.addEventListener('resize',function(){if($('md').open){mFit();mRender()}});
$('mcl').onclick=function(){$('md').close()};
$('mok').onclick=function(){var cam={yaw:MC.yaw,pitch:MC.pitch,frac:MC.frac,dist:MC.dist,ox:MC.ox,oy:MC.oy},l=mk('3Dモデル');
  MQ.draw(l.x,W,H,MS,cam,{style:MO.style,shadow:false,joints:false,light:MO.light,ss:Math.max(1,Math.min(2,3000/Math.max(W,H)))});
  if($('mtr').checked)l.o=.5;
  layers.splice(layers.indexOf(cur)+1,0,l);cur=l;rl();$('md').close()};

/* ---- 入力・確認・メッセージ(ブラウザの標準ダイアログが使えない環境でも動くように、自前で作る) ---- */
var toastT=null;
function toast(msg,ms){var t=$('toast'),d=document.querySelector('dialog[open]');(d||document.body).appendChild(t);t.textContent=msg;t.classList.add('on');
  clearTimeout(toastT);toastT=setTimeout(function(){t.classList.remove('on')},ms||3200)}
function askBox(title,msg,def,btns,cb){$('at').textContent=title;$('am').textContent=msg||'';var inp=$('ai'),bx=$('ab'),done=false;
  inp.style.display=def==null?'none':'';inp.value=def==null?'':def;bx.innerHTML='';
  btns.forEach(function(lb,i){var b=document.createElement('button');b.textContent=lb;if(i===0)b.className='on';
    b.onclick=function(){done=true;$('ad').close();cb(i,inp.value)};bx.appendChild(b)});
  inp.onkeydown=function(e){if(e.key==='Enter'){e.preventDefault();bx.children[0].onclick()}};
  $('ad').onclose=function(){if(!done)cb(-1,'')};
  $('ad').showModal();if(def!=null){inp.focus();inp.select()}}
function askText(title,def,cb,msg){askBox(title,msg,def,['OK','キャンセル'],function(i,v){if(i===0)cb(v)})}
function askYes(msg,cb){askBox('確認',msg,null,['はい','キャンセル'],function(i){if(i===0)cb()})}

/* ---- ギャラリー ---- */
var GDB=null,curId=null,curName='無題',gDirty=false,gHDirty=false,gMsg='',gMsgT=0;
function mdirty(){gDirty=true;gHDirty=true}
function lsGet(k){try{return localStorage.getItem(k)}catch(e){return null}}
function lsSet(k,v){try{localStorage.setItem(k,v)}catch(e){}}
function gOpen(cb){if(GDB)return cb(GDB);
  try{var rq=indexedDB.open('atelierpaint',1);
    rq.onupgradeneeded=function(){var db=rq.result;db.createObjectStore('works',{keyPath:'id'});db.createObjectStore('data',{keyPath:'id'})};
    rq.onsuccess=function(){GDB=rq.result;cb(GDB)};rq.onerror=function(){cb(null)};rq.onblocked=function(){cb(null)}}
  catch(e){cb(null)}}
function gAll(cb){gOpen(function(db){if(!db)return cb(null);var r=db.transaction('works').objectStore('works').getAll();r.onsuccess=function(){cb(r.result||[])};r.onerror=function(){cb(null)}})}
function gGetBoth(id,cb){gOpen(function(db){if(!db)return cb();var tx=db.transaction(['works','data']),a,b;
  tx.objectStore('works').get(id).onsuccess=function(e){a=e.target.result};tx.objectStore('data').get(id).onsuccess=function(e){b=e.target.result};
  tx.oncomplete=function(){cb(a,b)};tx.onerror=function(){cb()}})}
function gPutBoth(meta,data,cb){gOpen(function(db){if(!db)return cb(false);var tx=db.transaction(data?['works','data']:['works'],'readwrite');
  tx.objectStore('works').put(meta);if(data)tx.objectStore('data').put(data);tx.oncomplete=function(){cb(true)};tx.onerror=tx.onabort=function(){cb(false)}})}
function gDelete(id,cb){gOpen(function(db){if(!db)return cb(false);var tx=db.transaction(['works','data'],'readwrite');
  tx.objectStore('works').delete(id);tx.objectStore('data').delete(id);tx.oncomplete=function(){cb(true)};tx.onerror=function(){cb(false)}})}
function newId(){return Date.now().toString(36)+Math.random().toString(36).slice(2,6)}
function p2(n){return('0'+n).slice(-2)}
function fmtDate(t){var d=new Date(t);return d.getFullYear()+'/'+(d.getMonth()+1)+'/'+d.getDate()+' '+p2(d.getHours())+':'+p2(d.getMinutes())}
function defaultName(){var d=new Date();return'無題の絵 '+(d.getMonth()+1)+'/'+d.getDate()+' '+p2(d.getHours())+':'+p2(d.getMinutes())}
function gStatus(msg){if(msg){gMsg=msg;gMsgT=Date.now()+2500}
  var base='作品: '+curName+(curId?(gDirty?' ●変更あり':' ✓保存済み'):' (未保存)');
  $('gst').textContent=(gMsg&&Date.now()<gMsgT)?gMsg:base;$('gst').title=base}
function mkThumb(){var t=comp(),sc=Math.min(1,260/Math.max(W,H)),c=document.createElement('canvas');c.width=Math.max(1,Math.round(W*sc));c.height=Math.max(1,Math.round(H*sc));
  c.getContext('2d').drawImage(t,0,0,c.width,c.height);return c.toDataURL('image/jpeg',.82)}
function gSave(silent,cb){
  if(!curId){if(silent)return;askText('作品の名前',defaultName(),function(n){curName=(n||'').trim().slice(0,30)||defaultName();curId=newId();doSave()},'ギャラリーに保存します。');return}
  doSave();
  function doSave(){
    var id=curId,nm=curName,w=W,h=H,th=mkThumb(),ls=layers.map(function(l){return{n:l.n,v:l.v,o:l.o,m:l.m,k:l.clip?1:0}}),left=layers.length,blobs=[];
    gDirty=false;
    function fin(){gPutBoth({id:id,name:nm,t:Date.now(),w:w,h:h,th:th},{id:id,rl:rlPlain(),layers:ls.map(function(x,i){x.b=blobs[i];return x})},function(ok){
      if(ok){lsSet('ps_last',id);gStatus('保存しました');if(!silent)toast('「'+nm+'」をギャラリーに保存しました')}
      else{gDirty=true;if(!silent)toast('保存できませんでした。この画面ではブラウザの保存機能が制限されているようです。ファイルをダウンロードして、ブラウザで直接開いてお使いください。「HTMLで保存」でも絵を残せます。',7000)}
      if(cb)cb(ok);if($('gd').open)gRender()})}
    layers.forEach(function(l,i){l.c.toBlob(function(b){blobs[i]=b;if(--left===0)fin()},'image/png')})}}
function gLeave(cb){if(!(gDirty&&gHDirty))return cb();
  askBox('いまの絵は保存されていません','別の絵に切り替える前に、ギャラリーに保存しますか?',null,['保存して切り替える','保存しないで切り替える','キャンセル'],function(i){
    if(i===0)gSave(false,function(ok){if(ok)cb()});else if(i===1)cb()})}
function gOpenWork(id){gGetBoth(id,function(meta,data){if(!meta||!data){toast('この作品を開けませんでした。');return}
  var arr=[],left=data.layers.length;if(!left)return;
  data.layers.forEach(function(ly,i){var fr=new FileReader();fr.onload=function(){arr[i]={n:ly.n,v:ly.v,o:ly.o,m:ly.m,k:ly.k,u:fr.result};
    if(--left===0){init(meta.w,meta.h,arr);if(data.rl)rlLoad(data.rl);curId=id;curName=meta.name;gDirty=false;gHDirty=false;lsSet('ps_last',id);gStatus('開きました');if($('gd').open)$('gd').close()}};
    fr.readAsDataURL(ly.b)})})}
function gAutoResume(hasSaved){if(hasSaved)return;gAll(function(list){if(!list||!list.length||gDirty)return;var last=lsGet('ps_last'),w=list.filter(function(x){return x.id===last})[0];if(w)gOpenWork(w.id)})}
function gRender(){gAll(function(list){var el=$('gl');el.innerHTML='';
  if(list===null){el.textContent='このブラウザでは、ギャラリーが使えません。「HTMLで保存」で絵をファイルに残してください。';return}
  list.sort(function(a,b){return b.t-a.t});
  if(!list.length){el.textContent='まだ作品がありません。「いまの絵を保存」で、最初の1枚を入れましょう。'}
  list.forEach(function(w){var c=document.createElement('div');c.className='gc'+(w.id===curId?' cur':'');
    var im=document.createElement('img');im.src=w.th;im.alt=w.name;im.onclick=function(){openIt(w.id)};
    var n=document.createElement('div');n.className='gn';n.textContent=w.name;n.title=w.name;
    var m=document.createElement('div');m.className='gm';m.textContent=fmtDate(w.t)+' / '+w.w+'×'+w.h+(w.id===curId?' / 開いている絵':'');
    var b=document.createElement('div');b.className='gb';
    [['開く',function(){openIt(w.id)}],['名前',function(){askText('作品の名前',w.name,function(s){s=s.trim().slice(0,30);if(!s)return;w.name=s;gPutBoth(w,null,function(){if(w.id===curId)curName=s;gRender()})})}],
     ['複製',function(){gGetBoth(w.id,function(meta,data){if(!meta||!data)return;var id=newId(),m2={};for(var k in meta)m2[k]=meta[k];m2.id=id;m2.name=meta.name+' のコピー';m2.t=Date.now();
        gPutBoth(m2,{id:id,layers:data.layers},function(){gRender()})})}],
     ['削除',function(){askYes('「'+w.name+'」を削除しますか?(元に戻せません)',function(){gDelete(w.id,function(){if(w.id===curId){curId=null;gDirty=true}gRender()})})}]].forEach(function(a){
        var bt=document.createElement('button');bt.textContent=a[0];bt.onclick=a[1];b.appendChild(bt)});
    c.appendChild(im);c.appendChild(n);c.appendChild(m);c.appendChild(b);el.appendChild(c)});
  try{navigator.storage.estimate().then(function(e){$('gus').textContent='保存容量: '+(e.usage/1048576).toFixed(1)+'MB 使用中(ブラウザが許す上限の目安: '+Math.round(e.quota/1048576)+'MB)'})}catch(err){}})}
function openIt(id){if(id===curId&&!gDirty){$('gd').close();return}gLeave(function(){gOpenWork(id)})}
$('bgal').onclick=function(){$('gd').showModal();gRender()};
$('gcl').onclick=function(){$('gd').close()};
$('bsvg').onclick=function(){gSave(false)};
$('gsv').onclick=function(){gSave(false,function(){})};
$('gnw').onclick=function(){askText('新しいキャンバスの大きさ(幅x高さ)','1200x800',function(s){var m=s.match(/(\d+)\s*[x×*,]\s*(\d+)/i);if(!m){toast('「1200x800」のように入力してください');return}
  gLeave(function(){init(Math.min(4000,+m[1]),Math.min(4000,+m[2]));curId=null;curName='無題';gDirty=false;gHDirty=false;$('gd').close();gStatus('新しい絵です')})})};
document.addEventListener('keydown',function(e){if((e.ctrlKey||e.metaKey)&&e.key.toLowerCase()==='s'){e.preventDefault();gSave(false)}});
window.addEventListener('beforeunload',function(e){if(gDirty&&gHDirty){e.preventDefault();e.returnValue=''}});
setInterval(function(){if(curId&&gDirty&&!document.querySelector('dialog[open]'))gSave(true);gStatus()},45000);
setInterval(function(){gStatus()},1500);

/* ---- パース定規 ---- */
var RL=null,RLC=['rgba(77,141,255,','rgba(255,159,28,','rgba(47,181,122,'],RLB=['#4d8dff','#ff9f1c','#2fb57a'];
function rlDef(m){var w=W,h=H;return m===1?[[w*.5,h*.45]]:m===2?[[-w*.3,h*.45],[w*1.3,h*.45]]:[[-w*.3,h*.45],[w*1.3,h*.45],[w*.5,h*2.5]]}
function rlNew(){return{m:0,snap:1,show:1,n:24,op:55,grid:0,sets:{1:rlDef(1),2:rlDef(2),3:rlDef(3)},W:W,H:H,lock:null}}
function rlV(){return RL&&RL.sets[RL.m]||[]}
function rlPlain(){if(!RL)return null;var o={};for(var k in RL)if(k!=='lock')o[k]=RL[k];return o}
function rlSave(){try{localStorage.setItem('ps_ruler',JSON.stringify(rlPlain()))}catch(e){}}
function rlLoad(o){RL=rlNew();if(o){for(var k in o)if(k!=='lock'&&o[k]!==undefined)RL[k]=o[k]}
  [1,2,3].forEach(function(m){if(!RL.sets[m]||RL.sets[m].length!==m)RL.sets[m]=rlDef(m)});rlOnInit();rlUI()}
function rlOnInit(){if(!RL)return;
  if(RL.W&&RL.H&&(RL.W!==W||RL.H!==H)){var fx=W/RL.W,fy=H/RL.H;[1,2,3].forEach(function(m){RL.sets[m].forEach(function(v){v[0]*=fx;v[1]*=fy})})}
  RL.W=W;RL.H=H;rlSync();drawRuler()}
function rlTool(){$('rhd').classList.toggle('edit',tool==='ruler');placeHandles()}
function rlLabels(){var v=rlV();$('rnv').textContent=RL.n;$('rov').textContent=RL.op+'%';
  if(v.length){$('rhrv').textContent=Math.round(v[0][1]/H*100)+'%';if(RL.m===1)$('rx1v').textContent=Math.round(v[0][0]/W*100)+'%';if(v.length>1)$('rspv').textContent=Math.round(Math.abs(v[1][0]-v[0][0])/2/W*100)+'%';if(v.length>2)$('rv3v').textContent=Math.round(v[2][1]/H*100)+'%'}}
function rlSync(){if(!RL)return;var v=rlV();
  if(v.length){$('rhr').value=Math.round(v[0][1]/H*100);if(RL.m===1)$('rx1').value=Math.round(v[0][0]/W*100);if(v.length>1)$('rsp').value=Math.round(Math.abs(v[1][0]-v[0][0])/2/W*100);if(v.length>2)$('rv3').value=Math.round(v[2][1]/H*100)}
  rlLabels()}
function rlUI(){if(!RL)return;document.querySelectorAll('[data-rm]').forEach(function(b){b.classList.toggle('on',+b.dataset.rm===RL.m)});
  $('rsn').checked=!!RL.snap;$('rsh').checked=!!RL.show;$('rgd').checked=!!RL.grid;$('rn').value=RL.n;$('ro').value=RL.op;
  var m=RL.m,d=function(id,on){$(id).style.display=on?'':'none'};
  ['rsn','rsh'].forEach(function(id){$(id).parentNode.style.display=m?'flex':'none'});
  d('rrx',m===1);d('rrs2',m>=2);d('rrv',m===3);d('rrg',m===1);
  ['rn','ro','rhr'].forEach(function(id){$(id).parentNode.style.display=m?'':'none'});
  $('rtl').style.display=m?'':'none';$('rrs').style.display=m?'':'none';
  $('rlh').textContent=['定規を使いません。「1点」「2点」「3点」から選ぶと、ガイド線が出ます。',
    '1点透視: 1つの消失点に向かう線と、水平・垂直の線に沿って描けます。','2点透視: 2つの消失点に向かう線と、垂直の線に沿って描けます。','3点透視: 3つの消失点に向かう線に沿って描けます。'][m];
  rlSync();drawRuler()}
function rlSetMode(m){if(!RL)return;RL.m=m;rlSave();rlUI();if(m&&tool!=='ruler'){}}
function rlRays(c,vx,vy,n,col,al,rc){var inside=vx>=rc[0]&&vx<=rc[2]&&vy>=rc[1]&&vy<=rc[3],angs=[],k;
  if(inside){for(k=0;k<n;k++)angs.push(2*Math.PI*k/n)}
  else{var cx=(rc[0]+rc[2])/2,cy=(rc[1]+rc[3])/2,ac=Math.atan2(cy-vy,cx-vx),mn=0,mx=0;
    [[rc[0],rc[1]],[rc[2],rc[1]],[rc[2],rc[3]],[rc[0],rc[3]]].forEach(function(q){var d=Math.atan2(q[1]-vy,q[0]-vx)-ac;while(d>Math.PI)d-=2*Math.PI;while(d<-Math.PI)d+=2*Math.PI;if(d<mn)mn=d;if(d>mx)mx=d});
    for(k=0;k<n;k++)angs.push(ac+mn+(mx-mn)*(n>1?k/(n-1):.5))}
  var L=Math.hypot(rc[2]-rc[0],rc[3]-rc[1])+Math.hypot(vx-rc[0],vy-rc[1])+60;c.strokeStyle=col+al+')';c.lineWidth=1;c.beginPath();
  angs.forEach(function(a){c.moveTo(vx,vy);c.lineTo(vx+Math.cos(a)*L,vy+Math.sin(a)*L)});c.stroke()}
function drawRuler(){if(!RL)return;var cv=$('rg'),r=view.getBoundingClientRect(),dpr=window.devicePixelRatio||1,cw=Math.round(r.width*dpr),ch=Math.round(r.height*dpr);
  if(cv.width!==cw||cv.height!==ch){cv.width=cw;cv.height=ch;cv.style.width=r.width+'px';cv.style.height=r.height+'px'}
  var c=cv.getContext('2d');c.setTransform(dpr,0,0,dpr,0,0);c.clearRect(0,0,r.width,r.height);placeHandles();
  if(!RL.m||!RL.show)return;var v=rlV(),rc=[PX,PY,PX+W*Z,PY+H*Z],al=(RL.op/100).toFixed(2);
  c.save();c.beginPath();c.rect(rc[0],rc[1],rc[2]-rc[0],rc[3]-rc[1]);c.clip();
  for(var i=0;i<v.length;i++)rlRays(c,X2(v[i][0]),Y2(v[i][1]),RL.n,RLC[i],al,rc);
  var hy=Y2(v[0][1]);c.setLineDash([9,6]);c.lineWidth=1.5;c.strokeStyle='rgba(229,56,59,'+al+')';c.beginPath();c.moveTo(rc[0],hy);c.lineTo(rc[2],hy);c.stroke();c.setLineDash([]);
  if(RL.m===1&&RL.grid){var yh=v[0][1],base=Math.max(H*.55,H-yh);c.strokeStyle=RLC[0]+al+')';c.lineWidth=1;c.beginPath();
    for(var k=0;k<60;k++){var d=base/(1+k*.4),dn=base/(1+(k+1)*.4);if((d-dn)*Z<5)break;var y1=Y2(yh+d),y2=Y2(yh-d);c.moveTo(rc[0],y1);c.lineTo(rc[2],y1);c.moveTo(rc[0],y2);c.lineTo(rc[2],y2)}c.stroke()}
  c.restore()}
var RLH=[];
function placeHandles(){var host=$('rhd');if(!RLH.length){for(var i=0;i<3;i++){(function(i){var e=document.createElement('div');e.className='vph';e.textContent=i+1;e.style.borderColor=RLB[i];host.appendChild(e);RLH.push(e);
    e.addEventListener('pointerdown',function(ev){if(tool!=='ruler'||e.classList.contains('out'))return;ev.preventDefault();ev.stopPropagation();e.setPointerCapture(ev.pointerId);e._d=true});
    e.addEventListener('pointermove',function(ev){if(!e._d)return;var r=view.getBoundingClientRect(),x=X1(ev.clientX-r.left),y=Y1(ev.clientY-r.top),v=rlV();if(!v[i])return;
      v[i][0]=x;v[i][1]=y;if(i<2&&v.length>1){v[1-i][1]=y}rlSync();drawRuler()});
    var end=function(){if(e._d){e._d=false;rlSave()}};e.addEventListener('pointerup',end);e.addEventListener('pointercancel',end)})(i)}}
  if(!RL)return;var v=rlV(),cw=view.clientWidth,ch=view.clientHeight;
  RLH.forEach(function(e,i){if(!RL.m||!RL.show||i>=v.length){e.style.display='none';return}
    var sx=X2(v[i][0]),sy=Y2(v[i][1]),ins=sx>=14&&sx<=cw-14&&sy>=14&&sy<=ch-14,x=Math.min(Math.max(sx,18),cw-18),y=Math.min(Math.max(sy,18),ch-18);
    e.style.display='grid';e.style.left=x+'px';e.style.top=y+'px';e.classList.toggle('out',!ins);e.title=ins?'消失点'+(i+1)+'(「定規」ツールでドラッグ)':'消失点'+(i+1)+'は画面の外です。縮小すると見えます'})}
/* 定規に沿って描く: 描き始めの点から、近い向きのガイドの線に吸い付ける */
function rlDirs(px,py){var out=[],v=rlV();
  v.forEach(function(q){var dx=q[0]-px,dy=q[1]-py,l=Math.hypot(dx,dy);if(l>14)out.push([dx/l,dy/l])});
  if(RL.m===1){out.push([1,0]);out.push([0,1])}if(RL.m===2)out.push([0,1]);return out}
function snapP(p){var s=a,dx=p.x-s.x,dy=p.y-s.y,L=Math.hypot(dx,dy);
  if(!RL.lock){if(L<10/Z)return{x:s.x,y:s.y,p:p.p};var ds=rlDirs(s.x,s.y),best=null,ba=1e9;
    ds.forEach(function(d){var c=Math.abs(dx*d[0]+dy*d[1])/L,an=Math.acos(Math.min(1,c));if(an<ba){ba=an;best=d}});RL.lock=best}
  if(!RL.lock)return p;var d=RL.lock,t=dx*d[0]+dy*d[1];return{x:s.x+d[0]*t,y:s.y+d[1]*t,p:p.p}}
document.querySelectorAll('[data-rm]').forEach(function(b){b.onclick=function(){rlSetMode(+b.dataset.rm)}});
$('rsn').onchange=function(){RL.snap=this.checked?1:0;rlSave()};
$('rsh').onchange=function(){RL.show=this.checked?1:0;rlSave();drawRuler()};
$('rgd').onchange=function(){RL.grid=this.checked?1:0;rlSave();drawRuler()};
$('rn').oninput=function(){RL.n=+this.value;rlSave();rlLabels();drawRuler()};
$('ro').oninput=function(){RL.op=+this.value;rlSave();rlLabels();drawRuler()};
$('rhr').oninput=function(){var v=rlV(),y=this.value/100*H;if(v[0])v[0][1]=y;if(v[1])v[1][1]=y;rlSave();rlLabels();drawRuler()};
$('rx1').oninput=function(){var v=rlV();if(v[0])v[0][0]=this.value/100*W;rlSave();rlLabels();drawRuler()};
$('rsp').oninput=function(){var v=rlV();if(v.length<2)return;var cx=(v[0][0]+v[1][0])/2,hf=this.value/100*W;v[0][0]=cx-hf;v[1][0]=cx+hf;rlSave();rlLabels();drawRuler()};
$('rv3').oninput=function(){var v=rlV();if(v[2])v[2][1]=this.value/100*H;rlSave();rlLabels();drawRuler()};
$('rtl').onclick=function(){selTool('ruler')};
$('rrs').onclick=function(){if(!RL.m)return;RL.sets[RL.m]=rlDef(RL.m);rlSave();rlSync();drawRuler()};
(function(){var o=null;try{o=JSON.parse(localStorage.getItem('ps_ruler')||'null')}catch(e){}rlLoad(o)})();
/* ---- 反転(表示だけ / キャンバス全体) ---- */
var VMX=false,VMY=false;
function X2(x){return VMX?PX+(W-x)*Z:PX+x*Z}
function Y2(y){return VMY?PY+(H-y)*Z:PY+y*Z}
function X1(sx){return VMX?W-(sx-PX)/Z:(sx-PX)/Z}
function Y1(sy){return VMY?H-(sy-PY)/Z:(sy-PY)/Z}
function vmSet(){applyT();$('vmx').classList.toggle('on',VMX);$('vmy').classList.toggle('on',VMY);moveUI();if(RL)drawRuler()}
$('vmx').onclick=function(){VMX=!VMX;vmSet();toast(VMX?'鏡(左右)にしました。もう一度押すと元に戻ります':'表示を元に戻しました')};
$('vmy').onclick=function(){VMY=!VMY;vmSet();toast(VMY?'鏡(上下)にしました。もう一度押すと元に戻ります':'表示を元に戻しました')};
document.addEventListener('keydown',function(e){if(e.ctrlKey||e.metaKey||e.altKey||document.querySelector('dialog[open]'))return;var t=e.target.tagName;if(t==='INPUT'&&e.target.type==='text'||t==='TEXTAREA'||t==='SELECT')return;
  if(e.key==='f'||e.key==='F'){VMX=!VMX;vmSet()}});
function maskFlip(h){if(!hasSel)return;var t=document.createElement('canvas');t.width=W;t.height=H;t.getContext('2d').drawImage(sel,0,0);
  sx.save();sx.clearRect(0,0,W,H);if(h==='h'){sx.translate(W,0);sx.scale(-1,1)}else{sx.translate(0,H);sx.scale(1,-1)}sx.drawImage(t,0,0);sx.restore();drawSel()}
function rlFlip(h){if(!RL)return;[1,2,3].forEach(function(m){RL.sets[m].forEach(function(v){if(h==='h')v[0]=W-v[0];else v[1]=H-v[1]})});rlSave();rlSync();drawRuler()}
function canvasFlip(h){flt=null;polyCancel();
  undoS.push({all:layers.map(function(l){return{l:l,d:l.x.getImageData(0,0,W,H)}}),rf:h});if(undoS.length>25)undoS.shift();redoS=[];mdirty();
  layers.forEach(function(l){var t=document.createElement('canvas');t.width=W;t.height=H;t.getContext('2d').drawImage(l.c,0,0);var c=l.x;c.save();c.clearRect(0,0,W,H);
    if(h==='h'){c.translate(W,0);c.scale(-1,1)}else{c.translate(0,H);c.scale(1,-1)}c.drawImage(t,0,0);c.restore()});
  maskFlip(h);rlFlip(h);dirty();moveUI();toast('キャンバス全体を'+(h==='h'?'左右':'上下')+'反転しました')}
$('cfx').onclick=function(){canvasFlip('h')};$('cfy').onclick=function(){canvasFlip('v')};
window.addEventListener('resize',fitView);
var saved=null;try{var t=$('proj').textContent.trim();if(t)saved=JSON.parse(t)}catch(err){}
if(saved&&saved.b){for(var kk in saved.b)CB[kk]=saved.b[kk];saveCB()}
if(saved&&saved.f){saved.f.forEach(function(c){if(FAV.indexOf(c)<0)FAV.push(c)});FAV=FAV.slice(0,16);saveFav();renderLists()}
if(saved&&saved.m&&saved.m.rot&&saved.m.rot.length===MQ.N){MS=saved.m;mSave()}
if(saved&&saved.rl)rlLoad(saved.rl);
if(saved&&saved.u&&saved.u.length){saved.u.forEach(function(x){UPO.push(x)});UPO=UPO.slice(0,24);mSaveU();renderUP()}
renderCB();
if(saved)init(saved.w,saved.h,saved.l);else init(1200,800);
gDirty=false;gHDirty=false;gAutoResume(!!saved);gStatus();
})();
</script>
</body>
</html>
