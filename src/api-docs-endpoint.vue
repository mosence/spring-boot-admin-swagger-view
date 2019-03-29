<!--
  - Copyright 2014-2018 the original author or authors.
  -
  - Licensed under the Apache License, Version 2.0 (the "License");
  - you may not use this file except in compliance with the License.
  - You may obtain a copy of the License at
  -
  -     http://www.apache.org/licenses/LICENSE-2.0
  -
  - Unless required by applicable law or agreed to in writing, software
  - distributed under the License is distributed on an "AS IS" BASIS,
  - WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  - See the License for the specific language governing permissions and
  - limitations under the License.
  -->

<template>
  <div 
    class="apidocs" 
    id="apidocs"/>
</template>

<script>
  import 'swagger-ui/dist/swagger-ui.css'
  import SwaggerUI from 'swagger-ui'

  export default {
    props: {
      instance: { //<1>
        type: Object,
        required: true
      }
    },
    data: () => ({
    }),
    async created() {
      const response = await this.instance.axios.get('actuator/api-docs');
      let api = response.data;
      const ui = SwaggerUI({
        dom_id: '#apidocs',
        spec: api,
        filter: true
      })
    }
  };
</script>

<style>
  .apidocs {
    font-size: 20px;
    width: 100%;
  }
</style>
