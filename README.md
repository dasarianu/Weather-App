# Weather-App
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">

    <title>Weather App</title>

    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: Arial, sans-serif;
        }

        body {
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            background: linear-gradient(135deg, #4facfe, #00f2fe);
            padding: 20px;
        }

        .weather-app {
            width: 100%;
            max-width: 430px;
            background: white;
            padding: 30px;
            border-radius: 25px;
            text-align: center;
            box-shadow: 0 15px 40px rgba(0, 0, 0, 0.2);
        }

        .weather-app h1 {
            color: #222;
            margin-bottom: 25px;
        }

        .weather-app h1 span {
            color: #2196f3;
        }

        /* Search */

        .search-box {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
        }

        .search-box input {
            flex: 1;
            padding: 13px;
            border: 1px solid #ddd;
            border-radius: 10px;
            outline: none;
            font-size: 16px;
        }

        .search-box button {
            padding: 13px 18px;
            border: none;
            background: #2196f3;
            color: white;
            border-radius: 10px;
            cursor: pointer;
            font-weight: bold;
        }

        .search-box button:hover {
            background: #1976d2;
        }

        /* Weather */

        .weather-info {
            display: none;
        }

        .city {
            font-size: 27px;
            font-weight: bold;
            margin-top: 10px;
        }

        .local-time {
            color: #777;
            margin-top: 7px;
        }

        .weather-icon {
            width: 110px;
            margin: 15px auto 0;
        }

        .temperature {
            font-size: 60px;
            font-weight: bold;
            color: #222;
        }

        .description {
            font-size: 20px;
            color: #555;
            text-transform: capitalize;
            margin-bottom: 25px;
        }

        /* Weather Details */

        .details {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 12px;
        }

        .detail-box {
            background: #f2f7fc;
            padding: 18px;
            border-radius: 12px;
        }

        .detail-box p {
            color: #666;
            margin-bottom: 7px;
        }

        .detail-box h3 {
            font-size: 20px;
            color: #222;
        }

        .message {
            color: #777;
            margin-top: 15px;
        }

        .error {
            color: red;
            margin-top: 15px;
        }

        /* Mobile */

        @media (max-width: 500px) {

            .weather-app {
                padding: 22px;
            }

            .temperature {
                font-size: 50px;
            }

            .search-box input {
                width: 100%;
            }

            .search-box button {
                padding: 12px;
            }
        }
    </style>
</head>

<body>

    <div class="weather-app">

        <h1>🌤️ Weather<span>Now</span></h1>

        <!-- Search Box -->

        <div class="search-box">

            <input
                type="text"
                id="cityInput"
                placeholder="Enter city name"
            >

            <button onclick="getWeather()">
                Search
            </button>

        </div>

        <p id="message" class="message">
            Enter a city to check the weather
        </p>

        <p id="error" class="error"></p>


        <!-- Weather Information -->

        <div id="weatherInfo" class="weather-info">

            <div id="city" class="city">
                Hyderabad, IN
            </div>

            <div id="localTime" class="local-time">
                Local Time
            </div>

            <img
                id="weatherIcon"
                class="weather-icon"
                src=""
                alt="Weather Icon"
            >

            <div id="temperature" class="temperature">
                0°C
            </div>

            <div id="description" class="description">
                Clear Sky
            </div>


            <!-- Details -->

            <div class="details">

                <div class="detail-box">

                    <p>💧 Humidity</p>

                    <h3 id="humidity">
                        -- %
                    </h3>

                </div>


                <div class="detail-box">

                    <p>💨 Wind Speed</p>

                    <h3 id="wind">
                        -- m/s
                    </h3>

                </div>


                <div class="detail-box">

                    <p>🌡️ Feels Like</p>

                    <h3 id="feelsLike">
                        -- °C
                    </h3>

                </div>


                <div class="detail-box">

                    <p>📊 Pressure</p>

                    <h3 id="pressure">
                        -- hPa
                    </h3>

                </div>

            </div>

        </div>

    </div>


    <script>

        /* =========================================
           OPENWEATHER API KEY
           ========================================= */

        const API_KEY = "YOUR_API_KEY";


        /* =========================================
           GET WEATHER
           ========================================= */

        function getWeather() {

            const city =
                document.getElementById("cityInput")
                .value
                .trim();


            if (city === "") {

                document.getElementById("error")
                    .textContent =
                    "Please enter a city name.";

                return;
            }


            if (API_KEY === "YOUR_API_KEY") {

                document.getElementById("error")
                    .textContent =
                    "Please add your OpenWeather API key.";

                return;
            }


            document.getElementById("message")
                .textContent =
                "Loading weather...";


            document.getElementById("error")
                .textContent = "";


            const API_URL =
                "https://api.openweathermap.org/data/2.5/weather?q="
                + encodeURIComponent(city)
                + "&appid="
                + API_KEY
                + "&units=metric";


            fetch(API_URL)

                .then(function(response) {

                    if (!response.ok) {

                        throw new Error(
                            "City not found"
                        );

                    }

                    return response.json();

                })

                .then(function(data) {

                    displayWeather(data);

                })

                .catch(function(error) {

                    document.getElementById("weatherInfo")
                        .style.display = "none";

                    document.getElementById("message")
                        .textContent = "";

                    document.getElementById("error")
                        .textContent =
                        "City not found. Please enter a valid city.";

                });

        }


        /* =========================================
           DISPLAY WEATHER
           ========================================= */

        function displayWeather(data) {

            document.getElementById("message")
                .textContent = "";


            document.getElementById("error")
                .textContent = "";


            document.getElementById("weatherInfo")
                .style.display = "block";


            /* City */

            document.getElementById("city")
                .textContent =
                data.name + ", " + data.sys.country;


            /* Temperature */

            document.getElementById("temperature")
                .textContent =
                Math.round(data.main.temp) + "°C";


            /* Description */

            document.getElementById("description")
                .textContent =
                data.weather[0].description;


            /* Humidity */

            document.getElementById("humidity")
                .textContent =
                data.main.humidity + "%";


            /* Wind */

            document.getElementById("wind")
                .textContent =
                data.wind.speed + " m/s";


            /* Feels Like */

            document.getElementById("feelsLike")
                .textContent =
                Math.round(data.main.feels_like) + "°C";


            /* Pressure */

            document.getElementById("pressure")
                .textContent =
                data.main.pressure + " hPa";


            /* Weather Icon */

            const icon =
                data.weather[0].icon;


            document.getElementById("weatherIcon")
                .src =
                "https://openweathermap.org/img/wn/"
                + icon
                + "@2x.png";


            /* Local Time */

            const cityTime =
                new Date(
                    Date.now()
                    + data.timezone * 1000
                    - new Date().getTimezoneOffset() * 60000
                );


            document.getElementById("localTime")
                .textContent =
                "Local Time: "
                + cityTime.toLocaleString(
                    "en-IN",
                    {
                        timeZone: "UTC",
                        dateStyle: "medium",
                        timeStyle: "short"
                    }
                );

        }


        /* =========================================
           ENTER KEY SEARCH
           ========================================= */

        document
            .getElementById("cityInput")
            .addEventListener(
                "keypress",
                function(event) {

                    if (event.key === "Enter") {

                        getWeather();

                    }

                }
            );

    </script>

</body>
</html>
