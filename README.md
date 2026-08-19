---
layout: none
---
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>T9 Stacking Calculator</title>
  <style>
    :root {
      --bg: #f8fafc;
      --card-bg: #ffffff;
      --border: #e2e8f0;
      --accent: #1e40af;
      --text: #0f172a;
      --text-muted: #64748b;
      --group-g9: #2563eb;
      --group-s9: #7c3aed;
      --group-g8: #db2777;
      --group-s8: #059669;
    }

    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
    }

    body {
      background-color: var(--bg);
      color: var(--text);
      padding: 20px;
      display: flex;
      justify-content: center;
      min-height: 100vh;
    }

    .container {
      max-width: 1000px;
      width: 100%;
    }

    header {
      margin-bottom: 24px;
      text-align: center;
    }

    h1 {
      font-size: 1.8rem;
      font-weight: 700;
      margin-bottom: 6px;
      color: var(--text);
    }

    p.subtitle {
      color: var(--text-muted);
      font-size: 0.95rem;
    }

    p.instruction {
      color: var(--text-muted);
      font-size: 0.85rem;
      font-style: italic;
      margin-top: 6px;
    }

    .input-card {
      background: var(--card-bg);
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 20px;
      margin-bottom: 24px;
      display: flex;
      flex-direction: column;
      gap: 16px;
      box-shadow: 0 1px 3px rgba(0, 0, 0, 0.05);
    }

    .input-group {
      display: flex;
      flex-direction: column;
      gap: 6px;
    }

    label {
      font-size: 0.85rem;
      text-transform: uppercase;
      letter-spacing: 0.05em;
      color: var(--text-muted);
      font-weight: 600;
    }

    input[type="number"] {
      background: #f1f5f9;
      border: 1px solid var(--border);
      color: var(--text);
      padding: 12px 16px;
      border-radius: 8px;
      font-size: 1.1rem;
      font-weight: 600;
      outline: none;
      transition: border-color 0.2s, background-color 0.2s;
    }

    input[type="number"]:focus {
      border-color: var(--accent);
      background: #ffffff;
    }

    .stat-row {
      display: flex;
      justify-content: space-between;
      align-items: center;
      padding-top: 12px;
      border-top: 1px solid var(--border);
    }

    .stat-label {
      font-size: 0.95rem;
      font-weight: 600;
      color: var(--text-muted);
    }

    .stat-value {
      font-size: 1.25rem;
      font-weight: 700;
      color: #1e3a8a;
      font-family: "Courier New", Courier, monospace;
    }

    .main-layout {
      display: grid;
      grid-template-columns: 1fr;
      gap: 24px;
    }

    @media (min-width: 800px) {
      .main-layout {
        grid-template-columns: 1fr 1.2fr;
      }
    }

    .left-column {
      display: flex;
      flex-direction: column;
      gap: 24px;
    }

    .card {
      background: var(--card-bg);
      border: 1px solid var(--border);
      border-radius: 12px;
      padding: 20px;
      overflow-x: auto;
      box-shadow: 0 1px 3px rgba(0, 0, 0, 0.05);
    }

    .card h2 {
      font-size: 1.2rem;
      margin-bottom: 4px;
      color: var(--text);
    }

    .section-note {
      font-size: 0.8rem;
      color: var(--text-muted);
      margin-bottom: 14px;
      padding-bottom: 10px;
      border-bottom: 1px solid var(--border);
    }

    .card-header-line {
      padding-bottom: 10px;
      border-bottom: 1px solid var(--border);
      margin-bottom: 14px;
    }

    table {
      width: 100%;
      border-collapse: collapse;
      text-align: left;
      font-size: 0.95rem;
    }

    th, td {
      padding: 10px 12px;
      border-bottom: 1px solid var(--border);
      color: var(--text);
    }

    th {
      color: var(--text-muted);
      font-weight: 600;
      font-size: 0.8rem;
      text-transform: uppercase;
    }

    tr:last-child td {
      border-bottom: none;
    }

    .badge {
      display: inline-block;
      padding: 2px 8px;
      border-radius: 4px;
      font-size: 0.75rem;
      font-weight: 700;
      color: #ffffff;
    }

    .badge-g9 { background-color: var(--group-g9); }
    .badge-s9 { background-color: var(--group-s9); }
    .badge-g8 { background-color: var(--group-g8); }
    .badge-s8 { background-color: var(--group-s8); }

    .num-val {
      font-family: "Courier New", Courier, monospace;
      font-weight: 700;
      color: #1e3a8a;
      text-align: right;
    }
  </style>
</head>
<body>

  <div class="container">
    <header>
      <h1>T9 Stacking Calculator</h1>
      <p class="subtitle">Calculates all troop types, monsters, and mercenaries based on input value</p>
      <p class="instruction">Change the number of the 'base value' until you match your total leadership</p>
    </header>

    <div class="input-card">
      <div class="input-group">
        <label for="baseInput">Base Value - Ranged Units G9</label>
        <input type="number" id="baseInput" value="13000" placeholder="Enter Base Value..." oninput="calculate()">
      </div>
      <div class="stat-row">
        <span class="stat-label">Total Leadership Required</span>
        <span class="stat-value" id="totalLeadership">0</span>
      </div>
    </div>

    <div class="main-layout">
      <!-- Left Column: Mercenaries Top, Monsters Bottom -->
      <div class="left-column">
        <!-- Mercenaries Card -->
        <div class="card">
          <h2>Mercenaries</h2>
          <p class="section-note">Max number of mercenaries, adjust per availability</p>
          <table>
            <thead>
              <tr>
                <th>Unit Name</th>
                <th style="text-align: right;">Count</th>
              </tr>
            </thead>
            <tbody id="mercenariesTable"></tbody>
          </table>
        </div>

        <!-- Monsters Card -->
        <div class="card">
          <h2>Monsters</h2>
          <p class="section-note">Max numbers. Add M9 and then adjust M8/M7 to match your Dominance</p>
          <table>
            <thead>
              <tr>
                <th>Unit Name</th>
                <th style="text-align: right;">Count</th>
              </tr>
            </thead>
            <tbody id="monstersTable"></tbody>
          </table>
        </div>
      </div>

      <!-- Right Column: Troop Calculations -->
      <div class="right-column">
        <div class="card">
          <h2 class="card-header-line">Troops</h2>
          <table>
            <thead>
              <tr>
                <th>Group</th>
                <th>Troop Type</th>
                <th style="text-align: right;">Count</th>
              </tr>
            </thead>
            <tbody id="troopsTable"></tbody>
          </table>
        </div>
      </div>
    </div>
  </div>

  <script>
    function calculate() {
      const baseInput = parseFloat(document.getElementById('baseInput').value);
      const troopsTable = document.getElementById('troopsTable');
      const monstersTable = document.getElementById('monstersTable');
      const mercenariesTable = document.getElementById('mercenariesTable');
      const totalLeadershipEl = document.getElementById('totalLeadership');

      troopsTable.innerHTML = '';
      monstersTable.innerHTML = '';
      mercenariesTable.innerHTML = '';

      if (isNaN(baseInput)) {
        totalLeadershipEl.innerText = '0';
        return;
      }

      const B = baseInput;
      const K5 = B * 5510;

      // Group G9
      const c5 = B;
      const d5 = c5;
      const c6 = c5 + 16;
      const d6 = c6 / 20;
      const c7 = c5 + 8;
      const d7 = c7 / 2;
      const c8 = c7 + 40;
      const d8 = c8;

      // Group G8
      const c15 = c5 * 1.805;
      const d15 = c15;
      const c16 = c15 + 6;
      const d16 = c16 / 20;
      const c17 = c15 + 2;
      const d17 = c17 / 2;
      const c18 = c17 + 34;
      const d18 = c18;

      // Group S9
      const c10 = c15 / 1.7999;
      const d10 = c10;
      const c11 = c10 + 2;
      const d11 = c11 / 20;
      const c12 = c10 + 2;
      const d12 = c12 / 2;
      const c13 = c10 + 70;
      const d13 = c13;

      // Group S8
      const c20 = c15 + 38;
      const d20 = c20;
      const c23 = c20 + 60;
      const d23 = c23;
      const c21 = c23 + 2;
      const d21 = c21 / 20;
      const c22 = c23 - 20;
      const d22 = c22 / 2;

      // Cell C25: Total Leadership
      const totalLeadership = c5 + c6 + c7 + c8 + c10 + c11 + c12 + c13 + c15 + c16 + c17 + c18 + c20 + c21 + c22 + c23;
      totalLeadershipEl.innerText = Math.floor(totalLeadership).toLocaleString();

      const troopsData = [
        // Group 1
        { group: 'G9', type: 'Corax II', count: d6 },
        { group: 'S9', type: 'Lions II', count: d11 },
        { group: 'G8', type: 'Corax I', count: d16 },
        { group: 'S8', type: 'Lions I', count: d21 },
        { divider: true },
        // Group 2
        { group: 'G9', type: 'Mounted', count: d7 },
        { group: 'S9', type: 'Mounted', count: d12 },
        { group: 'G8', type: 'Mounted', count: d17 },
        { group: 'S8', type: 'Mounted', count: d22 },
        { divider: true },
        // Group 3
        { group: 'G9', type: 'Ranged', count: d5 },
        { group: 'G9', type: 'Melee', count: d8 },
        { group: 'S9', type: 'Ranged', count: d10 },
        { group: 'S9', type: 'Melee', count: d13 },
        { divider: true },
        // Group 4
        { group: 'G8', type: 'Ranged', count: d15 },
        { group: 'G8', type: 'Melee', count: d18 },
        { group: 'S8', type: 'Ranged', count: d20 },
        { group: 'S8', type: 'Melee', count: d23 },
      ];

      // Monsters Calculations (M9, M8, M7)
      const m6 = (K5 / 1210000) * 0.9 - 2;
      const m7 = (K5 / 670000) * 0.9 - 3;
      const m8 = (K5 / 310000) * 0.9 - 3;

      const monstersData = [
        { name: 'M9', val: m6 },
        { name: 'M8', val: m7 },
        { name: 'M7', val: m8 },
      ];

      // Mercenaries Calculations
      const l11 = (K5 / 690000) * 0.9;
      const m11 = l11 * 0.9;

      const l12 = (K5 / 470000) * 0.9;
      const m12 = l12 * 0.9;

      const l13 = (K5 / 410000) * 0.9;
      const m13 = l13 * 0.9;

      const l14 = (K5 / 220000) * 0.9;
      const m14 = l14 * 0.9;

      const l15 = (K5 / 440000) * 0.9;
      const m15 = l15 * 0.9;

      const m16 = B * 0.0408;
      const m17 = (K5 / 25000) * 0.9;
      const m18 = (K5 / 22000) * 0.9;
      const m19 = (K5 / 11000) * 0.9;

      const mercenariesData = [
        { name: 'Wyvern', val: m11 },
        { name: 'Warden', val: m12 },
        { name: 'Salamander', val: m13 },
        { name: 'Warregal/Jago', val: m14 },
        { name: 'Cannoneer', val: m15 },
        { name: 'Ariel', val: m16 },
        { name: 'Epic Hunter', val: m17 },
        { name: 'Riders', val: m18 },
        { name: 'Troops', val: m19 },
      ];

      function fmt(v) {
        return Math.floor(v).toLocaleString();
      }

      // Render Troops
      troopsData.forEach(item => {
        if (item.divider) {
          const tr = document.createElement('tr');
          tr.innerHTML = `<td colspan="3" style="padding: 4px; background: rgba(0,0,0,0.02);"></td>`;
          troopsTable.appendChild(tr);
          return;
        }

        const badgeClass = `badge-${item.group.toLowerCase()}`;
        const tr = document.createElement('tr');
        tr.innerHTML = `
          <td><span class="badge ${badgeClass}">${item.group}</span></td>
          <td><strong>${item.type}</strong></td>
          <td class="num-val">${fmt(item.count)}</td>
        `;
        troopsTable.appendChild(tr);
      });

      // Render Mercenaries
      mercenariesData.forEach(item => {
        const tr = document.createElement('tr');
        tr.innerHTML = `
          <td><strong>${item.name}</strong></td>
          <td class="num-val">${fmt(item.val)}</td>
        `;
        mercenariesTable.appendChild(tr);
      });

      // Render Monsters
      monstersData.forEach(item => {
        const tr = document.createElement('tr');
        tr.innerHTML = `
          <td><strong>${item.name}</strong></td>
          <td class="num-val">${fmt(item.val)}</td>
        `;
        monstersTable.appendChild(tr);
      });
    }

    calculate();
  </script>
</body>
</html>
