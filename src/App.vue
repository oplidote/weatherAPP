<template>
  <div>
    <header class="header">
      <a href="index.html" class="logo">Daily Weather</a>
    </header>
    <div class="search-form">
      <input
        type="text"
        name="city"
        id="city"
        placeholder="ex) 서울 / 부산 / 대구"
      />
      <button class="search" @click="goSearch"></button>
    </div>

    <div class="result-wrap" v-show="showResult">
      <div class="info-wrap">
        <div class="info-box">
          <span>기온</span>
          <em id="temp"></em>
        </div>
        <div class="info-box">
          <span>체감온도</span>
          <em id="feel-temp"></em>
        </div>
        <div class="info-box">
          <span>습도</span>
          <em id="humidity"></em>
        </div>
        <div class="info-box">
          <span>날씨</span>
          <em id="weather"></em>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from "vue";

export default {
  name: "App",
  components: {},
  setup() {
    const showResult = ref(false);
    const goSearch = () => {
      let city = document.getElementById("city").value;
      if (!city) {
        alert("검색어를 입력해주세요 !");
        return;
      } else {
        city = cityName(city);
        // 오픈웨더맵 로그인 키
        let key = "89387cb975ca5ede5877cd62d36ec15c";
        // api url 화씨 => 도씨 출력 옵션 units
        const url = `https://api.openweathermap.org/data/2.5/weather?q=${city}&appid=${key}&units=metric`;
        fetch(url)
          .then((res) => {
            if (res.status === 200) {
              showResult.value = true;
              return res.json();
            } else {
              console.error(`HTTP error! status: ${res.status}`);
              alert(
                "검색에 실패했습니다.조회가 되지 않을 시 지역명을 영문으로 재검색해보세요."
              );
            }
          })
          .then((jsonData) => {
            console.log(jsonData.main);
            console.log(jsonData.weather);
            let temp = document.getElementById("temp");
            let humidity = document.getElementById("humidity");
            let feel_temp = document.getElementById("feel-temp");
            let weather = document.getElementById("weather");
            temp.innerHTML = `약 ${jsonData.main.temp.toFixed(1)} ℃`;
            humidity.innerHTML = `약 ${jsonData.main.humidity.toFixed(1)} %`;
            feel_temp.innerHTML = `약 ${jsonData.main.feels_like.toFixed(1)} ℃`;
            weather.innerHTML = `${jsonData.weather[0].main}`;
          })
          .catch((err) => {
            console.log(err);
          });
      }
    };
    // 입력 값 영문 변환
    const cityName = (_name) => {
      if (_name == "서울") return "seoul";
      else if (_name == "부산") return "busan";
      else if (_name == "인천") return "incheon";
      else if (_name == "대구") return "daegu";
      else if (_name == "대전") return "daejeon";
      else if (_name == "광주") return "gwangju";
      else if (_name == "수원") return "suwon";
      else if (_name == "울산") return "ulsan";
      else if (_name == "고양") return "goyang-si";
      else if (_name == "용인") return "yongin";
      else if (_name == "창원") return "changwon";
      else if (_name == "성남") return "Seongnam-si";
      else if (_name == "청주") return "Cheongju-si";
      else if (_name == "부천") return "Bucheon-si";
      else if (_name == "화성") return "Hwasun";
      else if (_name == "남양주") return "namyangju";
      else if (_name == "전주") return "jeonju";
      else if (_name == "천안") return "cheonan";
      else if (_name == "안산") return "ansan-si";
      else if (_name == "안양") return "anyang-si";
      else if (_name == "김해") return "kimhae";
      else if (_name == "평택") return "pyeongtaek";
      else if (_name == "포항") return "pohang";
      else if (_name == "제주") return "jeju-do";
      else if (_name == "파주") return "paju";
      else if (_name == "의정부") return "uijeongbu-si";
      else if (_name == "김포") return "gimpo-si";
      else if (_name == "구미") return "gumi";
      else if (_name == "양산") return "yangsan";
      else if (_name == "광명") return "Kwangmyŏng";
      else if (_name == "아산") return "asan";
      else if (_name == "익산") return "iksan";
      else if (_name == "춘천") return "chuncheon";
      else if (_name == "경산") return "gyeongsan-si";
      else if (_name == "군포") return "gunpo";
      else if (_name == "군산") return "gunsan";
    };
    return {
      goSearch,
      showResult,
    };
  },
};
</script>
<style>
</style>
