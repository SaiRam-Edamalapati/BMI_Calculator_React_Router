# Ex06 BMI Calculator
## Date: 24.3.2026

## AIM
To develop a responsive and interactive Body Mass Index (BMI) Calculator using React that allows users to input their height and weight, and calculates their BMI to categorize their health status (e.g., Underweight, Normal, Overweight, Obese).

## DESIGN STEPS

### STEP 1: Initialize React Project

<li>Create a new React app using create-react-app.</li>
<li>Install React Router using:</li>
npm install react-router-dom

### STEP 2: Set Up Routing

Create routing structure with react-router-dom:

<li>Home route (/) – Intro or Navigation</li>

<li>BMI Calculator route (/bmi)</li>

<li>Result route (/result)</li>

### STEP 3: Design the BMI Form Page

<li>Create a form to accept Height (in cm or m) and Weight (in kg).</li>

<li>On form submit, navigate to the result page with entered values via URL query params or context/state.</li>

## STEP 4: Handle Input Validation

<li>Check if height and weight are valid numbers.</li>

<li>Optionally, show error messages for invalid inputs.</li>

### STEP 5: Perform BMI Calculation

<li>In the result component:

<li>Extract height and weight from the route (URL or passed state).</li>

<li>Apply the BMI formula:</li>

![image](https://github.com/user-attachments/assets/ec785506-c96b-489e-8783-fb1a5d36101a)
​
 
<li>Convert height from cm to m if needed.</li></li>

### STEP 6: Display Result

<li>Show calculated BMI.</li>

<li>Show category based on BMI range:

<li>Underweight, Normal, Overweight, Obese, etc.</li></li>

### STEP 7: Navigation Options

<li>Provide a button to go back to the BMI form to calculate again.</li>

### STEP 8: Enhancements

<li>Add styling using CSS or Tailwind.</li>

## PROGRAM
# index.html
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BMI Calculator - Check Your Health Status</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .container {
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            padding: 40px;
            max-width: 500px;
            width: 100%;
        }

        .header {
            text-align: center;
            margin-bottom: 30px;
        }

        .header h1 {
            color: #333;
            font-size: 28px;
            margin-bottom: 8px;
        }

        .header p {
            color: #666;
            font-size: 14px;
        }

        .input-group {
            margin-bottom: 25px;
        }

        label {
            display: block;
            margin-bottom: 8px;
            color: #333;
            font-weight: 600;
            font-size: 14px;
        }

        input[type="number"] {
            width: 100%;
            padding: 12px 15px;
            border: 2px solid #e0e0e0;
            border-radius: 8px;
            font-size: 16px;
            transition: border-color 0.3s;
        }

        input[type="number"]:focus {
            outline: none;
            border-color: #667eea;
            box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
        }

        .button-group {
            display: flex;
            gap: 10px;
            margin-bottom: 30px;
        }

        button {
            flex: 1;
            padding: 12px;
            border: none;
            border-radius: 8px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s;
        }

        .btn-calculate {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }

        .btn-calculate:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(102, 126, 234, 0.3);
        }

        .btn-clear {
            background: #f0f0f0;
            color: #333;
        }

        .btn-clear:hover {
            background: #e0e0e0;
        }

        .error {
            background: #fee;
            color: #c33;
            padding: 12px;
            border-radius: 8px;
            margin-bottom: 20px;
            display: none;
            font-size: 14px;
        }

        .result-container {
            display: none;
            background: #f8f9fa;
            border-radius: 12px;
            padding: 25px;
            margin-bottom: 20px;
            border-left: 5px solid #667eea;
        }

        .result-container.show {
            display: block;
        }

        .bmi-value {
            font-size: 42px;
            font-weight: bold;
            color: #667eea;
            margin-bottom: 10px;
        }

        .bmi-category {
            font-size: 20px;
            font-weight: 600;
            margin-bottom: 15px;
            color: #333;
        }

        .bmi-description {
            color: #666;
            font-size: 14px;
            line-height: 1.6;
        }

        .category-underweight { border-left-color: #3498db !important; }
        .category-underweight .bmi-category { color: #3498db; }

        .category-normal { border-left-color: #2ecc71 !important; }
        .category-normal .bmi-category { color: #2ecc71; }

        .category-overweight { border-left-color: #f39c12 !important; }
        .category-overweight .bmi-category { color: #f39c12; }

        .category-obese { border-left-color: #e74c3c !important; }
        .category-obese .bmi-category { color: #e74c3c; }

        .info-section {
            background: #f0f4ff;
            padding: 20px;
            border-radius: 8px;
            margin-top: 20px;
            font-size: 13px;
            color: #555;
            line-height: 1.6;
        }

        .info-section h3 {
            color: #667eea;
            font-size: 14px;
            margin-bottom: 10px;
        }

        .bmi-ranges {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
            margin-top: 10px;
        }

        .range-item {
            background: white;
            padding: 10px;
            border-radius: 6px;
            font-size: 12px;
            text-align: center;
        }

        @media (max-width: 600px) {
            .container {
                padding: 25px;
            }

            .header h1 {
                font-size: 24px;
            }

            .bmi-value {
                font-size: 32px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>BMI Calculator</h1>
            <p>Calculate your Body Mass Index and check your health status</p>
        </div>

        <div class="error" id="error"></div>

        <div class="input-group">
            <label for="height">Height (cm)</label>
            <input type="number" id="height" placeholder="Enter your height in cm" min="50" max="300">
        </div>

        <div class="input-group">
            <label for="weight">Weight (kg)</label>
            <input type="number" id="weight" placeholder="Enter your weight in kg" min="20" max="500">
        </div>

        <div class="button-group">
            <button class="btn-calculate" onclick="calculateBMI()">Calculate BMI</button>
            <button class="btn-clear" onclick="clearFields()">Clear</button>
        </div>

        <div class="result-container" id="resultContainer">
            <div class="bmi-value" id="bmiValue"></div>
            <div class="bmi-category" id="bmiCategory"></div>
            <div class="bmi-description" id="bmiDescription"></div>
        </div>

        <div class="info-section">
            <h3>BMI Categories</h3>
            <div class="bmi-ranges">
                <div class="range-item"><strong style="color: #3498db;">Underweight</strong><br>&lt; 18.5</div>
                <div class="range-item"><strong style="color: #2ecc71;">Normal</strong><br>18.5 - 24.9</div>
                <div class="range-item"><strong style="color: #f39c12;">Overweight</strong><br>25 - 29.9</div>
                <div class="range-item"><strong style="color: #e74c3c;">Obese</strong><br>≥ 30</div>
            </div>
        </div>
    </div>

    <script>
        function calculateBMI() {
            const heightInput = document.getElementById('height');
            const weightInput = document.getElementById('weight');
            const errorDiv = document.getElementById('error');
            const resultContainer = document.getElementById('resultContainer');

            // Clear previous error
            errorDiv.style.display = 'none';
            errorDiv.textContent = '';

            // Validation
            const height = parseFloat(heightInput.value);
            const weight = parseFloat(weightInput.value);

            if (!height || height <= 0) {
                showError('Please enter a valid height');
                return;
            }
            if (!weight || weight <= 0) {
                showError('Please enter a valid weight');
                return;
            }

            // Calculate BMI
            const heightInMeters = height / 100;
            const bmi = weight / (heightInMeters * heightInMeters);

            // Determine category and description
            let category = '';
            let categoryClass = '';
            let description = '';

            if (bmi < 18.5) {
                category = 'Underweight';
                categoryClass = 'category-underweight';
                description = 'You may need to gain weight. Consider consulting with a healthcare provider.';
            } else if (bmi < 25) {
                category = 'Normal Weight';
                categoryClass = 'category-normal';
                description = 'Great! You are at a healthy weight. Keep up the good lifestyle habits!';
            } else if (bmi < 30) {
                category = 'Overweight';
                categoryClass = 'category-overweight';
                description = 'You may want to consider a healthier diet and exercise routine.';
            } else {
                category = 'Obese';
                categoryClass = 'category-obese';
                description = 'Please consult with a healthcare provider for personalized advice.';
            }

            // Display result
            resultContainer.className = 'result-container show ' + categoryClass;
            document.getElementById('bmiValue').textContent = bmi.toFixed(1);
            document.getElementById('bmiCategory').textContent = category;
            document.getElementById('bmiDescription').textContent = description;

            // Provide voice feedback
            speakResult(bmi, category);
        }

        function speakResult(bmi, category) {
            if ('speechSynthesis' in window) {
                const msg = new SpeechSynthesisUtterance(`Your BMI is ${bmi.toFixed(1)} and you are ${category}`);
                msg.rate = 1;
                window.speechSynthesis.speak(msg);
            }
        }

        function clearFields() {
            document.getElementById('height').value = '';
            document.getElementById('weight').value = '';
            document.getElementById('resultContainer').classList.remove('show');
            document.getElementById('error').style.display = 'none';
            document.getElementById('height').focus();
        }

        function showError(message) {
            const errorDiv = document.getElementById('error');
            errorDiv.textContent = message;
            errorDiv.style.display = 'block';
        }

        // Allow Enter key to calculate
        document.getElementById('weight').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') calculateBMI();
        });

        document.getElementById('height').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') calculateBMI();
        });
    </script>
</body>
</html>
```


## OUTPUT

<img width="1904" height="908" alt="image" src="https://github.com/user-attachments/assets/c9ad1d8d-87fc-422f-a526-ded70c3ce37c" />


<img width="1177" height="911" alt="image" src="https://github.com/user-attachments/assets/856bedfe-e6b2-4657-b94b-26a7fff4f612" />

## RESULT
The BMI Calculator successfully takes user input for height and weight, performs the BMI calculation in real-time using React state and event handling, and displays the BMI value along with the corresponding health category.
