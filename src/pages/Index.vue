<template>
  <q-page class="flex flex-center full-height q-ma-md">
    <q-btn
      label="Get Data"
      @click="getData"
      style="position: relative; top: -300px"
    />
    <iframe
      ref="form"
      style="position: absolute; top: 50; left: 10; right: 10; bottom: 10; border: none;"
      height="50%"
      width="100%"
      src="/form.html"
    />
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  data () {
    return {
      schema:
  {
    type: 'object',
    properties: {
      objectSection1: {
        title: "I'm a section",
        description: "I'm a description shown as a paragraph on top of section",
        type: 'object',
        properties: {
          stringProp1: {
            type: 'string',
            title: "I'm a property in section 1"
          }
        }
      },
      objectSection2: {
        title: "I'm another section",
        type: 'object',
        properties: {
          stringProp2: {
            type: 'string',
            title: "I'm a property in section 2"
          }
        },
        allOf: [
          {
            title: "I'm a subsection",
            properties: {
              stringProp3: {
                type: 'string',
                title: "I'm a property in subsection 2.1"
              }
            }
          },
          {
            title: "I'm another subsection",
            properties: {
              stringProp4: {
                type: 'string',
                title: "I'm a property in subsection 2.2"
              }
            }
          }
        ]
      }
    }
  }
    }
  },
  async mounted () {
    const child = this.$refs.form.contentWindow

    // Wait for form to load
    await new Promise((resolve) => {
      const check = () => {
        if (child && child.vm) {
          resolve()
        } else {
          setTimeout(check, 100)
        }
      }

      check()
    })

    child.vm.$data.schema = this.schema
  },
  methods: {
    getData () {
      alert(JSON.stringify(this.$refs.form.contentWindow.vm.$data.model))
    }
  }
}
</script>
