<template>
	<v-app>
	<v-navigation-drawer app clipped v-model="drawer">
		<v-list>
			<v-list-item to="/">
				<v-list-item-action>
					<v-icon>library_books</v-icon>
				</v-list-item-action>
				<v-list-item-content>
					<v-list-item-title>报告生成</v-list-item-title>
				</v-list-item-content>
			</v-list-item>
			<v-list-item to="/history">
				<v-list-item-action>
					<v-icon>history</v-icon>
				</v-list-item-action>
				<v-list-item-content>
					<v-list-item-title>历史日志</v-list-item-title>
				</v-list-item-content>
			</v-list-item>
		</v-list>
	</v-navigation-drawer>
	<v-app-bar app dense clipped-left dark color="orange darken-4">
		<v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
		<v-toolbar-title>
			华三报告生成
		</v-toolbar-title>
	</v-app-bar>
	<v-content>
		<transition name="fade">
			<router-view/>
		</transition>
	</v-content>
	</v-app>
</template>

<script>
export default {
  name: 'App',
  data: () => ({
	  drawer: null
  })
};
</script>

<style>
.fade-enter-active, .fade-leave-active {
  transition-property: opacity;
  transition-duration: .15s;
}
.fade-enter-active {
  transition-delay: .15s;
}
.fade-enter, .fade-leave-active {
  opacity: 0
}
</style>