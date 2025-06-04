
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Live Calculator</title>
<style>
  body {
    margin:0; background:#222; display:flex; flex-direction:column; align-items:center; min-height:100vh;
    font-family:sans-serif; color:#eee; padding: 40px 0;
  }
  h1 {
    font-weight: 700; font-size: 2.8rem; margin-bottom: 25px; color: #ff6978; user-select:none;
    text-shadow: 0 0 8px #ff6978cc;
  }
  .calc {
    background:#2c2f4a; border-radius:15px; padding:15px; width:320px; box-shadow:0 8px 20px #1c1f33cc;
  }
  .screen {
    background:#1b1d2a; height:50px; border-radius:10px; margin-bottom:15px; text-align:right; font-size:2rem; line-height:50px; padding:0 15px; user-select:none; overflow-x:auto;
  }
  .keys {
    display:grid; grid-template-columns:repeat(4,1fr); gap:12px;
  }
  button {
    background:#3b3f6d; border:none; border-radius:10px; font-size:1.2rem; color:#eee;
    box-shadow:0 3px 6px #181a27aa;
    transition: background 0.3s, transform 0.1s;
    cursor:pointer;
  }
  button:hover {background:#505697;}
  button:active {
    background:#7379c3;
    transform: scale(0.95);
  }
  button.operator {
    background:#ff6978; box-shadow:0 3px 10px #ff4c5faa;
  }
  button.operator:hover {background:#e64e5b;}
  button.operator:active {background:#cc3d4b;}
  button.zero {grid-column:span 2;}
</style>
</head>
<body>
  <h1>Live Calculator</h1>
  <div class="calc" role="application" aria-label="Calculator">
    <div class="screen" id="disp">0</div>
    <div class="keys">
      <button class="operator" data-action="clear">AC</button>
      <button class="operator" data-action="sign">±</button>
      <button class="operator" data-action="percent">%</button>
      <button class="operator" data-op="/">÷</button>

      <button data-num="7">7</button>
      <button data-num="8">8</button>
      <button data-num="9">9</button>
      <button class="operator" data-op="*">×</button>

      <button data-num="4">4</button>
      <button data-num="5">5</button>
      <button data-num="6">6</button>
      <button class="operator" data-op="-">−</button>

      <button data-num="1">1</button>
      <button data-num="2">2</button>
      <button data-num="3">3</button>
      <button class="operator" data-op="+">+</button>

      <button class="zero" data-num="0">0</button>
      <button data-num=".">.</button>
      <button class="operator" data-action="equals">=</button>
    </div>
  </div>
<script>
(() => {
  const disp=document.getElementById('disp');
  let curr='0', prev=null, op=null, reset=false;

  function update(v){disp.textContent=v;}
  function clear(){curr='0'; prev=null; op=null; reset=false; update(curr);}
  function input(num){
    if(reset){curr=num==='.'?'0.':num; reset=false;}
    else if(num==='.' && curr.includes('.')) return;
    else curr=(curr==='0' && num!=='.')?num:curr+num;
    update(curr);
  }
  function toggleSign(){
    if(curr==='0') return;
    curr=curr.startsWith('-')?curr.slice(1):'-'+curr;
    update(curr);
  }
  function percent(){
    curr=(parseFloat(curr)/100).toString();
    update(curr);
  }
  function calc(){
    if(op===null || prev===null) return;
    let res=0, a=parseFloat(prev), b=parseFloat(curr);
    switch(op){
      case '+': res=a+b; break;
      case '-': res=a-b; break;
      case '*': res=a*b; break;
      case '/': res=b!==0?a/b:'Error'; break;
    }
    curr=res.toString();
    op=null; prev=null; reset=true;
    update(curr);
  }
  function setOp(o){
    if(op && !reset) calc();
    else reset=false;
    prev=curr; op=o; reset=true;
  }

  document.querySelectorAll('button').forEach(btn=>{
    btn.onclick=()=>{
      const n=btn.dataset.num, a=btn.dataset.action, o=btn.dataset.op;
      if(n!==undefined) input(n);
      else if(a){
        if(a==='clear') clear();
        else if(a==='sign') toggleSign();
        else if(a==='percent') percent();
        else if(a==='equals') calc();
      }
      else if(o) setOp(o);
    };
  });
  clear();
})();
</script>
</body>
</html>
