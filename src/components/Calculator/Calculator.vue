<template>
  <div class="calculator-container">
    <!--     -->
    <div class="calculator-input-container">
      <span class="label">Utalt összeg</span>
      <input
        class="calculator-input"
        id="money"
        type="text"
        placeholder="0"
        :value="formattedMoney"
        @input="updateMoney"
      />
    </div>
    <!-- s -->
    <div class="calculator-row">
      <table id="firstTable">
        <tbody>
          <tr>
            <td><span class="title">Tranzakciós illeték kalkulátor</span></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td><span class="title">Bank ktg. Kalkulátor</span></td>
            <td colspan="3">
              <span class="title">Vállalkozást terhelő ktg össz</span>
            </td>
            <td><span class="title">Ibanfirst</span></td>
          </tr>
          <tr>
            <td></td>
            <td colspan="2">2013.01.01</td>
            <td colspan="2">2024.08.01</td>
            <td></td>
            <td>Előtt</td>
            <td>2024.08.01</td>
            <td>Után</td>
            <td>2024.08.01</td>
          </tr>
          <tr>
            <td></td>
          </tr>
          <tr>
            <td></td>
            <td>0,30%</td>
            <td>10 000 Ft</td>
            <td>0,45%</td>
            <td>20 000 Ft</td>
            <td>0,03%</td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
          </tr>
          <tr>
            <td>Banki átutalás</td>
            <td>{{ tranzIlletekBankiElso }} Ft</td>
            <td>{{ tranzIlletekBankiMax }} Ft</td>
            <td>{{ tranzIlletekBankimasodik }} Ft</td>
            <td>{{ tranzIlletekBankiMaxMasodik }} Ft</td>
            <td>{{ bankKtgUtalas }} Ft</td>
            <td>{{ valalkozasBankiElottMasodik }} Ft</td>
            <td></td>
            <td>{{ valalkozasBankiUtanMasodik }} FT</td>
            <td>5000 Ft</td>
          </tr>
          <tr>
            <td>Illetékmentességi határ</td>
            <td>20 000 Ft</td>
            <td></td>
            <td>50 000 Ft</td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
          </tr>
          <tr>
            <td></td>
          </tr>
          <tr>
            <td>Konverziós Illeték</td>
            <td></td>
            <td></td>
            <td>0,45%</td>
            <td>20 000 Ft</td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
          </tr>
          <tr>
            <td></td>
            <td></td>
            <td></td>
            <td>{{ tranzIlletekBankimasodik }} Ft</td>
            <td>{{ tranzIlletekBankiMaxMasodik }} Ft</td>
            <td></td>
            <td></td>
            <td></td>
            <td>{{ tranzIlletekBankiMaxMasodik }} Ft</td>
            <td></td>
          </tr>
          <tr>
            <td></td>
            <td></td>
            <td></td>
            <td>50 000 Ft</td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
            <td></td>
          </tr>
        </tbody>
      </table>
    </div>
    <!-- s -->
    <div class="calculator-input-container">
      <span class="label">Megtakarítás</span>
      <div class="megtakaritas" :class="{ negative: megtakaritas < 0 }">
        {{ megtakaritas }} Ft
      </div>
    </div>
  </div>
</template>

<script>
export default {
  setup() {
    return {};
  },
  data() {
    return {
      money: 80000000,
      save: 0,
    };
  },
  methods: {
    updateMoney(event) {
      const value = event.target.value
        .replace(/\s+/g, "")
        .replace("forint", "")
        .trim();
      const numericValue = parseFloat(value);
      this.money = isNaN(numericValue) ? 0 : numericValue;
    },
  },
  computed: {
    formattedMoney() {
      return this.money.toLocaleString("hu-HU");
    },
    tranzIlletekBankiElso() {
      const calculation = this.money * 0.003;
      return isNaN(calculation) || calculation === Infinity
        ? Number(0).toLocaleString("hu-HU", { maximumFractionDigits: 0 })
        : calculation.toLocaleString("hu-HU", { maximumFractionDigits: 0 });
    },
    tranzIlletekBankiMax() {
      let calculation = 0;

      if (this.tranzIlletekBankiElso < 10000) {
        calculation = this.tranzIlletekBankiElso;
      } else {
        calculation = 10000;
      }

      return isNaN(calculation) || calculation === Infinity
        ? Number(0).toLocaleString("hu-HU", { maximumFractionDigits: 0 })
        : calculation.toLocaleString("hu-HU", { maximumFractionDigits: 0 });
    },
    tranzIlletekBankimasodik() {
      const calculation = (this.money - 50000) * 0.0045;

      return isNaN(calculation) || calculation === Infinity
        ? Number(0).toLocaleString("hu-HU", { maximumFractionDigits: 0 })
        : calculation.toLocaleString("hu-HU", { maximumFractionDigits: 0 });
    },
    tranzIlletekBankiMaxMasodik() {
      let calculation = 0;

      if (parseInt(this.tranzIlletekBankimasodik.replace(/\s+/g, "")) < 20000) {
        calculation = parseInt(
          this.tranzIlletekBankimasodik.replace(/\s+/g, "")
        );
      } else {
        calculation = 20000;
      }
      return isNaN(calculation) || calculation === Infinity
        ? Number(0).toLocaleString("hu-HU", { maximumFractionDigits: 0 })
        : calculation.toLocaleString("hu-HU", { maximumFractionDigits: 0 });
    },
    bankKtgUtalas() {
      const calculation = this.money * 0.0003;
      return isNaN(calculation) || calculation === Infinity
        ? Number(0).toLocaleString("hu-HU", { maximumFractionDigits: 0 })
        : calculation.toLocaleString("hu-HU", { maximumFractionDigits: 0 });
    },
    valalkozasBankiElottMasodik() {
      let tranz = 0;

      if (this.tranzIlletekBankiElso < 10000) {
        tranz = this.tranzIlletekBankiElso;
      } else {
        tranz = 10000;
      }
      const calculation =
        parseInt(tranz) + parseInt((this.money * 0.0003).toFixed(0));
      return isNaN(calculation) || calculation === Infinity
        ? Number(0).toLocaleString("hu-HU", { maximumFractionDigits: 0 })
        : calculation.toLocaleString("hu-HU", { maximumFractionDigits: 0 });
    },
    valalkozasBankiUtanMasodik() {
      let tranz = 0;

      if (parseInt(this.tranzIlletekBankimasodik.replace(/\s+/g, "")) < 20000) {
        tranz = this.tranzIlletekBankimasodik.replace(/\s+/g, "");
      } else {
        tranz = 20000;
      }
      const calculation = parseInt(tranz) + this.money * 0.0003;
      return isNaN(calculation) || calculation === Infinity
        ? Number(0).toLocaleString("hu-HU", { maximumFractionDigits: 0 })
        : calculation.toLocaleString("hu-HU", { maximumFractionDigits: 0 });
    },
    megtakaritas() {
      const calculation =
        parseInt(this.valalkozasBankiUtanMasodik.replace(/\s+/g, "")) - 5000;
      return isNaN(calculation) || calculation === Infinity
        ? Number(0).toLocaleString("hu-HU", { maximumFractionDigits: 0 })
        : calculation.toLocaleString("hu-HU", { maximumFractionDigits: 0 });
    },
  },
};
</script>

<style lang="postcss" scoped>
table {
  @apply text-sm primary-font font-bold very-dark-cyan-text;
  font-family: Helvetica;
  border-collapse: collapse;
  width: 100%;
}

td {
  border: 1px solid var(--dark-grayish-cyan-1);
  padding: 4px;
  text-align: center;
}

td:empty {
  border: none;
}

#firstTable tr td:nth-child(1) {
  text-align: start;
}

#firstTable tr:nth-child(1) td,
#firstTable tr:nth-child(3) td:nth-child(1),
#firstTable tr:nth-child(7) td:nth-child(1) {
  border: none;
}

.calculator-container {
  @apply flex flex-wrap bg-white w-9/12 h-auto mx-auto rounded-xl p-8;
  margin-top: 5vh;
}

/* Input Styles */
.calculator-input-container {
  @apply w-full py-5 flex-col flex;
}
.calculator-input {
  @apply h-7  rounded p-5 text-center primary-font very-dark-cyan-text font-semibold cursor-pointer;
  background-color: var(--light-grayish-cyan-2);
  font-size: var(--form-font-size);
}

.megtakaritas {
  @apply h-7  rounded p-5 primary-font very-dark-cyan-text font-semibold;
  background-color: var(--light-grayish-cyan-2);
  font-size: var(--form-font-size);
  display: flex;
  align-items: center;
  justify-content: center;
  color: darkgreen;
  background-color: #37cf38;
}

.calculator-input:focus-visible {
  @apply outline-none;
}

.negative {
  background-color: red;
  color: white;
}

.calculator-input.error,
.calculator-input.error:focus-visible,
.calculator-input.error:hover:enabled {
  outline: 2px solid hsl(13deg 33% 60%);
}
.calculator-input:focus-visible,
.calculator-input:hover:enabled {
  /* outline-color: hsl(173deg 31% 50%); */
  outline: 2px solid hsl(173deg 31% 50%);
}

.label {
  @apply text-center text-sm my-2 primary-font font-bold dark-grayish-cyan-1-text;
  font-family: Helvetica;
}
.label.error {
  color: hsl(13deg 33% 60%);
}

.title {
  font-family: Helvetica;
  @apply text-sm primary-font font-bold very-dark-cyan-text;
}

.calculator-row {
  @apply w-full;
}

/* Utility Classes */
.primary-font {
  font-family: Helvetica;
}
.strong-cyan-text {
  color: var(--strong-cyan);
}
.very-dark-cyan-text {
  color: var(--very-dark-cyan);
}
.dark-grayish-cyan-1-text {
  color: var(--dark-grayish-cyan-1);
}
.dark-grayish-cyan-2-text {
  color: var(--dark-grayish-cyan-2);
}
.bg-very-dark-cyan {
  background-color: var(--very-dark-cyan);
}
.bg-strong-cyan {
  background-color: var(--strong-cyan);
}
.bg-dark-grayish-cyan-1 {
  background-color: var(--dark-grayish-cyan-1);
}
.bg-dark-grayish-cyan-2 {
  background-color: var(--dark-grayish-cyan-2);
}

/* Custom styling*/
input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

@media only screen and (max-width: 982px) {
  .calculator-container {
    @apply flex-col mx-0 w-full;
  }
  .calculator-input-container {
    @apply px-0;
  }
  .reset-button {
    @apply my-10;
  }
  .calculator-input {
    font-size: 24px;
  }
  .calculator-column-left {
    @apply px-0;
  }
}
</style>
