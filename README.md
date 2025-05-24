# eco-footprint-calculator

Creating an eco-footprint calculator as a web application requires multiple components. For simplicity, let's look at a basic version using Python and the Flask framework. This example won't cover a database or user authentication (which you might want to add for a production-level application) but will include basic energy consumption, travel habits, and waste production calculations.

### Prerequisites

Before running the program, you need to install Flask:

```bash
pip install Flask
```

Here's a basic structure:

```python
from flask import Flask, render_template, request

app = Flask(__name__)

@app.route('/')
def home():
    return render_template('index.html')

@app.route('/calculate', methods=['POST'])
def calculate_footprint():
    try:
        # Get form data
        energy_consumption = float(request.form.get('energy', '0'))
        travel_miles = float(request.form.get('travel', '0'))
        waste_production = float(request.form.get('waste', '0'))

        # Basic calculations (placeholder values)
        carbon_from_energy = energy_consumption * 0.233  # kg CO2 per kWh
        carbon_from_travel = travel_miles * 0.271  # kg CO2 per mile driven
        carbon_from_waste = waste_production * 0.9  # kg CO2 per kg waste

        total_carbon = carbon_from_energy + carbon_from_travel + carbon_from_waste

        # Output some recommendations based on the total carbon footprint
        recommendations = []
        if energy_consumption > 100:
            recommendations.append('Consider switching to energy-saving appliances.')
        if travel_miles > 1000:
            recommendations.append('Consider using public transport or carpooling.')
        if waste_production > 50:
            recommendations.append('Consider recycling more to reduce waste.')

        return render_template('result.html', total_carbon=total_carbon, recommendations=recommendations)
    
    except ValueError as ve:
        return f"Error: Invalid input. Please ensure all fields are filled with numbers. {ve}"
    except Exception as e:
        return f"Unexpected error: {e}"

if __name__ == '__main__':
    app.run(debug=True)
```

### HTML Templates

Create a folder named `templates` in the same directory as your Python script and add two HTML files.

#### `index.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Eco Footprint Calculator</title>
</head>
<body>
    <h1>Eco Footprint Calculator</h1>
    <form action="/calculate" method="POST">
        <label for="energy">Energy Consumption (kWh):</label>
        <input type="text" id="energy" name="energy" required><br><br>

        <label for="travel">Travel Distance (miles):</label>
        <input type="text" id="travel" name="travel" required><br><br>

        <label for="waste">Waste Production (kg):</label>
        <input type="text" id="waste" name="waste" required><br><br>

        <input type="submit" value="Calculate">
    </form>
</body>
</html>
```

#### `result.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Results</title>
</head>
<body>
    <h1>Your Carbon Footprint</h1>
    <p>Total Carbon Footprint: {{ total_carbon }} kg CO2</p>
    <h2>Recommendations:</h2>
    <ul>
        {% for recommendation in recommendations %}
            <li>{{ recommendation }}</li>
        {% endfor %}
    </ul>
    <a href="/">Go back</a>
</body>
</html>
```

### How the Application Works

1. **Home Page (`index.html`)**: A simple form collects data about energy consumption, travel, and waste production.

2. **Calculation (`/calculate`)**: The form data is sent to the `/calculate` route where it processes the input to calculate the carbon footprint and generate recommendations accordingly. Error handling is in place to inform users of invalid inputs.

3. **Results Page (`result.html`)**: Shows the calculated carbon footprint and provides recommendations to reduce it.

### Run the Application

To run the application, simply execute your Python file in the terminal:

```bash
python your_script_name.py
```

Then, go to `http://127.0.0.1:5000` in your web browser to interact with the application.

This basic setup can be expanded with real databases, sophisticated calculation models, and enhanced UI as you develop further.