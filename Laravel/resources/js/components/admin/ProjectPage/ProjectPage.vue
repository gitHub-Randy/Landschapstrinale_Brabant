<template>
    <v-container fluid fill-height pt-3 pb-5>
        <v-layout row fill-height justify-space-around>
            <v-flex xs6 class="ml-5">
                <projects-view :parent="this" :headers="headers" :values="filteredProjects"  v-if="currentPageState === ProjectPageStates.viewMode"></projects-view>
                <projects-new :parent="this" v-else-if="currentPageState === ProjectPageStates.newMode"></projects-new>
                <project-edit :parent="this" ref="projectEditSection" v-show="currentPageState === ProjectPageStates.editMode"></project-edit>
            </v-flex>
            <v-flex d-flex xs5>
                <map-section></map-section>
            </v-flex>
        </v-layout>
    </v-container>
</template>

<script>
    import MapSection from '../Map';
    import ProjectsView from './ProjectsView';
    import ProjectsNew from './ProjectsNew';
    import ProjectEdit from './ProjectsEdit';

    export default {
        name: "ProjectList",
        components: {
            MapSection,
            ProjectsView,
            ProjectsNew,
            ProjectEdit
        },
        data() {
            return {
                ProjectPageStates: {'viewMode': 0, 'editMode': 1, 'newMode': 2},
                currentPageState: 0,
                headers: [
                    {
                        text: 'Naam',
                        align: "left",
                        value: 'name',
                    },
                    {
                        text: 'Categorie',
                        align: 'left',
                        value: 'category',
                    },
                    {
                        text: 'Beschrijving',
                        align: 'left',
                        value: 'information',
                    },
                ],
                values: [],
                filteredProjects: []
            }
        },
        methods: {
            newProjectButtonPressed() {
                this.currentPageState = this.ProjectPageStates.newMode;
            },
            enableViewMode() {
                this.currentPageState = this.ProjectPageStates.viewMode;
                this.values = [];
                axios.get('/getProjects')
                    .then(({data}) => {
                        for(let i = 0; i < data.length; i++){
                            this.values.push({
                                id: data[i].id,
                                name: data[i].name,
                                category: data[i].category,
                                information: data[i].information
                            });
                        }
                        this.filteredProjects = this.values;
                    });
            },
            editAProject(product) {
                this.currentPageState = this.ProjectPageStates.editMode;
                this.$refs.projectEditSection.projectEditSection(product);
            },
            filterList(search) {
                this.filteredProjects = this.values.filter(project => {
                    return project.name.toLowerCase().includes(search.toLowerCase());
                });
            }
        },
        mounted(){
            this.enableViewMode();
        }
    }
</script>

<style>
    .projectTable .v-datatable .v-datatable__actions .v-datatable__actions__select {
        display: none;
    }

    .projectTable .v-table__overflow {
        overflow-y: hidden;
        padding-right: 15px;
    }

    .removeScrollBar::-webkit-scrollbar {
        display: none;
    }

    .removeScrollBar {
        scrollbar-width: none;
    }

    .vb > .vb-dragger {
        z-index: 5;
        width: 12px;
        right: 0;
    }

    .vb > .vb-dragger > .vb-dragger-styler {
        -webkit-backface-visibility: hidden;
        backface-visibility: hidden;
        -webkit-transform: rotate3d(0, 0, 0, 0);
        transform: rotate3d(0, 0, 0, 0);
        -webkit-transition: background-color 100ms ease-out,
        margin 100ms ease-out,
        height 100ms ease-out;
        transition: background-color 100ms ease-out,
        margin 100ms ease-out,
        height 100ms ease-out;
        background-color: rgba(38, 38, 38, 0.1);
        margin: 5px 5px 5px 0;
        border-radius: 20px;
        height: calc(100% - 10px);
        display: block;
    }

    .vb.vb-scrolling-phantom > .vb-dragger > .vb-dragger-styler {
        background-color: rgba(38, 38, 38, 0.3);
    }

    .vb > .vb-dragger:hover > .vb-dragger-styler {
        background-color: rgba(38, 38, 38, 0.5);
        margin: 0px;
        height: 100%;
    }

    .vb.vb-dragging > .vb-dragger > .vb-dragger-styler {
        background-color: rgba(38, 38, 38, 0.5);
        margin: 0px;
        height: 100%;
    }

    .vb.vb-dragging-phantom > .vb-dragger > .vb-dragger-styler {
        background-color: rgba(38, 38, 38, 0.5);
    }
</style>
