<template>
    <div class="col-12">
        <div class="card contain-card mb-4">
            <div class="card-header">
                Logs
                <div class="input-group input-group-sm float-right col-6">
                    <div class="input-group-prepend">
                        <span class="input-group-text" id="inputGroup-sizing-sm">Search</span>
                    </div>
                    <input v-model="search" type="text" class="form-control">
                </div>
            </div>
            <div class="card-body">
                <p v-if="logs.length === 0" class="text-monospace sm">
                    Loading Logs...
                </p>
                <div v-for="(log, index) in logs">
                    <span class="badge badge-secondary small mr-2">{{log.time}}</span>
                    <span class="text-monospace small">{{log.message}}</span>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import Api from "../API";

    const moment = require('moment');

    export default {
        name: 'Logs',
        data() {
            return {
                logs_record: [],
                last: "",
                search: "",
                t: null
            }
        },
        computed: {
            logs() {
                if (this.search) {
                    return this.logs_record.filter(o => o.message.includes(this.search));
                } else {
                    return this.logs_record
                }
            }
        },
        async created() {
            await this.getLogs()
            if (!this.t) {
                this.t = setInterval(async () => {
                    await this.lastLog()
                }, 10000)
            }
        },
        beforeDestroy() {
            clearInterval(this.t)
        },
        methods: {
            parseLog(data) {
                const ts = data.split(":")
                return {
                    time: moment(ts[0], "X").format("YYYY-MM-DD hh:mm:ss"),
                    message: data.split(ts[0] + ": ")[1]
                }
            },
            cleanLog(l) {
                const splitLog = l.split(": ")
                const last = splitLog.slice(1);
                return last.join(": ")
            },
            async getLogs() {
                const l = await Api.logs()
                l.forEach((d) => {
                    this.logs_record.push(this.parseLog(d))
                })
                this.last = this.cleanLog(l[l.length - 1])
            },
            async lastLog() {
                const log = await Api.logs_last()
                const cleanLast = this.cleanLog(log)
                if (this.last !== cleanLast) {
                    this.last = cleanLast
                    this.logs_record.unshift(this.parseLog(log))
                }
            }
        }
    }
</script>
