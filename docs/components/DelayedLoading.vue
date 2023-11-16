<template>
  <treeselect
    :multiple="true"
    :options="options"
    :load-options="loadOptions"
    placeholder="Try expanding any folder option..."
    v-model="value"
    />
</template>

<script>
  import { LOAD_CHILDREN_OPTIONS } from '@riophae/vue-treeselect'

  // We just use `setTimeout()` here to simulate an async operation
  // instead of requesting a real API server for demo purpose.
  const simulateAsyncOperation = fn => {
    setTimeout(fn, 500)
  }

  export default {
    data: () => ({
      value: [ 'child1' ],
      options: [ {
        id: 'success',
        label: 'With children',
        // Declare an unloaded branch node.
        children: null,
      }, {
        id: 'no-children',
        label: 'With no children',
        children: null,
      }, {
        id: 'failure',
        label: 'Demonstrates error handling',
        children: null,
      } ],
    }),

    methods: {
      loadOptions({ action, parentNode, callback }) {
        // Typically, do the AJAX stuff here.
        // Once the server has responded,
        // assign children options to the parent node & call the callback.

        if (action === LOAD_CHILDREN_OPTIONS) {
          switch (parentNode.id) {
          case 'success': {
            simulateAsyncOperation(() => {
              parentNode.children = [ {
                id: 'child1',
                label: 'Child option 1',
              }, {
                id: 'child2',
                label: 'Child option 2',
              } ]
              callback()
            })
            break
          }
          case 'no-children': {
            simulateAsyncOperation(() => {
              parentNode.children = []
              callback()
            })
            break
          }
          case 'failure': {
            simulateAsyncOperation(() => {
              callback(new Error('Failed to load options: network error.'))
            })
            break
          }
          default: /* empty */
          }
        }
      },
    },
  }
</script>
