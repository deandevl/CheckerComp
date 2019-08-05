<template>
  <div class='demo_container'>
    <section class="button_section">
      <button v-on:click="set_weekend">Set the Weekend</button>
      <button v-on:click="set_mon_fri">Set Monday and Friday</button>
      <button v-on:click="set_shopping">Set Shopping List</button>
      <button v-on:click="get_days">Get Checked Days</button>
    </section>

    <section class="checkers_section">
      <checker-comp
          heading="Days of the Week"
          drop_panel_height="160px"
          :blur_panel="blur_panel"
          :items="items_dow"
          :css_variables="css_variables_days_of_week"
          v-on:checker_comp_checked_indices="dow_changed">
      </checker-comp>

      <checker-comp
          heading="Shopping List"
          :items="items_shop"
          :css_variables="css_variables"
          v-on:checker_comp_checked_indices="shopping_changed">
      </checker-comp>

      <checker-comp
          heading="Gender"
          :single_check="gender_single_check"
          :items="items_gender"
          :css_variables="css_variables"
          v-on:checker_comp_checked_indices="gender_changed">
      </checker-comp>
    </section>
    <section class="pbox">
      <p>
        rownwnn andlle  aljnd;3e; .akdnend  qnfdslns alne. andl;; irjlsek  sljdnel
        ewlnsl lsnkei s nelnsndl  slhelnskdn lslsn; a;ke;;;0ekd  lsnlsnel sllls
        2nwlenkf lsmksnel; slasll slenen ls nellllsn 5nrlnelwn lsnl  slnwnl  rien
        wqnelnwlen tltnylnr lsnenl wqnsna slns yktnu iims wemr eeenelsn ssysnwl snn
        rlrnelwn lsnen  rlnelwn alqnfkt ndlnwln eerlnw qnerndlsnl yytl alsn lkd5lr
        endsbx alnsnr uyntlrnl lln wndjf prpt wenrnf qwns duu tnrhdhh slnfldn wekr
      </p>
    </section>
  </div>
</template>

<script>
  import CheckerComp from 'checkercomp';

  export default {
    name: "App",
    data(){
      return {
        items_dow: [
          {text: 'Sunday', checked: false},
          {text: 'Monday', checked: false},
          {text: 'Tuesday', checked: false},
          {text: 'Wednesday', checked: true},
          {text: 'Thursday', checked: false},
          {text: 'Friday', checked: false},
          {text: 'Saturday', checked: false}
        ],
        items_shop: [
          {text: 'bread', checked: false},
          {text: 'milk', checked: false},
          {text: 'lettuce', checked: false},
          {text: 'ice cream', checked: false},
          {text: 'potatoes', checked: false},
          {text: 'hamburger', checked: false},
          {text: 'orange juice', checked: false}
        ],
        items_gender: [
          {text: 'Male', checked: false},
          {text: 'Female', checked: false}
        ],
        gender_single_check: true,
        blur_panel: false,
        css_variables: {
          checker_comp_items_panel_background: 'lightgreen'
        },
        css_variables_days_of_week: {
          checker_comp_items_panel_position: 'absolute',
          checker_comp_items_panel_z_index: '100'
        }
      };
    },
    components: {
      CheckerComp
    },
    methods: {
      //methods that responds to 'checker_comp_checked_indices' event
      dow_changed: function(indices){
        console.log(`Days of Week Indices: ${indices}`);
      },
      shopping_changed: function(indices){
        console.log(`Shopping Indices: ${indices}`);
      },
      gender_changed: function(indices){
        console.log(`Gender Indices: ${indices}`);
      },
      set_weekend: function(){
        this._reset();
        this.items_dow[0].checked = true;
        this.items_dow[6].checked = true;
      },
      set_mon_fri: function(){
        this._reset();
        this.items_dow[1].checked = true;
        this.items_dow[5].checked = true;
      },
      set_shopping: function(){
        this.items_shop[1].checked = true;
        this.items_shop[3].checked = true;
        this.items_shop[5].checked = true;
      },
      get_days: function(){
        let days = [];
        for(let item of this.items_dow){
          if(item.checked){
            days.push(item.text);
          }
        }
        console.log(days);
      },
      _reset: function(){
        for(let item of this.items_dow){
          item.checked = false;
        }
      }
    }
  }
</script>

<style>
  .demo_container {
    padding: 2rem;
  }
  .button_section {
    margin-bottom: 3rem;
  }
  .checkers_section {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    width: 100%;
    height: 100%;
    max-width: 70rem;
    max-height: 60rem;
    margin-top: 2rem;
  }
  .pbox {
    margin-top: 2rem;
  }
</style>