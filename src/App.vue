<template>
  <div
    id="app"
    :class="
      typeof weather.main != 'undefined' && Math.round(weather.main.temp) > 20
        ? 'warm'
        : ''
    "
    >
    <main>
      <div class="search-box">
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model="query"
          @keypress="fetchWeather"
          @mouseenter="getClock"
        />
      </div>
      <div class="weather-wrap" v-if="typeof weather.main != 'undefined'">
        <div class="location-box">
          <div class="location">
            {{ weather.name }}, {{ weather.sys.country }}
          </div>
          <div class="date">{{ dateBuilder() }}</div>
          <div class="clock">
            <div class="bg2">
              <h2 class="s">{{ day }}</h2>
            </div>
            <div class="bg">
              <h2 id="h">{{ hours }}</h2>
            </div>
            <h2>:</h2>
            <div class="bg">
              <h2 id="m">{{ minutes }}</h2>
            </div>
          </div>
        </div>
        <div class="clock"></div>
        <div class="weather-box">
          <div class="temp">{{ weather.main.temp }}℃</div>
          <div class="weather">{{ weather.weather[0].main }}</div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  clock: function () {
    return { hours: 0, minutes: 0 };
  },

  data: function () {
    return {
      api_key: "203a7ce25e898c0f80c7e8316fb90304",
      url_base: "https://api.openweathermap.org/data/2.5/",
      query: "seoul",
      weather: {},
    };
  },
  methods: {
    getClock() {
      const date = new Date();

      if (date.getHours() > 13) {
        this.hours = date.getHours() - 12;
      } else if (date.getHours() == 0) {
        this.hours = 12;
      } else {
        this.hours = date.getHours();
      }
      this.minutes = this.PadStartMinutes(date.getMinutes());
    },
    setTime() {
      setInterval(this.getClock, 1000);
    },
    PadStartMinutes(digit) {
      return ("0" + digit).slice(-2);
    },

    mounted() {
      this.setTime();
    },

    fetchWeather: function (e) {
      if (e.key == "Enter") {
        let fetchUrl = `${this.url_base}weather?q=${this.query}&units=metric&APPID=${this.api_key}`;
        fetch(fetchUrl)
          .then((res) => {
            console.log(res);
            return res.json();
          })
          .then((results) => {
            return this.setResult(results);
          });
      }
    },
    setResult: function (results) {
      this.weather = results;
    },
    dateBuilder: function () {
      let d = new Date();
      let months = [
        "1월",
        "2월",
        "3월",
        "4월",
        "5월",
        "6월",
        "7월",
        "8월",
        "9월",
        "10월",
        "11월",
        "12월",
      ];
      let days = [
        "일요일",
        "월요일",
        "화요일",
        "수요일",
        "목요일",
        "금요일",
        "토요일",
      ];
      let day = days[d.getDay()];
      let date = d.getDate();
      let month = months[d.getMonth()];
      let year = d.getFullYear();
      return `${year}년 ${month} ${date}일 ${day}`;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: "montserrat", sans-serif;
}
#app {
  background-image: url("./assets/question.gif");
  background-size: cover;
  background-position: bottom;
  transition: 0.4s;
}
#clear{
  
}
#rain {
  background-image: url("/src/assets/rain.gif");
}
main {
  min-height: 100vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
}
.search-box {
  width: 100%;
  margin-bottom: 30px;
}
.search-box .search-bar {
  display: block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}
.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0px 16px 0px;
}
.location-box .location {
  color: #fff;
  font-size: 32px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
}
.location-box .date {
  color: #fff;
  font-size: 20px;
  font-weight: 300;
  font-style: italic;
  text-align: center;
}
.weather-box {
  text-align: center;
}
.weather-box .temp {
  display: inline-block;
  padding: 10px 25px;
  color: #fff;
  font-size: 82px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.weather-box .weather {
  color: #fff;
  font-size: 40px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}
.clock {
  color: #000;
  font-size: 56px;
  text-align: center;
  position: relative;
  top: 50%;
  left: 43%;
  transform: translate(-51%, 8%);
  display: flex;
  align-items: center;
  justify-content: center;
}

h2 {
  color: white;
}

.bg {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 4em;
  height: 3em;
  background: inherit;
  position: relative;
  border-radius: 30%;
  box-shadow: inset -2px -2px 5px rgba(255, 255, 255, 1),
    inset 3px 3px 5px rgba(0, 0, 0, 0.2);
}

.bg2 {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 4em;
  height: 4em;
  font-size: 25px;
  position: relative;
  border-radius: 30%;
}

#m {
  margin: 0 10px;
}
</style>

