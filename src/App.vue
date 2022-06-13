<template>
  <div>
    <header class="header">
      <a href="index.html" class="logo">Weather We Go ?</a>
    </header>
    <input type="text" name="city" id="city" placeholder="ex)서울 / 부산 / 의정부" />
    <button @click="goSearch">결과 확인</button>

    <div class="result-wrap">
      <div id="result" v-show="showResult"></div>
      <span>기온</span>
      <div id="temp"></div>
      <span>체감온도</span>
      <div id="feel-temp"></div>
      <span>습도</span>
      <div id="humidity"></div>
      <span>날씨</span>
      <div id="weather"></div>
      <span>가시거리</span>
      <div id="visibility"></div>
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
      showResult.value = false;
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
            let result = document.getElementById("result");
            if (res.status === 200) {
              return res.json();
            } else {
              showResult.value = true;
              console.error(`HTTP error! status: ${res.status}`);
              result.innerHTML = "검색에 실패했습니다. 지역명을 영문으로 재검색해주세요."
            }
          })
          .then((jsonData) => {
            console.log(jsonData.main);
            console.log(jsonData.weather);
            showResult.value = true;
            let result = document.getElementById("result");
            let temp = document.getElementById("temp");
            let humidity = document.getElementById("humidity");
            let feel_temp = document.getElementById("feel-temp");
            let weather = document.getElementById("weather");
            let visibility = document.getElementById("visibility");
            temp.innerHTML = `약 ${jsonData.main.temp.toFixed(1)} ℃`;
            humidity.innerHTML = `약 ${jsonData.main.humidity.toFixed(1)} %`;
            feel_temp.innerHTML = `약 ${jsonData.main.feels_like.toFixed(1)} ℃`;
            weather.innerHTML = `${jsonData.weather[0].main}`;
            visibility.innerHTML = `${jsonData.visibility / 1000} km`;
            result.innerHTML = "나가지마.."
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
    }
    return {
      goSearch,
      showResult,
    };
  },
};
</script>
<style>
</style>
