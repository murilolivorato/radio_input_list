<template>
  <div v-if="componentIsLoaded">
    <div  v-for="i  in Math.ceil(listOptions.length / dividedby ) " :key="i" :class="'columns ' + stylecol">
        <!-- col -->
        <template   v-for="(feature , index) in listOptions.slice((i - 1) * dividedby , i * dividedby )" :key="index">
            <div :class="radioStyle"  >
                <input :value="feature.id" :id="name + '_' + feature.id"  v-model="setListValue" type="radio" >
                <label  :class="'radio-label ' + labelstyle " :for="name + '_' + feature.id"  >{{ feature.title }}</label>
            </div>
          <template v-if="listOptions.length % dividedby != 0 && Object.keys(listOptions).length-1 == (( i * dividedby ) + index ) - dividedby  ">
            <div class="column empty" :key="j + index + 1"  v-for="j  in makeEmptyCols(index)"></div>
          </template>
        </template>
        <!-- end col -->
    </div>
  </div>
</template>

<script setup>
import { computed, onMounted, ref } from 'vue'
const emit = defineEmits(['update:value'])
const props = defineProps({
  dividedby: { type: Number },
  list: { type: Array },
  value: { type: [Number, String] },
  name: { type: String },
  size: { type: String, default: null },
  empyvalue: { type: Boolean, default: false },
  emptytext: { type: String, default: 'Selecione' },
  stylecol: { type: String, default: '' },
  labelstyle: { type: String, default: '' }
})
const listValue = ref(null)
const listOptions = ref([])
const componentIsLoaded = ref(false)

const setFormOptions = () => {
  if (props.empyvalue) {
    listOptions.value = [...[{ id: null, title: props.emptytext }], ...props.list]
    return
  }
  listOptions.value = Object.assign([], props.list)
}

const makeEmptyCols = (index) => {
  switch (index) {
    case 0:
      return props.dividedby - 1
    case 1:
      return props.dividedby - 2
    case 2:
      return props.dividedby - 3
    default:
      return props.dividedby - 4
  }
}

const radioStyle = computed(() => {
  if (!props.size) {
    return 'column radio'
  }

  if (props.size === 'small') {
    return 'column radio-small'
  }
  return 'column'
})

const setListValue = computed({
  get () {
    return listValue.value
  },
  set (value) {
    listValue.value = value
    emit('update:value', value)
  }
})

onMounted(async () => {
  setFormOptions()
  listValue.value = props.value
  // COMPONENT IS LOADED
  componentIsLoaded.value = true
})

</script>

<style scoped>
    /* ----------------------- divs */

    .radio {
        margin: 0.5rem;
    }
    .radio input[type="radio"] {
        position: absolute;
        opacity: 0;
    }
    .radio input[type="radio"] + .radio-label:before {
        content: '';
        background: #f4f4f4;
        border-radius: 100%;
        border: 1px solid #b4b4b4;
        display: inline-block;
        width: 1.4em;
        height: 1.4em;
        position: relative;
        top: -0.2em;
        margin-right: 1em;
        vertical-align: top;
        cursor: pointer;
        text-align: center;
        -webkit-transition: all 250ms ease;
        transition: all 250ms ease;
    }
    .radio-small {
        margin:0px;
      padding:12px 0 0 16px;
    }
    .txtwhite {
      color:#FFF;
    }
    .radio-small input[type="radio"] {
        position: absolute;
        opacity: 0;
    }

    .radio-small input[type="radio"] + .radio-label:before {
        content: '';
        background: #f4f4f4;
        border-radius: 100%;
        border: 1px solid #b4b4b4;
        display: inline-block;
        width: 1.2em;
        height: 1.2em;
        position: relative;
        margin-right: 1em;
        vertical-align: top;
        cursor: pointer;
        text-align: center;
        top: -0.1em;
        -webkit-transition: all 250ms ease;
        transition: all 250ms ease;
    }
    input[type="radio"]:checked + .radio-label:before {
        background-color: #3197EE;
        box-shadow: inset 0 0 0 4px #f4f4f4;
    }
    input[type="radio"]:focus + .radio-label:before {
        outline: none;
        border-color: #3197EE;
    }
    input[type="radio"]:disabled + .radio-label:before {
        box-shadow: inset 0 0 0 4px #f4f4f4;
        border-color: #b4b4b4;
        background: #b4b4b4;
    }
    input[type="radio"] + .radio-label:empty:before {
        margin-right: 0;
    }
    .radio-small label.radio-label {
      padding:0px !important;
      font-size:16px;
    }
    div.column {
      padding-bottom:20px;
    }
</style>
