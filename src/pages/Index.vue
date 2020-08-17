<template>
  <q-page class="bg-grey-3 column">
    <!-- Formulario para agregar items -->
    <div class="row q-pa-md bg-primary">
      <q-input
        @keyup.enter="agregarItem"
        bottom-slots
        v-model="textoNuevoItem"
        placeholder="Ingrese nuevo item"
        class="col"
        bg-color="white"
        filled
        dense>
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="agregarItem" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white q-pa-md" separator bordered>
      <q-item
        v-for="(item, i) in items"
        :key="i"
        tag="label"
        v-ripple
        clickeable
        :class="{'bg-blue-1 listo': item.listo == true}">
        <q-item-section avatar>
          <q-checkbox v-model="item.listo" color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{item.texto}}</q-item-label>
        </q-item-section>
        <q-item-section side>
          <q-btn v-if="item.listo" flat round color="blue" icon="delete" @click.stop="eliminarItem(i)"  />
        </q-item-section>
      </q-item>
    </q-list>
  </q-page>
</template>

<script>
export default {
  name: 'Index',
  data () {
    return {
      items: [
        {
          texto: 'cloro',
          listo: false
        },
        {
          texto: 'pan',
          listo: false
        },
        {
          texto: 'chocolate',
          listo: false
        }
      ],
      textoNuevoItem: ''
    }
  },
  methods: {
    eliminarItem (i) {
      this.$q.dialog({
        dark: true,
        title: 'Alerta',
        message: '¿Estás seguro de eliminar este item?'
      }).onOk(() => {
        // Esto es cuando el usuario esta OK con eliminar el item
        this.items.splice(i, 1)
        this.$q.notify('Item eliminado')
      })
    },
    agregarItem () {
      if (this.textoNuevoItem === '') {
        return
      }
      const nuevoItem = {
        texto: this.textoNuevoItem,
        listo: false
      }
      this.items.push(nuevoItem)
      this.textoNuevoItem = ''
    }
  }
}
</script>

<style lang="scss">
  .listo {
    .q-item__label {
      text-decoration: line-through;
    }
  }
</style>
