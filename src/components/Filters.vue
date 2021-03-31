<template>
    <div id="filters">
        <span class="clear-filters" @click="clearFilters">Clear All Filters</span>
        <Select
          placeholder="Assocation"
          :options="[
            { title: 'All', value: null },
            { title: 'GHSA', value: '18bad24aaa' },
            { title: 'UIL', value: '542bc38f95' }
          ]"
          v-model="value.assocation" />
        <Select
          placeholder="Date"
          :options="[
            { title: 'All Dates', value: null },
            { title: 'Today', value: 'Today' },
            { title: 'Tomorrow', value: 'Tomorrow' },
            { title: 'This Weekend', value: 'Weekend' },
            { title: 'Date Range', value: 'Range' }
          ]"
          v-bind:value="date"
          v-on:input="setDateRange" />
          <DateRangePicker v-model="value.dateRange" v-on:cleared="clearDateFilter" v-if="date === 'Range'" />
    </div>
</template>

<script>
  import Select from './Select.vue';
  import DateRangePicker from './DateRangePicker.vue';
  import moment from 'moment';

  export default {
    name: 'App',
    components: { Select, DateRangePicker },
    props: {
        value: Object,
    },
    data() {
      return {
        assocation: null,
        date: null,
      }
    },
    methods: {
      clearFilters() {
        this.assocation = null;
        this.date = null;
        
        this.$emit('input', {
          assocation: null,
          dateRange: {
              startDate: null,
              endDate: null,
          }
        });
      },
      clearDateFilter() {
        this.date = null;
        
        this.$emit('input', {
          assocation: this.value.assocation,
          dateRange: {
              startDate: null,
              endDate: null,
          }
        });
      },
      setDateRange(value) {
        let today = moment();
        let tomorrow = moment(today).add(1, 'days');
        let endOfWeek = moment(today).endOf('week').add(1, 'days');
        let friday = moment(endOfWeek).subtract(2, 'days');

        const dateRangeMapping = {
          All: {
            startDate: null,
            endDate: null
          },
          Today: {
            startDate: moment(today).startOf('day'),
            endDate: moment(today).endOf('day'),
          },
          Tomorrow: {
            startDate: moment(tomorrow).startOf('day'),
            endDate: moment(tomorrow).endOf('day'),
          },
          Weekend: {
            startDate: moment(friday).startOf('day'),
            endDate: moment(endOfWeek).endOf('day'),
          }
        }

        this.date = value;
        value = (value === null) ? "All" : value;

        if(value === "Range") {
            return false;
        }

        this.$emit('input', {
          assocation: this.value.assocation,
          dateRange: dateRangeMapping[value],
        });
      }
    }
  }
</script>

<style lang="scss">
  #filters {
    position: relative;
    margin: 0 30px 30px 30px;

    #datepicker {
      margin: 30px auto 60px auto;

      @media screen and (min-width: 500px) {
        margin: 30px 60px 60px 60px;
      }
    }

    .clear-filters {
      display: block;
      color: #0e75d2;
      cursor: pointer;
      text-transform: uppercase;
      font-size: 12px;
      font-weight: 800;
      text-align: center;
      margin-bottom: 30px;
    }
  }
</style>
