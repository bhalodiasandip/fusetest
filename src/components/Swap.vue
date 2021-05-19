<template>
  <div class="d-flex justify-content-center">

    <b-card class="col-5 swap-container" no-body bg-variant="dark" text-variant="white" title="SWAP">
      <template #header>
        <h4 class="mb-0">SWAP</h4>
      </template>
      <b-card-body>
        <div>
          <b-input-group>

            <b-form-input @input="onChangeFromValue" v-model="fromValue" placeholder="0.0"></b-form-input>
            <template #append>
              <v-select class="currency-select" :clearable="false" placeholder="Select a token" :options="options"
                        :reduce="currency => currency.code"
                        v-model="fromCurrency"
                        label="code">
                <template v-slot:option="option">
                  <img :src="option.icon" class="currency-icon"/>
                  <span class="currency-code">{{ option.code }}</span>
                </template>
                <template v-slot:selected-option="option">
                  <img :src="option.icon" class="currency-icon"/>
                  <span class="currency-code">{{ option.code }}</span>
                </template>
              </v-select>
            </template>

          </b-input-group>
        </div>
        <div class="my-2">
          <span @click="swapCurrency"><img height="35px" width="35px" src="/images/swap.png" /></span>
        </div>
        <div>
          <b-input-group>

            <b-form-input @input="onChangeToValue" v-model="toValue" placeholder="0.0"></b-form-input>
            <template #append>
              <v-select class="currency-select" :clearable="false" placeholder="Select a token" :options="options"
                        :reduce="currency => currency.code"
                        v-model="toCurrency"
                        label="code">
                <template v-slot:option="option">
                  <img :src="option.icon" class="currency-icon"/>
                  <span class="currency-code">{{ option.code }}</span>
                </template>
                <template v-slot:selected-option="option">
                  <img :src="option.icon" class="currency-icon"/>
                  <span class="currency-code">{{ option.code }}</span>
                </template>
              </v-select>
            </template>

          </b-input-group>
        </div>
        <b-row v-if="fromCurrency && toCurrency && perUnitValue" class="mt-4">
          <b-col cols="2" class="text-left">Price</b-col>
          <b-col cols="10" class="text-right">
            {{perUnitValue}} {{toPerUnit}} per {{fromPerUnit}}
            <span class="per-unit-price" @click="swapPerUnit"><img height="22px" width="22px" src="/images/horizontal_swap.png" /></span>
          </b-col>
        </b-row>
      </b-card-body>


    </b-card>
  </div>
</template>

<script>
export default {
  name: 'Swap',
  props: {
    msg: String
  },
  watch: {
    fromCurrency: function (val) {
      if(val && this.toCurrency) {
        let selectedFromCurrency = this.options.find(option => option.code === val)
        let selectedToCurrency = this.options.find(option => option.code === this.toCurrency)
        this.fromPerUnit = val
        this.toPerUnit = selectedToCurrency.code
        if(this.lastChange == "from") {
          let inputValue = this.fromValue
          let convertedValue = (inputValue * selectedToCurrency.perFuse) / selectedFromCurrency.perFuse
          this.toValue = convertedValue
        }
        else{
          let inputValue = this.toValue;
          let convertedValue = (inputValue * selectedFromCurrency.perFuse) / selectedToCurrency.perFuse
          this.fromValue = convertedValue
        }
      }
    },
    toCurrency: function (val) {
      if(val && this.fromCurrency) {
        let selectedToCurrency = this.options.find(option => option.code === val)
        let  selectedFromCurrency = this.options.find(option => option.code === this.fromCurrency)
        this.fromPerUnit = selectedFromCurrency.code
        this.toPerUnit = val
        if(this.lastChange == "from") {
          let inputValue = this.fromValue
          let convertedValue = (inputValue * selectedToCurrency.perFuse) / selectedFromCurrency.perFuse
          this.toValue = convertedValue
        }
        else{
          let inputValue = this.toValue;
          let convertedValue = (inputValue * selectedFromCurrency.perFuse) / selectedToCurrency.perFuse
          this.fromValue = convertedValue
        }
      }
    },
    fromPerUnit: function(val){
      let perUnitFromCurrency = this.options.find(option => option.code === val)
      let perUnitToCurrency = this.options.find(option => option.code === this.toPerUnit)
      if(perUnitFromCurrency && perUnitToCurrency) {
        let convertedValue = perUnitToCurrency.perFuse / perUnitFromCurrency.perFuse
        this.perUnitValue = convertedValue
      }
    }
  },
  data() {
    return {
      fromValue: 0.0,
      toValue: 0.0,
      fromCurrency: "",
      toCurrency: "",
      fromPerUnit: "",
      toPerUnit: "",
      perUnitValue: null,
      lastChange: "from",
      options: [
        {
          code: 'FUSE',
          icon: '/images/FUSE.svg',
          perFuse: 1,
          disabled: false
        },
        {
          code: 'DAI',
          icon: '/images/DAI.png',
          perFuse: 0.261426,
          disabled: false
        },
        {
          code: 'fUSD',
          icon: '/images/fUSD.png',
          perFuse: 0.104998,
          disabled: false
        },
        {
          code: 'G$',
          icon: '/images/G$.png',
          perFuse: 505.765,
          disabled: false
        },
        {
          code: 'GRT',
          icon: '/images/GRT.png',
          perFuse: 0.112828,
          disabled: false
        },
        {
          code: 'KNC',
          icon: '/images/KNC.png',
          perFuse: 0.0354588,
          disabled: false
        },
        {
          code: 'LINK',
          icon: '/images/LINK.png',
          perFuse: 0.00447887,
          disabled: false
        },
        {
          code: 'OM',
          icon: '/images/OM.png',
          perFuse: 0.382165,
          disabled: false
        },
        {
          code: 'USDC',
          icon: '/images/USDC.png',
          perFuse: 0.119826,
          disabled: false
        },
        {
          code: 'USDT',
          icon: '/images/USDT.png',
          perFuse: 0.132892,
          disabled: false
        },
        {
          code: 'WBTC',
          icon: '/images/WBTC.png',
          perFuse: 0.000003885,
          disabled: false
        },
        {
          code: 'WETH',
          icon: '/images/WETH.png',
          perFuse: 0.0000550373,
          disabled: false
        },
        {
          code: 'WFUSE',
          icon: '/images/WFUSE.png',
          perFuse: 1,
          disabled: false
        },
      ]
    }
  },
  methods: {
    onChangeFromValue(){
      if(this.fromCurrency && this.toCurrency) {
        let selectedFromCurrency = this.options.find(option => option.code === this.fromCurrency)
        let selectedToCurrency = this.options.find(option => option.code === this.toCurrency)
        let convertedValue = (this.fromValue * selectedToCurrency.perFuse) / selectedFromCurrency.perFuse
        this.toValue = convertedValue
        this.lastChange = "from"
      }
    },
    onChangeToValue(){
      if(this.fromCurrency && this.toCurrency) {
        let selectedFromCurrency = this.options.find(option => option.code === this.fromCurrency)
        let selectedToCurrency = this.options.find(option => option.code === this.toCurrency)
        let convertedValue = (this.toValue * selectedFromCurrency.perFuse) / selectedToCurrency.perFuse
        this.fromValue = convertedValue
        this.lastChange = "to"
      }
    },
    swapCurrency(){
      if(this.fromCurrency && this.toCurrency && this.fromValue)
      {
        let currentFromCurrencyCode = this.fromCurrency;
        let currentToCurrencyCode = this.toCurrency;
        this.toCurrency = currentFromCurrencyCode;
        this.fromCurrency = currentToCurrencyCode;
      }
    },
    swapPerUnit(){
      if(this.fromPerUnit && this.toPerUnit)
      {
        let currentFromPerUnit = this.fromPerUnit;
        let currentToPerUnit = this.toPerUnit;
        this.toPerUnit = currentFromPerUnit;
        this.fromPerUnit = currentToPerUnit;
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}

.swap-container::v-deep .per-unit-price{
  position: relative;
  top: -2px;
}
</style>
