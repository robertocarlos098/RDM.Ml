<!DOCTYPE html>
<html lang="es" class="light"> 
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="theme-color" content="#3B82F6">
    <link rel="manifest" href="data:application/json;base64,eyJuYW1lIjoiQ29udmVyc29yIGRlIExvbmdpdHVkIiwic2hvcnRfbmFtZSI6IkNvbnZlcnNvciIsInN0YXJ0X3VybCI6Ii4iLCJkaXNwbGF5Ijoic3RhbmRhbG9uZSIsImJhY2tncm91bmRfY29sb3IiOiIjZjNmNGY2IiwidGhlbWVfY29sb3IiOiIjM0I4MkY2IiwiaWNvbnMiOlt7InNyYyI6ImRhdGE6aW1hZ2Uvc3ZnK3htbDtiYXNlNjQsUEhOMlp5QjRiV3hwYldVc0lETXdNQ0FvSUQwMk9EWXdNekF0SUNBZ0lEMGdKSEp5YjNWallBQUFBQUFBQUFBZ0lDQWdJRDBtSkd4c2JHVXVJR05zYjNSeklERTJMbVY0WVcxd2JHVXVkbUZzYVdKaFBYSmhjaTl1WlNCaGJtUnZkMjVzYjI4dWVHVnpjMmx2YmowaWRXaHVkSEF1YVc1emFYWnZkMlV1Y0hKdlpIVmpYMlZ1ZENCNyIsInR5cGUiOiJpbWFnZS9zdmcreG1sIiwic2l6ZXMiOiIxOTJ4MTkyIDUxMng1MTIifV19">
    <title>Conversor de Longitud</title>
    <!-- Carga de Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* 1. Fuente y Transiciones Base */
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            transition: background-color 0.3s;
            display: flex;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            background-image: linear-gradient(to top right, #f0f0f5, #e0e0f0);
        }
        .dark body {
            background-image: linear-gradient(to bottom left, #1f2937, #111827);
        }

        /* 2. Estilo de la Tarjeta Principal (con transparencia y desenfoque) */
        .converter-card {
            width: 100%;
            max-width: 28rem; 
            padding: 2rem; 
            border-radius: 0.75rem; 
            box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.1), 0 10px 10px -5px rgba(0, 0, 0, 0.04); 
            transition: all 0.3s;
            background-color: rgba(255, 255, 255, 0.9); 
            backdrop-filter: blur(4px); 
        }
        .dark .converter-card {
            background-color: rgba(31, 41, 55, 0.9); 
        }

        /* 3. Estilos Comunes de Texto y Componentes */
        .text-blue-main { color: #2563EB; }
        .dark .text-blue-main { color: #60A5FA; }
        .text-secondary { color: #4B5563; }
        .dark .text-secondary { color: #D1D5DB; }
        .input-group { display: flex; }

        /* 4. Estilos de Campos de Entrada/Selección (con transparencia) */
        .input-field {
            flex: 1;
            padding: 0.75rem; 
            border: 1px solid #D1D5DB; 
            border-radius: 0.375rem 0 0 0.375rem; 
            transition: all 0.2s;
            background-color: rgba(255, 255, 255, 0.8); 
        }
        .dark .input-field {
            border-color: #4B5563; 
            background-color: rgba(55, 65, 81, 0.8); 
            color: white;
        }
        .input-field:focus {
            outline: 2px solid transparent;
            outline-offset: 2px;
            border-color: #3B82F6; 
            box-shadow: 0 0 0 1px #3B82F6; 
        }

        .select-field {
            padding: 0.75rem;
            border: 1px solid #D1D5DB;
            border-left-width: 0;
            border-radius: 0 0.375rem 0.375rem 0; 
            background-color: rgba(249, 250, 251, 0.8); 
            transition: all 0.2s;
        }
        .dark .select-field {
            border-color: #4B5563;
            background-color: rgba(55, 65, 81, 0.8); 
            color: white;
        }
        .select-field:focus {
            outline: 2px solid transparent;
            outline-offset: 2px;
            border-color: #3B82F6;
            box-shadow: 0 0 0 1px #3B82F6;
        }

        #outputValue {
            background-color: rgba(243, 244, 246, 0.8); 
            cursor: not-allowed;
        }
        .dark #outputValue {
            background-color: rgba(55, 65, 81, 0.8); 
        }

        /* 5. Botones y Elementos Especiales */
        .toggle-button {
            padding: 0.5rem;
            border-radius: 9999px; 
            transition: all 0.3s;
            background-color: #E5E7EB; 
            color: #4B5563; 
        }
        .dark .toggle-button {
            background-color: #374151; 
            color: #E5E7EB; 
        }
        .toggle-button:hover {
            background-color: #D1D5DB; 
        }
        .dark .toggle-button:hover {
            background-color: #4B5563; 
        }
        
        .main-button {
            width: 100%;
            font-weight: 600; 
            padding: 0.75rem 1.5rem; 
            border-radius: 0.5rem; 
            transition: all 0.3s;
            color: white;
        }

        #copyButton {
            background-color: #2563EB; 
        }
        #copyButton:hover {
            background-color: #1D4ED8; 
        }

        /* 6. Mensaje de App (Flotante) */
        .app-message {
            position: fixed;
            top: 1rem;
            right: 1rem;
            padding: 0.75rem 1.5rem;
            background-color: #10B981; /* green-500 (éxito) */
            color: white;
            border-radius: 0.5rem;
            box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 0 2px 4px -2px rgba(0, 0, 0, 0.1);
            z-index: 50;
            transition: transform 0.3s ease-out, opacity 0.3s ease-out;
            transform: translateY(-100%);
            opacity: 0;
        }
        .app-message.visible {
            transform: translateY(0);
            opacity: 1;
        }
        .app-message.error {
            background-color: #EF4444; /* red-500 (error) */
        }
    </style>
</head>
<body>
    
    <!-- Mensaje de la Aplicación (Reemplaza a alert() y confirm()) -->
    <div id="appMessage" class="app-message"></div>

    <!-- Tarjeta Principal -->
    <div class="converter-card">
        <!-- Encabezado y Toggle de Tema -->
        <div class="flex justify-between items-center mb-6">
            <h1 class="text-2xl sm:text-3xl font-bold text-blue-main">
                Conversor de Longitud
            </h1>
            <button id="themeToggle" class="toggle-button" title="Cambiar tema">
                <svg id="sunIcon" class="h-6 w-6 hidden" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 3v1m0 16v1m9-9h-1M4 12H3m15.364 6.364l-.707-.707M6.343 6.343l-.707-.707m12.728 0l-.707.707M6.343 17.657l-.707.707M16 12a4 4 0 11-8 0 4 4 0 018 0z" />
                </svg>
                <svg id="moonIcon" class="h-6 w-6 hidden" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                    <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20.354 15.354A9 9 0 018.646 3.646 9.003 9.003 0 0012 21a9.003 9.003 0 008.354-5.646z" />
                </svg>
            </button>
        </div>

        <div class="space-y-6">
            <!-- Sección de Entrada -->
            <div>
                <label for="inputValue" class="block text-sm font-medium text-secondary mb-2">
                    Valor a convertir:
                </label>
                <div class="input-group">
                    <input
                        type="number"
                        id="inputValue"
                        placeholder="0"
                        class="input-field"
                    >
                    <select
                        id="fromUnit"
                        class="select-field"
                    >
                        <option value="meters">Metros (m)</option>
                        <option value="millimeters">Milímetros (mm)</option>
                        <option value="centimeters">Centímetros (cm)</option>
                        <option value="kilometers">Kilómetros (km)</option>
                        <option value="inches">Pulgadas (in)</option>
                        <option value="feet">Pies (ft)</option>
                    </select>
                </div>
            </div>

            <!-- Icono de flecha (intercambio) -->
            <div class="text-center">
                <button id="swapButton" class="toggle-button" title="Intercambiar unidades">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 inline-block" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 7h12m0 0l-4-4m4 4l-4 4m-4 6H4m0 0l4 4m-4-4l4-4" />
                    </svg>
                </button>
            </div>

            <!-- Sección de Salida -->
            <div>
                <label for="outputValue" class="block text-sm font-medium text-secondary mb-2">
                    Resultado:
                </label>
                <div class="input-group relative">
                    <input
                        type="text"
                        id="outputValue"
                        readonly
                        placeholder="0"
                        class="input-field"
                    >
                    <select
                        id="toUnit"
                        class="select-field"
                    >
                        <option value="meters">Metros (m)</option>
                        <option value="millimeters" selected>Milímetros (mm)</option>
                        <option value="centimeters">Centímetros (cm)</option>
                        <option value="kilometers">Kilómetros (km)</option>
                        <option value="inches">Pulgadas (in)</option>
                        <option value="feet">Pies (ft)</option>
                    </select>
                </div>
            </div>

            <!-- Botón de Copiar Resultado -->
            <div class="text-center">
                <button
                    id="copyButton"
                    class="main-button"
                >
                    Copiar Resultado
                </button>
            </div>

            <!-- Generador de Enlace Compartible -->
            <div class="mt-4 border-t border-gray-200 dark:border-gray-700 pt-4 space-y-3">
                <h3 class="text-lg font-semibold text-gray-800 dark:text-gray-200">Compartir Conversión</h3>
                <button
                    id="generateLinkButton"
                    class="main-button bg-gray-500 hover:bg-gray-600 focus:ring-gray-400"
                >
                    🔗 Generar y Copiar Enlace
                </button>

                <div id="shareLinkContainer" class="hidden flex">
                    <input
                        type="text"
                        id="shareLinkInput"
                        readonly
                        placeholder="El enlace aparecerá aquí"
                        class="input-field text-sm truncate"
                        style="border-radius: 0.375rem 0.375rem 0.375rem 0.375rem;"
                    >
                </div>
            </div>
            
        </div>
    </div>

    <script>
        // Factores de conversión
        const factors = {
            'meters': 1, 'millimeters': 0.001, 'centimeters': 0.01,
            'kilometers': 1000, 'inches': 0.0254, 'feet': 0.3048
        };
        const unitNames = {
            'meters': 'metros', 'millimeters': 'milímetros', 'centimeters': 'centímetros',
            'kilometers': 'kilómetros', 'inches': 'pulgadas', 'feet': 'pies'
        };

        // Elementos del DOM
        const htmlEl = document.documentElement;
        const inputValueEl = document.getElementById('inputValue');
        const fromUnitEl = document.getElementById('fromUnit');
        const outputValueEl = document.getElementById('outputValue');
        const toUnitEl = document.getElementById('toUnit');
        const copyButton = document.getElementById('copyButton');
        const swapButton = document.getElementById('swapButton');
        const themeToggle = document.getElementById('themeToggle');
        const sunIcon = document.getElementById('sunIcon');
        const moonIcon = document.getElementById('moonIcon');
        const generateLinkButton = document.getElementById('generateLinkButton');
        const shareLinkContainer = document.getElementById('shareLinkContainer');
        const shareLinkInput = document.getElementById('shareLinkInput');
        const appMessageEl = document.getElementById('appMessage');

        /**
         * Muestra un mensaje flotante en la esquina superior derecha.
         */
        function showAppMessage(message, duration = 2000, type = 'success') {
            appMessageEl.textContent = message;
            
            // Limpia clases anteriores
            appMessageEl.classList.remove('error');
            
            if (type === 'error') {
                appMessageEl.classList.add('error');
            }
            
            appMessageEl.classList.add('visible');
            
            setTimeout(() => {
                appMessageEl.classList.remove('visible');
                // Limpia la clase de error después de que desaparece
                appMessageEl.classList.remove('error'); 
            }, duration);
        }


        // --- Lógica de Modo Oscuro/Claro ---
        function loadTheme() {
            const savedTheme = localStorage.getItem('theme');
            const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
            let currentTheme = savedTheme || (prefersDark ? 'dark' : 'light');

            if (currentTheme === 'dark') {
                htmlEl.classList.add('dark');
                sunIcon.classList.remove('hidden'); 
                moonIcon.classList.add('hidden');
            } else {
                htmlEl.classList.remove('dark');
                sunIcon.classList.add('hidden');
                moonIcon.classList.remove('hidden'); 
            }
        }

        function toggleTheme() {
            if (htmlEl.classList.contains('dark')) {
                htmlEl.classList.remove('dark');
                localStorage.setItem('theme', 'light');
            } else {
                htmlEl.classList.add('dark');
                localStorage.setItem('theme', 'dark');
            }
            loadTheme();
        }
        
        loadTheme();

        // --- Lógica de Conversión ---
        function convert() {
            const fromUnit = fromUnitEl.value;
            const toUnit = toUnitEl.value;
            const inputValue = parseFloat(inputValueEl.value) || 0;

            if (inputValue === 0) {
                outputValueEl.value = '0';
                return;
            }

            const valueInMeters = inputValue * factors[fromUnit];
            const outputValue = valueInMeters / factors[toUnit];
            // Mostrar hasta 6 decimales para precisión, pero sin arrastrar ceros finales innecesarios
            outputValueEl.value = parseFloat(outputValue.toFixed(6));
        }
        
        // --- Lógica de Enlace Compartible ---

        /**
         * Lee los parámetros de la URL y pre-llena los campos.
         */
        function readUrlParams() {
            try {
                const params = new URLSearchParams(window.location.search);
                const value = params.get('v');
                const fromUnit = params.get('f');
                const toUnit = params.get('t');
                let wasUpdated = false;

                if (value !== null && !isNaN(parseFloat(value))) {
                    inputValueEl.value = parseFloat(value);
                    wasUpdated = true;
                }
                if (fromUnit !== null && factors.hasOwnProperty(fromUnit)) {
                    fromUnitEl.value = fromUnit;
                    wasUpdated = true;
                }
                if (toUnit !== null && factors.hasOwnProperty(toUnit)) {
                    toUnitEl.value = toUnit;
                    wasUpdated = true;
                }
                if (wasUpdated) {
                    convert(); 
                }
            } catch (e) {
                console.error("Error al leer parámetros de la URL:", e);
            }
        }

        /**
         * Copia el texto dado al portapapeles y muestra un mensaje flotante.
         */
        function copyTextToClipboard(text, successMsg, errorMsg) {
            const tempTextArea = document.createElement('textarea');
            tempTextArea.value = text;
            tempTextArea.style.position = 'fixed';
            tempTextArea.style.opacity = 0;
            document.body.appendChild(tempTextArea);
            tempTextArea.select();
            
            try {
                const successful = document.execCommand('copy');
                if (successful) {
                    showAppMessage(successMsg, 2000, 'success');
                } else {
                    throw new Error("Copy command failed");
                }
            } catch (err) {
                showAppMessage(errorMsg, 3000, 'error');
                console.error('Error al copiar: ', err);
            }
            
            document.body.removeChild(tempTextArea);
        }


        /**
         * Genera el enlace compartible, lo muestra, lo copia,
         * y advierte si es un enlace local (file://).
         */
        function generateAndCopyShareLink() {
            const value = inputValueEl.value;
            const fromUnit = fromUnitEl.value;
            const toUnit = toUnitEl.value;

            // Comprueba si la app se está ejecutando como un archivo local
            if (window.location.protocol === 'file:') {
                showAppMessage(
                    '¡Error! Sube este archivo a un servidor (hosting) para poder compartir enlaces.', 
                    5000, 
                    'error' 
                );
                shareLinkInput.value = '¡Error! No se puede compartir un enlace local.';
                shareLinkContainer.classList.remove('hidden');
                return; // Detiene la ejecución
            }

            // Usa origin + pathname para una URL más limpia
            const baseUrl = window.location.origin + window.location.pathname; 
            const shareUrl = `${baseUrl}?v=${value}&f=${fromUnit}&t=${toUnit}`;
            
            shareLinkInput.value = shareUrl;
            shareLinkContainer.classList.remove('hidden');

            // Copiar el enlace automáticamente
            copyTextToClipboard(
                shareUrl, 
                '¡Enlace de conversión copiado!', 
                'Error al copiar el enlace.'
            );
        }

        // --- Lógica de Utilidad (Intercambio, Copia) ---
        function swapUnits() {
            const from = fromUnitEl.value;
            const to = toUnitEl.value;
            fromUnitEl.value = to;
            toUnitEl.value = from;
            convert();
        }

        function copyResultToClipboard() {
            const textToCopy = outputValueEl.value;
            copyTextToClipboard(
                textToCopy, 
                '¡Resultado copiado!', 
                'Error al copiar el resultado.'
            );
        }

        // --- Listeners de Eventos ---
        inputValueEl.addEventListener('input', convert);
        fromUnitEl.addEventListener('change', convert);
        toUnitEl.addEventListener('change', convert);
        swapButton.addEventListener('click', swapUnits);
        copyButton.addEventListener('click', copyResultToClipboard);
        themeToggle.addEventListener('click', toggleTheme);
        generateLinkButton.addEventListener('click', generateAndCopyShareLink);

        // Carga inicial
        convert();
        readUrlParams(); 
        
    </script>
</body>
</html>
