# PrimerTrabajo
Actividad
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Contador de Estudiantes por Materia</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 600px;
            margin: 50px auto;
            padding: 20px;
            background-color: #f5f5f5;
        }
       
        .container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
       
        h1 {
            color: #333;
            text-align: center;
            margin-bottom: 30px;
        }
       
        .form-group {
            margin-bottom: 20px;
        }
       
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
            color: #555;
        }
       
        input[type="number"] {
            width: 100%;
            padding: 10px;
            border: 2px solid #ddd;
            border-radius: 5px;
            font-size: 16px;
            box-sizing: border-box;
        }
       
        input[type="number"]:focus {
            border-color: #4CAF50;
            outline: none;
        }
       
        input.error {
            border-color: #f44336;
        }
       
        .error-message {
            color: #f44336;
            font-size: 14px;
            margin-top: 5px;
            display: none;
        }
       
        button {
            background-color: #4CAF50;
            color: white;
            padding: 12px 30px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            font-size: 16px;
            width: 100%;
            margin-top: 10px;
        }
       
        button:hover {
            background-color: #45a049;
        }
       
        button:disabled {
            background-color: #cccccc;
            cursor: not-allowed;
        }
       
        .result {
            margin-top: 20px;
            padding: 15px;
            border-radius: 5px;
            display: none;
        }
       
        .result.success {
            background-color: #d4edda;
            border: 1px solid #c3e6cb;
            color: #155724;
        }
       
        .result.tie {
            background-color: #fff3cd;
            border: 1px solid #ffeaa7;
            color: #856404;
        }
       
        .info {
            background-color: #e3f2fd;
            border: 1px solid #bbdefb;
            color: #1565c0;
            padding: 15px;
            border-radius: 5px;
            margin-bottom: 20px;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>📚 Contador de Estudiantes por Materia</h1>
       
        <div class="info">
            <strong>Instrucciones:</strong> Ingresa la cantidad de estudiantes para cada materia y presiona "Validar" para ver cuál tiene más estudiantes.
        </div>
       
        <form id="studentForm">
            <div class="form-group">
                <label for="materia1">Cantidad de estudiantes en Matemáticas:</label>
                <input type="number" id="materia1" name="materia1" min="0" max="100" step="1">
                <div class="error-message" id="error1"></div>
            </div>
           
            <div class="form-group">
                <label for="materia2">Cantidad de estudiantes en Ciencias:</label>
                <input type="number" id="materia2" name="materia2" min="0" max="100" step="1">
                <div class="error-message" id="error2"></div>
            </div>
           
            <button type="submit" id="validateBtn">Validar</button>
        </form>
       
        <div class="result" id="result"></div>
    </div>

    <script>
        document.getElementById('studentForm').addEventListener('submit', function(e) {
            e.preventDefault();
            validateAndCompare();
        });

        function validateAndCompare() {
            const materia1Input = document.getElementById('materia1');
            const materia2Input = document.getElementById('materia2');
            const result = document.getElementById('result');
           
            // Limpiar errores previos
            clearErrors();
           
            const materia1Value = materia1Input.value.trim();
            const materia2Value = materia2Input.value.trim();
           
            let hasErrors = false;
           
            // Validaciones para Materia 1
            if (!validateInput(materia1Value, materia1Input, 'error1')) {
                hasErrors = true;
            }
           
            // Validaciones para Materia 2
            if (!validateInput(materia2Value, materia2Input, 'error2')) {
                hasErrors = true;
            }
           
            // Si hay errores, no continuar
            if (hasErrors) {
                result.style.display = 'none';
                return;
            }
           
            // Convertir a números
            const estudiantes1 = parseInt(materia1Value);
            const estudiantes2 = parseInt(materia2Value);
           
            // Comparar y mostrar resultado
            showResult(estudiantes1, estudiantes2);
        }
       
        function validateInput(value, inputElement, errorId) {
            const errorElement = document.getElementById(errorId);
           
            // Verificar si está vacío
            if (value === '') {
                showError(inputElement, errorElement, 'Este campo es obligatorio');
                return false;
            }
           
            // Verificar si es un número
            if (isNaN(value) || !Number.isInteger(parseFloat(value))) {
                showError(inputElement, errorElement, 'Debe ser un número entero');
                return false;
            }
           
            const numValue = parseInt(value);
           
            // Verificar si es negativo
            if (numValue < 0) {
                showError(inputElement, errorElement, 'No se permiten valores negativos');
                return false;
            }
           
            // Verificar límite máximo
            if (numValue > 100) {
                showError(inputElement, errorElement, 'El valor máximo es 100');
                return false;
            }
           
            return true;
        }
       
        function showError(inputElement, errorElement, message) {
            inputElement.classList.add('error');
            errorElement.textContent = message;
            errorElement.style.display = 'block';
        }
       
        function clearErrors() {
            const inputs = document.querySelectorAll('input[type="number"]');
            const errors = document.querySelectorAll('.error-message');
           
            inputs.forEach(input => input.classList.remove('error'));
            errors.forEach(error => {
                error.style.display = 'none';
                error.textContent = '';
            });
        }
       
        function showResult(estudiantes1, estudiantes2) {
            const result = document.getElementById('result');
            let message = '';
            let className = '';
           
            if (estudiantes1 > estudiantes2) {
                message = `🎉 <strong>Matemáticas</strong> tiene más estudiantes con <strong>${estudiantes1}</strong> estudiantes, comparado con <strong>${estudiantes2}</strong> en Ciencias.`;
                className = 'success';
            } else if (estudiantes2 > estudiantes1) {
                message = `🎉 <strong>Ciencias</strong> tiene más estudiantes con <strong>${estudiantes2}</strong> estudiantes, comparado con <strong>${estudiantes1}</strong> en Matemáticas.`;
                className = 'success';
            } else {
                message = `🤝 ¡Empate! Ambas materias tienen la misma cantidad de estudiantes: <strong>${estudiantes1}</strong>.`;
                className = 'tie';
            }
           
            result.innerHTML = message;
            result.className = `result ${className}`;
            result.style.display = 'block';
        }
       
        // Validación en tiempo real
        document.getElementById('materia1').addEventListener('input', function() {
            if (this.classList.contains('error')) {
                clearErrors();
            }
        });
       
        document.getElementById('materia2').addEventListener('input', function() {
            if (this.classList.contains('error')) {
                clearErrors();
            }
        });
    </script>
</body>
</html>
