<template>
  <v-container>
    <v-card v-for="(condition, id) of conditions" :key="condition.id" class="condition">
      <v-card-title>
        {{ condition.header }}
      </v-card-title>
      <v-combobox
        :items="conditionList"
        label="Выберете условие"
        @change="change($event, condition)"
        item-text="name"
        item-value="name"
      ></v-combobox>
      <v-container
        v-if="condition.condition.type === 'range'"
      >
        <section
          v-for="(value, index) of condition.value"
          :key="value.id"
          class="range condition-subheader"

        >
          <v-col cols="12" md="3" class="condition-subheader__arguments">
            <span class="condition-subheader__argument" v-if="index">Или</span>
            <h4 class="condition-subheader__header">{{ value.header }}</h4>
          </v-col>
          <v-col
            cols="12" md="9"
            class="condition-subheader__inputs"
          >
            <span>От</span>
            <v-text-field
              outlined
              dense
              v-model="value.value[0]"
              class="range__fields"
            >
            </v-text-field>

            <span>До</span>
            <v-text-field
              outlined
              dense
              v-model="value.value[1]"
              class="range__fields"
            >
            </v-text-field>
          </v-col>
        </section>
      </v-container>
      <v-container
        v-if="condition.condition.type === 'status'"
      >
        <section
          v-for="(value, index) of condition.value"
          :key="value.id"
          class="condition-subheader status"

        >
          <v-col cols="12" md="3" class="condition-subheader__arguments">
            <span class="condition-subheader__argument" v-if="index">Или</span>
            <h4 class="condition-subheader__header">{{ value.header }}</h4>
          </v-col>
          <v-col
            cols="12" md="5"
            class="condition-subheader__inputs"
          >
            <v-combobox
              class="status__fields"
              :items="conditionList"
              label="Выберете тип"
              item-text="name"
              item-value="name"
              outlined
              dense
            ></v-combobox>
          </v-col>
        </section>
      </v-container>
      <v-btn class="btn" v-if="condition.condition.type" @click="add(condition, condition.condition.type)">Добавить</v-btn>
      <v-btn class="btn" color="#f5405b" dark @click="delCondition(id)">Удалить условие</v-btn>
    </v-card>
    <v-btn class="btn" color="#67ba4e" dark @click="addCondition()">Добавить условие</v-btn>
    <v-btn class="btn">Далее</v-btn>
  </v-container>
</template>

<script>
export default {
  data () {
    return {
      conditions: [
        {
          header: 'Условие1',
          condition: {},
          value: [],
        }
      ],

      conditionList: [
        {
          id: 0,
          name: 'Возраст респондента',
          type: 'range'
        },
        {
          id: 1,
          name: 'Статус карты лояльности',
          type: 'status'
        },
      ],
    }
  },
  methods: {
    add (condition, type) {
      const value = {}

      if (type === 'range') {
        value.value = []
        value.header = 'Диапазон'
      } else if (type === 'status') {
        value.value = false
        value.header = 'Статус'
      }

      value.header += condition.value.length + 1

      condition.value.push(value)
    },
    change (e, condition) {
      condition.value = []
      condition.condition = e
    },
    del (condition) {
      condition.value.splice(condition.value.length - 1, 1)
    },
    delCondition (id) {
      this.conditions.splice(id, 1)
    },
    addCondition () {
      const condition = {}

      condition.header = 'Условие' + (this.conditions.length + 1)
      condition.value = []
      condition.condition = {}
      this.conditions.push(condition)
    },
  },
} // Можно было бы использовать классы
</script>

<style lang="sass" scoped>
  .condition
    padding: 20px
    margin: 10px 0px

  .condition-subheader__arguments
    display: flex
    padding: 0px

  .condition-subheader__argument
    height: 40px
    padding: 10px
    margin-right: 10px
    background: #ccffcc
    border-radius: 15px


  .condition-subheader
    display: flex

  .condition-subheader__header
    margin-top: 10px

  .condition-subheader__inputs
    display: flex
    flex-direction: row
    padding: 0px

    span
      margin-top: 10px
      margin-left: 20px
      margin-right: 10px

  .range__fields
    max-width: 150px

  .status__fields
    max-width: 300px

  .btn
    margin-right: 10px
</style>
