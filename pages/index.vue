<template>
    <v-row justify="center" align="center">
        <v-col cols="12" sm="8" md="6">
            <div class="text-center">
                <logo />
                <vuetify-logo />
            </div>
            <v-card>
                <v-card-title class="headline">
                    Nuxt.js(TypeScript) × Djangoで作るTodoアプリ
                </v-card-title>
                <v-card-text class="text-center">
                    <p>Front-End: Vuetify + Nuxt.js (TypeScript)</p>
                    <p>Back-End: Django3.1(Python)</p>
                    <p>Database: MySQL8.0</p>
                    <hr class="my-0" />
                </v-card-text>
                <v-card-actions>
                    <v-spacer />
                    <v-btn color="primary" nuxt to="/task"
                        >Let's create a task.</v-btn
                    >
                    <v-spacer />
                </v-card-actions>
            </v-card>
        </v-col>
    </v-row>
</template>

<script>
import Logo from '~/components/Logo.vue'
import VuetifyLogo from '~/components/VuetifyLogo.vue'

export default {
    components: {
        Logo,
        VuetifyLogo,
    },
}
</script>
