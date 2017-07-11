<template>
  <page-content page-title="Getting Started">
    <div class="main-content">
  
      <md-sidenav class="md-left" ref="leftSidenav" @open="open('Left')" @close="close('Left')">
        <md-toolbar class="md-large">
          <div class="md-toolbar-container">
            <h3 class="md-title">Sidenav content</h3>
          </div>
        </md-toolbar>
  
        <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit. Nisi cupiditate esse necessitatibus beatae nobis, deserunt ut est fugit, tempora deleniti, eligendi commodi doloribus. Nemo, assumenda possimus, impedit inventore perferendis iusto!</p>
      </md-sidenav>
  
      <md-toolbar md-theme="red">
        <md-button class="md-icon-button" @click="toggleLeftSidenav">
          <md-icon>menu</md-icon>
        </md-button>
        <h2 class="md-title" style="flex: 1">Vue Material</h2>
        <md-button class="md-raised">Add</md-button>
        <md-button class="md-raised md-warn" @click="login">Login</md-button>
      </md-toolbar>
  
      <md-dialog class="md-dialog-confirm" ref="login" @close="loginConfirmSnackbar('login')">
        <md-dialog-title>Login</md-dialog-title>
        <md-dialog-content>
          <md-input-container>
            <label>Email Address</label>
            <md-input type="email" name="email"></md-input>
          </md-input-container>
          <md-input-container>
            <label>Password</label>
            <md-input type="password" name="password"></md-input>
          </md-input-container>
        </md-dialog-content>
  
        <md-dialog-actions>
          <md-button class="md-accent md-raised" @click="">Login</md-button>
          <md-button class="md-primary" @click="close('login')">Close</md-button>
        </md-dialog-actions>
      </md-dialog>
  
      <md-table-card>
        <md-toolbar>
          <h1 class="md-title">Nutrition</h1>
          <md-button class="md-icon-button">
            <md-icon>filter_list</md-icon>
          </md-button>
  
          <md-button class="md-icon-button">
            <md-icon>search</md-icon>
          </md-button>
        </md-toolbar>
  
        <md-table-alternate-header md-selected-label="selected">
          <md-button class="md-icon-button">
            <md-icon>delete</md-icon>
          </md-button>
  
          <md-button class="md-icon-button">
            <md-icon>more_vert</md-icon>
          </md-button>
        </md-table-alternate-header>
  
        <md-table md-sort="calories">
          <md-table-header>
            <md-table-row>
              <md-table-head md-sort-by="dessert">Dessert (100g serving)</md-table-head>
              <md-table-head md-sort-by="type" width="100px">Type</md-table-head>
              <md-table-head md-sort-by="calories" md-numeric md-tooltip="The total amount of food energy and the given serving size">Calories (g)</md-table-head>
              <md-table-head md-sort-by="fat" md-numeric>Fat (g)</md-table-head>
              <md-table-head>
                <md-icon>message</md-icon>
                <span>Comments</span>
              </md-table-head>
            </md-table-row>
          </md-table-header>
  
          <md-table-body>
            <md-table-row v-for="(row, rowIndex) in nutrition" :key="rowIndex" :md-item="row" md-selection>
              <md-table-cell v-for="(column, columnIndex) in row" :key="columnIndex" :md-numeric="columnIndex !== 'dessert' && columnIndex !== 'comment' && columnIndex !== 'type'">
                <span v-if="columnIndex === 'comment'">{{ column }}</span>
  
                <md-button class="md-icon-button" v-if="columnIndex === 'comment'">
                  <md-icon>edit</md-icon>
                </md-button>
  
                <md-select placeholder="Type" :name="'type' + columnIndex" :id="'type' + columnIndex" v-model="nutrition[rowIndex].type" v-if="columnIndex === 'type'">
                  <md-option value="ice_cream">Ice Cream</md-option>
                  <md-option value="pastry">Pastry</md-option>
                  <md-option value="other">Other</md-option>
                </md-select>
  
                <span v-if="columnIndex !== 'type' && columnIndex !== 'comment'">{{ column }}</span>
              </md-table-cell>
            </md-table-row>
          </md-table-body>
        </md-table>
      </md-table-card>
  
      <md-subheader>VueChart and Chart.js Integration</md-subheader>

      <md-layout md-gutter>
        <md-layout md-width="100">
          <line-chart></line-chart>
        </md-layout>
      </md-layout>
  
      <md-snackbar :md-position="config.snakbarPosition" ref="snackbar" :md-duration="config.snackbarDuration">
        <span>{{ config.snackbarText }}</span>
        <md-button class="md-accent" md-theme="light-blue" @click="$refs.snackbar.close()">Ok</md-button>
      </md-snackbar>
  
    </div>
  </page-content>
</template>

<style lang="scss" scoped>
.main-content {
  position: relative;
}

article {
  max-width: 960px;
}

section+section,
article+article {
  margin-top: 36px;
}

.code-block,
.md-tabs {
  max-width: 100%;
}

.md-tab {
  border-top: 1px solid rgba(#000, .12);
  padding: 0;
}

iframe {
  height: auto;
  min-height: 620px;
}
</style>


<script>
import Vue from 'vue';
import VueCharts from 'vue-chartjs';

Vue.component('line-chart', {
  extends: VueCharts.Line,
  mounted() {
    this.renderChart({
      labels: ['January', 'February', 'March', 'April', 'May', 'June', 'July'],
      datasets: [
        {
          label: 'Data One',
          backgroundColor: '#f87979',
          data: [40, 39, 10, 40, 39, 80, 40]
        }, {
          label: 'Data Two',
          backgroundColor: '#c9c9c9',
          data: [60, 55, 32, 10, 2, 12, 53]
        }
      ]
    }, { responsive: true, maintainAspectRatio: false, width: '100%', height: '300' });

  }
});

export default {
  data: () => ({
    config: {
      snakbarPosition: 'top center',
      snackbarDuration: 4000,
      snackbarText: ''
    },
    nutrition: [
      {
        dessert: 'Frozen yogurt',
        type: 'ice_cream',
        calories: '159',
        fat: '6.0',
        comment: 'Icy'
      },
      {
        dessert: 'Ice cream sandwich',
        type: 'ice_cream',
        calories: '237',
        fat: '9.0',
        comment: 'Super Tasty'
      },
      {
        dessert: 'Eclair',
        type: 'pastry',
        calories: '262',
        fat: '16.0',
        comment: ''
      },
      {
        dessert: 'Cupcake',
        type: 'pastry',
        calories: '305',
        fat: '3.7',
        comment: ''
      },
      {
        dessert: 'Gingerbread',
        type: 'other',
        calories: '356',
        fat: '16.0',
        comment: ''
      }
    ]
  }),
  computed: () => ({
  }),
  methods: {
    toggleLeftSidenav() {
      this.$refs.leftSidenav.toggle();
    },
    login() {
      this.$refs.login.open();
    },
    close() {
      this.$refs.login.close();
    },
    loginConfirmSnackbar() {
      this.config.snackbarText = 'Sign in failed!';
      this.$refs.snackbar.open();
    }
  }
};
</script>
