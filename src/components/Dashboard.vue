<template>
  <div id="burger-table">
    <Message :msg="msg" v-show="msg" />
    <div>
      <div id="burger-table-heading">
        <div class="order-id">#:</div>
        <div>Cliente:</div>
        <div>Pão:</div>
        <div>Carne:</div>
        <div>Opcionais:</div>
        <div>Ações:</div>
      </div>
      <div id="burger-table-rows">
        <div class="burger-table-row" v-for="burger in burgers" :key=burger.id>
          <div class="order-number">{{ burger.id }}</div>
          <div>{{ burger.name }}</div>
          <div>{{ burger.bread }}</div>
          <div>{{ burger.meat }}</div>
          <div>
            <ul>
              <li v-for="(optional, index) in burger.optional" :key="index">{{ optional }}</li>
            </ul>
          </div>
          <div>
            <select name="status" id="status" class="status" @change="updateBurger($event, burger.id)">
              <option value="">Selecione</option>
              <option v-for="s in status" :key="s.id" :value="s.type" :selected="burger.status === s.type">{{ s.type }}</option>
            </select>
            <button class="btn-delete" @click="deleteBurger(burger.id)">Cancelar</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import Message from './Message';

  export default {
    name: 'Dashboard',
    components: {
      Message
    },
    data() {
      return {
        burgers: null,
        burger_id: null,
        status: [],
        msg: null
      }
    },
    methods: {
      async getStatus() {
        const req = await fetch('http://localhost:3000/status');
        const data = await req.json();

        this.status = data;
      },
      async getOrders() {
        const req = await fetch('http://localhost:3000/burgers');
        const data = await req.json();

        this.burgers = data;

        this.getStatus();
      },
      async deleteBurger(id) {
        const req = await fetch(`http://localhost:3000/burgers/${id}`, {
          method: 'DELETE'
        });

        // escrevendo mensagem de sucesso na tela
        this.msg = `Pedido removido com sucesso`;

        // limpando mensagem da tela
        setTimeout(() => this.msg = '', 3000);

        this.getOrders();
      },
      async updateBurger(event, id) {
        const option = event.target.value;

        const dataJson = JSON.stringify({ status: option });
        const req = await fetch(`http://localhost:3000/burgers/${id}`, {
          method: 'PATCH',
          headers: {
            'Content-Type': 'application/json'
          },
          body: dataJson
        });

        const res = await req.json();

        // escrevendo mensagem de sucesso na tela
        this.msg = `Pedido Nº ${res.id} atualizado com sucesso`;

        // limpando mensagem da tela
        setTimeout(() => this.msg = '', 3000);
      }
    },
    mounted() {
      this.getOrders();
    }
  }
</script>

<style scoped>
  #burger-table {
    max-width: 1200px;
    margin: 0 auto;
  }

  #burger-table-heading,
  #burger-table-rows,
  .burger-table-row {
    display: flex;
    flex-wrap: wrap;
  }

  #burger-table-heading {
    font-weight: bold;
    padding: 12px;
    border-bottom: 3px solid #333;
  }

  #burger-table-heading div,
  .burger-table-row div {
    width: 19%;
  }

  .burger-table-row {
    width: 100%;
    padding: 12px;
    border: 1px solid #CCC;
  }

  #burger-table-heading .order-id,
  .burger-table-row .order-number {
    width: 5%;
  }

  select {
    padding: 12px 6px;
    margin-right: 12px;
  }

  .btn-delete {
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

  .btn-delete:hover {
    background-color: transparent;
    color: #222;
  }
</style>
