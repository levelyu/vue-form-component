<template>
    <div id="app">
        <div id="nav">
            <ul>
                <li>
                    <router-link to="/form">Form</router-link>
                </li>
            </ul>
        </div>
        <hr>
        <router-view/>
    </div>
</template>
<script>
    export default {
      name: 'App',
    }
</script>
