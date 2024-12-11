<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Secret Santa Picker</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 20px;
        }
        h1 {
            color: #4CAF50;
        }
        .container {
            max-width: 600px;
            margin: auto;
            padding: 20px;
            border: 1px solid #ccc;
            border-radius: 10px;
        }
        #result {
            margin-top: 20px;
            font-size: 1.2em;
        }
        .hidden {
            display: none;
        }
        button {
            margin: 10px;
            padding: 10px 20px;
            font-size: 1em;
            cursor: pointer;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
        }
        button:hover {
            background-color: #45a049;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Secret Santa Picker</h1>
        <p>Select a participant to get their Secret Santa!</p>
        <div id="picker">
            <input id="nameInput" type="text" placeholder="Enter your name" />
            <button onclick="pickSanta()">Pick Secret Santa</button>
        </div>
        <div id="result" class="hidden"></div>
        <button onclick="resetPicker()">Reset Picker</button>
    </div>

    <script>
        const names = [
            "justin", "kyle", "jaryl", "mika", "jones", "allinah", "yosei",
            "yuma", "kazuya", "nicole", "jun", "cj", "andre"
        ];
        let remaining = [...names];
        const assigned = {};

        function pickSanta() {
            const input = document.getElementById("nameInput").value.toLowerCase();
            const resultDiv = document.getElementById("result");

            if (!names.includes(input)) {
                resultDiv.innerHTML = `<p style="color:red;">Invalid name! Please enter a valid participant.</p>`;
                resultDiv.classList.remove("hidden");
                return;
            }

            if (assigned[input]) {
                resultDiv.innerHTML = `<p style="color:blue;">You already picked! Your Secret Santa is <strong>${assigned[input]}</strong>.</p>`;
                resultDiv.classList.remove("hidden");
                return;
            }

            if (remaining.length === 1 && remaining.includes(input)) {
                resultDiv.innerHTML = `<p style="color:red;">Everyone has been assigned. Reset to start again!</p>`;
                resultDiv.classList.remove("hidden");
                return;
            }

            // Remove the picker from the remaining list
            const possibleChoices = remaining.filter(name => name !== input);
            if (possibleChoices.length === 0) {
                resultDiv.innerHTML = `<p style="color:red;">No one left to assign!</p>`;
                resultDiv.classList.remove("hidden");
                return;
            }

            const randomIndex = Math.floor(Math.random() * possibleChoices.length);
            const selected = possibleChoices[randomIndex];

            // Assign and remove from remaining
            assigned[input] = selected;
            remaining = remaining.filter(name => name !== selected);

            resultDiv.innerHTML = `<p style="color:green;">You picked <strong>${selected}</strong> as your Secret Santa!</p>`;
            resultDiv.classList.remove("hidden");
        }

        function resetPicker() {
            remaining = [...names];
            for (const key in assigned) delete assigned[key];
            document.getElementById("nameInput").value = "";
            document.getElementById("result").classList.add("hidden");
        }
    </script>
</body>
</html>
