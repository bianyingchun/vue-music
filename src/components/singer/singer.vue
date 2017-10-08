<template>
  <div class="singer" ref="singer">
    <list-view @select="selectSinger" :data="singers" ref="list"></list-view>
    <router-view></router-view>
  </div>
</template>

<script type="text/ecmascript-6">
  import {getSingerList} from 'api/singer'
  import {mapMutations} from 'vuex'
  import ListView from 'base/listview/listview'
  import {ERR_OK} from 'api/config'
  import {Singer} from 'common/js/singer'
  const HOT_SINGER_LEN = 10
  const HOT_NAME = '热门'
  export default {
  	data() {
  		return {
  			singers: []
  		}
  	},
  	created() {
  		this._getSingerList();
  	},
  	methods: {
      ...mapMutations({
        setSinger:  'SET_SINGER'
      }),
      selectSinger(singer) {
        this.$router.push({
          path: `/singer/${singer.id}`
        });
        this.setSinger(singer)
      },
  		_getSingerList() {
  			getSingerList().then((res) => {
  				if(ERR_OK === res.code) {
  					this.singers = this._normalizeSinger(res.data.list);
            console.log(this.singers)
  				}
  			})
  		},

  		_normalizeSinger(list) {
  			let map = {
  				hot: {
  					title: HOT_NAME,
  					items: []
  				}
  			};
  			list.forEach((item, index) => {
  				if(index < HOT_SINGER_LEN) {
  					map.hot.items.push(new Singer(item.Fsinger_mid, item.Fsinger_name));
  				}

          const key = item.Findex;
          if(!map[key]) {
            map[key] = {
              title: key,
              items: []
            }
          }
          map[key].items.push(new Singer(item.Fsinger_mid, item.Fsinger_name));
  			});
        let hot = [];
        let ret = [];
        for (let key in map) {
          let val = map[key]
          if (val.title.match(/[a-zA-Z]/)) {
            ret.push(val);
          } else if (val.title === HOT_NAME) {
            hot.push(val);
          }
        };
        ret.sort((a, b) => {
          return a.title.charCodeAt(0) - b.title.charCodeAt(0)
        })
        return hot.concat(ret);
  		},
  	},
    components: {
      listView: ListView
    }
  }
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
  .singer
  	position:fixed
  	top:88px
  	bottom:0
  	width:100%
</style>