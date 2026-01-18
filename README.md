<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Голосование</title>
  <style>
    * { margin: 0; padding: 0; box-sizing: border-box; }
    body { font-family: sans-serif; background: #f5f5f5; }
    .c { max-width: 600px; margin: 20px auto; padding: 20px; background: white; border-radius: 12px; box-shadow: 0 4px 20px rgba(0,0,0,0.1); }
    h1 { text-align: center; margin: 0 0 20px; color: #2c3e50; font-size: 1.8em; }
    .opts { margin: 20px 0; }
    .opt { display: flex; align-items: center; padding: 15px; border: 1px solid #e0e0e0; border-radius: 8px; margin-bottom: 10px; }
    .opt:hover { background: #f8f9fa; }
    .opt input { margin-right: 15px; width: 20px; height: 20px; }
    button { display: block; width: 100%; padding: 15px; background: #007bff; color: white; border: none; border-radius: 8px; cursor: pointer; font-size: 1.1em; margin: 20px 0; }
    button:hover { background: #0056b3; }
    .res { margin-top: 20px; padding: 20px; background: #f8f9fa; border-radius: 8px; border: 1px solid #e9ecef; }
    .res h3 { margin-bottom: 15px; color: #495057; font-size: 1.2em; }
    .item { padding: 12px 15px; background: #e9ecef; border-radius: 6px; margin-bottom: 10px; display: flex; justify-content: space-between; }
    
    @media (max-width:768px) {
      .c { margin: 10px; padding: 15px; }
      h1 { font-size: 1.5em; }
      .opt { padding: 12px; }
      button { padding: 12px; font-size: 1em; }
      .res { padding: 15px; }
      .res h3 { font-size: 1.1em; }
      .item { padding: 10px; }
    }

    @media (max-width:480px) {
      .c { margin: 5px; padding: 10px; }
      h1 { font-size: 1.3em; margin-bottom: 15px; }
      .opt { flex-direction: column; align-items: flex-start; }
      .opt input { margin: 0 0 8px 0; }
      button { font-size: 0.95em; padding: 10px; }
      .res h3 { font-size: 1em; }
    }
  </style>
</head>
<body>
  <div class="c">
    <h1>Опрос: Какой ваш любимый цвет?</h1>
    <div class="opts">
      <div class="opt"><input type="radio" name="c" value="r" id="r"><label for="r">Красный</label></div>
      <div class="opt"><input type="radio" name="c" value="b" id="b"><label for="b">Синий</label></div>
      <div class="opt"><input type="radio" name="c" value="g" id="g"><label for="g">Зелёный</label></div>
      <div class="opt"><input type="radio" name="c" value="y" id="y"><label for="y">Жёлтый</label></div>
    </div>
    <button onclick="v()">Проголосовать</button>
    <div class="res">
      <h3>Результаты голосования:</h3>
      <div id="l"></div>
    </div>
  </div>

  <script>
    const vts = JSON.parse(localStorage.getItem('v')) || {r:0,b:0,g:0,y:0};
    let cr = {...vts};

    function d() {
      const l = document.getElementById('l');
      if (!Object.keys(vts).some(k => vts[k] !== cr[k])) return;
      cr = {...vts};
      l.innerHTML = '';
      Object.entries(vts).forEach(([k,v]) => {
        const n = {'r':'Красный','b':'Синий','g':'Зелёный','y':'Жёлтый'}[k];
        l.innerHTML += `<div class="item"><span>${n}</span><span><strong>${v}</strong> голосов</span></div>`;
      });
    }

    function v() {
      const s = document.querySelector('input[name="c"]:checked');
      if (!s) { alert('Выберите вариант!'); return; }
      vts[s.value]++;
      localStorage.setItem('v', JSON.stringify(vts));
      d();
      alert('Голос учтен!');
    }

    setInterval(() => {
      const s = JSON.parse(localStorage.getItem('v'));
      if (s) Object.assign(vts, s);
      d();
    }, 1000);

    window.onload = d;
  </script>
</body>
</html>
