<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Monitor de Paros de Emergencia</title>
  <style>
    * { box-sizing: border-box; }
    body {
      margin: 0;
      font-family: Arial, Helvetica, sans-serif;
      background: #f4f7fb;
      color: #1f2937;
    }

    .header {
      background: linear-gradient(135deg, #0f172a, #1e3a8a);
      color: white;
      padding: 24px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.15);
    }

    .header h1 {
      margin: 0;
      font-size: 28px;
    }

    .header p {
      margin: 8px 0 0;
      opacity: 0.9;
    }

    .container {
      max-width: 1200px;
      margin: 24px auto;
      padding: 0 16px 32px;
    }

    .grid {
      display: grid;
      gap: 16px;
    }

    .grid-4 {
      grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
    }

    .grid-2 {
      margin-top: 16px;
      grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
    }

    .card {
      background: white;
      border-radius: 18px;
      padding: 18px;
      box-shadow: 0 6px 18px rgba(15, 23, 42, 0.08);
      border: 1px solid #e5e7eb;
    }

    .card h2 {
      margin: 0 0 14px;
      font-size: 18px;
      color: #111827;
    }

    .status-box {
      display: flex;
      align-items: center;
      justify-content: space-between;
      gap: 12px;
      padding: 14px;
      border-radius: 14px;
      background: #f8fafc;
      border: 1px solid #e5e7eb;
    }

    .status-title {
      font-size: 14px;
      color: #6b7280;
      margin-bottom: 6px;
    }

    .status-value {
      font-size: 20px;
      font-weight: 700;
    }

    .badge {
      padding: 8px 12px;
      border-radius: 999px;
      font-size: 13px;
      font-weight: 700;
      white-space: nowrap;
    }

    .ok { background: #dcfce7; color: #166534; }
    .alert { background: #fee2e2; color: #991b1b; }
    .warn { background: #fef3c7; color: #92400e; }
    .info { background: #dbeafe; color: #1d4ed8; }

    .mini-grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 12px;
    }

    .mini-card {
      background: #f8fafc;
      border: 1px solid #e5e7eb;
      border-radius: 14px;
      padding: 14px;
    }

    .mini-card .label {
      font-size: 13px;
      color: #6b7280;
      margin-bottom: 8px;
    }

    .mini-card .number {
      font-size: 28px;
      font-weight: 700;
    }

    .life-wrapper {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
      gap: 18px;
      align-items: start;
    }

    .life-card {
      text-align: center;
      padding: 8px 0;
    }

    .circle {
      width: 130px;
      height: 130px;
      margin: 0 auto 12px;
      border-radius: 50%;
      display: grid;
      place-items: center;
      font-weight: 700;
      font-size: 28px;
      color: #111827;
      background:
        radial-gradient(closest-side, white 72%, transparent 73% 100%),
        conic-gradient(#22c55e 0 92%, #e5e7eb 92% 100%);
      box-shadow: inset 0 0 0 1px #e5e7eb;
    }

    .circle.yellow {
      background:
        radial-gradient(closest-side, white 72%, transparent 73% 100%),
        conic-gradient(#f59e0b 0 78%, #e5e7eb 78% 100%);
    }

    .life-title {
      font-weight: 700;
      margin-bottom: 4px;
    }

    .life-subtitle {
      color: #6b7280;
      font-size: 14px;
    }

    table {
      width: 100%;
      border-collapse: collapse;
      overflow: hidden;
      border-radius: 12px;
    }

    th, td {
      text-align: left;
      padding: 12px 10px;
      border-bottom: 1px solid #e5e7eb;
      font-size: 14px;
    }

    th {
      color: #374151;
      background: #f8fafc;
    }

    .footer-note {
      margin-top: 16px;
      color: #6b7280;
      font-size: 13px;
    }

    @media (max-width: 640px) {
      .header h1 { font-size: 22px; }
      .status-value { font-size: 18px; }
      .mini-card .number { font-size: 24px; }
      .circle { width: 110px; height: 110px; font-size: 24px; }
    }
  </style>
</head>
<body>
  <header class="header">
    <h1>Panel de Monitoreo de Paros de Emergencia</h1>
    <p>Vista previa visual del sistema de monitoreo con ESP32</p>
  </header>

  <main class="container">
    <section class="grid grid-4">
      <div class="card">
        <h2>Estado general</h2>
        <div class="status-box">
          <div>
            <div class="status-title">Sistema</div>
            <div class="status-value">Operando</div>
          </div>
          <span class="badge ok">Normal</span>
        </div>
      </div>

      <div class="card">
        <h2>Relé de seguridad</h2>
        <div class="status-box">
          <div>
            <div class="status-title">Condición</div>
            <div class="status-value">Habilitado</div>
          </div>
          <span class="badge ok">Activo</span>
        </div>
      </div>

      <div class="card">
        <h2>Contactor</h2>
        <div class="status-box">
          <div>
            <div class="status-title">Salida</div>
            <div class="status-value">Energizado</div>
          </div>
          <span class="badge info">ON</span>
        </div>
      </div>

      <div class="card">
        <h2>Motor</h2>
        <div class="status-box">
          <div>
            <div class="status-title">Estado</div>
            <div class="status-value">En marcha</div>
          </div>
          <span class="badge ok">Funcionando</span>
        </div>
      </div>
    </section>

    <section class="grid grid-2">
      <div class="card">
        <h2>Estado de paros</h2>
        <div class="mini-grid">
          <div class="mini-card">
            <div class="label">Paro de emergencia 1</div>
            <div class="status-box">
              <strong>Normal</strong>
              <span class="badge ok">No activado</span>
            </div>
          </div>
          <div class="mini-card">
            <div class="label">Paro de emergencia 2</div>
            <div class="status-box">
              <strong>Normal</strong>
              <span class="badge ok">No activado</span>
            </div>
          </div>
        </div>
      </div>

      <div class="card">
        <h2>Conteo de activaciones</h2>
        <div class="mini-grid">
          <div class="mini-card">
            <div class="label">Activaciones Paro 1</div>
            <div class="number">14</div>
          </div>
          <div class="mini-card">
            <div class="label">Activaciones Paro 2</div>
            <div class="number">7</div>
          </div>
        </div>
      </div>
    </section>

    <section class="grid grid-2">
      <div class="card">
        <h2>Vida útil estimada</h2>
        <div class="life-wrapper">
          <div class="life-card">
            <div class="circle">92%</div>
            <div class="life-title">Paro 1</div>
            <div class="life-subtitle">Condición saludable</div>
          </div>
          <div class="life-card">
            <div class="circle yellow">78%</div>
            <div class="life-title">Paro 2</div>
            <div class="life-subtitle">Revisión recomendada</div>
          </div>
        </div>
      </div>

      <div class="card">
        <h2>Últimos eventos</h2>
        <table>
          <thead>
            <tr>
              <th>Hora</th>
              <th>Evento</th>
              <th>Estado</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>09:12</td>
              <td>Encendido del sistema</td>
              <td><span class="badge ok">Correcto</span></td>
            </tr>
            <tr>
              <td>09:25</td>
              <td>Prueba de paro 1</td>
              <td><span class="badge warn">Registrado</span></td>
            </tr>
            <tr>
              <td>10:03</td>
              <td>Rearme del relé</td>
              <td><span class="badge info">Completado</span></td>
            </tr>
            <tr>
              <td>10:18</td>
              <td>Motor reanudado</td>
              <td><span class="badge ok">Normal</span></td>
            </tr>
          </tbody>
        </table>
        <div class="footer-note">
          Esta es una vista preliminar. Más adelante se puede conectar con el ESP32 para mostrar datos reales en tiempo real.
        </div>
      </div>
    </section>
  </main>
</body>
</html>
