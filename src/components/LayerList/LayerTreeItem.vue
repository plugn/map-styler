<template>

	<ul v-if="isRoot" class="tile__root gu-unselectable">
		<layer-tree-item :model="listItem" :itemIndex="listKey" key="listKey" v-for="(listItem, listKey) in model"></layer-tree-item>
	</ul>

	<li v-else-if="isFolder" class="tile tile__folder">
		<span class="tile__name bold" @click.self="toggle">{{ model.id }} [ {{open ? '-' : '+'}} ]</span>
		<ul v-show="open" class="tile__list" :data-group="itemIndex">
			<li hidden></li>
			<layer-tree-item
					v-for="(listItem, listKey) in model.children"
					key="listKey"
					:model="listItem"
					:itemIndex="listKey"
					:data-group="itemIndex"></layer-tree-item>
		</ul>
	</li>

	<li v-else class="tile" @click.self="tweakLayer(model.id)">{{model.id}}</li>

</template>


<script>
	import {eventBus} from '../../main'

	export default {
		name: 'LayerTreeItem',
		props: {
			model: [Object, Array],
			itemIndex: [Number]
		},

		data() {
			return {
				open: false
			}
		},

		computed: {
			isFolder: function () {
				return this.model.children &&
					this.model.children.length
			},
			isRoot: function () {
				return Array.isArray(this.model);
			}
		},
		methods: {
			toggle: function () {
				if (this.isFolder) {
					this.open = !this.open;
				}
			},
			tweakLayer(layerId) {
				console.log('tweakLayer()', layerId);
				eventBus.$emit('tweakLayer', layerId);
			}
		}
	}

</script>

<style src="./LayerList.css" scoped></style>
