<template>
  	<div tabindex="0" ref="select" class="select-box" :class="{'disabled': disabled }" @click="openTextOptions()">
        <div class="tipos-box-tipo" >
            <svg class="svg-icons" v-if="icons" >
                <use :xlink:href="require('@/assets/img/icons/'+ getSVG() + '.svg')+'#layer'"></use>
            </svg>
            <input class="tipos-box-desc" :value="getItem()" :placeholder="placeholder" v-if="editable" />
            <div class="tipos-box-desc" v-text="getItem()" v-else />
            <svg class="svg-icons arrow-down">
                <use :xlink:href="require('@/assets/img/icons/menu-down.svg')+'#layer'"></use>
            </svg>
        </div>
        <div class="tipos-box" v-if="optmenu" >
            <div class="tipos-box-tipo" v-for="(item, i) in items" :desc="item.desc" @click="setSelectedItem(i)" :class="{ 'selected' : selectedIndex == i } " >
                <svg class="svg-icons"  v-if="icons">
                    <use :xlink:href="require('@/assets/img/icons/'+ item.svg + '.svg')+'#layer'"></use>
                </svg>
                <div class="tipos-box-desc"> {{getDescription(item)}} </div>
            </div>
        </div>
	</div>
</template>

<script>

export default {
  props: {
      icons: {
          default: 'false',
      },
      items: {
          default: {},
      },
      placeholder: {
          default: 'Selecione o item',
      },
      editable: {
          default: false,
      },
      disabled:{
          default: false,
      }
  },
  data() {
      return {
          selectedIndex: -1,
          itemsnum: 0,
          optmenu: false,
      }
  },
  computed: {
  },
  created() {
      this.itemsnum = this.items.length;
  },
  mounted() {
      let select = this.$refs['select'];
      select.addEventListener('focusout', event => {
           this.optmenu = false;
      });
  },
  methods: {
        openTextOptions(){
            if ( !this.disabled && this.itemsnum > 0){
                this.optmenu = !this.optmenu;
            }
        },
        setSelectedItem( index ){
            this.selectedIndex = index;
        },
        getSVG(){
            if (this.selectedIndex >= 0 && this.items[this.selectedIndex].svg.length > 0){
                return this.items[this.selectedIndex].svg;
            }else {
                return 'text-short';
            }
        },
        getItem(){            
            if ( this.selectedIndex >= 0){
                return this.getDescription(this.items[this.selectedIndex]);
            } else if (!this.editable) {
                return this.placeholder;
            }
        },
        getDescription(item) {
            if(!item) {
                return '';
            }

            if (typeof item == 'string') {
                return item;
            }
            return item.desc;
        }
    }
}

</script>

<style lang="scss" scoped>
@import "../../../styles.scss";

.novos-campos .select-box{
	background-color: $darker-bg-color;
    border-radius: 5px;
    padding: 15px;
    width: 100%;
    position: relative;

    > .tipos-box-tipo{
        justify-content: space-between;
    }
    > .tipos-box-tipo input,
    > .tipos-box-tipo i{
        font-size: 18px;
        margin-left: 0;
        margin-right: 0;
    }

    .tipos-box{
        position: absolute;
        top: -120px;
        right: -2px;
        background: #fff;
        box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
        border-radius: 5px;
        z-index: 1000;
        .tipos-box-tipo{
            padding: 15px 15px 15px 15px;

            > svg{
                margin-left: 18px;
                margin-right: 39px;
            }
        }
        .tipos-box-tipo:hover{
            background: $darker-bg-color;
        }
    }
    .tipos-box-tipo{
        display: flex;
        cursor: pointer;
        > svg{
            margin-top: 5px;
            font-size: 20px;
            fill: $light-color;
            cursor: pointer;
        }
        > svg.arrow-down{
            margin-right: 0;
            width: 25px;
            height: 25px;
        }
        input,
        div{
            font-size: 20px;
            color: $light-color;
            cursor: pointer;
        }
        &.selected{
            background-color: $darker-bg-color;
        }
        &:nth-child(7),
        &:nth-child(9),
        &:nth-child(11),{
            border-bottom: 0.5px solid $icon-color;
        }
    }
}

.select-box{
	background-color: #fff;
    border-radius: 5px;
    padding: 11px;
    width: 100%;
    border: 1px solid #DDDDDD;
    color: $subtitle-border;

    &.disabled{
        background: #E3E3E3;
    }

    > .tipos-box-tipo{
        justify-content: space-between;
    }
    > .tipos-box-tipo input,
    > .tipos-box-tipo i{
        font-size: 14px;
        margin-left: 0;
        margin-right: 0;
        border: none;
        padding: 0;
        background: $darker-bg-color;
        flex: 1 1 auto;
    }

    .tipos-box{
        position: absolute;
        top: 30px;
        right: 0;
        background: #fff;
        box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
        border-radius: 5px;
        z-index: 1000;
        width: 95%;

        .tipos-box-tipo{
            padding: 15px 15px 15px 15px;
        }
        .tipos-box-tipo:hover{
            background: $darker-bg-color;
        }
    }
    .tipos-box-tipo{
        display: flex;
        cursor: pointer;
        > svg{
            margin-top: 5px;
            font-size: 20px;
            fill: $light-color;
            cursor: pointer;
        }
        > svg.arrow-down{
            margin-right: 0;
            width: 25px;
            height: 25px;
        }
        div,
        input{
            font-size: 16px;
            line-height: 17px;
            padding-top: 6px;
            color: $light-color;
            cursor: pointer;
        }
        &.selected{
            background-color: $darker-bg-color;
        }
        &:nth-child(7),
        &:nth-child(9),
        &:nth-child(11),{
            border-bottom: 0.5px solid $icon-color;
        }
    }
}

</style>
