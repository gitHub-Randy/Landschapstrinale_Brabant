<template>
    <v-layout row fill-height align-center justify-center>
        <v-flex d-flex>
            <v-card-title class="display-2" style="color: #89a324;">
                Projecten
            </v-card-title>
        </v-flex>
        <v-flex fill-height xs6>
            <v-layout row fill-height align-center justify-end>
                <v-btn style="max-width: 10%; height: 55%" color="#89a324" @click="OnEditProjectButtonPressed">
                    <v-card style="white-space: normal; max-width: 60%; " color="transparent" flat class="white--text">
                        Project Toevoegen
                    </v-card>
                </v-btn>
                <v-text-field solo hide-details round
                              label="zoek een project"
                              class="search-bar pa-0" flat
                              append-icon="search"
                              v-model="search"
                ></v-text-field>
            </v-layout>
        </v-flex>
    </v-layout>
</template>

<script>
    export default {
        name: "ProjectsHeader",
        props: {
            OnEditProjectButtonPressed: {
                type: Function,
                required: true,
            },
            parent: {
                type: Object,
                required: true,
            }
        },
        data() {
            return {
                search: ""
            }
        },
        watch: {
            search() {
                this.parent.filterList(this.search);
            }
        }
    }
</script>

<style scoped>

</style>