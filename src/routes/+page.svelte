<script>

    import mountains from "$lib/assets/galahad_logo.png";
    import mappin from "$lib/assets/map-pin.svg";

    const OPEN_WEATHER_API_KEY = import.meta.env.VITE_OPENWEATHERMAP_API_KEY;
    if (!OPEN_WEATHER_API_KEY) {
        console.log("No OpenWeatherMap API key found. Please set VITE_OPENWEATHERMAP_API_KEY in your .env file.");
    }

    let current_time = new Date().toLocaleTimeString();
    let current_date = new Date().toLocaleDateString("en-GB", { weekday: 'long', month: 'long', day: 'numeric' });
    let weather_display = "";
    let weather_condition = "";
    let weather_temperature = "";
    let city = "";
    let iconurl = "";


    fetch("https://api.openweathermap.org/data/2.5/weather?q=Lawford&appid=" + OPEN_WEATHER_API_KEY + "&units=metric")
        .then(response => response.json())
        .then(data => {
            console.log(data);
            weather_condition = data.weather[0].main;
            // change condition to have y at the end
            if (weather_condition === "Clouds") {
                weather_condition = "Cloudy";
            }
            weather_temperature = Math.round(data.main.temp) + "°C";
            city = data.name;
            iconurl = "http://openweathermap.org/img/wn/" + data.weather[0].icon + "@2x.png";
            weather_display = " - " + weather_temperature + " & " + weather_condition
        })
        .catch(() => {
            console.log("Unable to get weather");
            weather_condition = "Unable to get weather";
        });

    setInterval(() => {
        current_time = new Date().toLocaleTimeString();
        current_date = new Date().toLocaleDateString("en-GB", { weekday: 'long', month: 'long', day: 'numeric' });
        }, 1000);

</script>

<div id="top_bar">
    <div id="details">
        {current_date}{weather_display}
        {#if iconurl !== ""}
            <img id="wicon" src="{iconurl}" alt="Weather icon">
        {/if}
    </div>
</div>


<h1 id="title">Where to?</h1>

<div id="search_div"><input id="search" type="search" /></div>

<div id="map_pin_div"><img id="map_pin" src={mappin} alt="map pin"></div>

<div id="image_div"><img id="image" src={mountains} alt="mountains"></div>


<style>

    :global(body){
        background-color: #2e2e2e;
        font-family: serif;
        text-align: center;
        padding: 15px;
        color: #F3E9D0;
        overflow: hidden;
        user-select: none;
    }

    #top_bar{
        text-align: center;
    }

    #details{
        font-family: "Quattrocento", monospace;
        font-size: 16px;
        margin: auto;
        letter-spacing: 1px;
        height: 30px;
    }

    #wicon{
        width: 35px;
        height: 35px;
        top: 10px;
        position: relative;
    }

    #title{
        padding-top: 250px;
        font-family: "Gravitas One", serif;
        font-size: 128px;
        margin: 0;
    }

    #search_div{
        font-family: "Quattrocento", serif;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    #search{
        width: 50%;
        height: 50px;
        font-size: 25px;
        color: #F3E9D0;
        font-family: "Quattrocento", "Fira Code", monospace;
        background: none;
        outline: none;
        margin-top: 60px;
        border: rgba(243, 233, 208, 0.99) 1px solid;
        border-radius: 10px;
        padding: 10px;
        caret-color: #F3E9D0;
        caret-shape: block;
    }

    #map_pin_div{
        margin-top: 200px;
    }

    #map_pin{
        width: 40px;
        height: 40px;
        top: 50%;
        left: 50%;
        color: #F3E9D0;
    }

    #image_div{
        position: absolute;
        bottom: 0;
        left: 0;
        right: 0;
        margin: 0;
        padding: 0;
    }

    #image{
        width: 100%;
        height: auto;
        transform: translateY(50%);
    }

</style>