<template>
  <div class="checkerComp" tabindex="0" v-on:blur="blur_it">
    <section class="checkerComp_headerSec">
      <span
          :class="is_open ? 'checkerComp_arrowIcon__open' : 'checkerComp_arrowIcon__closed'"
          v-on:click="icon_click">
      </span>
      <span v-show="heading">{{heading}}</span>
    </section>
    <section class="checkerComp_itemsSec" v-show="items">
      <div class="checkerComp_itemsPanel" :style="panel_style">
        <div
          :class="[item.checked ? 'checkerComp_item__checked' : 'checkerComp_item__notchecked', 'checkerComp_item']"
          v-for="(item,index) in items_computed"
          :key="index"
          v-on:click="item_click(index)">
          {{item.text}}
        </div>
      </div>
    </section>
  </div>
</template>

<script>
  export default {
    name: 'CheckerComp',
    data(){
      return {
        previous_index: null,
        is_open: false,
        panel_style: 'width: 0; height: 0; transition: all 3s; opacity: 0;'
      }
    },
    props: {
      heading: {
        type: String,
        default: null
      },
      items: {
        type: Array,
        default: null
      },
      single_check: {
        type: Boolean,
        default: false
      },
      drop_panel_height: {
        type: String,
        default: '100px'
      },
      blur_panel: {
        type: Boolean,
        default: true
      },
      css_variables: {
        type: Object,
        default: null
      }
    },
    computed: {
      items_computed: function(){
        const checked_indices = [];
        if(this.items !== null){
          for(let item of this.items.entries()){
            if(item[1].checked){
              this.previous_index = item[0];
              checked_indices.push(item[0]);
            }
          }
          this.$emit('checker_comp_checked_indices', checked_indices);
        }
        return this.items;
      }
    },
    methods: {
      blur_it: function(){
        if(this.is_open && this.blur_panel){
          this.is_open = false;
          this.panel_style = 'height: 0;transition: all 3s; opacity: 0;';
        }
      },
      icon_click: function(){
        this.toggle_panel_state();
      },
      item_click: function(idx){
        if(this.single_check && this.previous_index !== null){
          this.items[this.previous_index].checked = false;
        }
        this.items[idx].checked = !this.items[idx].checked;
        this.previous_index = idx;
      },
      toggle_panel_state: function(){
        if(this.is_open){
          this.is_open = false;
          this.panel_style = 'height: 0;transition: all 3s; opacity: 0;';
        }else {
          this.is_open = true;
          this.panel_style = 'height: ' + this.drop_panel_height + ';transition: all 3s; opacity: 1.0;';
        }
      }
    },
    mounted(){
      if(this.css_variables !== null){
        for(let key of Object.keys(this.css_variables)){
          this.$el.style.setProperty(`--${key}`, this.css_variables[key]);
        }
      }
    }
  }
</script>

<style lang="less">
  :root {
    --checker_comp_font_family: Verdana,serif;

    --checker_comp_heading_color: black;
    --checker_comp_heading_font_size: 1rem;
    --checker_comp_heading_font_weight: bold;

    --checker_comp_down_icon: '\21D3';
    --checker_comp_icon_color: black;

    --checker_comp_items_panel_position: static;
    --checker_comp_items_panel_z_index: auto;
    --checker_comp_items_panel_color: black;
    --checker_comp_items_panel_background: white;
    --checker_comp_items_panel_border: 1px solid black;

    --checker_comp_checkbox_border: 1px solid black;
    --checker_comp_notchecked_background: white;
    --checker_comp_checked_background: yellowgreen;

    --checker_comp_item_font_size: .8rem;
  }

  .checkerComp {
    display: block;
    font-family: var(--checker_comp_font_family);
    &:focus {
      outline: none;
    }

    &_headerSec {
      color: var(--checker_comp_heading_color);
      font-size: var(--checker_comp_heading_font_size);
      font-weight: var(--checker_comp_heading_font_weight);
    }

    &_arrowIcon__open::before {
      content: var(--checker_comp_down_icon);
      display: inline-block;
      color: var(--checker_comp_icon_color);
      transition: 600ms linear all;
      margin-right: .2rem;
      font-size: 1rem;
      cursor: pointer;
    }

    &_arrowIcon__closed::before {
      content: var(--checker_comp_down_icon);
      display: inline-block;
      color: var(--checker_comp_icon_color);
      transform: rotate(-90deg);
      transition: 600ms linear all;
      margin-right: .2rem;
      font-size: 1rem;
      cursor: pointer;
    }

    &_itemsSec {
      position: relative;
    }

    &_itemsPanel {
      color: var(--checker_comp_items_panel_color);
      background: var(--checker_comp_items_panel_background);
      border: var(--checker_comp_items_panel_border);
      border-bottom-left-radius: 4px;
      border-bottom-right-radius: 4px;
      overflow: auto;
      padding: .5rem;
      width: max-content;
      position: var(--checker_comp_items_panel_position);
      z-index: var(--checker_comp_items_panel_z_index);
      transition: all 2s ease-in-out;

      &::-webkit-scrollbar-track {
        background-color: #F5F5F5;
        border-radius: 10px;
        -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3);
      }
      &::-webkit-scrollbar {
        background-color: transparent;
        width: 12px;
        height: 12px;
      }
      &::-webkit-scrollbar-thumb {
        background-color: #D62929;
        border-radius: 10px;
        -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,.3);
      }
    }

    &_item {
      margin-top: .6rem;
      font-size: var(--checker_comp_item_font_size);
      cursor: pointer;
    }

    &_item__checked::before {
      content: '\2713';
      display: inline-block;
      vertical-align: .2em;
      width: .8em;
      height: .8em;
      margin-right: .2em;
      border-radius: .2em;
      border: var(--checker_comp_checkbox_border);
      background: var(--checker_comp_checked_background);
      text-indent: .15em;
      line-height: .65;
    }

    &_item__notchecked::before {
      content: '\a0';
      display: inline-block;
      vertical-align: .2em;
      width: .8em;
      height: .8em;
      margin-right: .2em;
      border-radius: .2em;
      border: var(--checker_comp_checkbox_border);
      background: var(--checker_comp_notchecked_background,white);
      text-indent: .15em;
      line-height: .65;
    }
  }
</style>