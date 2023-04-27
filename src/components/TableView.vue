<script setup>
import { ref } from 'vue';
import axios from 'axios';

const chefUserName = ref('');

const minNumber = ref(0);

const maxNumber = ref(999999999);

const dishesData = ref([]);

const dishesAll = ref([]);

const submitForm = async () => {
  const data = {
    chefUserName: chefUserName.value,
  };

  try {
    // const response = await axios.post('https://laazmi.pk/get_all_dishes_for_a_chef', data);
    // console.log(response.data.DishesData);
    // const filteredData = response.data.DishesData.filter(dish => dish.dishUnitPrice > 1000);
    // dishesData.value = filteredData;
    // chefUserName.value = '';
    // webUserName.value = '';
    // const count = filteredData.length;
    // console.log(`Total number of dishes with unit price greater than 1000: ${count}`);


    const response = await axios.post('https://laazmi.pk/get_all_dishes_for_a_chef', data);
    console.log(response.data.DishesData);
    dishesAll.value = response.data.DishesData;
    dishesData.value = response.data.DishesData;
    chefUserName.value = '';
    webUserName.value = '';
  } catch (error) {
    console.log(error);
  }
};
const filterForm = () => {
  const filteredData = dishesAll.value.filter((dish) => {
    return dish.dishUnitPrice >= minNumber.value && dish.dishUnitPrice <= maxNumber.value;
  });
  dishesData.value = filteredData;
  console.log(filteredData.length);
};


</script>

<template>
    <div>
        <form class="new-form upper-form" @submit.prevent="submitForm" >
            <div class="form-div">
                <label>Device User Name</label>
                <input type="text" v-model="chefUserName" placeholder="Device User Name">
            </div>
        
            <button type="submit">Display Time Sheet</button>
        </form>
        <form action="" class="new-form" @submit.prevent="filterForm">
            <div class="form-div">
                <label>Minimum Price</label>
                <input type="number" v-model="minNumber" placeholder="Device User Name">
            </div>
            <div class="form-div">
                <label>Maximum Price</label>
                <input type="number" v-model="maxNumber" placeholder="Device User Name">
            </div>
            <button type="submit">Display Time Sheet</button>
        </form>
        <table class="styled-table">
            <thead>
                <tr>
                    <td>Dish ID</td>
                    <td>Dish Name</td>
                    <td>Dish Category</td>
                    <td>Unit Price</td>
                    <td>Unit Type</td>
                </tr>
            </thead>
            
            <tbody>
                <tr v-for="entry in dishesData" :key="entry.id">
                    <td>{{ entry.dishID }}</td>
                    <td>{{ entry.dishName }}</td>
                    <td>{{ entry.dishCategory }}</td>
                    <td>{{ entry.dishUnitPrice }}</td>
                    <td>{{ entry.dishUnitType }}</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>



<style scoped>
.new-form {
    display: flex;
    text-align: left;
    position: relative;
    border-radius: 12px 12px 0 0;
    padding: 15px;
    box-sizing: border-box;
    background: #ecf0f3;
    box-shadow: 14px 14px 20px #cbced1, -14px -14px 20px white;
}

.upper-form {
    border-radius: 12px 12px 12px 12px;
    margin-bottom: 30px;
    justify-content: center;
    justify-items: center;
}
.form-div {
    display: flex;
    flex-direction: column;
    margin-right: 30px;
}

label, input, button {
  display: block;
  width: 100%;
  padding: 0;
  border: none;
  outline: none;
  box-sizing: border-box;
}

label {
  margin-bottom: 4px;
}

label:nth-of-type(2) {
  margin-top: 12px;
}

input::placeholder {
  color: gray;
}

input {
    width: 250px;
    background: #ecf0f3;
    padding: 10px;
    padding-left: 20px;
    height: 50px;
    font-size: 14px;
    border-radius: 50px;
    box-shadow: inset 6px 6px 6px #cbced1, inset -6px -6px 6px white;
}

button {
  color: white;
  margin-top: 25px;
  background: #009879;
  height: 50px;
  min-width: 250px;
  max-width: 300px ;
  border-radius: 50px;
  cursor: pointer;
  font-weight: 900;
  box-shadow: 6px 6px 6px #cbced1, -6px -6px 6px white;
  transition: 0.5s;
}

button:hover {
  box-shadow: none;
  height: 45px;
  margin-top: 27px;
}


.styled-table {
    border-collapse: collapse;
    font-size: 0.9em;
    font-family: sans-serif;
    min-width: 100%;
    box-shadow: 0 0 20px rgba(0, 0, 0, 0.15);
}

.styled-table thead tr {
    background-color: #009879;
    color: #ffffff;
    text-align: left;
}


.styled-table th,
.styled-table td {
    padding: 12px 15px;
    min-width: 140px;
}

.styled-table tbody tr {
    border-bottom: 1px solid #dddddd;
}

.styled-table tbody tr  td img {
    max-width: 100px;
    border-radius: 100px;
}

.styled-table tbody tr:nth-of-type(even) {
    background-color: #f3f3f3;
}

.styled-table tbody tr:last-of-type {
    border-bottom: 2px solid #009879;
}

.styled-table tbody tr.active-row {
    font-weight: bold;
    color: #009879;
}
</style>