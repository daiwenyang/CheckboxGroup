<template>
    <div class="kms-checkbox-group" :class="{'kms-block-group':!inline}">
        <div class="caret-right-container" @click.stop="showCheckbox" :class="{'kms-block-caret':!inline}">
            <i class="el-icon-caret-right" :class="{'rotate-icon':showChildren}"></i>
        </div>
        <div class="kms-checkbox-contanier" :class="{'kms-block-checkbox':!inline}">
            <div class="kms-checkbox-item" >
                <el-checkbox :disabled="disable" :indeterminate="isIndeterminate" v-model="checkAll" @change="handleCheckAllChange">
                    <slot name="title" :value="defaultValue"><span>全选</span></slot>
                </el-checkbox>
            </div>
            <div class="kms-checkbox-item" v-show="showChildren">
                <div v-if="!inline">
                    <el-checkbox-group :disabled="disable" v-model="checkedList" @change="handleCheckedCitiesChange">
                        <div v-for="(item,index) in defaultValue[defaultProps.children]" :key="index">
                            <div class="block-checkbox">
                                <el-checkbox  
                                :label="item[defaultProps.key]" >{{item[defaultProps.label]}}
                                </el-checkbox>
                                <el-button type="text" :disabled="disable">字段设置</el-button>
                            </div>
                            <div class="detail-container" v-show="false">
                                <detail-checkbox-group
                                :default-props="defaultProps"
                                :disable="disable"
                                :value="value">
                                </detail-checkbox-group>
                            </div>
                        </div>
                    </el-checkbox-group>
                </div>
                <el-checkbox-group :disabled="disable" v-else v-model="checkedList" @change="handleCheckedCitiesChange">
                    <el-checkbox style="width:20%;" v-for="(item,index) in defaultValue[defaultProps.children]" :label="item[defaultProps.key]" :key="index">{{item[defaultProps.label]}}</el-checkbox>
                </el-checkbox-group>
            </div>
        </div>
    </div>
</template>

<script>
import DetailCheckboxGroup from './index.js'
export default {
  name: "kms-checkbox-group",
  mixins: [],
  components:{
      DetailCheckboxGroup
  },
  props: {
    inline:{
        type:Boolean,
        default:()=>{
            return false
        }
    },
    disable:{
        type:Boolean,
        default:()=>{
            return false
        }
    },
    value:{
       type:Object,
       required:true,
       default:()=> {
           return {}
       }, 
    },
    defaultProps:{
        type:Object,
        default:()=>{
            return {
                label:'label',
                key:'id',
                checked:'checked',
                children:'childList'
            }
        }
    }
  },
  data() {
    return {
        defaultValue:this.value,
        checkAll:false,
        checkedList:[],
        showChildren:false,
    };
  },
  mounted(){
      
  },
  beforeUpdate(){
    
  },
  computed:{
      isIndeterminate(){
          if(this.checkedList.length > 0 && this.checkedList.length < this.value[this.defaultProps.children].length){
              return true 
          }else{
              return false
          }
      }
  },
  watch:{
      value:{
        immediate: true,
        handler:function(newVal,old){
            this.checkedList = newVal[this.defaultProps.children].filter(item=>item[this.defaultProps.checked]).map(item => item[this.defaultProps.key])
        }
      },
      'checkedList.length':{
        immediate: true,
        handler:function(newVal,old){
            if(newVal === this.value[this.defaultProps.children].length){
                this.checkAll = true;
            }else{
                this.checkAll = false;
            }
        }
      }
  },
  methods: {
      showCheckbox(){
          this.showChildren = !this.showChildren;
      },
      handleCheckAllChange(val){
          if(val){
              this.checkedList = this.defaultValue[this.defaultProps.children].map(item => item[this.defaultProps.key])
          }else{
              this.checkedList = [];
          }
      },
      handleCheckedCitiesChange(val){
          
      },
      resetCheckList(){
          this.checkedList = this.value[this.defaultProps.children].filter(item=>item[this.defaultProps.checked]).map(item => item[this.defaultProps.key])
      }
  }
};
</script>

<style lang="scss" scoped>
    .kms-block-group{
        border-bottom:1px solid #ccc;
    }
    .kms-checkbox-group{
        display:flex;
        flex-direction: row;
        justify-content:flex-start;
        flex-wrap: nowrap;
        width:100%;
        .caret-right-container{
            width:30px;
            line-height:36px;
            .rotate-icon{
                transform:rotate(90deg);
            }
        }
        .caret-right-container.kms-block-caret{
            line-height:50px;
        }
        .kms-checkbox-contanier{
            width:calc(100% - 30px);
        }
        .kms-checkbox-item{
            line-height:36px;
        }
        .kms-block-checkbox{
            .detail-container{
                border-top:1px solid #ccc;
            }
            .block-checkbox{
                line-height:50px;
                border-top:1px solid #ccc;
                display:flex;
                flex-direction: row;
                justify-content: space-between;
            }
            .kms-checkbox-item{
                line-height:50px;
            }
        }
    }
</style>