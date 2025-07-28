<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Basic Calculator</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Font - Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #e2e8f0; /* bg-gray-200 */
        }
        .calculator-grid {
            display: grid;
            grid-template-columns: repeat(4, 1fr); /* 4 columns, equal width */
            gap: 1rem; /* gap-4 */
        }
        .calculator-button {
            padding: 1.5rem; /* p-6 */
            font-size: 1.75rem; /* text-3xl */
            font-weight: 600; /* font-semibold */
            border-radius: 0.75rem; /* rounded-xl */
            cursor: pointer;
            transition: background-color 0.2s, transform 0.1s, box-shadow 0.2s;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1); /* shadow-md */
            display: flex;
            justify-content: center;
            align-items: center;
        }
        .calculator-button:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 10px rgba(0, 0, 0, 0.15);
        }
        .calculator-button:active {
            transform: translateY(0);
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        }

        /* Specific button colors */
        .btn-number {
            background-color: #cbd5e1; /* bg-gray-300 */
            color: #1a202c; /* text-gray-900 */
        }
        .btn-number:hover {
            background-color: #94a3b8; /* hover:bg-gray-400 */
        }
        .btn-operator {
            background-color: #60a5fa; /* bg-blue-400 */
            color: white;
        }
        .btn-operator:hover {
            background-color: #3b82f6; /* hover:bg-blue-500 */
        }
        .btn-clear {
            background-color: #ef4444; /* bg-red-500 */
            color: white;
        }
        .btn-clear:hover {
            background-color: #dc2626; /* hover:bg-red-600 */
        }
        .btn-equals {
            background-color: #22c55e; /* bg-green-500 */
            color: white;
            grid-column: span 2; /* Make it span two columns */
        }
        .btn-equals:hover {
            background-color: #16a34a; /* hover:bg-green-600 */
        }
        .btn-decimal {
            background-color: #cbd5e1; /* bg-gray-300 */
            color: #1a202c; /* text-gray-900 */
        }
        .btn-decimal:hover {
            background-color: #94a3b8; /* hover:bg-gray-400 */
        }

        /* Responsive adjustments */
        @media (max-width: 640px) {
            .calculator-container {
                width: 95%;
                padding: 1.5rem;
            }
            .calculator-button {
                padding: 1rem;
                font-size: 1.5rem;
            }
            .calculator-grid {
                gap: 0.75rem;
            }
        }
    </style>
</head>
<body>
    <div class="calculator-container bg-white p-8 rounded-2xl shadow-2xl w-full max-w-md border-4 border-blue-500">
        <!-- Calculator Display -->
        <input type="text" id="display" readonly value="0"
               class="w-full h-20 text-right text-5xl font-bold bg-gray-800 text-green-400 rounded-xl p-4 mb-6
                      border-2 border-gray-700 focus:outline-none focus:ring-2 focus:ring-blue-400
                      overflow-hidden whitespace-nowrap overflow-x-auto"
               style="direction: ltr;"> <!-- Ensure left-to-right text direction -->

        <!-- Calculator Buttons Grid -->
        <div class="calculator-grid">
            <button class="calculator-button btn-clear" data-value="C">C</button>
            <button class="calculator-button btn-operator" data-value="/">&divide;</button>
            <button class="calculator-button btn-operator" data-value="*">&times;</button>
            <button class="calculator-button btn-operator" data-value="-">-</button>

            <button class="calculator-button btn-number" data-value="7">7</button>
            <button class="calculator-button btn-number" data-value="8">8</button>
            <button class="calculator-button btn-number" data-value="9">9</button>
            <button class="calculator-button btn-operator" data-value="+">+</button>

            <button class="calculator-button btn-number" data-value="4">4</button>
            <button class="calculator-button btn-number" data-value="5">5</button>
            <button class="calculator-button btn-number" data-value="6">6</button>

            <button class="calculator-button btn-number" data-value="1">1</button>
            <button class="calculator-button btn-number" data-value="2">2</button>
            <button class="calculator-button btn-number" data-value="3">3</button>

            <button class="calculator-button btn-number" data-value="0">0</button>
            <button class="calculator-button btn-decimal" data-value=".">.</button>
            <button class="calculator-button btn-equals" data-value="=">=</button>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const display = document.getElementById('display');
            const buttons = document.querySelectorAll('.calculator-button');

            let currentInput = '0';
            let operator = null;
            let previousInput = '';
            let resetDisplay = false; // Flag to clear display after an operation or equals

            // Function to update the display
            function updateDisplay() {
                display.value = currentInput;
            }

            // Handle button clicks
            buttons.forEach(button => {
                button.addEventListener('click', () => {
                    const value = button.dataset.value;

                    if (value === 'C') {
                        // Clear all
                        currentInput = '0';
                        operator = null;
                        previousInput = '';
                        resetDisplay = false;
                    } else if (value === '=') {
                        if (operator && previousInput !== '') {
                            try {
                                // Evaluate the expression
                                // Using eval() for simplicity in this basic example.
                                // In a production app, use a safer math expression parser.
                                let expression = previousInput + operator + currentInput;
                                let result = eval(expression);

                                // Handle floating point inaccuracies
                                if (Number.isFinite(result) && Math.abs(result) < 1e-9) {
                                    result = 0; // Treat very small numbers as 0
                                } else if (Number.isFinite(result)) {
                                    result = parseFloat(result.toFixed(10)); // Limit decimals
                                }


                                currentInput = result.toString();
                                previousInput = '';
                                operator = null;
                                resetDisplay = true; // Reset display for next input
                            } catch (e) {
                                currentInput = 'Error';
                                previousInput = '';
                                operator = null;
                                resetDisplay = true;
                                console.error("Calculation Error:", e);
                            }
                        }
                    } else if (['+', '-', '*', '/'].includes(value)) {
                        // Handle operators
                        if (operator && !resetDisplay) {
                            // If there's an existing operator and we haven't just calculated,
                            // evaluate the current expression before applying the new operator.
                            try {
                                let expression = previousInput + operator + currentInput;
                                let result = eval(expression);
                                if (Number.isFinite(result) && Math.abs(result) < 1e-9) {
                                    result = 0;
                                } else if (Number.isFinite(result)) {
                                    result = parseFloat(result.toFixed(10));
                                }
                                previousInput = result.toString();
                            } catch (e) {
                                currentInput = 'Error';
                                previousInput = '';
                                operator = null;
                                resetDisplay = true;
                                console.error("Operator Chaining Error:", e);
                                updateDisplay();
                                return;
                            }
                        } else {
                            previousInput = currentInput;
                        }
                        operator = value;
                        resetDisplay = true; // Prepare to clear display for next number input
                    } else if (value === '.') {
                        // Handle decimal point
                        if (resetDisplay) {
                            currentInput = '0.'; // Start with 0. if display was reset
                            resetDisplay = false;
                        } else if (!currentInput.includes('.')) {
                            currentInput += '.';
                        }
                    } else {
                        // Handle numbers
                        if (currentInput === '0' || resetDisplay) {
                            currentInput = value;
                            resetDisplay = false;
                        } else {
                            currentInput += value;
                        }
                    }
                    updateDisplay();
                });
            });

            updateDisplay(); // Initialize display with '0'
        });
    </script>
</body>
</html>
