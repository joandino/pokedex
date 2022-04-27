<template>
    <v-dialog
    v-model="dialog"
    width="50%"
    persistent
    >
        <v-card>
            <v-card-title class="text-h5 blue lighten-2">
                <h3>{{this.pokeinfo.name}}</h3>
            </v-card-title>

            <v-container>
                <v-row no-gutters>
                <v-col
                    cols="12"
                    sm="6"
                    md="6"
                >
                    Coming soon...
                </v-col>
                <v-col
                    cols="6"
                    md="6"
                >
                </v-col>
                </v-row>
            </v-container>

            <v-divider></v-divider>

            <v-card-actions>
            <v-spacer></v-spacer>
            <v-btn
                color="primary"
                text
                @click="closeDialog()"
            >
                Close
            </v-btn>
            </v-card-actions>
        </v-card>
    </v-dialog>
</template>

<script>
export default {
    name: 'PokeCard',
    props: ['pokeinfo', 'showDialog'],
    methods: {
        closeDialog() {
            this.$emit('closeDialog', true);
        }
    },
    computed: {
        dialog() {
            return this.showDialog;
        }
    }
}
</script>

<style>
h3::first-letter {
    text-transform: capitalize;
}
</style>