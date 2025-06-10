<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Date to Alpha-Digit Converter</title>
    <!-- Tailwind CSS CDN for easy styling -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Custom font */
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f3f4f6; /* Light gray background */
        }
        /* Simple spinner for loading state */
        .spinner {
            border: 4px solid rgba(255, 255, 255, 0.3);
            border-top: 4px solid #fff;
            border-radius: 50%;
            width: 20px;
            height: 20px;
            animation: spin 1s linear infinite;
        }
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
    </style>
</head>
<body class="flex items-center justify-center min-h-screen p-4">
    <div class="bg-white p-8 rounded-xl shadow-lg w-full max-w-md border border-gray-200">
        <h1 class="text-3xl font-bold text-center text-gray-800 mb-6">
            Date Alpha Converter
        </h1>

        <p class="text-sm text-gray-600 mb-6 text-center">
            Enter a date in DD-MM-YY format to convert digits to alphabets.
        </p>

        <!-- Digit to Alpha Mapping Table -->
        <div class="mb-6 bg-blue-50 p-4 rounded-lg border border-blue-200">
            <h2 class="text-lg font-semibold text-blue-800 mb-3">Mapping:</h2>
            <div class="grid grid-cols-5 gap-2 text-sm text-blue-700">
                <div class="font-medium">0 = A</div>
                <div class="font-medium">1 = B</div>
                <div class="font-medium">2 = C</div>
                <div class="font-medium">3 = D</div>
                <div class="font-medium">4 = E</div>
                <div class="font-medium">5 = F</div>
                <div class="font-medium">6 = G</div>
                <div class="font-medium">7 = H</div>
                <div class="font-medium">8 = I</div>
                <div class="font-medium">9 = J</div>
            </div>
        </div>

        <div class="mb-6">
            <label for="dateInput" class="block text-gray-700 text-sm font-medium mb-2">
                Enter Date (DD-MM-YY):
            </label>
            <input
                type="text"
                id="dateInput"
                class="shadow-sm appearance-none border border-gray-300 rounded-lg w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-transparent transition duration-200"
                placeholder="e.g., 21-05-25"
                maxlength="8"
            >
            <p id="inputError" class="text-red-600 text-xs mt-1 hidden">
                Please enter a valid date in DD-MM-YY format (e.g., 01-01-23).
            </p>
        </div>

        <button
            id="convertButton"
            class="w-full bg-blue-600 hover:bg-blue-700 text-white font-bold py-3 px-4 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-opacity-75 transition duration-200 transform hover:scale-105 shadow-md mb-4"
        >
            Convert Date
        </button>

        <!-- LLM-powered feature button for system explanation -->
        <button
            id="explainButton"
            class="w-full bg-purple-600 hover:bg-purple-700 text-white font-bold py-3 px-4 rounded-lg focus:outline-none focus:ring-2 focus:ring-purple-500 focus:ring-opacity-75 transition duration-200 transform hover:scale-105 shadow-md flex items-center justify-center mb-4"
        >
            <span id="explainButtonText">✨ Explain System ✨</span>
            <div id="explainLoadingSpinner" class="spinner ml-2 hidden"></div>
        </button>


        <div id="resultContainer" class="mt-8 p-6 bg-green-50 border border-green-200 rounded-lg text-center hidden">
            <h2 class="text-xl font-semibold text-green-800 mb-3">Converted Alpha-Digit Date:</h2>
            <p id="convertedDate" class="text-2xl font-extrabold text-green-900 break-words mb-4"></p>
            <div class="flex flex-col sm:flex-row items-center justify-center gap-3">
                <button
                    id="copyButton"
                    class="inline-flex items-center bg-green-600 hover:bg-green-700 text-white font-medium py-2 px-4 rounded-lg focus:outline-none focus:ring-2 focus:ring-green-500 focus:ring-opacity-75 transition duration-200 text-sm"
                >
                    <svg class="w-4 h-4 mr-2" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8 5H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-2M8 5a2 2 0 002 2h2a2 2 0 002-2M8 5a2 2 0 012-2h2a2 2 0 012 2m0 0h-2M12 15h.01M12 12h.01M12 9h.01"></path>
                    </svg>
                    Copy to Clipboard
                </button>
                <span id="copyMessage" class="text-sm text-green-700 hidden">Copied!</span>

                <!-- New LLM-powered feature button for specific explanation -->
                <button
                    id="explainConvertedButton"
                    class="inline-flex items-center bg-indigo-600 hover:bg-indigo-700 text-white font-medium py-2 px-4 rounded-lg focus:outline-none focus:ring-2 focus:ring-indigo-500 focus:ring-opacity-75 transition duration-200 text-sm"
                >
                    <span id="explainConvertedButtonText">✨ Explain Converted Date ✨</span>
                    <div id="explainConvertedLoadingSpinner" class="spinner ml-2 hidden"></div>
                </button>
            </div>
        </div>

        <div id="messageBox" class="fixed inset-0 bg-gray-600 bg-opacity-50 flex items-center justify-center p-4 z-50 hidden">
            <div class="bg-white p-6 rounded-lg shadow-xl text-center max-w-sm w-full border border-gray-300">
                <h3 id="messageBoxTitle" class="text-lg font-bold text-gray-800 mb-3"></h3>
                <p id="messageBoxContent" class="text-gray-700 mb-4 whitespace-pre-wrap text-left"></p>
                <button id="messageBoxClose" class="bg-blue-500 hover:bg-blue-600 text-white py-2 px-4 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-400">
                    OK
                </button>
            </div>
        </div>

    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const dateInput = document.getElementById('dateInput');
            const convertButton = document.getElementById('convertButton');
            const convertedDateDisplay = document.getElementById('convertedDate');
            const resultContainer = document.getElementById('resultContainer');
            const copyButton = document.getElementById('copyButton');
            const copyMessage = document.getElementById('copyMessage');
            const inputError = document.getElementById('inputError');

            // LLM Feature Elements for System Explanation
            const explainButton = document.getElementById('explainButton');
            const explainButtonText = document.getElementById('explainButtonText');
            const explainLoadingSpinner = document.getElementById('explainLoadingSpinner');

            // LLM Feature Elements for Converted Date Explanation
            const explainConvertedButton = document.getElementById('explainConvertedButton');
            const explainConvertedButtonText = document.getElementById('explainConvertedButtonText');
            const explainConvertedLoadingSpinner = document.getElementById('explainConvertedLoadingSpinner');

            // Message Box Elements
            const messageBox = document.getElementById('messageBox');
            const messageBoxTitle = document.getElementById('messageBoxTitle');
            const messageBoxContent = document.getElementById('messageBoxContent');
            const messageBoxClose = document.getElementById('messageBoxClose');

            // Mapping object for digits to alphabets
            const alphaMap = {
                '0': 'A', '1': 'B', '2': 'C', '3': 'D', '4': 'E',
                '5': 'F', '6': 'G', '7': 'H', '8': 'I', '9': 'J'
            };

            // Reverse mapping for alpha to digits (useful for explanation)
            const reverseAlphaMap = {
                'A': '0', 'B': '1', 'C': '2', 'D': '3', 'E': '4',
                'F': '5', 'G': '6', 'H': '7', 'I': '8', 'J': '9'
            };


            // Function to show custom message box
            function showMessageBox(title, content) {
                messageBoxTitle.textContent = title;
                messageBoxContent.textContent = content;
                messageBox.classList.remove('hidden');
            }

            // Event listener for message box close button
            messageBoxClose.addEventListener('click', () => {
                messageBox.classList.add('hidden');
            });

            // Function to validate date format (DD-MM-YY)
            function isValidDateFormat(dateStr) {
                // Regex to match DD-MM-YY format (e.g., 01-01-23)
                const regex = /^\d{2}-\d{2}-\d{2}$/;
                return regex.test(dateStr);
            }

            // Main conversion function
            function convertToAlphaDigit() {
                const inputDate = dateInput.value.trim();

                // Validate input format
                if (!isValidDateFormat(inputDate)) {
                    inputError.classList.remove('hidden');
                    resultContainer.classList.add('hidden'); // Hide result if input is invalid
                    return; // Stop execution if invalid
                } else {
                    inputError.classList.add('hidden'); // Hide error if format is valid
                }

                let converted = '';
                for (let i = 0; i < inputDate.length; i++) {
                    const char = inputDate[i];
                    // If the character is a digit, convert it using the map
                    if (alphaMap[char]) {
                        converted += alphaMap[char];
                    } else {
                        // Otherwise, keep the character as is (e.g., hyphen)
                        converted += char;
                    }
                }

                convertedDateDisplay.textContent = converted;
                resultContainer.classList.remove('hidden'); // Show the result container
                copyMessage.classList.add('hidden'); // Hide "Copied!" message on new conversion
            }

            // Event listener for the Convert button
            convertButton.addEventListener('click', convertToAlphaDigit);

            // Allow pressing Enter in the input field to trigger conversion
            dateInput.addEventListener('keypress', (event) => {
                if (event.key === 'Enter') {
                    convertButton.click();
                }
            });

            // Copy to clipboard functionality
            copyButton.addEventListener('click', () => {
                const textToCopy = convertedDateDisplay.textContent;
                if (textToCopy) {
                    try {
                        // Use execCommand for broader compatibility in iframes
                        const tempInput = document.createElement('textarea');
                        tempInput.value = textToCopy;
                        document.body.appendChild(tempInput);
                        tempInput.select();
                        document.execCommand('copy');
                        document.body.removeChild(tempInput);

                        copyMessage.classList.remove('hidden'); // Show "Copied!" message
                        setTimeout(() => {
                            copyMessage.classList.add('hidden');
                        }, 2000); // Hide after 2 seconds
                    } catch (err) {
                        // Fallback for cases where execCommand might fail (unlikely in Canvas)
                        showMessageBox('Copy Failed', 'Could not copy text to clipboard automatically. Please copy it manually: ' + textToCopy);
                    }
                }
            });

            // LLM Feature: Explain System
            explainButton.addEventListener('click', async () => {
                // Show loading spinner
                explainButtonText.classList.add('hidden');
                explainLoadingSpinner.classList.remove('hidden');
                explainButton.disabled = true; // Disable button during loading

                const prompt = "Explain in 2-3 concise paragraphs the purpose and benefits of converting numerical dates (DD-MM-YY) into an alpha-digit format (0=A, 1=B, ..., 9=J) specifically for use by shop floor operators. Focus on how this system helps reduce confusion and improve accuracy.";

                let chatHistory = [];
                chatHistory.push({ role: "user", parts: [{ text: prompt }] });
                const payload = { contents: chatHistory };
                const apiKey = ""; // Canvas will automatically provide the API key at runtime

                try {
                    const apiUrl = https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey};
                    const response = await fetch(apiUrl, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(payload)
                    });

                    const result = await response.json();

                    if (result.candidates && result.candidates.length > 0 &&
                        result.candidates[0].content && result.candidates[0].content.parts &&
                        result.candidates[0].content.parts.length > 0) {
                        const explanation = result.candidates[0].content.parts[0].text;
                        showMessageBox('Understanding the New System', explanation);
                    } else {
                        showMessageBox('Error', 'Could not get an explanation. Please try again.');
                    }
                } catch (error) {
                    console.error('Error calling Gemini API for system explanation:', error);
                    showMessageBox('Error', 'Failed to connect to the explanation service. Please check your network and try again.');
                } finally {
                    // Hide loading spinner and re-enable button
                    explainButtonText.classList.remove('hidden');
                    explainLoadingSpinner.classList.add('hidden');
                    explainButton.disabled = false;
                }
            });

            // LLM Feature: Explain Converted Date
            explainConvertedButton.addEventListener('click', async () => {
                const convertedDate = convertedDateDisplay.textContent;

                if (!convertedDate) {
                    showMessageBox('No Date Converted', 'Please convert a date first to get an explanation.');
                    return;
                }

                // Show loading spinner
                explainConvertedButtonText.classList.add('hidden');
                explainConvertedLoadingSpinner.classList.remove('hidden');
                explainConvertedButton.disabled = true; // Disable button during loading

                // Construct the prompt for the LLM
                // First, reverse convert the alpha-digit date back to numerical for context
                let numericalDate = '';
                for (let i = 0; i < convertedDate.length; i++) {
                    const char = convertedDate[i];
                    if (reverseAlphaMap[char]) {
                        numericalDate += reverseAlphaMap[char];
                    } else {
                        numericalDate += char; // Keep hyphens as is
                    }
                }

                const prompt = `The original numerical date was "${numericalDate}" which was converted to "${convertedDate}" using a system where 0=A, 1=B, 2=C, 3=D, 4=E, 5=F, 6=G, 7=H, 8=I, 9=J.

                Please explain how the alpha-digit date "${convertedDate}" was derived from the numerical date "${numericalDate}". Break down the conversion for each part (day, month, year) and clearly show which numerical digit corresponds to which alphabet. Keep the explanation clear and concise, suitable for a shop floor operator.`;

                let chatHistory = [];
                chatHistory.push({ role: "user", parts: [{ text: prompt }] });
                const payload = { contents: chatHistory };
                const apiKey = ""; // Canvas will automatically provide the API key at runtime

                try {
                    const apiUrl = https://generativelanguage.googleapis.com/v1beta/models/gemini-2.0-flash:generateContent?key=${apiKey};
                    const response = await fetch(apiUrl, {
                        method: 'POST',
                        headers: { 'Content-Type': 'application/json' },
                        body: JSON.stringify(payload)
                    });

                    const result = await response.json();

                    if (result.candidates && result.candidates.length > 0 &&
                        result.candidates[0].content && result.candidates[0].content.parts &&
                        result.candidates[0].content.parts.length > 0) {
                        const explanation = result.candidates[0].content.parts[0].text;
                        showMessageBox(Explanation for ${convertedDate}, explanation);
                    } else {
                        showMessageBox('Error', 'Could not get an explanation for the converted date. Please try again.');
                    }
                } catch (error) {
                    console.error('Error calling Gemini API for converted date explanation:', error);
                    showMessageBox('Error', 'Failed to connect to the explanation service. Please check your network and try again.');
                } finally {
                    // Hide loading spinner and re-enable button
                    explainConvertedButtonText.classList.remove('hidden');
                    explainConvertedLoadingSpinner.classList.add('hidden');
                    explainConvertedButton.disabled = false;
                }
            });
        });
    </script>
</body>
</html>
