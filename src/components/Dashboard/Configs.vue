<template>
    <div>
        <h3>Configuration</h3>
        For security reasons, all database credentials cannot be editted from this page.

        <codemirror ref="cm" v-show="true" :value="code" :options="cmOptions" class="mt-4"></codemirror>

        <button @click.prevent="save" class="btn col-12 btn-primary mt-3">Save</button>
    </div>
</template>

<script>
    import Api from "../../API";

    import {codemirror} from 'vue-codemirror'
    import 'codemirror/lib/codemirror.css'
    import 'codemirror/mode/yaml/yaml.js'

    export default {
        components: {
            codemirror
        },
        data() {
            return {
                code: this.configs,
                cmOptions: {
                    height: 700,
                    tabSize: 4,
                    lineNumbers: true,
                    matchBrackets: true,
                    mode: "text/x-yaml",
                    line: true
                }
            }
        },
        computed: {
            codemirror () {
                return this.$refs.cm.codemirror
            }
        },
        async mounted() {
            this.configs = await Api.configs()
        },
        methods: {
            async save() {
                try {
                    await Api.configs_save(this.configs)
                } catch (e) {
                    window.console.error(e)
                }
            }
        }
    }
</script>
