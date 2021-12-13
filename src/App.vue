<template>
    <vue-jasper  :url="jasperUrl" :username="jasperUsername" :password="jasperPassword"/>
</template>

<script>
import VueJasper from 'vue-jasper'


export default {
    components: {
        VueJasper
    },
    data() {
        return {
            jasperUrl: 'http://localhost:8080/jasperserver-pro/rest_v2',
            jasperUsername: 'jasperadmin',
            jasperPassword: 'jasperadmin'
        }
    },
}

</script>