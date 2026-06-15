<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Estado de Edición - Reality</title>
    <!-- Importamos Poppins, una fuente moderna y más lúdica -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap" rel="stylesheet">
    
    <style>
        :root {
            --primary-color: #a1007d;
            --text-main: #222222; /* Ligeramente más oscuro para mejor lectura con la nueva fuente */
            --text-muted: #888888;
            --bg-color: #ffffff;
            --box-bg: #fdfdfd;
        }

        body {
            margin: 0;
            padding: 0;
            font-family: 'Poppins', sans-serif; /* Aplicamos la nueva fuente */
            background-color: var(--bg-color);
            color: var(--text-main);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            text-align: center;
        }

        .container {
            width: 90%;
            max-width: 650px;
            padding: 40px 20px;
            /* Aseguramos espacio abajo para que los controles fijos no tapen nada en móvil */
            margin-bottom: 80px; 
        }

        h1 {
            text-transform: uppercase;
            letter-spacing: 2px;
            font-weight: 700; /* Fuente más gruesa */
            font-size: 28px;
            margin-bottom: 50px;
            color: var(--text-main);
        }

        .status-label {
            font-size: 13px;
            text-transform: uppercase;
            letter-spacing: 1px;
            color: var(--text-muted);
            margin-bottom: 12px;
            font-weight: 600;
        }

        .status-box {
            border: 2px solid var(--primary-color); /* Borde un poco más grueso */
            background-color: var(--box-bg);
            padding: 30px 20px;
            border-radius: 16px; /* Esquinas mucho más redondeadas y lúdicas */
            box-shadow: 0 6px 20px rgba(161, 0, 125, 0.08);
            margin-bottom: 30px;
            transition: all 0.3s ease;
        }

        .current-stage {
            font-size: 22px;
            font-weight: 700; /* Fuente muy gruesa para el estado */
            color: var(--primary-color);
            margin: 0;
            line-height: 1.3;
        }

        .next-stage-container {
            margin-bottom: 60px;
        }

        .next-stage-label {
            font-size: 13px;
            color: var(--text-muted);
            text-transform: uppercase;
            letter-spacing: 1px;
            font-weight: 600;
        }

        .next-stage {
            font-size: 16px;
            color: var(--text-main);
            margin-top: 8px;
            font-weight: 400;
        }

        .progress-wrapper {
            margin-top: 20px;
        }

        .progress-bar-container {
            width: 100%;
            height: 12px; /* Barra un poco más gruesa */
            background-color: #f0f0f0;
            border-radius: 20px; /* Redondeado total */
            overflow: hidden;
            margin-bottom: 15px;
        }

        .progress-fill {
            height: 100%;
            background-color: var(--primary-color);
            width: 0%;
            border-radius: 20px;
            transition: width 0.8s cubic-bezier(0.4, 0, 0.2, 1);
        }

        .percentage {
            font-size: 20px;
            font-weight: 700;
            color: var(--primary-color);
        }

        /* --- NUEVOS ESTILOS PARA LOS CONTROLADORES (MENOS VISIBLES) --- */
        .controls {
            position: fixed; /* Fijo en la pantalla */
            bottom: 15px;
            right: 15px;
            background-color: rgba(255, 255, 255, 0.9); /* Fondo semi-transparente */
            padding: 10px;
            border-radius: 12px;
            border: 1px solid #eeeeee;
            box-shadow: 0 2px 10px rgba(0,0,0,0.05);
            text-align: right;
            z-index: 100;
            
            /* Menor contraste y protagonismo por defecto */
            opacity: 0.3; 
            transition: opacity 0.3s ease;
        }

        /* Se vuelve visible al pasar el ratón */
        .controls:hover {
            opacity: 1;
        }

        .editor-label {
            font-size: 10px;
            color: #aaaaaa;
            text-transform: uppercase;
            letter-spacing: 1px;
            margin-bottom: 5px;
            display: block;
            font-weight: 600;
        }

        select {
            padding: 6px 10px;
            border: 1px solid #dddddd;
            border-radius: 8px;
            font-family: 'Poppins', sans-serif;
            font-size: 11px; /* Letra pequeña */
            color: #666666;
            cursor: pointer;
            outline: none;
            background-color: #ffffff;
            width: auto;
            max-width: 180px; /* No ocupa mucho ancho */
        }

        select:focus {
            border-color: #cccccc;
        }

        /* Ajuste para móviles pequeños */
        @media (max-width: 480px) {
            h1 { font-size: 22px; }
            .current-stage { font-size: 18px; }
            .controls {
                width: calc(100% - 30px);
                right: 15px;
                left: 15px;
                text-align: center;
                box-sizing: border-box;
            }
            select { max-width: 100%; width: 100%; }
        }
    </style>
</head>
<body>

    <div class="container">
        <h1>EDICIÓN REALITY</h1>

        <div class="status-label">Estado Actual del Proyecto</div>
        <div class="status-box">
            <p class="current-stage" id="currentStageText">Cargando...</p>
        </div>

        <div class="next-stage-container">
            <div class="next-stage-label">Siguiente etapa:</div>
            <div class="next-stage" id="nextStageText">Cargando...</div>
        </div>

        <div class="progress-wrapper">
            <div class="progress-bar-container">
                <div class="progress-fill" id="progressFill"></div>
            </div>
            <div class="percentage" id="percentageText">0% completado</div>
        </div>
    </div>

    <!-- Panel de control flotante en la esquina -->
    <div class="controls">
        <label for="stageSelector" class="editor-label">Panel Editor</label>
        <select id="stageSelector" onchange="updateProgress(this.value)"></select>
    </div>

    <script>
        // Mismas etapas, misma lógica
        const stages = [
            "Transferencia y respaldo de datos",
            "Organización de material",
            "Visionado de material",
            "Corte de clips y selección de momentos",
            "Sincronización de audio boom y filmadora",
            "Calado de los clips en pantalla verde (croma, color y rotoscopía)",
            "Descarga de recursos como títulos, transiciones y elementos gráficos",
            "Armado de primera base",
            "Elaboración de voz en off (guion y voz)",
            "Selección musical",
            "Reajuste de primera base (primer corte)",
            "Ajuste en base a feedback",
            "Reajuste de segunda base (segundo corte)",
            "Colorización",
            "Postproducción de sonido y limpieza de audio",
            "Ajuste de base final (corte final)",
            "Exporte final"
        ];

        const selector = document.getElementById('stageSelector');
        const currentStageText = document.getElementById('currentStageText');
        const nextStageText = document.getElementById('nextStageText');
        const progressFill = document.getElementById('progressFill');
        const percentageText = document.getElementById('percentageText');

        stages.forEach((stage, index) => {
            let option = document.createElement('option');
            option.value = index;
            // Simplificamos el texto en el selector
            option.textContent = `Paso ${index + 1}`; 
            selector.appendChild(option);
        });

        function updateProgress(index) {
            index = parseInt(index);
            const totalStages = stages.length;
            
            currentStageText.textContent = stages[index];

            if (index < totalStages - 1) {
                nextStageText.textContent = stages[index + 1];
            } else {
                nextStageText.textContent = "🎉 ¡Proyecto finalizado y listo para entrega! 🎉";
            }

            // Calculo de porcentaje (0% en el paso 1, 100% en el último)
            let percentage = Math.round((index / (totalStages - 1)) * 100);
            
            progressFill.style.width = percentage + '%';
            percentageText.textContent = percentage + '% completado';
        }

        // Inicializar
        updateProgress(0);
    </script>
</body>
</html>
