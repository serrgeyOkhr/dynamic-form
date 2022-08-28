<template>
  <div class="form">
    <Select v-for="(param, index) in selectedParams" :key="index" :selectedParam="param" :optionValues="selectData" :selectId="index" @changeValue="changeParams" />
    <button @click="saveData">Сохранить</button>
  </div>
</template>

<script>
import { ref } from '@vue/reactivity';
import Select from "./Select.vue";
export default {
  name: 'HelloWorld',
  components: {
    Select
  },
  props: {

  },
  setup() {
    const DATA = [
      {
        select_city: "Novosibirsk",
        select_ceh: "ceh9",
        select_manager: ["Vadim", "Marina"],
      },
      {
        select_city: "Novosibirsk",
        select_ceh: "ceh4",
        select_manager: ["Alex", "Max"],
      },
      {
        select_city: "Omsk",
        select_ceh: "ceh1",
        select_manager: ["Alex", "Leo", "Artem"],
      },
      {
        select_city: "Kazan",
        select_ceh: "ceh",
        select_manager: ["Sergey"],
      },
    ];
    const ALL_CREW = [
      {
        select_CREW: "Novosibirsk_NightCrew_One",
        select_city: "Novosibirsk",
        select_workTime: "20:00 - 08:00",
      },
      {
        select_CREW: "Novosibirsk_NightCrew_Two",
        select_city: "Novosibirsk",
        select_workTime: "20:00 - 08:00",
      },
      {
        select_CREW: "Novosibirsk_DayCrew_One",
        select_city: "Novosibirsk",
        select_workTime: "08:00 - 20:00",
      },
      {
        select_CREW: "Novosibirsk_DayCrew_Two",
        select_city: "Novosibirsk",
        select_workTime: "08:00 - 20:00",
      },
      {
        select_CREW: "Omsk_NightCrew_One",
        select_city: "Omsk",
        select_workTime: "20:00 - 08:00",
      },
      {
        select_CREW: "Omsk_NightCrew_Two",
        select_city: "Omsk",
        select_workTime: "20:00 - 08:00",
      },
      {
        select_CREW: "Omsk_DayCrew_One",
        select_city: "Omsk",
        select_workTime: "08:00 - 20:00",
      },
      {
        select_CREW: "Omsk_DayCrew_Two",
        select_city: "Omsk",
        select_workTime: "08:00 - 20:00",
      },
      {
        select_CREW: "Kazan_NightCrew_One",
        select_city: "Kazan",
        select_workTime: "20:00 - 08:00",
      },
      {
        select_CREW: "Kazan_NightCrew_Two",
        select_city: "Kazan",
        select_workTime: "20:00 - 08:00",
      },
      {
        select_CREW: "Kazan_DayCrew_One",
        select_city: "Kazan",
        select_workTime: "08:00 - 20:00",
      },
      {
        select_CREW: "Kazan_DayCrew_Two",
        select_city: "Kazan",
        select_workTime: "08:00 - 20:00",
      },
    ];

    const selectedParams = ref({
      select_city: "",
      select_ceh: "",
      select_manager: "",
      select_workTime: "",
      select_CREW: "",
    })
    getParamsFromCookie(selectedParams)

    const selectData = ref({
      select_city: [],
      select_ceh: [],
      select_manager: [],
      select_workTime: [],
      select_CREW: [],
    })

    getData(selectData, selectedParams)

    function getData(output, params) {
      output.value.select_city = getFormatedData('city', params)
      output.value.select_ceh = getFormatedData('ceh', params)
      output.value.select_manager = getFormatedData('manager', params)
      output.value.select_workTime = getFormatedData('workTime', params)
      output.value.select_CREW = getFormatedData('crew', params)
    }


    function getFormatedData(id, params) {
      switch (id) {
        case 'city':
          return DATA.map((el) => el.select_city).filter((el, index, array) => array.indexOf(el) == index)

        case 'ceh':
          return DATA.filter((el) => params.value.select_city != '' ? el.select_city == params.value.select_city : el).map((el) => el.select_ceh)

        case 'manager':
          return DATA
            .filter((el) => params.value.select_city != '' ? el.select_city == params.value.select_city : el)
            .filter((el) => params.value.select_ceh != '' ? el.select_ceh == params.value.select_ceh : el)
            .map((el) => el.select_manager)
            .flat()

        case 'workTime':
          return ALL_CREW.map((el) => el.select_workTime).filter((el, index, array) => array.indexOf(el) == index)

        case 'crew':
          return ALL_CREW
            .filter((el) => params.value.select_city != '' ? el.select_city == params.value.select_city : el)
            .filter((el) => params.value.select_workTime != '' ? el.select_workTime == params.value.select_workTime : el)
            .map((el) => el.select_CREW)

        default:
          break;
      }
    }

    function changeParams(elem){
      if (elem.id === 'select_city') {
        setDefault()
      }
      selectedParams.value[elem.id] = elem.value
      getData(selectData, selectedParams)
    }

    function setDefault() {
      selectedParams.value.select_CREW=''
      selectedParams.value.select_ceh=''
      selectedParams.value.select_manager=''
      selectedParams.value.select_workTime=''
    }

    function getParamsFromCookie(output) {
      output.value
      if (document.cookie !== '') {
        output.value = JSON.parse(document.cookie)
      }
    }

    function saveData(){
      document.cookie = JSON.stringify(selectedParams.value)
    }
    return {
      selectedParams,
      selectData,
      ALL_CREW,
      saveData,
      changeParams
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .form {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
</style>
