<template>
  <!--template必须有一个根节点-->
  <div>
    <div class="col-xs-3">
      <button class="btn btn-primary" disabled v-show="haveUnfinished">待办事项<a href="#" class="badge">{{unFinishedCount}}</a>
      </button>
    </div>
    <div class="col-xs-5">
       <button class="btn btn-sm" :style="{marginLeft: '5px'}" v-for="btn in btns" @click="tell(btn.type)" :class="{success: btn.type === filtertype}" :type="btn.type">
         {{btn.text}}
       </button>
    </div>
      <!--<ul class="nav nav-pills col-xs-7">
        <li v-for="btn in btns" :class="{active: btn.type === filtertype}" @click="tell(btn.type)"><a
          href="#">{{btn.text}}</a></li>
      </ul>-->
    <div class="col-xs-2">
      <button class="btn btn-danger btn-sm" @click="clear" v-show="haveFinished">删除已完成</button>
    </div>
  </div>
</template>
<script>
  import * as filterTypes from './filter-types'
  import * as types from '../store/types'
  import {mapActions, mapState, mapGetters} from 'vuex';
  export default {
    data() {
      return {
        btns: [
          {type: filterTypes.ALL, text: "所有"},
          {type: filterTypes.ACTIVE, text: "未完成"},
          {type: filterTypes.COMPLETED, text: "已完成"}
        ],
      }

    },
    updated() {
      console.log(this.filtertype);
    },
    props: ["filtertype"],
    components: {},
    methods: {
      ...mapActions([types.CLEAR_COMPLETED, types.CHANGE_FILTER]),
      clear() {
        this[types.CLEAR_COMPLETED]();
      },
      tell(type) {
        this.$emit("changefilter", type);
      }
    },
    computed: {
      ...mapState(['todos']),
      ...mapGetters(['haveUnfinished', 'haveFinished']),
      unFinishedCount() {
//              return this.todos.reduce((count, todo)=>count + !todo.completed, 0)
        return this.todos.filter(todo => !todo.completed).length;
      },
      finishedCount() {
        return this.todos.filter(todo => todo.completed).length;
      }
    }
  }
</script>
<style scoped="">
  .success {
    background: forestgreen;
  }
</style>
