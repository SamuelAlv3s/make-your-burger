<template>
  <div>
    <form id="burger-form" @submit="createBurger($event)">
      <div class="input-container">
        <label for="name">Nome do Cliente</label>
        <input
          type="text"
          id="name"
          v-model="name"
          placeholder="Digite o nome do cliente"
        />
      </div>

      <div class="input-container">
        <label for="bread">Escolha o pão:</label>
        <select name="bread" id="bread" v-model="bread">
          <option value="">Selecione o seu pão</option>
          <option v-for="bread in breads" :key="bread.id" :value="bread.tipo">
            {{ bread.tipo }}
          </option>
        </select>
      </div>

      <div class="input-container">
        <label for="meat">Escolha a carne:</label>
        <select name="meat" id="meat" v-model="meat">
          <option value="">Selecionea o tipo de carne</option>
          <option v-for="meat in meats" :key="meat.id" :value="meat.tipo">
            {{ meat.tipo }}
          </option>
        </select>
      </div>

      <div class="optional-container input-container">
        <label id="optional-title" for="optional"
          >Selecione os opcionais:</label
        >
        <div
          v-for="optional in optionalsData"
          :key="optional.id"
          class="checkbox-container"
        >
          <input
            type="checkbox"
            name="optional"
            v-model="optionals"
            :value="optional.tipo"
          />
          <span>{{ optional.tipo }}</span>
        </div>
      </div>

      <div class="input-container">
        <input type="submit" class="submit-btn" value="Criar meu Burger" />
      </div>
    </form>
  </div>
</template>
<script>
export default {
  name: "BurgerForm",
  data() {
    return {
      breads: null,
      meats: null,
      optionalsData: null,
      name: null,
      bread: null,
      meat: null,
      optionals: [],
      status: "Solicitado",
      msg: null,
    };
  },
  methods: {
    async getIngredients() {
      const req = await fetch("http://localhost:3000/ingredientes");
      const data = await req.json();
      console.log(data);

      this.breads = data.paes;
      this.meats = data.carnes;
      this.optionalsData = data.opcionais;
    },
    async createBurger(e) {
      e.preventDefault();

      const data = {
        name: this.name,
        bread: this.bread,
        meat: this.meat,
        optionals: Array.from(this.optionals),
        status: "Solicitado",
      };

      const dataJson = JSON.stringify(data);

      const req = await fetch("http://localhost:3000/burgers", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: dataJson,
      });

      const res = await req.json();
      console.log(res);

      this.name = "";
      this.bread = "";
      this.meat = "";
      this.optionals = "";
    },
  },
  mounted() {
    this.getIngredients();
  },
};
</script>
<style scoped>
#burger-form {
  max-width: 400px;
  margin: 0 auto;
}

.input-container {
  display: flex;
  flex-direction: column;
  margin-bottom: 20px;
}

label {
  font-weight: bold;
  margin-bottom: 15px;
  color: #222;
  padding: 5px 10px;
  border-left: 4px solid #fcba03;
}

input,
select {
  padding: 5px 10px;
  width: 100%;
}

.optional-container {
  flex-direction: row;
  flex-wrap: wrap;
}

#optional-title {
  width: 100%;
}

.checkbox-container {
  display: flex;
  align-items: center;
  width: 50%;
  margin-bottom: 20px;
}

.checkbox-container span,
.checkbox-container input {
  width: auto;
}

.checkbox-container span {
  margin-left: 6px;
  font-weight: bold;
}

.submit-btn {
  width: 100%;
  background-color: #222;
  color: #fcba03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  transition: 0.5s;
}

.submit-btn:hover {
  background-color: transparent;
  color: #222;
}
</style>
