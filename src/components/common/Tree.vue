<template>
  <div class="tree-wrap" :class="[{'wrap wrap--contained' : contained}]">
  <ul class="tree" :class="['tree-' + name , mode ? 'tree--' + mode : '']">
<!--{{parentLevel+'+'+level}}-->
    <pre v-if="debug && level == 1" class="row">
      ℹ️ tree

      SLOTS:
        no slots

      PROPS:
        • name (string)
        • debug (boolean)
        • contained (boolean)
        • print-levels (array) eg, for a 4 nested level tree: [1,0,0,0]
        • print-level-imgs (array) eg, for a 4 nested level tree: [1,0,0,0]
        • print-level-text (array) eg, for a 4 nested level tree: [1,0,0,0]
        • tree (array) eg. [
                            {
                              title: 'Web Design - NO URL',
                              url: '',
                              target: '_blank',
                              class: 'col col--3-12',
                              image: {
                                  src: 'https://placeholdit.imgix.net/~text?txtsize=33&txt=ICON_HERE&w=400&h=400',
                                  srcRetina: 'https://placeholdit.imgix.net/~text?txtsize=33&txt=ICON_HERE&w=400&h=400',
                                  ratio: '3-2' //see welanstrap/_images.scsss for possible ratio values
                              },
                              items: [
                                {
                                  title: 'Web Design 1 - EXT URL',
                                  url: 'https://welance-handbook.herokuapp.com/welance-development-guidelines.html',
                                  target: '_blank',
                                  class: 'col col--3-12',
                                  image: {
                                    src: 'https://placeholdit.imgix.net/~text?txtsize=33&txt=ICON_HERE&w=400&h=400',
                                    srcRetina: 'https://placeholdit.imgix.net/~text?txtsize=33&txt=ICON_HERE&w=400&h=400',
                                    ratio: '3-2' //see welanstrap/_images.scsss for possible ratio values
                                  },
                                  items: [
                                    {
                                      title: '-- 3rd level',
                                      url: 'https://welance-handbook.herokuapp.com/welance-development-guidelines.html',
                                      target: '_blank',
                                      class: 'col col--3-12',
                                      image: {
                                        src: 'https://placeholdit.imgix.net/~text?txtsize=33&txt=ICON_HERE&w=400&h=400',
                                        srcRetina: 'https://placeholdit.imgix.net/~text?txtsize=33&txt=ICON_HERE&w=400&h=400',
                                        ratio: '3-2' //see welanstrap/_images.scsss for possible ratio values
                                      },
                                      items: [
                                      {
                                        title: '--- 4th level',
                                        url: 'https://welance-handbook.herokuapp.com/welance-development-guidelines.html',
                                        target: '_blank',
                                        class: 'col col--3-12',
                                        image: {
                                          src: 'https://placeholdit.imgix.net/~text?txtsize=33&txt=ICON_HERE&w=400&h=400',
                                          srcRetina: 'https://placeholdit.imgix.net/~text?txtsize=33&txt=ICON_HERE&w=400&h=400',
                                          ratio: '3-2' //see welanstrap/_images.scsss for possible ratio values
                                        }
                                      }
                                    ]
                                    }
                                  ]
                                },
                                {
                                  title: 'Web Design 2 - internal URL',
                                  url: '/styles',
                                  target: '_blank',
                                  class: 'col col--3-12',
                                  image: {
                                    src: 'https://placeholdit.imgix.net/~text?txtsize=33&txt=ICON_HERE&w=400&h=400',
                                    srcRetina: 'https://placeholdit.imgix.net/~text?txtsize=33&txt=ICON_HERE&w=400&h=400',
                                    ratio: '3-2' //see welanstrap/_images.scsss for possible ratio values
                                  }
                                },
                                {
                                  title: 'Web Design 2',
                                  url: '/',
                                  target: '_blank',
                                  class: 'col col--3-12',
                                  image: {
                                    src: 'https://placeholdit.imgix.net/~text?txtsize=33&txt=ICON_HERE&w=400&h=400',
                                    srcRetina: 'https://placeholdit.imgix.net/~text?txtsize=33&txt=ICON_HERE&w=400&h=400',
                                    ratio: '3-2' //see welanstrap/_images.scsss for possible ratio values
                                  }
                                }
                              ]
                            },
                            ...
                            ]
    </pre>
    <li v-if="level <= printLevels"
        class="tree__item"
        :class="[ 'tree__item-' + index,
                  columnClass && !ignoreClass ? columnClass : 'col--12-12',
                  (parentLevel === openParentLevel && level === openLevel) || level === 1 ? 'tree__item--visible' : 'tree__item--hidden',
                  openAll === true ? 'tree__item--always-visible' : '',
                  (index === selectedParentLevel && level <= selectedLevel) ? 'tree__item--open' : 'tree__item--close'
                ]"
        v-for="(item,index) in tree">

        <router-link v-if="urlType(item.url) === 'internal'" :to="item.url">
          <div v-if="item.image.src && printLevelImgs[level-1]" class="img"
                :class="[item.image.ratio ? 'img--ratio-' + item.image.ratio : '']"
                :style="{ backgroundImage: 'url(' + item.image.src + ')' }">
          </div>
          <template v-if="printLevelText[level-1]">{{item.title}}</template>
        </router-link>

        <a v-else-if="urlType(item.url) === 'external'" :href="item.url">
          <div v-if="item.image.src && printLevelImgs[level-1]" class="img"
                :class="[item.image.ratio ? 'img--ratio-' + item.image.ratio : '']"
                :style="{ backgroundImage: 'url(' + item.image.src + ')' }">
          </div>
          <template v-if="printLevelText[level-1]">{{item.title}}</template>
        </a>

        <a href v-else-if="urlType(item.url) === 'function'" v-on:click.stop.prevent="item.url" >
          <div v-if="item.image.src && printLevelImgs[level-1]" class="img"
                :class="[item.image.ratio ? 'img--ratio-' + item.image.ratio : '']"
                :style="{ backgroundImage: 'url(' + item.image.src + ')' }">
          </div>
          <template v-if="printLevelText[level-1]">{{item.title}}</template>
        </a>

        <template v-else>
          <div v-if="item.image.src && printLevelImgs[level-1]" class="img"
                :class="[item.image.ratio ? 'img--ratio-' + item.image.ratio : '']"
                :style="{ backgroundImage: 'url(' + item.image.src + ')' }">
          </div>
          <template v-if="printLevelText[level-1]">{{item.title}}</template>
        </template>


        <span @click.stop="toggleAccordion(index, level);"
              v-if="item.items && item.items.length && level <= printLevels-1 && mode === 'accordion' && openAll === false" class="tree__arrow"
              :class="(index === selectedParentLevel && level <= selectedLevel) ? 'tree__arrow--open' : 'tree__arrow--close'">
              <!--{{parentLevel}} = {{openParentLevel}} & {{level}} <= {{openLevel}}-->
        </span>
        <span @click.stop="toggleAccordion(index, level);"
              v-if="item.items && item.items.length && level <= printLevels-1 && mode === 'tree' && openAll === false" class="tree__arrow"
              :class="(index === selectedParentLevel && level <= selectedLevel) ? 'tree__arrow--open' : 'tree__arrow--close'">
              <!--{{parentLevel}} = {{openParentLevel}} & {{level}} <= {{openLevel}}-->
        </span>
        <!--<span class="tree__arrow tree__arrow--close" v-if="(parentLevel === openParentLevel && level >= selectedLevel) && item.items"></span>-->

        <tree
          v-if="item.items && level <= printLevels"
          :name="'tree-' + index"
          :debug="true"
          :contained="false"
          :open-all="openAll"
          :mode="mode"
          :open-parent-level="selectedParentLevel"
          :open-level="selectedLevel"
          :parent-level="index"
          :level="level+1"
          :print-levels="printLevels"
          :print-level-imgs="printLevelImgs"
          :print-level-text="printLevelText"
          :ignore-class="true"
          :tree="item.items">
        </tree>

    </li>

  </ul>
  </div>
</template>

<script>
export default {
    name: 'Tree',
    props:
    {
      'name': {
        type: String,
        default: 'default'
      },
      'debug': {
        type: Boolean,
        default: false
      },
      'contained': {
        type: Boolean,
        default: false
      },
      'mode': {
        type: String,
        default: ''
      },
      'level': {
        type: Number,
        default: 1
      },
      'parentLevel': {
        type: Number,
        default: 0
      },
      'openLevel': {
        type: Number,
        default: 0
      },
      'openParentLevel': {
        type: Number,
        default: 0
      },
      'openAll': {
        type: Boolean,
        default: false
      },
      'printLevels': {
        type: Number,
        default: 0
      },
      'columnClass': {
        type: String,
        default: ''
      },
      'ignoreClass': {
        type: Boolean,
        default: false
      },
      'printLevelImgs': {
        type: Array,
        default: []
      },
      'printLevelText': {
        type: Array,
        default: []
      },
      'tree': {
        type: Array,
        default: []
      }
    },
    data (){
      return{
        selectedLevel : 0,
        selectedParentLevel : 0
      }
    },
    methods: {
      toggleAccordion : function(parentLevel, level){
        // console.log('clicked', parentLevel+'-'+this.selectedParentLevel, level+1+'-'+this.selectedLevel);
        // alert(parentLevel+'-'+level);
        if((parentLevel === this.selectedParentLevel) && (level + 1 === this.selectedLevel)){
          this.selectedLevel = level - 1;
        }else{
          this.selectedLevel = level + 1;
        }
        this.selectedParentLevel = parentLevel;
        // console.log('selected:', this.selectedParentLevel, this.selectedLevel);
        // console.log('open:', this.openParentLevel, this.openLevel);
      },
      urlType: function (value) {
        if(typeof value == 'function'){
          return 'function';
        }else if(value.indexOf('http') !== -1){
          return 'external';
        }else if(value === '' || value === undefined || value === 'undefined'){
          return '';
        }else{
          return 'internal';
        }
      }
    }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss">
@import "~styles/main-sass-only.scss";

.tree{
  width:100%;

  &-wrap{
    margin: 0 auto;
    padding: 0;
  }
  &--visible{
    opacity: 1;
    height: auto;
  }
  &--hidden{
    opacity: 0;
    height: 0;
  }

  @include mq($until: 'md'){
    text-align: left;
    text-indent: 5%;
  }

  &__item{
    align-self: flex-start;
    .img{
      min-width: 100%;
    }
  }

  &--accordion{
    align-self: flex-start;
    color: $brand-secondary;
    a{
      color: $brand-secondary;
    }
    li {
      padding: 0;
      background: $brand-primary;
    }
    ul li{
      background: lighten($brand-primary, 8%);
      ul li{
        background: lighten($brand-primary, 12%);
        ul li{
          background: lighten($brand-primary, 22%);
          ul li{
            background: lighten($brand-primary, 27%);
            ul li{
              background: lighten($brand-primary, 35%);
            }
            ul li{
              background: lighten($brand-primary, 40%);
            }
          }
        }
      }
    }
    .tree__item{
      position: relative;
      overflow: hidden;
      transition: all .3s ease-in-out;
      &--open{
        color: $white;
        a{
          color: $white;
        }
      }
      &--visible{
        opacity: 1;
        height: auto;
      }
      &--hidden{
        opacity: 0;
        height: 0;
      }
      &--always-visible{
        opacity: 1!important;
        height: auto!important;
      }
    }
    .tree__arrow{
      cursor: pointer;
      color: $brand-secondary;
      transition: all .1s ease-in-out;
      width: 50px;
      height: 50px;
      position: absolute;
      right: 2%;
      top: 0;
      &::before{
        width: 50px;
        height: 50px;
        position: absolute;
        text-align: center;
        left: 0;
        line-height: 50px;
        content: "▶";
      }
      &--open{
        transform: rotate(90deg);
        color: $white;
        //color: red;
      }
      &--close{

      }
    }
  }
  &--tree{
    align-self: flex-start;

    li {
      padding: 0;
    }
    ul li{
      font-size: 1rem;
      ul li{
        font-size: .8rem;
        ul li{
          font-size: .8rem;
          ul li{
            font-size: .8rem;
            ul li{
              font-size: .6rem;
            }
            ul li{
              font-size: .6rem;
            }
          }
        }
      }
    }
    .tree__item{
      text-align: left;
      @include pb(0);
      @include pt(0);
      * > *{
        @include pl(1);
      }

      @include pr(1);
      transition: all .3s ease-in-out;
      &--visible{
        opacity: 1;
        height: auto;
        display: block;
      }
      &--hidden{
        opacity: 0;
        height: 0;
        display: none;
      }
      &--always-visible{
        opacity: 1!important;
        height: auto!important;
        display: block;
      }
    }
    .tree__arrow{
      cursor: pointer;
      position: absolute;
      color: $brand-primary;
      right: 5%;
      transition: all .2s ease-in-out;
      &::before{
        content: "+";
      }
      &--open{
        &::before{
          content: "-";
        }
      }
      &--close{
      }
    }
  }
};

</style>
