<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NVK English Dictionary</title>
    <style>
        /* Reset CSS */
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: Arial, sans-serif;
        }

        /* Galaxy background styling */
        body {
            display: flex;
            align-items: center;
            justify-content: center;
            min-height: 100vh;
            background: radial-gradient(circle at bottom, #1c1c3c, #000000) no-repeat center center fixed;
            color: #333;
            overflow: hidden;
        }

        /* Adding galaxy-like star effects */
        body::before {
            content: "";
            position: absolute;
            width: 100vw;
            height: 100vh;
            background: radial-gradient(circle, rgba(255,255,255,0.2) 1px, transparent 1px),
                        radial-gradient(circle, rgba(255,255,255,0.1) 1px, transparent 1px);
            background-size: 10px 10px, 5px 5px;
            opacity: 0.5;
            z-index: 0;
        }

        /* Background text watermark styling */
        .background-text {
            position: absolute;
            font-size: 200px;
            color: rgba(200, 200, 200, 0.2); /* Light gray and transparent */
            transform: rotate(-45deg);
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%) rotate(-45deg);
            z-index: 1;
        }

        /* Additional NVK text for harmony */
        .background-nvk {
            position: absolute;
            font-size: 80px;
            color: rgba(255, 255, 255, 0.1); /* Very light gray and transparent */
            z-index: 0;
            user-select: none; /* Prevent text selection */
        }

        .nvk1 {
            top: 10%;
            left: 5%;
            transform: translate(-50%, -50%) rotate(-30deg);
        }

        .nvk2 {
            top: 30%;
            right: 10%;
            transform: translate(50%, -50%) rotate(30deg);
        }

        .nvk3 {
            bottom: 20%;
            left: 15%;
            transform: translate(-50%, 50%) rotate(15deg);
        }

        /* Search container styling */
        .search-container {
            position: relative;
            text-align: center;
            max-width: 600px;
            width: 100%;
            padding: 30px;
            border-radius: 10px;
            background-color: rgba(255, 255, 255, 0.9);
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.3);
            z-index: 2;
        }

        h1 {
            font-size: 28px;
            margin-bottom: 20px;
            color: #333;
        }

        .search-input {
            width: 70%;
            padding: 12px;
            font-size: 16px;
            border: 2px solid #ddd;
            border-radius: 5px;
            outline: none;
            transition: all 0.3s ease;
        }

        .search-input:focus {
            border-color: #007bff;
        }

        .search-button {
            padding: 12px 20px;
            font-size: 16px;
            background-color: #007bff;
            color: #fff;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: all 0.3s ease;
            margin-left: 10px;
        }

        .search-button:hover {
            background-color: #0056b3;
        }

        /* Styling for the results section */
        .result {
            margin-top: 20px;
            font-size: 18px;
            text-align: left;
            color: #333;
        }

        .definition {
            font-size: 16px;
            color: #555;
            margin-top: 8px;
        }

        .example {
            font-size: 14px;
            color: #777;
            margin-top: 4px;
            font-style: italic;
        }

        /* Error styling */
        .error {
            color: red;
            font-size: 16px;
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <!-- Watermark Background -->
    <div class="background-text">NVK</div>

    <!-- Additional NVK Backgrounds -->
    <div class="background-nvk nvk1">NVK</div>
    <div class="background-nvk nvk2">NVK</div>
    <div class="background-nvk nvk3">NVK</div>

    <!-- Search Container -->
    <div class="search-container">
        <h1>NVK English Dictionary</h1>
        <input type="text" id="searchInput" class="search-input" placeholder="Enter a word...">
        <button class="search-button" onclick="searchWord()">Search</button>
        <div id="result" class="result"></div>
        <div id="error" class="error"></div>
    </div>

    <script>
        // Function to search word when pressing Enter key
        document.getElementById("searchInput").addEventListener("keypress", function(event) {
            if (event.key === "Enter") {
                searchWord();
            }
        });

        async function searchWord() {
            const word = document.getElementById("searchInput").value.trim();
            const resultDiv = document.getElementById("result");
            const errorDiv = document.getElementById("error");

            resultDiv.innerHTML = "";  // Clear previous results
            errorDiv.textContent = ""; // Clear previous errors

            if (!word) {
                errorDiv.textContent = "Please enter a word to search.";
                return;
            }

            try {
                // Fetch data from Dictionary API
                const response = await fetch(`https://api.dictionaryapi.dev/api/v2/entries/en/${word}`);
                if (!response.ok) throw new Error("Word not found");

                const data = await response.json();

                // Display the word and its meanings
                resultDiv.innerHTML = `<h2>${word}</h2>`;
                data[0].meanings.forEach(meaning => {
                    const partOfSpeech = meaning.partOfSpeech;
                    const definition = meaning.definitions[0].definition;
                    const example = meaning.definitions[0].example;

                    resultDiv.innerHTML += `
                        <div>
                            <strong>${partOfSpeech}</strong>: <span class="definition">${definition}</span>
                            ${example ? `<div class="example">Example: "${example}"</div>` : ""}
                        </div>`;
                });
            } catch (error) {
                errorDiv.textContent = "Word not found. Please try another word.";
            }
        }
    </script>
</body>
</html>
