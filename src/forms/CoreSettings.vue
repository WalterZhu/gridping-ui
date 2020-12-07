<template>
    <form @submit.prevent="saveSettings">
        <div class="card">
            <div class="card-header">Statping Settings</div>
            <div class="card-body">
                <div class="form-group">
                    <label> name </label>
                    <input v-model="core.name" type="text" class="form-control" placeholder="Great Uptime" id="name">
                </div>

                <div class="form-group">
                    <label> title </label>
                    <input v-model="core.title" type="text" class="form-control" placeholder="Great Uptime" id="title">
                </div>

                <div class="form-group">
                    <label>{{ $t('description') }}</label>
                    <input v-model="core.description" type="text" class="form-control" placeholder="Great Uptime"
                           id="description">
                </div>

                <div class="form-group">
                    <label></label>
                    <input v-model="core.description" type="text" class="form-control" placeholder="Great Uptime"
                           id="description">
                </div>

                <div class="form-group">
                        <label>{{ $t('domain') }}</label>
                        <input v-model="core.domain" type="url" class="form-control" id="domain">
                </div>

                <div class="form-group">
                    <label>{{ $t('footer') }}</label>
                    <textarea v-model="core.footer" rows="4" class="form-control" id="footer">{{core.footer}}</textarea>
                    <small class="form-text text-muted">{{ $t('footer_notes') }}</small>
                </div>

                <div class="form-group">
                    <label>{{ $t('language') }}</label>
                    <select v-model="core.language" class="form-control">
                        <option value="en">English</option>
                        <option value="es">Spanish</option>
                        <option value="fr">French</option>
                        <option value="ru">Russian</option>
                        <option value="de">German</option>
                        <option value="ja">Japanese</option>
                        <option value="it">Italian</option>
                        <option value="ko">Korean</option>
                        <option value="zh">Chinese</option>
                    </select>
                </div>

            </div>
            <div class="card-footer">
                <button @click.prevent="saveSettings" id="save_core" type="submit" class="btn btn-primary btn-block"
                        v-bind:disabled="loading">
                    <font-awesome-icon v-if="loading" icon="circle-notch" class="mr-2" spin/>
                    {{ $t('save_settings') }}
                </button>
            </div>
        </div>
    </form>
</template>

<script>
    import Api from '../API'

    export default {
        name: 'CoreSettings',
        data() {
            return {
                loading: false
            }
        },
        computed: {
            core() {
                return this.$store.getters.core
            }
        },
        methods: {
            async saveSettings() {
                this.loading = true
                const c = this.core
                await Api.core_save(c)
                this.$store.commit('setCore', c)
                this.$i18n.locale = c.language || "en";
                this.loading = false
            },
            selectAll() {
                this.$refs.input.select();
            }
        }
    }
</script>
