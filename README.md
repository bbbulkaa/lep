<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Моделирование электромагнитного поля ЛЭП-110 кВ</title>
  <script src="https://cdn.plot.ly/plotly-2.35.2.min.js"></script>
  <style>
    :root {
      --bg: #0f172a;
      --panel: #111827;
      --card: #1f2937;
      --text: #e5e7eb;
      --muted: #9ca3af;
      --accent: #3b82f6;
      --accent2: #ef4444;
      --line: #374151;
      --ok: #10b981;
    }

    * { box-sizing: border-box; }

    body {
      margin: 0;
      font-family: Arial, sans-serif;
      background: linear-gradient(180deg, #0b1020 0%, #111827 100%);
      color: var(--text);
    }

    .container {
      width: min(1400px, 96%);
      margin: 24px auto 40px;
    }

    .hero {
      background: rgba(17, 24, 39, 0.92);
      border: 1px solid var(--line);
      border-radius: 18px;
      padding: 24px;
      box-shadow: 0 10px 30px rgba(0,0,0,.25);
      margin-bottom: 20px;
    }

    .hero h1 {
      margin: 0 0 8px;
      font-size: 30px;
      line-height: 1.2;
    }

    .hero p {
      margin: 4px 0;
      color: var(--muted);
    }

    .grid {
      display: grid;
      grid-template-columns: 360px 1fr;
      gap: 20px;
      align-items: start;
    }

    .panel {
      background: rgba(17, 24, 39, 0.92);
      border: 1px solid var(--line);
      border-radius: 18px;
      padding: 18px;
      box-shadow: 0 10px 30px rgba(0,0,0,.2);
      position: sticky;
      top: 16px;
    }

    .panel h2, .content h2 {
      margin-top: 0;
      font-size: 22px;
    }

    .section-title {
      margin: 18px 0 10px;
      font-size: 14px;
      text-transform: uppercase;
      letter-spacing: .08em;
      color: #93c5fd;
    }

    .form-grid {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 12px;
    }

    .field {
      display: flex;
      flex-direction: column;
      gap: 6px;
    }

    .field.full { grid-column: 1 / -1; }

    label {
      font-size: 13px;
      color: var(--muted);
    }

    input, select {
      width: 100%;
      padding: 10px 12px;
      background: #0b1220;
      color: var(--text);
      border: 1px solid var(--line);
      border-radius: 10px;
      outline: none;
    }

    input:focus, select:focus {
      border-color: var(--accent);
      box-shadow: 0 0 0 3px rgba(59,130,246,.15);
    }

    .btns {
      display: flex;
      gap: 10px;
      margin-top: 16px;
      flex-wrap: wrap;
    }

    button {
      border: none;
      border-radius: 12px;
      padding: 12px 16px;
      font-weight: 700;
      cursor: pointer;
      transition: .2s ease;
    }

    .primary {
      background: var(--accent);
      color: white;
    }

    .primary:hover { filter: brightness(1.08); }

    .secondary {
      background: #374151;
      color: white;
    }

    .secondary:hover { filter: brightness(1.08); }

    .content {
      display: flex;
      flex-direction: column;
      gap: 20px;
    }

    .card {
      background: rgba(17, 24, 39, 0.92);
      border: 1px solid var(--line);
      border-radius: 18px;
      padding: 16px;
      box-shadow: 0 10px 30px rgba(0,0,0,.2);
    }

    .results {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 14px;
    }

    .metric {
      background: #0b1220;
      border: 1px solid var(--line);
      border-radius: 14px;
      padding: 14px;
    }

    .metric .k {
      color: var(--muted);
      font-size: 13px;
      margin-bottom: 6px;
    }

    .metric .v {
      font-size: 22px;
      font-weight: 700;
    }

    .log {
      background: #0b1220;
      border: 1px solid var(--line);
      border-radius: 14px;
      padding: 14px;
      white-space: pre-wrap;
      line-height: 1.5;
      font-family: Consolas, monospace;
      overflow-x: auto;
    }

    .footer-note {
      color: var(--muted);
      font-size: 13px;
      margin-top: 8px;
    }

    @media (max-width: 1100px) {
      .grid {
        grid-template-columns: 1fr;
      }
      .panel {
        position: static;
      }
    }

    @media (max-width: 700px) {
      .form-grid,
      .results {
        grid-template-columns: 1fr;
      }
      .hero h1 {
        font-size: 24px;
      }
    }
  </style>
</head>
<body>
  <div class="container">
    <div class="hero">
      <h1>Моделирование электромагнитного поля ЛЭП</h1>
      <p><strong>Автор:</strong> Губайдуллин Б.А.</p>
      <p><strong>Техникум:</strong> Альметьевский политехнический техникум, 2026</p>
      <p>Интерактивная модель электрического и магнитного полей воздушной линии электропередачи.</p>
    </div>

    <div class="grid">
      <aside class="panel">
        <h2>Параметры модели</h2>

        <div class="section-title">Электрофизические параметры</div>
        <div class="form-grid">
          <div class="field">
            <label for="uNom">Номинальное напряжение, кВ</label>
            <input id="uNom" type="number" step="1" value="110">
          </div>
          <div class="field">
            <label for="iNom">Ток нагрузки, А</label>
            <input id="iNom" type="number" step="1" value="300">
          </div>
          <div class="field">
            <label for="freq">Частота, Гц</label>
            <input id="freq" type="number" step="1" value="50">
          </div>
          <div class="field">
            <label for="obsHeight">Высота наблюдения, м</label>
            <input id="obsHeight" type="number" step="0.1" value="1.8">
          </div>
        </div>

        <div class="section-title">Геометрия ЛЭП</div>
        <div class="form-grid">
          <div class="field">
            <label for="h">Высота подвеса, м</label>
            <input id="h" type="number" step="0.1" value="12">
          </div>
          <div class="field">
            <label for="d">Полурасстояние между крайними и средней фазой, м</label>
            <input id="d" type="number" step="0.1" value="4">
          </div>
          <div class="field">
            <label for="rWire">Радиус провода, м</label>
            <input id="rWire" type="number" step="0.0001" value="0.0075">
          </div>
          <div class="field">
            <label for="layout">Расположение фаз</label>
            <select id="layout">
              <option value="horizontal" selected>Горизонтальное</option>
              <option value="vertical">Вертикальное</option>
              <option value="triangle">Треугольное</option>
            </select>
          </div>
        </div>

        <div class="section-title">Расчётная область</div>
        <div class="form-grid">
          <div class="field">
            <label for="xMin">X min, м</label>
            <input id="xMin" type="number" step="1" value="-30">
          </div>
          <div class="field">
            <label for="xMax">X max, м</label>
            <input id="xMax" type="number" step="1" value="30">
          </div>
          <div class="field">
            <label for="yMin">Y min, м</label>
            <input id="yMin" type="number" step="1" value="0">
          </div>
          <div class="field">
            <label for="yMax">Y max, м</label>
            <input id="yMax" type="number" step="1" value="20">
          </div>
          <div class="field">
            <label for="nx">Узлы по X</label>
            <input id="nx" type="number" step="1" value="160">
          </div>
          <div class="field">
            <label for="ny">Узлы по Y</label>
            <input id="ny" type="number" step="1" value="90">
          </div>
        </div>

        <div class="btns">
          <button class="primary" onclick="runModel()">Пересчитать</button>
          <button class="secondary" onclick="resetDefaults()">Сбросить</button>
        </div>

        <div class="footer-note">
          Для GitHub Pages: создай репозиторий, добавь этот код в файл <strong>index.html</strong> и включи Pages.
        </div>
      </aside>

      <main class="content">
        <div class="card">
          <h2>Ключевые результаты</h2>
          <div class="results" id="metrics"></div>
        </div>

        <div class="card">
          <h2>Электрическое поле</h2>
          <div id="electricMap" style="height: 520px;"></div>
        </div>

        <div class="card">
          <h2>Магнитное поле</h2>
          <div id="magneticMap" style="height: 520px;"></div>
        </div>

        <div class="card">
          <h2>Профиль распределения на высоте наблюдения</h2>
          <div id="profiles" style="height: 560px;"></div>
        </div>

        <div class="card">
          <h2>Текстовый отчёт</h2>
          <div class="log" id="report"></div>
        </div>
      </main>
    </div>
  </div>

  <script>
    const EPS0 = 8.854e-12;
    const MU0 = 4 * Math.PI * 1e-7;
    const SQRT2 = Math.sqrt(2);

    const defaults = {
      uNom: 110,
      iNom: 300,
      freq: 50,
      obsHeight: 1.8,
      h: 12,
      d: 4,
      rWire: 0.0075,
      layout: "horizontal",
      xMin: -30,
      xMax: 30,
      yMin: 0,
      yMax: 20,
      nx: 160,
      ny: 90
    };

    function getVal(id) {
      const el = document.getElementById(id);
      return el.type === "select-one" ? el.value : parseFloat(el.value);
    }

    function linspace(start, end, n) {
      if (n <= 1) return [start];
      const arr = new Array(n);
      const step = (end - start) / (n - 1);
      for (let i = 0; i < n; i++) arr[i] = start + i * step;
      return arr;
    }

    function complex(re, im) { return { re, im }; }
    function cAdd(a, b) { return { re: a.re + b.re, im: a.im + b.im }; }
    function cMulReal(a, k) { return { re: a.re * k, im: a.im * k }; }
    function cAbs(a) { return Math.hypot(a.re, a.im); }
    function phasor(ampl, angleDeg) {
      const ang = angleDeg * Math.PI / 180;
      return { re: ampl * Math.cos(ang), im: ampl * Math.sin(ang) };
    }

    function clampRadius(r, minR) {
      return Math.max(r, minR);
    }

    function buildWires(params) {
      const { h, d, Uamp, Inom, layout } = params;

      if (layout === "vertical") {
        return [
          { name: "A", x: 0,   y: h + d, V: phasor(Uamp,   0), I: phasor(Inom,   0) },
          { name: "B", x: 0,   y: h,     V: phasor(Uamp, -120), I: phasor(Inom, -120) },
          { name: "C", x: 0,   y: h - d, V: phasor(Uamp, 120), I: phasor(Inom, 120) }
        ];
      }

      if (layout === "triangle") {
        return [
          { name: "A", x: -d,  y: h,     V: phasor(Uamp,   0), I: phasor(Inom,   0) },
          { name: "B", x:  d,  y: h,     V: phasor(Uamp, -120), I: phasor(Inom, -120) },
          { name: "C", x:  0,  y: h + d, V: phasor(Uamp, 120), I: phasor(Inom, 120) }
        ];
      }

      return [
        { name: "A", x: -d, y: h, V: phasor(Uamp,   0), I: phasor(Inom,   0) },
        { name: "B", x:  0, y: h, V: phasor(Uamp, -120), I: phasor(Inom, -120) },
        { name: "C", x:  d, y: h, V: phasor(Uamp, 120), I: phasor(Inom, 120) }
      ];
    }

    function calcCapacitanceApprox(y0, rWire) {
      return 2 * Math.PI * EPS0 / Math.log((2 * y0) / rWire);
    }

    function calculateFieldsAtPoint(x, y, wires, rWire) {
      let Ex = complex(0, 0);
      let Ey = complex(0, 0);
      let Bx = complex(0, 0);
      let By = complex(0, 0);

      for (const wire of wires) {
        const x0 = wire.x;
        const y0 = wire.y;

        const dx1 = x - x0;
        const dy1 = y - y0;
        const dx2 = x - x0;
        const dy2 = y + y0;

        let r1 = clampRadius(Math.hypot(dx1, dy1), rWire);
        let r2 = clampRadius(Math.hypot(dx2, dy2), rWire);

        const Capprox = calcCapacitanceApprox(y0, rWire);
        const tau = cMulReal(wire.V, Capprox);

        const kE = 1 / (2 * Math.PI * EPS0);

        const exFactor = (dx1 / (r1 * r1)) - (dx2 / (r2 * r2));
        const eyFactor = (dy1 / (r1 * r1)) - (dy2 / (r2 * r2));

        Ex = cAdd(Ex, cMulReal(tau, kE * exFactor));
        Ey = cAdd(Ey, cMulReal(tau, kE * eyFactor));

        let r = clampRadius(Math.hypot(dx1, dy1), rWire);
        const kB = MU0 / (2 * Math.PI * r * r);

        Bx = cAdd(Bx, cMulReal(wire.I, -kB * dy1));
        By = cAdd(By, cMulReal(wire.I,  kB * dx1));
      }

      const Eeff = Math.hypot(cAbs(Ex), cAbs(Ey)) / SQRT2 / 1000; // кВ/м
      const Beff = Math.hypot(cAbs(Bx), cAbs(By)) / SQRT2 * 1e6;   // мкТл

      return { Eeff, Beff };
    }

    function calculateGrid(params, wires) {
      const xs = linspace(params.xMin, params.xMax, params.nx);
      const ys = linspace(params.yMin, params.yMax, params.ny);

      const E = [];
      const B = [];

      for (let j = 0; j < ys.length; j++) {
        const eRow = [];
        const bRow = [];
        for (let i = 0; i < xs.length; i++) {
          const res = calculateFieldsAtPoint(xs[i], ys[j], wires, params.rWire);
          eRow.push(res.Eeff);
          bRow.push(res.Beff);
        }
        E.push(eRow);
        B.push(bRow);
      }

      return { xs, ys, E, B };
    }

    function calculateProfile(params, wires) {
      const xObs = linspace(-35, 35, 500);
      const Eobs = [];
      const Bobs = [];

      for (const x of xObs) {
        const res = calculateFieldsAtPoint(x, params.obsHeight, wires, params.rWire);
        Eobs.push(res.Eeff);
        Bobs.push(res.Beff);
      }

      return { xObs, Eobs, Bobs };
    }

    function findMax(arr) {
      let idx = 0;
      for (let i = 1; i < arr.length; i++) {
        if (arr[i] > arr[idx]) idx = i;
      }
      return idx;
    }

    function firstBelow(x, y, threshold) {
      for (let i = 0; i < x.length; i++) {
        if (x[i] >= 0 && y[i] <= threshold) {
          return { x: x[i], value: y[i] };
        }
      }
      return null;
    }

    function fmt(n, d = 2) {
      if (!isFinite(n)) return "—";
      return Number(n).toFixed(d);
    }

    function renderMetrics(data) {
      const metrics = document.getElementById("metrics");
      metrics.innerHTML = data.map(item => `
        <div class="metric">
          <div class="k">${item.label}</div>
          <div class="v">${item.value}</div>
        </div>
      `).join("");
    }

    function renderReport(params, wires, profile) {
      const idxMaxE = findMax(profile.Eobs);
      const idxMaxB = findMax(profile.Bobs);

      const eBorder = firstBelow(profile.xObs, profile.Eobs, 0.5);
      const bBorder = firstBelow(profile.xObs, profile.Bobs, 0.5);

      const lastExtremeX = Math.max(...wires.map(w => w.x));

      const lines = [];
      lines.push("============================================================");
      lines.push("РЕЗУЛЬТАТЫ МОДЕЛИРОВАНИЯ ЭМП ЛЭП");
      lines.push("============================================================");
      lines.push(`Номинальное напряжение: ${fmt(params.uNom, 1)} кВ`);
      lines.push(`Ток нагрузки: ${fmt(params.iNom, 1)} А`);
      lines.push(`Частота: ${fmt(params.freq, 1)} Гц`);
      lines.push(`Высота подвеса проводов: ${fmt(params.h, 2)} м`);
      lines.push(`Полурасстояние между фазами: ${fmt(params.d, 2)} м`);
      lines.push(`Радиус провода: ${fmt(params.rWire, 4)} м`);
      lines.push(`Высота наблюдения: ${fmt(params.obsHeight, 2)} м`);
      lines.push(`Схема расположения: ${params.layout}`);
      lines.push("");
      lines.push(`Максимальная напряжённость E: ${fmt(profile.Eobs[idxMaxE], 2)} кВ/м на x = ${fmt(profile.xObs[idxMaxE], 2)} м`);
      lines.push(`Максимальная индукция B: ${fmt(profile.Bobs[idxMaxB], 2)} мкТл на x = ${fmt(profile.xObs[idxMaxB], 2)} м`);
      lines.push("");

      if (eBorder) {
        lines.push(`Граница по E <= 0.5 кВ/м: ${fmt(eBorder.x, 2)} м от центра (${fmt(eBorder.x - lastExtremeX, 2)} м от крайней фазы)`);
      } else {
        lines.push("Граница по E <= 0.5 кВ/м в расчётном диапазоне не найдена");
      }

      if (bBorder) {
        lines.push(`Граница по B <= 0.5 мкТл: ${fmt(bBorder.x, 2)} м от центра (${fmt(bBorder.x - lastExtremeX, 2)} м от крайней фазы)`);
      } else {
        lines.push("Граница по B <= 0.5 мкТл в расчётном диапазоне не найдена");
      }

      lines.push("");
      lines.push("ПРИМЕЧАНИЕ:");
      lines.push("Модель использует инженерные приближения:");
      lines.push("- электрическое поле: метод зеркальных отражений;");
      lines.push("- магнитное поле: закон Био–Савара;");
      lines.push("- ёмкость провода над землёй: приближённая оценка.");
      lines.push("Для учебного проекта и визуального анализа этого достаточно.");

      document.getElementById("report").textContent = lines.join("\n");
    }

    function renderElectricMap(grid, wires, params) {
      const traces = [
        {
          z: grid.E,
          x: grid.xs,
          y: grid.ys,
          type: "heatmap",
          colorscale: "Reds",
          colorbar: { title: "E, кВ/м" }
        },
        {
          x: wires.map(w => w.x),
          y: wires.map(w => w.y),
          mode: "markers+text",
          type: "scatter",
          text: wires.map(w => w.name),
          textposition: "top center",
          marker: { size: 11, color: "black" },
          name: "Провода"
        }
      ];

      const layout = {
        paper_bgcolor: "rgba(0,0,0,0)",
        plot_bgcolor: "#0b1220",
        font: { color: "#e5e7eb" },
        margin: { l: 60, r: 30, t: 40, b: 55 },
        xaxis: {
          title: "Расстояние от центра ЛЭП, м",
          gridcolor: "#334155",
          zerolinecolor: "#475569",
          range: [params.xMin, params.xMax]
        },
        yaxis: {
          title: "Высота, м",
          gridcolor: "#334155",
          zerolinecolor: "#475569",
          range: [params.yMin, params.yMax]
        },
        title: "Электрическое поле",
        showlegend: false
      };

      Plotly.newPlot("electricMap", traces, layout, { responsive: true, displaylogo: false });
    }

    function renderMagneticMap(grid, wires, params) {
      const traces = [
        {
          z: grid.B,
          x: grid.xs,
          y: grid.ys,
          type: "heatmap",
          colorscale: "Blues",
          colorbar: { title: "B, мкТл" }
        },
        {
          x: wires.map(w => w.x),
          y: wires.map(w => w.y),
          mode: "markers+text",
          type: "scatter",
          text: wires.map(w => w.name),
          textposition: "top center",
          marker: { size: 11, color: "black" },
          name: "Провода"
        }
      ];

      const layout = {
        paper_bgcolor: "rgba(0,0,0,0)",
        plot_bgcolor: "#0b1220",
        font: { color: "#e5e7eb" },
        margin: { l: 60, r: 30, t: 40, b: 55 },
        xaxis: {
          title: "Расстояние от центра ЛЭП, м",
          gridcolor: "#334155",
          zerolinecolor: "#475569",
          range: [params.xMin, params.xMax]
        },
        yaxis: {
          title: "Высота, м",
          gridcolor: "#334155",
          zerolinecolor: "#475569",
          range: [params.yMin, params.yMax]
        },
        title: `Магнитное поле (I = ${fmt(params.iNom, 0)} А)`,
        showlegend: false
      };

      Plotly.newPlot("magneticMap", traces, layout, { responsive: true, displaylogo: false });
    }

    function renderProfiles(profile, params) {
      const eLimitPopulation = new Array(profile.xObs.length).fill(0.5);
      const eLimitStaff = new Array(profile.xObs.length).fill(5.0);
      const bLimitPopulation = new Array(profile.xObs.length).fill(0.5);
      const bLimitStaff = new Array(profile.xObs.length).fill(100);

      const traces = [
        {
          x: profile.xObs,
          y: profile.Eobs,
          type: "scatter",
          mode: "lines",
          name: "E, кВ/м",
          line: { color: "#ef4444", width: 3 },
          xaxis: "x",
          yaxis: "y"
        },
        {
          x: profile.xObs,
          y: eLimitPopulation,
          type: "scatter",
          mode: "lines",
          name: "Норма населения E = 0.5 кВ/м",
          line: { color: "#f59e0b", width: 2, dash: "dash" },
          xaxis: "x",
          yaxis: "y"
        },
        {
          x: profile.xObs,
          y: eLimitStaff,
          type: "scatter",
          mode: "lines",
          name: "Норма персонала E = 5 кВ/м",
          line: { color: "#dc2626", width: 2, dash: "dot" },
          xaxis: "x",
          yaxis: "y"
        },
        {
          x: profile.xObs,
          y: profile.Bobs,
          type: "scatter",
          mode: "lines",
          name: "B, мкТл",
          line: { color: "#3b82f6", width: 3 },
          xaxis: "x2",
          yaxis: "y2"
        },
        {
          x: profile.xObs,
          y: bLimitPopulation,
          type: "scatter",
          mode: "lines",
          name: "Норма населения B = 0.5 мкТл",
          line: { color: "#f59e0b", width: 2, dash: "dash" },
          xaxis: "x2",
          yaxis: "y2"
        },
        {
          x: profile.xObs,
          y: bLimitStaff,
          type: "scatter",
          mode: "lines",
          name: "Норма персонала B = 100 мкТл",
          line: { color: "#dc2626", width: 2, dash: "dot" },
          xaxis: "x2",
          yaxis: "y2"
        }
      ];

      const layout = {
        paper_bgcolor: "rgba(0,0,0,0)",
        plot_bgcolor: "#0b1220",
        font: { color: "#e5e7eb" },
        grid: { rows: 1, columns: 2, pattern: "independent" },
        margin: { l: 55, r: 25, t: 40, b: 55 },
        xaxis: {
          title: "Расстояние от центра ЛЭП, м",
          gridcolor: "#334155"
        },
        yaxis: {
          title: "Напряжённость E, кВ/м",
          gridcolor: "#334155",
          rangemode: "tozero"
        },
        xaxis2: {
          title: "Расстояние от центра ЛЭП, м",
          gridcolor: "#334155"
        },
        yaxis2: {
          title: "Магнитная индукция B, мкТл",
          gridcolor: "#334155",
          rangemode: "tozero"
        },
        title: `Профили на высоте ${fmt(params.obsHeight, 2)} м`,
        legend: {
          orientation: "h",
          y: -0.18
        }
      };

      Plotly.newPlot("profiles", traces, layout, { responsive: true, displaylogo: false });
    }

    function validateParams(params) {
      const checks = [
        [params.uNom > 0, "Номинальное напряжение должно быть > 0"],
        [params.iNom > 0, "Ток должен быть > 0"],
        [params.freq > 0, "Частота должна быть > 0"],
        [params.h > 0, "Высота подвеса должна быть > 0"],
        [params.d > 0, "Расстояние между фазами должно быть > 0"],
        [params.rWire > 0, "Радиус провода должен быть > 0"],
        [params.xMax > params.xMin, "X max должен быть больше X min"],
        [params.yMax > params.yMin, "Y max должен быть больше Y min"],
        [params.nx >= 40, "Узлов по X должно быть не меньше 40"],
        [params.ny >= 30, "Узлов по Y должно быть не меньше 30"],
        [params.obsHeight >= params.yMin && params.obsHeight <= params.yMax, "Высота наблюдения должна быть в пределах расчётной области"]
      ];

      for (const [ok, msg] of checks) {
        if (!ok) throw new Error(msg);
      }
    }

    function collectParams() {
      const uNom = getVal("uNom");
      const iNom = getVal("iNom");
      const freq = getVal("freq");
      const obsHeight = getVal("obsHeight");
      const h = getVal("h");
      const d = getVal("d");
      const rWire = getVal("rWire");
      const layout = document.getElementById("layout").value;
      const xMin = getVal("xMin");
      const xMax = getVal("xMax");
      const yMin = getVal("yMin");
      const yMax = getVal("yMax");
      const nx = Math.round(getVal("nx"));
      const ny = Math.round(getVal("ny"));

      const U_nom = uNom * 1000;
      const U_ph = U_nom / Math.sqrt(3);
      const U_amp = U_ph * Math.sqrt(2);

      return {
        uNom, iNom, freq, obsHeight,
        h, d, rWire, layout,
        xMin, xMax, yMin, yMax, nx, ny,
        U_nom, U_ph, U_amp,
        Inom: iNom
      };
    }

    function runModel() {
      try {
        const params = collectParams();
        validateParams(params);

        const wires = buildWires(params);
        const grid = calculateGrid(params, wires);
        const profile = calculateProfile(params, wires);

        const idxMaxE = findMax(profile.Eobs);
        const idxMaxB = findMax(profile.Bobs);
        const eBorder = firstBelow(profile.xObs, profile.Eobs, 0.5);
        const bBorder = firstBelow(profile.xObs, profile.Bobs, 0.5);

        renderMetrics([
          { label: "Максимум E", value: `${fmt(profile.Eobs[idxMaxE], 2)} кВ/м` },
          { label: "Координата max E", value: `${fmt(profile.xObs[idxMaxE], 2)} м` },
          { label: "Максимум B", value: `${fmt(profile.Bobs[idxMaxB], 2)} мкТл` },
          { label: "Координата max B", value: `${fmt(profile.xObs[idxMaxB], 2)} м` },
          { label: "Граница по E ≤ 0.5 кВ/м", value: eBorder ? `${fmt(eBorder.x, 2)} м` : "не найдена" },
          { label: "Граница по B ≤ 0.5 мкТл", value: bBorder ? `${fmt(bBorder.x, 2)} м` : "не найдена" }
        ]);

        renderElectricMap(grid, wires, params);
        renderMagneticMap(grid, wires, params);
        renderProfiles(profile, params);
        renderReport(params, wires, profile);
      } catch (err) {
        document.getElementById("report").textContent = "Ошибка: " + err.message;
        alert("Ошибка: " + err.message);
      }
    }

    function resetDefaults() {
      for (const key in defaults) {
        const el = document.getElementById(key);
        if (!el) continue;
        el.value = defaults[key];
      }
      runModel();
    }

    runModel();
  </script>
</body>
</html>
