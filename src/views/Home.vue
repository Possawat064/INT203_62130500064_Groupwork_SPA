<template>
  <div class="bg-white h-screen home">
    <h1 class="text-4xl pt-16 pb-16 text-center font-bold">Menu</h1>
    <div class="container pl-72 text-2xl mb-16 border-2">
      <div class="Menu-container">

        <form @submit.prevent="submitForm">
          <base-card>
            <h2 class="heading pl-16 text-3xl mb-4 mt-4 font-bold">Add Menu</h2>

            <label class="label" for="name">Menu Name </label>

            <input
              class="input"
              :class="{ 'bg-red-50': invalidMenuNameInput }"
              id="name"
              type="text"
              v-model.trim="enteredMenuName"
              @blur="validateMenuName"
            />

            <p v-if="invalidNameInput" class="text-red-500">
              Please enter Menu name!
            </p>

            <label class="label" for="type"> Type </label>

            <input
              class="input"
              :class="{ 'bg-red-50': invalidTypeInput }"
              id="type"
              type="text"
              v-model.trim="enteredType"
              @blur="validateType"
            />

            <p v-if="invalidTypeInput" class="text-red-500">
              Please enter Menu Type !
            </p>

            <button class="btn bg-green-500 text-white py-2 px-4 rounded ml-8">
              Add Menu
            </button>
          </base-card>
        </form>
      </div>
    </div>

    <div class="pl-72 pr-72 text-2xl">
      <ul v-for="food in Menu" :key="food.id">
        <base-card>
          <li>
            Menu Name :
            <span>{{ food.name }}</span> | Type :
            <span> {{ food.type }}</span>
            <button @click="showData(food)" class="bg-green-500 m-1">
              <img src="../assets/edit.svg" alt="" />
            </button>
            <button @click="deleteMenu(food.id)" class="bg-red-500 m-1">
              <img src="../assets/delete.svg" alt="" />
            </button>
          </li>
        </base-card>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "Menu",
  data() {
    return {
      Menu: [],
      url: "http://localhost:5000/Menu",
      isEdit: false,
      editId: "",
      enteredMenuName: "",
      enteredType: "",
      invalidMenuNameInput: false,
      invalidTypeInput: false,
    };
  },

  methods: {
    submitForm() {
      this.invalidMenuNameInput = this.enteredMenuName === "" ? true : false;
      this.invalidTypeInput = this.enteredType === "" ? true : false;
      console.log(`name value: ${this.enteredMenuName}`);
      console.log(`type value: ${this.enteredType}`);
      console.log(`invalid name: ${this.invalidMenuNameInput}`);
      console.log(`invalid type: ${this.invalidTypeInput}`);

      if (this.enteredMenuName !== "" && this.enteredType !== "") {
        if (this.isEdit) {
          this.editMenu({
            id: this.editId,
            name: this.enteredMenuName,
            type: this.enteredType,
          });
        } else {
          this.addMenu({
            name: this.enteredMenuName,
            type: this.enteredType,
          });
        }
      }
      this.enteredMenuName = "";
      this.enteredType = "";
    },

    showData(oldMenu) {
      this.isEdit = true;
      this.editId = oldMenu.id;
      this.enteredMenuName = oldMenu.name;
      this.enteredType = oldMenu.type;
    },

    async editMenu(editingMenu) {
      const res = await fetch(`${this.url}/${editingMenu.id}`, {
        method: "PUT",
        headers: {
          "content-type": "application/json",
        },
        body: JSON.stringify({
          name: editingMenu.name,
          type: editingMenu.type,
        }),
      });
      const data = await res.json();
      this.Menu = this.Menu.map((food) =>
        food.id === editingMenu.id
          ? { ...food, name: data.name, type: data.type }
          : food
      );
      this.isEdit = false;
      this.editId = "";
      this.enteredMenuName = "";
      this.enteredType = "";
    },

    async getMenu() {
      const res = await fetch("http://localhost:5000/Menu");
      const data = await res.json();
      return data;
    },

    async deleteMenu(deleteId) {
      await fetch(`${this.url}/${deleteId}`, {
        method: "DELETE",
      });
      this.Menu = this.Menu.filter((food) => food.id !== deleteId);
    },

    async addMenu(newMenu) {
      const res = await fetch(this.url, {
        method: "POST",
        headers: {
          "content-type": "application/json",
        },
        body: JSON.stringify({
          name: newMenu.name,
          type: newMenu.type,
        }),
      });
      const data = await res.json();
      this.Menu = [...this.Menu, data];
    },
  },

  async created() {
    this.Menu = await this.getMenu();
  },
};
</script>