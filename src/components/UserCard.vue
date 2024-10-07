<template>
  <div>
    <div class="filter-toolbar">
      <button @click="applyGenderFilter('all')">Всі</button>
      <button @click="applyGenderFilter('Male')">Чоловіки</button>
      <button @click="applyGenderFilter('Female')">Жінки</button>
    </div>

    <div>
      <div class="user-container" v-if="filteredUsersList.length">
        <div
          v-for="(user, idx) in filteredUsersList"
          :key="idx"
          :class="['user-card', user.age >= 18 ? 'adult' : 'minor']"
        >
          <img :src="user.photo" alt="User photo" class="user-avatar" />
          <h2>{{ user.firstName }} {{ user.lastName }}</h2>
          <p>Стать: {{ user.gender }}</p>
          <p v-if="user.age >= 18">Вік: {{ user.age }}</p>
          <p>Посада: {{ user.position }}</p>
          <h3>Хобі</h3>
          <ul>
            <li v-for="(hobby, index) in user.hobbies" :key="index">
              {{ hobby }}
            </li>
          </ul>
        </div>
      </div>
      <p v-else class="user-container empty">Немає користувачів</p>
    </div>
  </div>
</template>

<script lang="ts">
import { User } from "@/Models/User";
import { defineComponent, reactive, ref } from "vue";

export default defineComponent({
  name: "UserList",
  setup() {
    const usersData = generateUsers();
    const users = reactive(usersData);
    const filteredUsersList = ref([...users]);
    const selectedGenderFilter = ref("all");

    const applyGenderFilter = (gender: string) => {
      selectedGenderFilter.value = gender;
      filteredUsersList.value =
        gender === "all"
          ? users
          : users.filter((user) => user.gender === gender);
    };

    return {
      filteredUsersList,
      applyGenderFilter,
    };
  },
});

function generateUsers(): User[] {
  const users: User[] = [];

  for (let i = 0; i < 10; i++) {
    const newUser = new User(
      `Ім'я${i}`,
      `Прізвище${i}`,
      i % 2 === 0 ? "Male" : "Female",
      15 + i,
      `Посада${i}`,
      ["Читання", "Ігри", "Спорт"]
    );
    users.push(newUser);
  }

  return users;
}
</script>

<style scoped>
.user-container {
  display: flex;
  flex-wrap: wrap;
}

.filter-toolbar {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
}

.filter-toolbar button {
  padding: 8px 12px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

.filter-toolbar button:hover {
  background-color: #0056b3;
}

.user-card {
  border: 1px solid #333;
  padding: 12px;
  width: 30%;
  margin: 8px;
  border-radius: 8px;
  text-align: center;
}

.adult {
  background-color: #0aea3e;
}

.minor {
  background-color: #4000ff;
}

.user-avatar {
  width: 140px;
  height: 140px;
  border-radius: 50%;
  object-fit: cover;
  margin-bottom: 8px;
}

.empty {
  margin: 20px;
  text-align: center;
  color: #777;
}
</style>
