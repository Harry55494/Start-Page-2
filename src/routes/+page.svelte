<script>

    const OPEN_WEATHER_API_KEY = import.meta.env.VITE_OPENWEATHERMAP_API_KEY;
    if (!OPEN_WEATHER_API_KEY) {
        console.log("No OpenWeatherMap API key found. Please set VITE_OPENWEATHERMAP_API_KEY in your .env file.");
    }

    let current_time = new Date().toLocaleTimeString();
    let current_date = new Date().toLocaleDateString("en-GB", { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' });
    let weather_condition = "";
    let weather_temperature = "";
    let city = "";
    let iconcode = "";
    export let iconurl = "";


    fetch("https://api.openweathermap.org/data/2.5/weather?q=Lawford&appid=" + OPEN_WEATHER_API_KEY + "&units=metric")
        .then(response => response.json())
        .then(data => {
            console.log(data);
            weather_condition = data.weather[0].main;
            weather_temperature = Math.round(data.main.temp) + "°C";
            city = data.name;
            iconcode = data.weather[0].icon;
            iconurl = "http://openweathermap.org/img/wn/" + iconcode + "@2x.png";
        })
        .catch(() => {
            console.log("Unable to get weather");
            weather_condition = "Unable to get weather";
        });

    setInterval(() => {
        current_time = new Date().toLocaleTimeString();
        current_date = new Date().toLocaleDateString("en-GB", { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' });
        }, 1000);

</script>

<div id="top_bar">
    <div>{current_time}<br>{current_date}</div>
    <div id="weather">{city}<br><div id="weather_details"><img id="wicon" src="{iconurl}" alt="Weather icon"> {weather_condition} | {weather_temperature}</div></div>
</div>


<h1>Welcome to SvelteKit :)</h1>
<p>Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation</p>

<div id="search_div"><h2>> cd ~/   </h2><input id="search" type="search" /></div>




<style>

    :global(body){
        background-color: #212126;
        font-family: sans-serif;
        text-align: center;
        padding: 15px;
        color: white;
    }

    #top_bar{
        display: flex;
        align-items: center;
        justify-content: space-between;
        margin: 10px;
        font-family: "Fira Code", monospace;
        font-size: 15px;
        line-height: 1.8;
        text-align: left;
    }

    #weather{
        text-align: right;
    }

    #weather_details{
        display: flex;
        text-align: right;
        align-items: center;
    }

    #wicon{
        width: 30px;
        height: 30px;
        padding-bottom: 4px;
        padding-right: 8px;
    }

    #search_div{
        font-family: "Fira Code", monospace;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    #search{
        width: 50%;
        height: 40px;
        font-size: 30px;
        border: none;
        outline: none;
        background: none;
        margin: 20px;
        color: white;
        caret-color: white;
        caret-shape: block;

    }

</style>