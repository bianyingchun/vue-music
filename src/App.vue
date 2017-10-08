<template>
  <div id="app">
		<v-header></v-header>   
		<tab></tab>
		<keep-alive>
			<router-view></router-view>
		</keep-alive>
    <player></player>
  </div>
</template>

<script type="text/ecmascript-6">
import header from 'components/header/header'
import tab from 'components/tab/tab'
import player from 'components/player/player'
export default {
  components: {
  	'v-header' :header,
  	tab,
    player
  }
}
</script>

<style scoped lang="stylus" rel="stylesheet/stylus">
</style>
