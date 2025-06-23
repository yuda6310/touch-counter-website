<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aplikasi Penghitung Sentuhan</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts - Poppins -->
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700;800&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Poppins', sans-serif; /* Changed font to Poppins */
            /* Gradient background for aesthetic look */
            background: linear-gradient(135deg, #f0f4f8 0%, #d9e2ec 100%);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            overflow: hidden; /* Prevent scroll */
        }
        /* Main container for the app, defining its interactive area */
        .app-container {
            position: relative; /* Needed for absolute positioning of the touch area */
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: space-between; /* Distribute space between elements */
            padding: 2rem; /* Add padding inside the container */
            background-color: white;
            border-radius: 3rem; /* More rounded corners for aesthetic */
            box-shadow: 0 20px 50px rgba(0, 0, 0, 0.15); /* Stronger shadow */
            width: 90vw; /* Make it wider on mobile */
            max-width: 500px;
            height: 80vh; /* Make it taller */
            max-height: 700px;
            transition: all 0.3s ease-in-out;
        }

        /* The large touch area, covering most of the container */
        #touch-area {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%; /* Cover the entire container */
            cursor: pointer;
            z-index: 10; /* Below counter and reset button, but above background */
            background-color: transparent; /* Make it invisible but clickable */
            border-radius: 3rem; /* Match container border-radius */
            transition: transform 0.1s ease-out, box-shadow 0.1s ease-out;
            user-select: none; /* Prevent text selection on tap */
            -webkit-tap-highlight-color: transparent; /* Remove highlight on tap */
        }
        #touch-area:active {
            transform: scale(0.98); /* Slightly shrink on active/touch */
            box-shadow: inset 0 0 15px rgba(0, 0, 0, 0.1); /* Subtle inner shadow on active */
        }

        /* Ensure title, counter display, and reset button are on top of the touch area */
        .z-20 {
            z-index: 20;
        }

        /* Animation for the counter display when it updates */
        #counter-display {
            transition: transform 0.05s ease-out; /* Add subtle animation to counter text */
        }
        #counter-display.pop {
            transform: scale(1.1); /* Make it slightly larger temporarily */
        }

        /* Message box for alerts/notifications */
        .message-box {
            position: fixed;
            top: 20px;
            left: 50%;
            transform: translateX(-50%);
            background-color: #28a745; /* Green for success */
            color: white;
            padding: 10px 20px;
            border-radius: 8px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
            z-index: 1000;
            opacity: 0;
            transition: opacity 0.3s ease-in-out;
            font-size: 0.9rem;
        }
        .message-box.show {
            opacity: 1; /* Make it visible */
        }
    </style>
</head>
<body>
    <!-- Main container for the counter application -->
    <div class="app-container">

        <!-- The large, full-layer touch area for incrementing the counter -->
        <div id="touch-area"></div>

        <!-- Title of the application -->
        <h1 class="text-4xl font-bold text-gray-800 z-20 mb-auto mt-4">Ketuk Penghitung</h1>

        <!-- Counter display area, prominently displayed -->
        <div id="counter-display" class="text-8xl font-extrabold text-indigo-700 z-20 mb-auto mt-auto">
            0
        </div>

        <!-- Reset button, positioned at the bottom -->
        <button id="reset-button" class="w-full py-4 bg-red-500 text-white text-xl font-semibold rounded-xl shadow-lg hover:bg-red-600 focus:outline-none focus:ring-4 focus:ring-red-300 focus:ring-opacity-75 transition-colors duration-200 z-20 mt-auto mb-4">
            Reset
        </button>
    </div>

    <!-- Message Box Element for displaying notifications -->
    <div id="message-box" class="message-box"></div>

    <script>
        // Get references to DOM elements
        const counterDisplay = document.getElementById('counter-display');
        const touchArea = document.getElementById('touch-area'); // The new full-layer touch area
        const resetButton = document.getElementById('reset-button');
        const messageBox = document.getElementById('message-box');

        // Initialize counter variable
        let count = 0;

        // Function to show a temporary message (e.g., "Reset!")
        function showMessage(message, type = 'success') {
            messageBox.textContent = message;
            messageBox.className = 'message-box show'; // Reset classes and make it visible
            if (type === 'error') {
                messageBox.style.backgroundColor = '#dc3545'; // Red for errors
            } else {
                messageBox.style.backgroundColor = '#28a745'; // Green for success
            }
            setTimeout(() => {
                messageBox.classList.remove('show'); // Hide after 2 seconds
            }, 2000);
        }

        // Function to update the counter display and add a subtle animation
        function updateDisplay() {
            counterDisplay.textContent = count;
            // Add a "pop" class to trigger the scale animation
            counterDisplay.classList.add('pop');
            // Remove the "pop" class shortly after to reset for next animation
            setTimeout(() => {
                counterDisplay.classList.remove('pop');
            }, 100);
        }

        // Event listener for the large touch area to increment the counter
        touchArea.addEventListener('click', () => {
            count++; // Increment count
            updateDisplay(); // Update display with animation
        });

        // Event listener for the reset button
        resetButton.addEventListener('click', (event) => {
            // Prevent the click on the reset button from also triggering the touch area
            event.stopPropagation();
            if (count > 0) {
                count = 0; // Reset count to 0
                updateDisplay(); // Update display
                showMessage('Penghitung di-reset!', 'success'); // Show success message
            } else {
                showMessage('Penghitung sudah 0!', 'error'); // Show error message if already 0
            }
        });

        // Initialize display on page load
        updateDisplay();
    </script>
</body>
</html>



