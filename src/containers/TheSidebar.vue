<template>
<CSidebar fixed :minimize="minimize" :show="show" @update:show="(value) => $store.commit('set', ['sidebarShow', value])">
    <CSidebarBrand class="d-md-down-none" to="/">
        <CIcon class="c-sidebar-brand-full" name="logo" size="custom-size" :height="35" viewBox="0 0 125.5 35" />
        <CIcon class="c-sidebar-brand-minimized" name="logo" size="custom-size" :height="35" viewBox="0 0 110 134" />
    </CSidebarBrand>
    <CRenderFunction flat :contentToRender="computedSidebar" />
    <!-- <CSidebarMinimizer class="d-md-down-none" @click.native="$store.commit('set', ['sidebarMinimize', !minimize])" /> -->
</CSidebar>
</template>

<script>
import nav from './_nav'
import {
    mapGetters,
} from 'vuex'
export default {
    data() {
        return {
            role: "user",
        }
    },
    name: 'TheSidebar',
    computed: {
        show() {
            return this.$store.state.sidebarShow
        },
        minimize() {
            return this.$store.state.sidebarMinimize
        },
        currentItems() {
            //sidebar items are not shown until role is known
            if (this.role === "unknown") {
                return [];
            }
            return nav.navItems.filter(item => {
                if (this.rolesusuario === undefined) {
                    return [];
                } else {
                    return !item.permissions || this.rolesusuario.some(r => item.permissions.includes(r.name))

                }

            });
        },
        computedSidebar() {
            return [{
                _name: "CSidebarNav",
                _children: this.currentItems
            }];
        },
        ...mapGetters({
            rolesusuario: 'auth/roles'
        })
    },
}
</script>
