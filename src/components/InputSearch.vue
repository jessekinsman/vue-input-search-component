<template>
  <div class="input-search">
    <div class="autocomplete">
      <div class="input-group">
        <input :placeholder="placeholder" :id="id" :ref="id" v-model="search" v-bind:class="{ 'is-invalid' : error == true }" @input.prevent="searchList" @click.prevent="searchList" class="form-control" @keydown.up="arrowUp" @keydown.down="arrowDown" @keydown.enter="enter" autocomplete="off">
        <div class="input-group-append" @click="close()">
          <span class="close input-group-text" :class="iconclass"><img v-if="!iconclass" class="close-icon" src="../assets/delete.svg" /></span>
        </div>
      </div>
      <ul class="autocomplete-results list-group" v-show="matchedSearch.length > 0" :style="getStyle" >
        <li v-for="(ele, i) in matchedSearch" :key="i" @click="selectItem(ele)" :class="{ 'is-active': i === keyIndex }" class="autocomplete-result list-group-item">{{ele.name}}</li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'InputSearch',
  props: {
    list: Array,
    placeholder: String,
    id: String,
    iconclass: String,
    current: Number,
    error: Boolean,
    checktop: Boolean
  },
  created: function(){
    if (this.current !== 0) {
      const curr = this.current;
      this.item = this.list.find(function(item) {
        return item.id == curr;
      });
      if (typeof this.item !== 'undefined' && this.item.hasOwnProperty('name')) {
        this.search = this.item.name;
      }
    }
  },
  mounted: function() {
    document.addEventListener("click", this.handleClick);
  },
  destroyed: function() {
    document.removeEventListener("click", this.handleClick);
  },
  methods: {
    searchList: function() {
      // Calculate viewport is for if we want to check if their is space below before displaying
      if (this.checktop && !this.calcViewport(this.$refs[this.id])) {
         this.bottom = this.setBottomValue(this.$refs[this.id]);
      }
      const searchTerm = this.search;
      if (searchTerm.trim() == "") {
        this.matchedSearch = this.list;
        this.selectItem({id: 0, name: ""});
      }
      this.matchedSearch = this.list.filter(function(item, arr, ind) {
        if (item.name.toLowerCase().indexOf(searchTerm.toLowerCase()) > -1) {
          return item;
        }
      });
    },
    selectItem: function(item) {
      this.search = item.name;
      this.matchedSearch = [];
      this.item = item;
      this.$emit("selected", this.item);
    },
    calcViewport: function(ref) {
      if (ref) {
        const menuHeight = 200;
        const elr = ref.getBoundingClientRect();
        const win = window.innerHeight;
        const top = elr.top;
        const height = ref.offsetHeight + menuHeight;
        const sp = win - (top + height);
        return sp > 0;
      }
      return true;
    },
    setBottomValue: function(ref) {
      return ref ? ref.offsetHeight : 0 ;
    },
    close: function() {
      this.selectItem({id: 0, name: ""});
    },
    arrowDown: function() {
      if (this.keyIndex < this.matchedSearch.length) {
        this.keyIndex+=1;
      }
    },
    arrowUp: function() {
      if (this.keyIndex > 0) {
        this.keyIndex-=1;
      }
    },
    enter: function() {
      this.selectItem(this.matchedSearch[this.keyIndex]);
      this.keyIndex = -1;
    },
    handleClick: function(e) {
      if (!this.$el.contains(e.target)) {
        this.matchedSearch = [];
        this.keyIndex = -1;
        if (typeof this.item !== 'undefined' && this.item.hasOwnProperty("name") && this.item.name.trim() !== "") {
          this.search = this.item.name;
        }
      }
    }
  },
  data: function() {
    return {
      exError: "",
      item: {},
      search: "",
      matchedSearch: [],
      keyIndex: 0,
      displayAbove: false,
      bottom: 0
    };
  },
  computed: {
    getStyle: function() {
      if (this.bottom !== 0) {
        return "bottom: " + this.bottom + "px;";
      }
      return "";
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.input-search {
  width: 100%;
}

.autocomplete {
 position: relative;
 z-index: 1;
}

.autocomplete .input-group-append .close {
  margin: 0;
}

.autocomplete-results {
 padding: 0;
 margin: 0;
 border: 1px solid #dedddd;
 overflow: auto;
 position: absolute;
 max-height: 200px;
 width: 100%;
 background-color: #fff;
 background-color: rgba(255,255,255,0.8);
}

.autocomplete-result {
 list-style: none;
 text-align: left;
 padding: 4px 2px;
 cursor: pointer;
}

.autocomplete-result:hover, .is-active {
 background-color: #17a2b8!important;
 color: white;
}

.fa-close.input-group-text {
  padding: .6em .75em;
}

.close-icon {
  width: 10px;
}
/* styles normally part of bootstrap */
.input-group {
    position: relative;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -ms-flex-wrap: wrap;
    flex-wrap: wrap;
    -webkit-box-align: stretch;
    -ms-flex-align: stretch;
    align-items: stretch;
    width: 100%;
}

.input-group > .form-control:not(:last-child), .input-group > .custom-select:not(:last-child) {
    border-top-right-radius: 0;
    border-bottom-right-radius: 0;
}

.input-group > .form-control, .input-group > .custom-select, .input-group > .custom-file {
    position: relative;
    -webkit-box-flex: 1;
    -ms-flex: 1 1 auto;
    flex: 1 1 auto;
    width: 1%;
    margin-bottom: 0;
}

.form-control {
    display: block;
    width: 100%;
    padding: 0.375rem 0.75rem;
    font-size: 1rem;
    line-height: 1.5;
    color: #495057;
    background-color: #fff;
    background-clip: padding-box;
    border: 1px solid #ced4da;
    border-radius: 0.25rem;
    transition: border-color 0.15s ease-in-out, box-shadow 0.15s ease-in-out;
}

input {
    overflow: visible;
}

.input-group-append {
    margin-left: -1px;
}

.input-group-prepend, .input-group-append {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
}

.autocomplete .input-group-append .close {
    margin: 0;
}

.close:not(:disabled):not(.disabled) {
    cursor: pointer;
}

.input-group-append > .input-group-text {
    border-top-left-radius: 0;
    border-bottom-left-radius: 0;
}

.fa-close.input-group-text {
    padding: .6em .75em;
}
.fa {
    display: inline-block;
    font: normal normal normal 14px/1 FontAwesome;
    font-size: inherit;
    text-rendering: auto;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
}

.close {
    float: right;
    font-size: 1.5rem;
    font-weight: 700;
    line-height: 1;
    color: #000;
    text-shadow: 0 1px 0 #fff;
    opacity: .5;
}

.input-group-text {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-align: center;
    -ms-flex-align: center;
    align-items: center;
    padding: 0.375rem 0.75rem;
    margin-bottom: 0;
    font-size: 1rem;
    font-weight: 400;
    line-height: 1.5;
    color: #495057;
    text-align: center;
    white-space: nowrap;
    background-color: #e9ecef;
    border: 1px solid #ced4da;
    border-radius: 0.25rem;
}

.flex-column {
    -webkit-box-orient: vertical !important;
    -webkit-box-direction: normal !important;
    -ms-flex-direction: column !important;
    flex-direction: column !important;
}

.fa-remove:before, .fa-close:before, .fa-times:before {
    content: "\f00d";
}

.list-group {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
    -ms-flex-direction: column;
    flex-direction: column;
    padding-left: 0;
    margin-bottom: 0;
}

.list-group-item {
    position: relative;
    display: block;
    padding: 0.75rem 1.25rem;
    margin-bottom: -1px;
    background-color: #fff;
    border: 1px solid rgba(0, 0, 0, 0.125);
}

.form-control.is-invalid {
    border-color: #dc3545;
}



</style>
