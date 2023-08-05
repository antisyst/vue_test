<template>
  <div class="main_section">
    <div class="filter_section">
      <label class="section_content">Страна</label>
      <div class="search_field">
        <i class="fa-solid fa-magnifying-glass"></i>
        <input type="text" v-model="countryFilter" @input="applyFilters" placeholder="Поиск стран" />
      </div>
      <div class="country_check_section">
        <div>
          <input type="checkbox" class="ui-checkbox" value="Греция" v-model="countryCheckFilter" @change="applyFilters" />
          <label>Греция</label>
        </div>
         <div>
          <input type="checkbox" class="ui-checkbox" value="Азербайджан" v-model="countryCheckFilter" @change="applyFilters" />
          <label>Азербайджан</label>
        </div>
        <div>
          <input type="checkbox" class="ui-checkbox" value="Албания" v-model="countryCheckFilter" @change="applyFilters" />
          <label>Албания</label>
        </div>
      </div>
      <label class="section_content">Тип</label>
      <div class="type_filter">
        <div>
          <input type="checkbox" class="ui-checkbox" value="Отель" v-model="typeFilter" @change="applyFilters" />
          <label>Отель</label>
        </div>
        <div>
          <input type="checkbox" class="ui-checkbox" value="Апартаменты" v-model="typeFilter" @change="applyFilters" />
          <label>Апартаменты</label>
        </div>
      </div>
      <label class="section_content">Количество звезд</label>
      <div class="stars_section">
        <div>
          <input type="checkbox" class="ui-checkbox" value="1" v-model="starsFilter" @change="applyFilters" />
          <label>1 звезда</label>
        </div>
        <div>
          <input type="checkbox" class="ui-checkbox" value="2" v-model="starsFilter" @change="applyFilters" />
          <label>2 звезда</label>
        </div>
        <div>
          <input type="checkbox" class="ui-checkbox" value="3" v-model="starsFilter" @change="applyFilters" />
          <label>3 звезда</label>
        </div>
        <div>
          <input type="checkbox" class="ui-checkbox" value="4" v-model="starsFilter" @change="applyFilters" />
          <label>4 звезда</label>
        </div>
        <div>
          <input type="checkbox" class="ui-checkbox" value="5" v-model="starsFilter" @change="applyFilters" />
          <label>5 звезда</label>
        </div>
      </div>
      <div class="review_quant_section">
        <label class="section_content">Количество отзывов (от)</label>
        <input type="text" placeholder="Например, от 10" v-model.number="reviewsFilter" min="0" max="36" @input="applyFilters" />
      </div>
      <label class="section_content">Цена</label>
      <span>{{ priceFilter }}</span>
      <input type="range" v-model.number="priceFilter" min="0" max="5000" step="100" @input="applyFilters" />
      <div class="button_section">
        <button @click="applyFilters" class="apply_button">Применить фильтр</button>
        <button @click="clearFilters"><i class="fa-solid fa-xmark"></i> Очистить фильтр</button>
      </div>
    </div>
    <div class="hotel-list">
      <div v-if="filteredHotels.length > 0">
        <div v-for="hotel in filteredHotels" :key="hotel.name" class="card_body_section">
          <div class="card_body_left">
            <div class="card-header">
              <h3 class="card-title">{{ hotel.name }}</h3>
              <div class="card_info_section">
                <div>
                  <span v-for="star in stars" :key="star" :class="{ 'yellow-star': star <= hotel.stars, 'gray-star': star > hotel.stars }">&#9733;</span>
                </div>
                <div class="card_body_info_type">
                    {{ hotel.type }}
                </div>
                <div class="cart_review_quant">
                  <span>{{ hotel.reviews_amount }}</span> отзыва
                </div>
                <div class="card_info_country">
                  <i class="fa-solid fa-location-dot"></i> {{ hotel.country }}
                </div>
              </div>
              <div class="review_description">
                <p class="card-text">{{ hotel.description }}</p>
              </div>
            </div>
        </div>
          <div class="card_body_right">
              <div class="card_info_price">
                <span>{{ hotel.min_price }}  ₽‎</span>
                Цена за 1 ночь
              </div>
              <div class="book_info_bottom">
                <img src="./assets/book.svg" alt="">
                Забронировать
              </div>
          </div>
        </div>
      </div>
      <div v-else class="record_not_found">
        <img src="./assets/error.svg" alt="">
        <h2>По данным параметрам ничего не найдено</h2>
        <p>Попробуйте изменить параметры фильтрации или вернуться в общий каталог</p>
      <button @click="clearFilters"> Очистить фильтр</button>
      </div>
    </div>
  </div>
</template>

<script lang="js">
export default {
  data() {
    return {
      "hotels": [
    {
      "name": "Marina Inn",
      "country": "Греция",
      "address": "Фалираки, Родос, Греция",
      "stars": 4,
      "type": "Отель",
      "description": "Этот 4-звездочный отель расположен в центре города. На его территории есть бассейн с террасой и сауна. Из номеров открывается вид на море или на средневековый город.",
      "services": ["Пляж","Кондиционер","Открытый бассейн","Бесплатная парковка","Бесплатный WiFi","Вид на море","Бесплатный завтрак"],
      "min_price": 2789.00,
      "currency": "RUR",
      "rating": 4.5,
      "reviews_amount": 18,
      "last_review": "Отличное расположение. Вкусный завтрак. Отдыхали с детьми - все понравилось."
    },
    {
      "name": "Mondrian Suites",
      "country": "Греция",
      "address": "Фалираки, Родос, Греция",
      "stars": 5,
      "type": "Апартаменты",
      "description": "Из окон открывается вид на город.К услугам гостей номера-студио с балконом и чайником в 2,7 км от пляжа.",
      "services": ["Пляж","Кондиционер","Открытый бассейн","Платная парковка","Бесплатный WiFi","Вид на море"],
      "min_price": 3245.20,
      "currency": "RUR",
      "rating": 5,
      "reviews_amount": 4,
      "last_review": "Потрясающее место, в номере есть все необходимое. Красивый вид на море."
    },
    {
      "name": "Sunny Appartments",
      "country": "Греция",
      "address": "Родос, Родос, Греция",
      "stars": 2,
      "type": "Апартаменты",
      "description": "Все номера и апартаменты оборудованы кондиционером и телевизором с плоским экраном. Также в распоряжении гостей тостер и чайник.",
      "services": ["Пляж","Кондиционер","Бесплатная парковка","Бесплатный WiFi"],
      "min_price": 1153.00,
      "currency": "RUR",
      "rating": 2.4,
      "reviews_amount": 36,
      "last_review": "Бассейн очень маленький. Кормят невкусно. Больше не поедем."
    },
    {
      "name": "Super All Inclusive Hotel",
      "country": "Греция",
      "address": "Родос, Родос, Греция",
      "stars": 4,
      "type": "Отель",
      "description": "Все номера оснащены телевизором с плоским экраном. Из некоторых номеров открывается вид на море или город.",
      "services": ["Пляж","Кондиционер","Открытый бассейн","Бесплатный WiFi","Вид на море","Бесплатный завтрак"],
      "min_price": 3689.00,
      "currency": "RUR",
      "rating": 4.1,
      "reviews_amount": 14,
      "last_review": "Недалёко от пляжа и старого города, вокруг много разных магазинчиков"
    },
    {
      "name": "Adams Hotel",
      "country": "Греция",
      "address": "Родос, Родос, Греция",
      "stars": 3,
      "type": "Отель",
      "description": "Отель расположен всего в 100 метрах от пляжа и в 5-ти минутах ходьбы от исторической части города, недалеко от всех основных достопримечательностей. Из отеля открывается вид на море. К услугам гостей спокойный открытый бассейн.",
      "services": ["Пляж","Кондиционер","Открытый бассейн","Бесплатная парковка","Бесплатный WiFi","Бесплатный завтрак"],
      "min_price": 1896.00,
      "currency": "RUR",
      "rating": 0,
      "reviews_amount": 0,
      "last_review": ""
    }
  ],
      countryFilter: "",
      typeFilter: [],
      countryCheckFilter: [],
      starsFilter: [],
      reviewsFilter: null,
      priceFilter: 5000,
      minPriceFilter: 0,
      maxPriceFilter: 4000,
      filteredHotels: [],
    };
  },
  computed: {
        stars() {
      return [1, 2, 3, 4, 5];
    },
    filteredHotels() {
      let filtered = this.hotels;

      if (this.countryFilter) {
        filtered = filtered.filter((hotel) =>
          hotel.country.toLowerCase().includes(this.countryFilter.toLowerCase())
        );
      }

      if (this.typeFilter.length > 0) {
        filtered = filtered.filter((hotel) => this.typeFilter.includes(hotel.type));
      }

      if (this.countryCheckFilter.length > 0) {
        filtered = filtered.filter((hotel) =>
          this.countryCheckFilter.includes(hotel.country)
        );
      }

      if (this.starsFilter.length > 0) {
        filtered = filtered.filter((hotel) =>
          this.starsFilter.includes(hotel.stars.toString())
        );
      }

      if (this.reviewsFilter !== null) {
        filtered = filtered.filter(
          (hotel) => hotel.reviews_amount ?? 0 >= this.reviewsFilter
        );
      }

      filtered = filtered.filter(
        (hotel) =>
          hotel.min_price >= this.minPriceFilter && hotel.min_price <= this.maxPriceFilter
      );

      filtered = filtered.filter((hotel) => hotel.min_price <= this.priceFilter);

      return filtered;
    },
  },
  methods: {
    applyFilters() {
      console.log("Filters applied!");
    },
    clearFilters() {
      this.countryFilter = "";
      this.typeFilter = [];
      this.starsFilter = [];
      this.reviewsFilter = null;
      this.priceFilter = 0;
      this.countryCheckFilter = [];
    },
  },
};
</script>


<style scoped lang="scss">
  .main_section {
    display: flex;
    flex-direction: row;
    align-items: flex-start;
    padding-top: 4vh;
    justify-content: space-evenly;
    vertical-align: top;
    height: 100vh;
  }
  .search_field {
    display: flex;
    flex-direction: row;
    border: 1px solid #EAEAEA;
    height: 45px;
    width: 270px;
    border-radius: 8px;
    margin-bottom: 1vh;
    align-items: center;
    justify-content: center;
    transition: all 0.3s ease-out;


    i {
      margin-right: 5px;
      transition: all 0.3s ease-out;
    }
    input {
      outline: none;
      border: none;
      color: #868686;
      padding-left: 6px;
      height: 37px;
      font-size: 16px;

      &::placeholder {
        font-size: 16px;
      }
    }
  }
  .type_filter {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: center;
    height: auto;
    width: 270px;
    border-radius: 8px;
    padding: 23px 20px;
    margin-top: 5px;
    margin-bottom: 2vh;
    background: #FAFAFA;

   label {
    margin-left: 10px;
   }

    div {
      vertical-align: middle;
      display: flex;
      flex-direction: row;
      align-items: center;
      margin: 5px;
      justify-content: center;
    }
  }
  .stars_section {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: center;
    height: auto;
    width: 270px;
    padding: 23px 20px;
    border-radius: 8px;
    border: 1px solid #EAEAEA;

    label {
    margin-left: 10px;
   }

    div {
      display: flex;
      margin: 5px;
      flex-direction: row;
      align-items: center;
      justify-content: center;
    }
  }
  .country_check_section {
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: center;
    height: auto;
    width: 270px;
    padding: 23px 20px;
    border-radius: 8px;
    border: 1px solid #EAEAEA;

    label {
    margin-left: 10px;
   }

    div {
      display: flex;
      margin: 5px;
      flex-direction: row;
      align-items: center;
      justify-content: center;
    }
  }
  .review_quant_section {
    margin-top: 1.3vh;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: center;

    input {
      outline: none;
      border: none;
      padding-left: 6px;
      height: 37px;
      border: 1px solid #EAEAEA;
      height: 45px;
      width: 270px;
      padding: 20px 15px;
      border-radius: 8px;
      font-size: 16px;

      &::placeholder {
        color: #868686;
      }
    }
  }
  .filter_section {
    flex-basis: 20%;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
  }
  .hotel-list {
    flex-basis: 40%;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
  .apply_button {
    width: 260px;
  }
  .button_section {
    margin-top: 1vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    flex-wrap: nowrap;

    button {
      width: 275px;
      border-radius: 15px;
      outline: none;
      font-weight: bold;
      margin: 7px 0;
      height: 56px;
      cursor: pointer;

      &:nth-child(1) {
        background: #6A53F5;
        border: none;
        color: var(--var-color-white);
      }
      &:nth-child(2) {
        border: 1px solid #EAEAEA;
        background: var(--var-color-white);

        i {
          margin-right: 4px;
        }
      }
    }
  }

    // CARD BODY ------------

  .card_body_section {
    border: 1px solid #EAEAEA;
    height: 180px;
    display: flex;
    flex-direction: row;
    padding: 27px 20px;
    align-items: flex-start;
    justify-content: space-between;
    border-radius: 15px;
    margin: 10px 0;
    width: 760px;
    
    ul {
      list-style: none;
    }
  }
  .yellow-star {
  color: #FFA900;
  user-select: none;
}

.gray-star {
  color: #EAEAEA;
  user-select: none;
}
.card_info_section {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: flex-start;
  flex-wrap: wrap;
  
  .card_body_info_type {
    margin-left: 12px;
    color: #868686;

    &::after {
      content: '●';
      font-size: 9px;
      margin-left: 6px;
      vertical-align: middle;
    }
  }
  .cart_review_quant {
    margin-left: 6px;
    color: #868686;

    span {
      font-size: 15px;
    }
  }
  .card_info_country {
    margin-left: 10px;
    color: #3A3A3A;
    font-size: 14.5px;

    i {
      margin-right: 5px;
    }
  }
}
.card-title {
  color: #3A3A3A;
  font-size: 20px;
}
.card_body_left {
  flex-basis: 70%;
}
.card_body_right {
  flex-basis: 30%;
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  justify-content: space-between;
}
.review_description {
  margin-top: 11px;
  color: #3A3A3A;
}
.card_info_price {
  color: #868686;
  display: flex;
  margin-top: 3px;
  line-height: normal;
  flex-direction: column;
  font-size: 12px;
  span {
    font-weight: bold;
    color: #3A3A3A;
    font-size: 25px;
  }
}
.book_info_bottom {
  padding: 15px 20px;
  margin-top: 28px;
  border-radius: 15px;
  cursor: pointer;
  background: #6b53f524;
  color: #6A53F5;

  img {
    vertical-align: middle;
    margin-top: -3px;
    width: 18px;
    margin-right: 4px;
  }
  
}
.record_not_found {
  width: 760px;
  margin-top: 3.5vh;
  height: 500px;
  display: flex;
  flex-direction: column;
  border: 1px solid #EAEAEA;
  align-items: center;
  text-align: center;
  border-radius: 15px;
  justify-content: center;

  h2 {
    color: #3A3A3A;
    margin-top: 1.5vh;
  }
  p {
    color: #969696;
    padding: 5px 18%;
    text-align: center;
  }
  button {
    padding: 17px 40px;
    margin-top: 15px;
    border-radius: 15px;
    cursor: pointer;
    background: #6b53f524;
    color: #6A53F5;
    outline: none;
    font-weight: bold;
    border: none;
  }
}
.hotel-list {
  margin-top: 2.5vh;
}
</style>
