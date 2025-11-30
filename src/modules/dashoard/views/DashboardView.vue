<template>
  <div class="flex h-screen bg-gray-50 overflow-hidden">
    <!-- Sidebar -->
    <aside
      class="bg-gradient-to-b from-slate-900 to-slate-800 text-white flex flex-col transition-all duration-300 flex-shrink-0"
      :class="sidebarExpanded ? 'w-64' : 'w-20'"
    >
      <!-- Header/Logo -->
      <div class="p-4 border-b border-slate-700">
        <div class="flex items-center justify-between">
          <div v-show="sidebarExpanded" class="flex items-center gap-3">
            <div class="w-10 h-10 bg-orange-500 rounded-lg flex items-center justify-center">
              <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-6 0a1 1 0 001-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 001 1m-6 0h6" />
              </svg>
            </div>
            <span class="font-bold text-lg">RestaurantPOS</span>
          </div>
          <button
            @click="toggleSidebar"
            class="p-2 hover:bg-slate-700 rounded-lg transition-colors"
          >
            <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4 6h16M4 12h16M4 18h16" />
            </svg>
          </button>
        </div>
      </div>

      <!-- Navigation -->
      <nav class="flex-1 p-4 space-y-2">
        <button
          v-for="item in menuItems"
          :key="item.id"
          @click="activeView = item.id"
          class="w-full flex items-center gap-3 px-4 py-3 rounded-lg transition-all duration-200 group"
          :class="activeView === item.id
            ? 'bg-orange-500 text-white shadow-lg shadow-orange-500/30'
            : 'hover:bg-slate-700 text-slate-300 hover:text-white'"
        >
          <component :is="item.icon" class="w-5 h-5 flex-shrink-0" />
          <span
            v-show="sidebarExpanded"
            class="font-medium"
          >
            {{ item.label }}
          </span>
          <span
            v-if="item.badge && sidebarExpanded"
            class="ml-auto bg-red-500 text-white text-xs px-2 py-1 rounded-full"
          >
            {{ item.badge }}
          </span>
        </button>
      </nav>

      <!-- Footer/User -->
      <div class="p-6 border-t border-slate-700 mt-auto">
        <div class="flex items-center gap-3">
          <div class="w-10 h-10 bg-slate-600 rounded-full flex items-center justify-center flex-shrink-0">
            <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" />
            </svg>
          </div>
          <div v-show="sidebarExpanded" class="overflow-hidden">
            <p class="text-sm font-medium truncate">Usuario Admin</p>
            <p class="text-xs text-slate-400 truncate">admin@restaurant.com</p>
          </div>
        </div>
      </div>
    </aside>

    <!-- Main Content -->
    <main class="flex-1 flex flex-col overflow-hidden">
      <!-- Top Bar -->
      <header class="bg-white border-b border-gray-200 px-6 py-4">
        <div class="flex items-center justify-between">
          <div>
            <h1 class="text-2xl font-bold text-gray-800">{{ currentViewTitle }}</h1>
            <p class="text-sm text-gray-500 mt-1">{{ currentViewSubtitle }}</p>
          </div>
          <div class="flex items-center gap-4">
            <!-- Notifications -->
            <button class="relative p-2 hover:bg-gray-100 rounded-lg transition-colors">
              <svg class="w-6 h-6 text-gray-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6.002 6.002 0 00-4-5.659V5a2 2 0 10-4 0v.341C7.67 6.165 6 8.388 6 11v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9" />
              </svg>
              <span class="absolute top-1 right-1 w-2 h-2 bg-red-500 rounded-full"></span>
            </button>
            <!-- Settings -->
            <button class="p-2 hover:bg-gray-100 rounded-lg transition-colors">
              <svg class="w-6 h-6 text-gray-600" fill="none" stroke="currentColor" viewBox="0 0 24 24">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M10.325 4.317c.426-1.756 2.924-1.756 3.35 0a1.724 1.724 0 002.573 1.066c1.543-.94 3.31.826 2.37 2.37a1.724 1.724 0 001.065 2.572c1.756.426 1.756 2.924 0 3.35a1.724 1.724 0 00-1.066 2.573c.94 1.543-.826 3.31-2.37 2.37a1.724 1.724 0 00-2.572 1.065c-.426 1.756-2.924 1.756-3.35 0a1.724 1.724 0 00-2.573-1.066c-1.543.94-3.31-.826-2.37-2.37a1.724 1.724 0 00-1.065-2.572c-1.756-.426-1.756-2.924 0-3.35a1.724 1.724 0 001.066-2.573c-.94-1.543.826-3.31 2.37-2.37.996.608 2.296.07 2.572-1.065z" />
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 12a3 3 0 11-6 0 3 3 0 016 0z" />
              </svg>
            </button>
          </div>
        </div>
      </header>

      <!-- Content Area -->
      <div class="flex-1 overflow-auto p-6">
        <!-- Mesas View -->
        <div v-if="activeView === 'mesas'" class="space-y-6">
          <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6">
            <div
              v-for="mesa in mesas"
              :key="mesa.id"
              class="bg-white rounded-xl shadow-sm border-2 p-6 cursor-pointer transition-all duration-200 hover:shadow-xl hover:scale-105"
              :class="getMesaStatusClass(mesa.status)"
              @click="selectMesa(mesa)"
            >
              <div class="flex items-center justify-between mb-4">
                <h3 class="text-lg font-bold">Mesa {{ mesa.numero }}</h3>
                <span
                  class="px-3 py-1 rounded-full text-xs font-semibold"
                  :class="getMesaStatusBadge(mesa.status)"
                >
                  {{ mesa.status }}
                </span>
              </div>
              <div class="space-y-2 text-sm text-gray-600">
                <p>Capacidad: {{ mesa.capacidad }} personas</p>
                <p v-if="mesa.mesero">Mesero: {{ mesa.mesero }}</p>
                <p v-if="mesa.total" class="font-semibold text-orange-600">Total: S/ {{ mesa.total.toFixed(2) }}</p>
              </div>
            </div>
          </div>
        </div>

        <!-- Cocina View -->
        <div v-if="activeView === 'cocina'" class="space-y-6">
          <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
            <!-- Pendientes -->
            <div class="bg-white rounded-xl shadow-sm p-6">
              <h3 class="text-lg font-bold mb-4 text-yellow-600">Pendientes ({{ pedidosPendientes.length }})</h3>
              <div class="space-y-3">
                <div
                  v-for="pedido in pedidosPendientes"
                  :key="pedido.id"
                  class="border border-yellow-200 bg-yellow-50 rounded-lg p-4"
                >
                  <div class="flex justify-between items-start mb-2">
                    <span class="font-bold text-gray-800">Mesa {{ pedido.mesa }}</span>
                    <span class="text-xs text-gray-500">{{ pedido.tiempo }}</span>
                  </div>
                  <ul class="space-y-1 text-sm">
                    <li v-for="(item, idx) in pedido.items" :key="idx">
                      {{ item.cantidad }}x {{ item.nombre }}
                    </li>
                  </ul>
                  <button class="mt-3 w-full bg-yellow-500 hover:bg-yellow-600 text-white py-2 rounded-lg transition-colors">
                    Iniciar
                  </button>
                </div>
              </div>
            </div>

            <!-- En Preparación -->
            <div class="bg-white rounded-xl shadow-sm p-6">
              <h3 class="text-lg font-bold mb-4 text-blue-600">En Preparación ({{ pedidosEnPreparacion.length }})</h3>
              <div class="space-y-3">
                <div
                  v-for="pedido in pedidosEnPreparacion"
                  :key="pedido.id"
                  class="border border-blue-200 bg-blue-50 rounded-lg p-4"
                >
                  <div class="flex justify-between items-start mb-2">
                    <span class="font-bold text-gray-800">Mesa {{ pedido.mesa }}</span>
                    <span class="text-xs text-gray-500">{{ pedido.tiempo }}</span>
                  </div>
                  <ul class="space-y-1 text-sm">
                    <li v-for="(item, idx) in pedido.items" :key="idx">
                      {{ item.cantidad }}x {{ item.nombre }}
                    </li>
                  </ul>
                  <button class="mt-3 w-full bg-blue-500 hover:bg-blue-600 text-white py-2 rounded-lg transition-colors">
                    Completar
                  </button>
                </div>
              </div>
            </div>

            <!-- Listos -->
            <div class="bg-white rounded-xl shadow-sm p-6">
              <h3 class="text-lg font-bold mb-4 text-green-600">Listos ({{ pedidosListos.length }})</h3>
              <div class="space-y-3">
                <div
                  v-for="pedido in pedidosListos"
                  :key="pedido.id"
                  class="border border-green-200 bg-green-50 rounded-lg p-4"
                >
                  <div class="flex justify-between items-start mb-2">
                    <span class="font-bold text-gray-800">Mesa {{ pedido.mesa }}</span>
                    <span class="text-xs text-gray-500">{{ pedido.tiempo }}</span>
                  </div>
                  <ul class="space-y-1 text-sm">
                    <li v-for="(item, idx) in pedido.items" :key="idx">
                      {{ item.cantidad }}x {{ item.nombre }}
                    </li>
                  </ul>
                  <button class="mt-3 w-full bg-green-500 hover:bg-green-600 text-white py-2 rounded-lg transition-colors">
                    Entregado
                  </button>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- Pedidos View -->
        <div v-if="activeView === 'pedidos'" class="space-y-6">
          <div class="bg-white rounded-xl shadow-sm overflow-hidden">
            <table class="w-full">
              <thead class="bg-gray-50 border-b border-gray-200">
                <tr>
                  <th class="px-6 py-4 text-left text-sm font-semibold text-gray-700">#Pedido</th>
                  <th class="px-6 py-4 text-left text-sm font-semibold text-gray-700">Mesa</th>
                  <th class="px-6 py-4 text-left text-sm font-semibold text-gray-700">Items</th>
                  <th class="px-6 py-4 text-left text-sm font-semibold text-gray-700">Estado</th>
                  <th class="px-6 py-4 text-left text-sm font-semibold text-gray-700">Total</th>
                  <th class="px-6 py-4 text-left text-sm font-semibold text-gray-700">Hora</th>
                  <th class="px-6 py-4 text-left text-sm font-semibold text-gray-700">Acciones</th>
                </tr>
              </thead>
              <tbody class="divide-y divide-gray-200">
                <tr
                  v-for="pedido in todosPedidos"
                  :key="pedido.id"
                  class="hover:bg-gray-50 transition-colors"
                >
                  <td class="px-6 py-4 text-sm font-medium text-gray-900">#{{ pedido.id }}</td>
                  <td class="px-6 py-4 text-sm text-gray-700">Mesa {{ pedido.mesa }}</td>
                  <td class="px-6 py-4 text-sm text-gray-700">{{ pedido.items.length }} items</td>
                  <td class="px-6 py-4">
                    <span
                      class="px-3 py-1 rounded-full text-xs font-semibold"
                      :class="getPedidoStatusClass(pedido.estado)"
                    >
                      {{ pedido.estado }}
                    </span>
                  </td>
                  <td class="px-6 py-4 text-sm font-semibold text-gray-900">S/ {{ pedido.total.toFixed(2) }}</td>
                  <td class="px-6 py-4 text-sm text-gray-600">{{ pedido.hora }}</td>
                  <td class="px-6 py-4">
                    <button class="text-orange-600 hover:text-orange-700 text-sm font-medium">
                      Ver detalle
                    </button>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'

const TableIcon = {
  template: `
    <svg fill="none" stroke="currentColor" viewBox="0 0 24 24">
      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 10h18M3 14h18m-9-4v8m-7 0h14a2 2 0 002-2V8a2 2 0 00-2-2H5a2 2 0 00-2 2v8a2 2 0 002 2z" />
    </svg>
  `
}

const ChefIcon = {
  template: `
    <svg fill="none" stroke="currentColor" viewBox="0 0 24 24">
      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6V4m0 2a2 2 0 100 4m0-4a2 2 0 110 4m-6 8a2 2 0 100-4m0 4a2 2 0 110-4m0 4v2m0-6V4m6 6v10m6-2a2 2 0 100-4m0 4a2 2 0 110-4m0 4v2m0-6V4" />
    </svg>
  `
}

const OrderIcon = {
  template: `
    <svg fill="none" stroke="currentColor" viewBox="0 0 24 24">
      <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2m-3 7h3m-3 4h3m-6-4h.01M9 16h.01" />
    </svg>
  `
}

const sidebarExpanded = ref(true)
const activeView = ref('mesas')

const menuItems = [
  { id: 'mesas', label: 'Mesas', icon: TableIcon },
  { id: 'cocina', label: 'Cocina', icon: ChefIcon, badge: 5 },
  { id: 'pedidos', label: 'Pedidos', icon: OrderIcon },
]

const mesas = ref([
  { id: 1, numero: 1, capacidad: 4, status: 'Disponible' },
  { id: 2, numero: 2, capacidad: 2, status: 'Ocupada', mesero: 'Juan Pérez', total: 85.50 },
  { id: 3, numero: 3, capacidad: 6, status: 'Reservada', mesero: 'María López' },
  { id: 4, numero: 4, capacidad: 4, status: 'Ocupada', mesero: 'Carlos Ruiz', total: 120.00 },
  { id: 5, numero: 5, capacidad: 2, status: 'Disponible' },
  { id: 6, numero: 6, capacidad: 8, status: 'Disponible' },
  { id: 7, numero: 7, capacidad: 4, status: 'Ocupada', mesero: 'Ana Torres', total: 95.75 },
  { id: 8, numero: 8, capacidad: 2, status: 'Disponible' },
])

const pedidosPendientes = ref([
  {
    id: 1,
    mesa: 2,
    tiempo: '5 min',
    items: [
      { nombre: 'Lomo Saltado', cantidad: 2 },
      { nombre: 'Ceviche', cantidad: 1 }
    ]
  },
  {
    id: 2,
    mesa: 4,
    tiempo: '2 min',
    items: [
      { nombre: 'Arroz con Pollo', cantidad: 1 },
      { nombre: 'Ensalada César', cantidad: 2 }
    ]
  },
])

const pedidosEnPreparacion = ref([
  {
    id: 3,
    mesa: 7,
    tiempo: '8 min',
    items: [
      { nombre: 'Tallarín Saltado', cantidad: 1 },
      { nombre: 'Causa Limeña', cantidad: 2 }
    ]
  },
])

const pedidosListos = ref([
  {
    id: 4,
    mesa: 2,
    tiempo: '12 min',
    items: [
      { nombre: 'Ají de Gallina', cantidad: 2 }
    ]
  },
])

const todosPedidos = ref([
  { id: 1, mesa: 2, items: [{ nombre: 'Lomo Saltado', cantidad: 2 }], estado: 'Pendiente', total: 85.50, hora: '14:30' },
  { id: 2, mesa: 4, items: [{ nombre: 'Ceviche', cantidad: 1 }], estado: 'En Preparación', total: 120.00, hora: '14:45' },
  { id: 3, mesa: 7, items: [{ nombre: 'Arroz con Pollo', cantidad: 1 }], estado: 'Listo', total: 95.75, hora: '15:00' },
  { id: 4, mesa: 1, items: [{ nombre: 'Tallarín Saltado', cantidad: 2 }], estado: 'Entregado', total: 68.00, hora: '13:20' },
])

const currentViewTitle = computed(() => {
  const titles = {
    mesas: 'Gestión de Mesas',
    cocina: 'Monitor de Cocina',
    pedidos: 'Historial de Pedidos'
  }
  return titles[activeView.value] || ''
})

const currentViewSubtitle = computed(() => {
  const subtitles = {
    mesas: 'Administra el estado y asignación de mesas',
    cocina: 'Seguimiento en tiempo real de pedidos en cocina',
    pedidos: 'Registro completo de todos los pedidos del día'
  }
  return subtitles[activeView.value] || ''
})

const toggleSidebar = () => {
  sidebarExpanded.value = !sidebarExpanded.value
}

const selectMesa = (mesa) => {
  console.log('Mesa seleccionada:', mesa)
}

const getMesaStatusClass = (status) => {
  const classes = {
    'Disponible': 'border-green-200 hover:border-green-400',
    'Ocupada': 'border-orange-200 hover:border-orange-400',
    'Reservada': 'border-blue-200 hover:border-blue-400'
  }
  return classes[status] || ''
}

const getMesaStatusBadge = (status) => {
  const badges = {
    'Disponible': 'bg-green-100 text-green-700',
    'Ocupada': 'bg-orange-100 text-orange-700',
    'Reservada': 'bg-blue-100 text-blue-700'
  }
  return badges[status] || ''
}

const getPedidoStatusClass = (estado) => {
  const classes = {
    'Pendiente': 'bg-yellow-100 text-yellow-700',
    'En Preparación': 'bg-blue-100 text-blue-700',
    'Listo': 'bg-green-100 text-green-700',
    'Entregado': 'bg-gray-100 text-gray-700'
  }
  return classes[estado] || ''
}
</script>

<style scoped>
* {
  transition-property: background-color, border-color, color, fill, stroke;
  transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
  transition-duration: 150ms;
}
</style>
