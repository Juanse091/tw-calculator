<template>
  <div class="container-calculator">
    <section class="ingresos">
      <label class="label" for="ingresos">Ingresos</label>
      <input class="inp" type="text" v-model="formattedValue" placeholder="Ingresar datos">
    </section>

    <section class="cuentas">
      <div class="col">
        <label class="label" for="Ganancias">Ganancias</label>
        <input class="inp" disabled :value="formattedGanancias">
      </div>
      <div class="col">
        <label class="label" for="Impuestos">Impuestos</label>
        <input class="inp" disabled :value="formattedImpuestos">
      </div>
      <div class="col">
        <label class="label" for="CBP">CBP</label>
        <input class="inp" disabled :value="formattedCBP">
      </div>
      <div class="col">
        <label class="label" for="Gastosop">G. Optvs</label>
        <input class="inp" disabled :value="formattedGastosOp">
      </div>
    </section>

    <section class="percentages">
      <div class="col">
        <input class="per" type="number" min="0" max="100" placeholder="%" v-model.number="per_ganancias">
      </div>
      <div class="col">
        <input class="per" type="number" min="0" max="100" placeholder="%" v-model.number="per_impuestos">
      </div>
      <div class="col">
        <input class="per" type="number" min="0" max="100" placeholder="%" v-model.number="per_CBP">
      </div>
      <div class="col">
        <input class="per" type="number" min="0" max="100" placeholder="%" v-model.number="per_gastosOp">
      </div>
    </section>

    <button @click="calculate">Calcular</button>
  </div>
</template>

<script>
export default {
  name: 'InterfaceCalculator',
  data() {
    return {
      per_ganancias: 0,
      per_impuestos: 0,
      per_CBP: 0,
      per_gastosOp: 0,
      ganancias: 0,
      impuestos: 0,
      CBP: 0,
      gastosOp: 0,
      value: ''
    };
  },
  computed: {
    formattedValue: {
      get() {
        return new Intl.NumberFormat('es-MX').format(this.value);
      },
      set(newValue) {
        this.value = newValue.replace(/,/g, '');
      }
    },
    formattedGanancias() {
      return this.formatCurrency(this.ganancias);
    },
    formattedImpuestos() {
      return this.formatCurrency(this.impuestos);
    },
    formattedCBP() {
      return this.formatCurrency(this.CBP);
    },
    formattedGastosOp() {
      return this.formatCurrency(this.gastosOp);
    }
  },
  methods: {
    formatCurrency(value) {
      return new Intl.NumberFormat('es-MX', {
        style: 'currency',
        currency: 'MXN'
      }).format(value);
    },
    calculate() {
      const sum_percent = this.per_ganancias + this.per_CBP + this.per_impuestos + this.per_gastosOp;

      if (sum_percent > 100) {
        alert('Los porcentajes deben sumar 100%');
        return;
      }

      const numericValue = parseFloat(this.value) || 0;

      this.ganancias = numericValue * (this.per_ganancias / 100);
      this.impuestos = numericValue * (this.per_impuestos / 100);
      this.CBP = numericValue * (this.per_CBP / 100);
      this.gastosOp = numericValue * (this.per_gastosOp / 100);

      // Guardar en localStorage
      localStorage.setItem('calculatorValues', JSON.stringify({
        per_ganancias: this.per_ganancias,
        per_impuestos: this.per_impuestos,
        per_CBP: this.per_CBP,
        per_gastosOp: this.per_gastosOp,
        ganancias: this.ganancias,
        impuestos: this.impuestos,
        CBP: this.CBP,
        gastosOp: this.gastosOp,
        value: this.value // Guardar el valor de los ingresos
      }));
    }
  },
  mounted() {
    const savedValues = localStorage.getItem('calculatorValues');
    if (savedValues) {
      const {
        per_ganancias,
        per_impuestos,
        per_CBP,
        per_gastosOp,
        ganancias,
        impuestos,
        CBP,
        gastosOp,
        value // Recuperar el valor de los ingresos
      } = JSON.parse(savedValues);

      this.per_ganancias = per_ganancias;
      this.per_impuestos = per_impuestos;
      this.per_CBP = per_CBP;
      this.per_gastosOp = per_gastosOp;
      this.ganancias = ganancias;
      this.impuestos = impuestos;
      this.CBP = CBP;
      this.gastosOp = gastosOp;
      this.value = value; // Restaurar el valor de los ingresos
    }
  }
};
</script>

<style scoped>
.container-calculator {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  background-color: black;
  color: white;
  padding: 20px;
}

.cuentas, .percentages {
  display: flex;
  gap: 1rem;
  width: 100%;
  margin-top: 1rem;
}

.col {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.label {
  user-select: none;
}

.inp, .per {
  width: 100%;
  padding: 0.5rem;
  font-size: 1rem;
}

button {
  padding: 0.5rem 1rem;
  background-color: #007BFF;
  color: white;
  border: none;
  cursor: pointer;
  margin-top: 1rem;
  margin-bottom: 2rem;
}

button:hover {
  background-color: #0056b3;
}

/* Para Chrome, Safari, Edge y Opera */
input[type="number"]::-webkit-outer-spin-button,
input[type="number"]::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

/* Para Firefox */
input[type="number"] {
  -moz-appearance: textfield;
}
</style>
