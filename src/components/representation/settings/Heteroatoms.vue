<template>
    <Panel :toggleable="modelHeteroatoms.length >= 1" v-model:collapsed="isCollapsed">
        <template #header>
            <i class="fab fa-hubspot fa-lg"></i> <div class="p-panel-title">{{ header }}</div>
        </template>
        <template #icons>
            <Button 
            :icon="allSelected ? 'fas fa-times-circle' : 'fas fa-check-circle'" 
            class="p-button-rounded p-button-text" 
            style="font-size:16px;" 
            @click="selectAll" 
            v-tooltip.left="ttpsa"
            :disabled="modelHeteroatoms.length == 0" />
            <!--<Button icon="fas fa-eye" class="p-button-rounded p-button-text" />-->
        </template>

        <Listbox 
        v-model="selectedHets" 
        :options="modelHeteroatoms" 
        :multiple="true" 
        :filter="true" 
        :filterPlaceholder="filterPlaceholder"
        optionLabel="name" 
        listStyle="max-height:200px" 
        style="width:100%"
        @change="onChange">
            <template #option="slotProps">
                <!-- TODO: FIX STYLES FOR MOUSE / LEAVE ACTIONS (IN IONS AS WELL) -->
                <div style="width:100%;padding:.5rem 1rem"
                    @mouseover="onHover(slotProps.option.id)"
                    @mouseleave="onLeave(slotProps.option.id)" >
                    <!-- TODO: MOVE TO STYLES (SETTINGS.VUE) -->
                    <div style="position:absolute;right:.5rem;z-index:100">
                        <Button 
                        icon="fas fa-bullseye" 
                        class="p-button-rounded p-button-text" 
                        v-tooltip.top="'asdasdasdasdasdasd'"
                        @click="centerHetero(slotProps.option.id)"  />
                    </div>
                    <div>{{slotProps.option.name}}</div>
                <!--TODO: CENTER BUTTON ON EACH HETERO 
                <i class="fab fas fa-eye"></i>-->
                </div>
            </template>
        </Listbox>

    </Panel>
</template>

<script>
import { ref, reactive, computed, watch, toRefs } from 'vue'
import structureSettings from '@/modules/structure/structureSettings'
export default {
    setup() {

        const { getCurrentChains, getChainContent } = structureSettings()

        const isCollapsed = ref(true)
        const allSelected = ref(false)

        const page = reactive({
            header: "Heteroatoms",
            filterPlaceholder: "Search heteroatom",
            ttpsa: "Select all heteroatoms"
        })

        let selectedHets = ref(null)

        // trick for creating reactivity with computed property
        const watchedChains = computed(() => getCurrentChains())

        const getModelContent = (wch, label) => {
            const chains = []
            for(const c of wch) chains.push(c.id)
            const allContent = getChainContent(label)
            return allContent.filter(item => chains.includes(item.id))
        }

        const computeHeteroList = (wch, label) => {
            const hets = []
            const het_chains = getModelContent(wch, label)
            for(const chain of het_chains) {
                for(const het of chain.heteroatoms) {
                    hets.push({
                        name: chain.id.toUpperCase() + ': ' + het.name + ' ' + het.num,
                        id: het.num + ':' + chain.id.toUpperCase() + '/' + het.model
                    })
                }
            }
            return hets
        }

        let modelHeteroatoms = computed(() => computeHeteroList(watchedChains.value, 'heteroatoms'))
        //console.log(modelHeteroatoms.value)

        const selectAll = () => {
            page.ttpsa = allSelected.value ? 'Select all heteroatoms' : 'Unselect all heteroatoms'
            selectedHets.value = allSelected.value ? null : modelHeteroatoms.value
            allSelected.value = !allSelected.value
        }

        // TODO: REPLACE BY COMPUTED GETTER / SETTER
        watch([watchedChains, selectedHets], (newValues, prevValues) => {
            const wch = newValues[0]
            const shts = newValues[1]
            modelHeteroatoms  =  computed(() => computeHeteroList(wch, 'heteroatoms'))
            //console.log(modelHeteroatoms.value)
            if(modelHeteroatoms.value.length < 1) isCollapsed.value = true
            if(!shts || shts.length === 0) allSelected.value = false
            if(shts && (shts.length === modelHeteroatoms.value.length)) allSelected.value = true
        })

        const centerHetero = (v) => {
            // TODO: SYNC WITH STAGE (IONS AS WELL!!!)
            console.log(v)
        }

        const onHover = (v) => {
            // TODO: SYNC WITH STAGE (IONS AS WELL!!!)
            console.log(v)
        }

        const onLeave = (v) => {
            // TODO: SYNC WITH STAGE (IONS AS WELL!!!)
            console.log(v)
        }

        const onChange = (e) => {
            //console.log(e.value)
            // TODO!!!! ADD TO NAVIGATION
            console.log(selectedHets.value)
        }

        return { 
            ...toRefs(page), isCollapsed, 
            modelHeteroatoms,
            selectedHets, centerHetero, onChange, onHover, onLeave,
            allSelected, selectAll
        }
    }
}
</script>

<style>
    
</style>