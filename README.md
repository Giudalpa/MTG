<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Torneo de Cartas - Tabla de Posiciones</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
        body { background-color: #f4f7f6; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; padding-top: 30px; }
        .dashboard-header { background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%); color: white; padding: 30px; border-radius: 15px; margin-bottom: 30px; box-shadow: 0 4px 15px rgba(0,0,0,0.1); }
        .card { border: none; border-radius: 15px; box-shadow: 0 4px 12px rgba(0,0,0,0.05); }
        .nav-pills .nav-link.active { background-color: #2a5298; }
        .nav-pills .nav-link { color: #2a5298; font-weight: 600; margin-right: 10px; }
        table { margin-bottom: 0 !important; }
        th { background-color: #2a5298 !important; color: white !important; }
        tr:hover { background-color: #f1f4f9; }
    </style>
</head>
<body>

<div class="container">
    <div class="dashboard-header text-center">
        <h1 class="display-5 fw-bold">🏆 Crónicas del Tablero</h1>
        <p class="lead mb-0">Resultados, posiciones y etapa final entre amigos</p>
    </div>

    <ul class="nav nav-pills justify-content-center mb-4" id="tournamentTabs" role="tablist">
        <li class="nav-item" role="presentation">
            <button class="nav-link active" id="fases-tab" data-bs-toggle="tab" data-bs-target="#fases" type="button" role="tab">📊 Tabla y Fases</button>
        </li>
        <li class="nav-item" role="presentation">
            <button class="nav-link" id="final-tab" data-bs-toggle="tab" data-bs-target="#final" type="button" role="tab">🔥 Gran Final</button>
        </li>
    </ul>

    <div class="tab-content" id="tournamentTabsContent">
        <div class="tab-pane fade show active" id="fases" role="tabpanel">
            <div class="card p-4">
                <h3 class="mb-3 text-secondary">Resultados e Ida/Vuelta</h3>
                <div class="table-responsive">
                    <table class="table table-striped table-hover align-middle" id="tabla-fases">
                        <tbody class="text-center py-5"><tr><td>Cargando datos de la fase clasificatoria...</td></tr></tbody>
                    </table>
                </div>
            </div>
        </div>

        <div class="tab-pane fade" id="final" role="tabpanel">
            <div class="card p-4">
                <h3 class="mb-3 text-secondary">Definición del Campeonato</h3>
                <div class="table-responsive">
                    <table class="table table-striped table-hover align-middle" id="tabla-final">
                        <tbody class="text-center py-5"><tr><td>Cargando datos de la final...</td></tr></tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
</div>

<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>

<script>
    // REMPLAZA ESTOS ENLACES CON LOS LINKS DE CSV QUE COPIASTE EN EL PASO 1
    const urlCSVFases = "TU_LINK_CSV_DE_HOJA_FASES_AQUI";
    const urlCSVFinal = "TU_LINK_CSV_DE_HOJA_FINAL_AQUI";

    async function cargarTabla(url, elementoId) {
        try {
            const respuesta = await fetch(url);
            const texto = await respuesta.text();
            
            // Convertir las líneas del CSV en filas y celdas
            const filas = texto.split("\n").map(fila => fila.split(","));
            let htmlContenido = "";

            filas.forEach((fila, index) => {
                // Saltar filas completamente vacías
                if(fila.join("").trim() === "") return;

                htmlContenido += "<tr>";
                fila.forEach(celda => {
                    if (index === 0) {
                        // Encabezados de la tabla
                        htmlContenido += `<th class="text-nowrap">${celda.trim()}</th>`;
                    } else {
                        // Celdas normales
                        htmlContenido += `<td class="text-nowrap">${celda.trim()}</td>`;
                    }
                });
                htmlContenido += "</tr>";
            });

            document.getElementById(elementoId).innerHTML = htmlContenido;
        } catch (error) {
            console.error("Error al cargar los datos:", error);
            document.getElementById(elementoId).innerHTML = `<tr><td class="text-danger">Error al conectar con los datos del torneo.</td></tr>`;
        }
    }

    // Inicializar la carga de datos al abrir la página
    window.addEventListener('DOMContentLoaded', () => {
        if(urlCSVFases !== "TU_LINK_CSV_DE_HOJA_FASES_AQUI") cargarTabla(urlCSVFases, 'tabla-fases');
        if(urlCSVFinal !== "TU_LINK_CSV_DE_HOJA_FINAL_AQUI") cargarTabla(urlCSVFinal, 'tabla-final');
    });
</script>
</body>
</html>
