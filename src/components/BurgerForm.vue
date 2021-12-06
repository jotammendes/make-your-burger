<template>
  <div>
    <p>Componente de mensagem de sucesso</p>
    <div>
      <form id="burger-form">
        <div class="input-container">
          <label for="name">Nome do Cliente</label>
          <input type="text" id="name" name="name" v-model="name" placeholder="Digite o seu nome">
        </div>
        <div class="input-container">
          <label for="bread">Escolha o pão:</label>
          <select id="bread" name="bread" v-model="bread">
            <option value="">Selecione o seu pão</option>
            <option v-for="bread in breads" :key="bread.id" :value="bread.type">{{ bread.type }}</option>
          </select>
        </div>
        <div class="input-container">
          <label for="meat">Escolha a carne:</label>
          <select id="meat" name="meat" v-model="meat">
            <option value="">Selecione a sua carne</option>
            <option v-for="meat in meats" :key="meat.id" :value="meat.type">{{ meat.type }}</option>
          </select>
        </div>
        <div id="optional-container" class="input-container">
          <label for="optional" id="optional-title">Selecione os opcionais:</label>
          <div class="checkbox-container" v-for="optionale in optionals" :key="optionale.id">
            <input type="checkbox" name="optional" v-model="optional" :value="optionale.type">
            <span>{{ optionale.type }}</span>
          </div>
        </div>
        <div class="input-container">
          <input type="submit" class="btn-submit" value="Criar meu Burger!">
        </div>
      </form>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'BurgerForm',
    data() {
      return {
        breads: null,
        meats: null,
        optionals: null,
        name: null,
        bread: null,
        meat: null,
        optional: [],
        status: 'Solicitado',
        msg: null
      }
    },
    methods: {
      async getIngredients() {
        const req = await fetch('http://localhost:3000/ingredients');
        const data = await req.json();

        this.breads = data.breads;
        this.meats = data.meats;
        this.optionals = data.optional;
      }
    },
    mounted() {
      this.getIngredients();
    }
  }
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
    border-left: 4px solid #FCBA03;
  }

  input, select {
    padding: 5px 10px;
    width: 300px;
  }

  #optional-container {
    flex-direction: row;
    flex-wrap: wrap;
  }

  #optional-title {
    width: 100%;
  }

  .checkbox-container {
    display: flex;
    align-items: flex-start;
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

  .btn-submit {
    background-color: #222;
    color: #FCBA03;
    font-weight: bold;
    border: 2px solid #222;
    padding: 10px;
    font-size: 16px;
    margin: 0 auto;
    cursor: pointer;
    transition: 0.5s;
  }

  .btn-submit:hover {
    background-color: transparent;
    color: #222;
  }
</style>
