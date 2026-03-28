
<svg xmlns="http://www.w3.org/2000/svg" width="400" height="400" viewBox="0 0 400 400">
  <rect width="400" height="400" fill="#0f0f1a"/>
  <circle cx="200" cy="200" r="180" fill="none" stroke="#ffffff22" stroke-width="1"/>
  <circle cx="200" cy="200" r="175" fill="none" stroke="#ffffff11" stroke-width="8"/>

  <!-- tick marks -->
  <g id="ticks"/>

  <!-- hands -->
  <line id="h" x1="200" y1="200" x2="200" y2="110" stroke="#ffffff" stroke-width="6" stroke-linecap="round"/>
  <line id="m" x1="200" y1="200" x2="200" y2="60"  stroke="#4fc3f7" stroke-width="4" stroke-linecap="round"/>
  <line id="s" x1="200" y1="220" x2="200" y2="50"  stroke="#ff6b6b" stroke-width="2" stroke-linecap="round"/>

  <!-- center dot -->
  <circle cx="200" cy="200" r="6" fill="#ffffff"/>
  <circle cx="200" cy="200" r="3" fill="#ff6b6b"/>

  <!-- date display -->
  <rect x="240" y="193" width="60" height="22" rx="4" fill="#ffffff15" stroke="#ffffff22" stroke-width="1"/>
  <text id="date" x="270" y="208" text-anchor="middle" fill="#ffffffaa" font-family="monospace" font-size="12"/>

  <script><![CDATA[
    const svg = document.querySelector('svg');
    const ns = 'http://www.w3.org/2000/svg';

    // draw tick marks
    const ticks = document.getElementById('ticks');
    for (let i = 0; i < 60; i++) {
      const angle = (i / 60) * 2 * Math.PI - Math.PI / 2;
      const isHour = i % 5 === 0;
      const r1 = isHour ? 155 : 165;
      const r2 = 172;
      const line = document.createElementNS(ns, 'line');
      line.setAttribute('x1', 200 + r1 * Math.cos(angle));
      line.setAttribute('y1', 200 + r1 * Math.sin(angle));
      line.setAttribute('x2', 200 + r2 * Math.cos(angle));
      line.setAttribute('y2', 200 + r2 * Math.sin(angle));
      line.setAttribute('stroke', isHour ? '#ffffffcc' : '#ffffff44');
      line.setAttribute('stroke-width', isHour ? '2.5' : '1');
      line.setAttribute('stroke-linecap', 'round');
      ticks.appendChild(line);

      if (isHour) {
        const num = i === 0 ? 12 : i / 5;
        const tr = 140;
        const txt = document.createElementNS(ns, 'text');
        txt.setAttribute('x', 200 + tr * Math.cos(angle));
        txt.setAttribute('y', 200 + tr * Math.sin(angle) + 5);
        txt.setAttribute('text-anchor', 'middle');
        txt.setAttribute('fill', '#ffffffaa');
        txt.setAttribute('font-family', 'sans-serif');
        txt.setAttribute('font-size', '16');
        txt.textContent = num;
        ticks.appendChild(txt);
      }
    }

    function rotate(el, deg, cx, cy, len) {
      const rad = (deg - 90) * Math.PI / 180;
      el.setAttribute('x2', cx + len * Math.cos(rad));
      el.setAttribute('y2', cy + len * Math.sin(rad));
      if (el.hasAttribute('x1') && el.id === 's') {
        el.setAttribute('x1', cx - 20 * Math.cos(rad));
        el.setAttribute('y1', cy - 20 * Math.sin(rad));
      }
    }

    function tick() {
      const now = new Date();
      const ms = now.getMilliseconds();
      const sec = now.getSeconds() + ms / 1000;
      const min = now.getMinutes() + sec / 60;
      const hr  = (now.getHours() % 12) + min / 60;

      rotate(document.getElementById('h'), hr  * 30,  200, 200, 90);
      rotate(document.getElementById('m'), min * 6,   200, 200, 140);
      rotate(document.getElementById('s'), sec * 6,   200, 200, 150);

      const days = ['Sun','Mon','Tue','Wed','Thu','Fri','Sat'];
      document.getElementById('date').textContent =
        days[now.getDay()] + ' ' + String(now.getDate()).padStart(2,'0');

      requestAnimationFrame(tick);
    }

    tick();
  ]]></script>
</svg>
