<template>
  <div id="panel" :class="weatherBg">
    <main>
      <div class="search-box">
        <label>Enter in City Name</label>
        <input
          type="text"
          class="search-bar"
          placeholder="Search..."
          v-model.trim="query"
          @keypress="fetchWeather"
        />
      </div>
      <transition name="slide-in" appear>
        <div class="weather-wrap" v-if="show">
          <div class="location-box">
            <div class="location">{{ cityAndCountry }}</div>
            <div class="date">{{ currentDateTime() }}</div>
          </div>

          <div class="weather-box">
            <div class="temp">{{ weatherTemp }}</div>
            <div class="weather">
              <img :src="icon" :alt="weather" /> {{ weather }}
            </div>
          </div>
        </div>
        <div v-else-if="!show && error === ''">
          <p>Please enter a valid city name</p>
        </div>
        <div v-else>
          <p>{{ error }}</p>
        </div>
      </transition>
    </main>
  </div>
</template>

<script>
import moment from 'moment';
export default {
  data() {
    return {
      api_key: 'c0ea3a1e7a6a819c31ca4d503b593fa0',
      url_base: 'https://api.openweathermap.org/data/2.5/',
      query: '',
      cityName: '',
      country: '',
      temp: '',
      weather: '',
      iconImg: '',
      weatherBg: '',
      show: false,
      error: '',
    };
  },
  computed: {
    cityAndCountry() {
      if (this.cityName != '' && this.country != '') {
        return `${this.cityName}, ${this.country}`;
      } else {
        return '';
      }
    },
    icon() {
      return `http://openweathermap.org/img/wn/${this.iconImg}.png`;
    },
    weatherTemp() {
      return Math.round(this.temp) + '°F';
    },
  },
  methods: {
    fetchWeather(event) {
      if (event.key === 'Enter') {
        fetch(
          `${this.url_base}weather?q=${this.query}&units=imperial&appid=${this.api_key}`
        )
          .then((response) => {
            if (response.ok) {
              this.show = true;
              return response.json();
            }
          })
          .then((data) => {
            this.cityName = data.name;
            this.country = data.sys.country;
            this.temp = data.main.temp;

            if (this.temp > 55) {
              this.weatherBg = 'warm';
            } else if (this.temp < 55) {
              this.weatherBg = 'cold';
            } else {
              this.temp = '';
              this.weatherBg = 'default-bg';
            }

            this.weather = data.weather[0].main;
            this.iconImg = data.weather[0].icon;
            console.log(data);
          })
          .catch((error) => {
            this.show = false;
            error = `"${this.query}" is an invalid city name, please enter a valid city name`;
            this.error = error;
          });
      } else {
        this.show = false;
        this.error = '';
      }
    },
    currentDateTime() {
      return moment().format('MMMM Do, YYYY');
    },
  },
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,400;0,500;0,700;0,900;1,300;1,700&display=swap');

#panel {
  background-size: cover;
  background-position: center;
  border-radius: 25px;
  box-shadow: 0 14px 28px rgba(0, 0, 0, 0.25), 0 10px 10px rgba(0, 0, 0, 0.22);
  transition: 0.4s;
  max-width: 440px;
}

.warm {
  background-image: url('../../assets/warm-bg.jpg');
}

.cold {
  background-image: url('../../assets/cold-bg.jpg');
}

.default-bg {
  background-color: #10163a;
}

main {
  min-height: 92vh;
  padding: 25px;
  background-image: linear-gradient(
    to bottom,
    rgba(0, 0, 0, 0.25),
    rgba(0, 0, 0, 0.75)
  );
  border-radius: 25px;
}

.search-box {
  width: 100%;
  margin-bottom: 30px;
}

label {
  color: #fff;
}

.search-box .search-bar {
  display: inline-block;
  width: 100%;
  padding: 15px;
  color: #313131;
  font-size: 20px;
  margin-top: 10px;
  appearance: none;
  border: none;
  outline: none;
  background: none;
  box-shadow: 0px 0px 8px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 2555, 255, 0.5);
  border-radius: 0px 16px 0px 16px;
  transition: 0.4s;
}

.search-box .search-bar:focus {
  box-shadow: 0px 0px 16px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 16px 0 16px 0;
}

p {
  color: #fff;
  font-size: 20px;
  font-weight: 500;
  text-align: center;
  text-shadow: 1px 3px rgba(0, 0, 0, 0.25);
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
  font-size: 102px;
  font-weight: 900;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
  background-color: rgba(255, 255, 255, 0.25);
  border-radius: 16px;
  margin: 30px 0px;
  box-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.weather-box .weather {
  color: #fff;
  font-size: 48px;
  font-weight: 700;
  font-style: italic;
  text-shadow: 3px 6px rgba(0, 0, 0, 0.25);
}

.slide-in-enter-from {
  opacity: 0;
  transform: translateY(-30px);
}
.slide-in-leave-to {
  opacity: 0;
  transform: translateY(-30px);
}
.slide-in-enter-active {
  transition: all 0.3s ease-out;
}
.slide-in-leave-active {
  transition: all 0.3s ease-in;
  position: absolute;
}
.slide-in-enter-to,
.slide-in-leave-from {
  opacity: 1;
  transform: translateY(0);
}

.slide-in-move {
  transition: transform 0.8s ease;
}

@media screen and (max-width: 600px) {
  main {
    min-height: auto;
    height: 600px;
  }
}
</style>
