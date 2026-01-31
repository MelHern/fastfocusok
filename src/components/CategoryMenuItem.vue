<template>
  <div 
    class="dropdown-category"
    @mouseenter="handleMouseEnter"
    @mouseleave="handleMouseLeave"
  >
    <div class="dropdown-item-container">
      <router-link 
        :to="`/category/${category.id}`"
        class="dropdown-item"
        :class="`dropdown-item-level-${level}`"
        @click.native="emit('close-dropdown')"
      >
        <span class="category-name">{{ category.name }}</span>
      </router-link>
      <span 
        v-if="hasChildren" 
        class="dropdown-arrow-right"
        :class="{ 'expanded': isExpanded }"
        @click.stop="toggleExpanded"
      >
        {{ isExpanded ? '▼' : '▶' }}
      </span>
    </div>
    
    <!-- SUBMENÚ RECURSIVO - En móvil se expande verticalmente -->
    <div 
      v-show="hasChildren && (isHovered || isExpanded)"
      class="dropdown-submenu"
      :class="`dropdown-submenu-level-${level + 1}`"
      @mouseenter="handleMouseEnter"
      @mouseleave="handleMouseLeave"
    >
      <CategoryMenuItem 
        v-for="child in category.children" 
        :key="child.id"
        :category="child"
        :level="level + 1"
        @close-dropdown="$emit('close-dropdown')"
      />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'

interface Category {
  id: string
  name: string
  children?: Category[]
}

interface Props {
  category: Category
  level: number
}

interface Emits {
  (e: 'close-dropdown'): void
}

const props = defineProps<Props>()
const emit = defineEmits<Emits>()

// Debug para verificar los datos
// console.log('CategoryMenuItem props:', props.category)

const isHovered = ref(false)
const isExpanded = ref(false)

const hasChildren = computed(() => {
  return props.category.children && props.category.children.length > 0
})

const toggleExpanded = () => {
  if (hasChildren.value) {
    isExpanded.value = !isExpanded.value
  }
}

const handleMouseEnter = () => {
  isHovered.value = true
}

const handleMouseLeave = () => {
  setTimeout(() => {
    isHovered.value = false
  }, 200) // Aumentado para mejor UX
}


</script>

<style scoped>
.dropdown-category {
  position: relative !important;
  border-bottom: 1px solid rgba(0, 0, 0, 0.05);
  transition: all 0.2s ease;
}

.dropdown-category:last-child {
  border-bottom: none;
}

.dropdown-category:hover {
  background: rgba(59, 130, 246, 0.03);
}

.dropdown-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 0.875rem 1.25rem;
  color: var(--gray-700);
  text-decoration: none;
  font-size: 0.875rem;
  transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
  border-left: 3px solid transparent;
  white-space: nowrap;
  width: 100%;
  box-sizing: border-box;
  position: relative;
  border-radius: 8px;
  margin: 0.125rem 0.5rem;
}

/* Estilos por nivel dinámicos */
.dropdown-item-level-0 {
  font-weight: 600;
  color: var(--gray-800);
  background: linear-gradient(135deg, rgba(59, 130, 246, 0.05) 0%, rgba(59, 130, 246, 0.02) 100%);
}

.dropdown-item-level-0::before {
  content: '📁';
  margin-right: 0.5rem;
  font-size: 0.875rem;
}

.dropdown-item-level-1 {
  font-weight: 500;
  color: var(--gray-700);
  padding-left: 2rem;
  position: relative;
  background: transparent;
}

.dropdown-item-level-1::before {
  content: '→';
  position: absolute;
  left: 0.75rem;
  color: var(--gray-400);
  font-size: 0.75rem;
  transition: all 0.2s ease;
}

.dropdown-item-level-2 {
  font-weight: 500;
  color: var(--gray-600);
  padding-left: 2.75rem;
  position: relative;
  background: transparent;
}

.dropdown-item-level-2::before {
  content: '→';
  position: absolute;
  left: 1.5rem;
  color: var(--gray-400);
  font-size: 0.7rem;
  transition: all 0.2s ease;
}

.dropdown-item-level-3 {
  font-weight: 500;
  color: var(--gray-600);
  padding-left: 3.5rem;
  position: relative;
  background: transparent;
}

.dropdown-item-level-3::before {
  content: '→';
  position: absolute;
  left: 2.25rem;
  color: var(--gray-400);
  font-size: 0.65rem;
  transition: all 0.2s ease;
}

.dropdown-item-level-4 {
  font-weight: 500;
  color: var(--gray-600);
  padding-left: 4.25rem;
  position: relative;
  background: transparent;
}

.dropdown-item-level-4::before {
  content: '→';
  position: absolute;
  left: 3rem;
  color: var(--gray-400);
  font-size: 0.65rem;
  transition: all 0.2s ease;
}

.dropdown-item-level-5 {
  font-weight: 500;
  color: var(--gray-600);
  padding-left: 5rem;
  position: relative;
  background: transparent;
}

.dropdown-item-level-5::before {
  content: '→';
  position: absolute;
  left: 3.75rem;
  color: var(--gray-400);
  font-size: 0.65rem;
  transition: all 0.2s ease;
}

/* Hover effects */
.dropdown-category:hover .dropdown-item {
  background: linear-gradient(135deg, rgba(59, 130, 246, 0.12) 0%, rgba(59, 130, 246, 0.08) 100%);
  color: var(--primary-blue);
  border-left-color: var(--primary-blue);
  transform: translateX(6px);
  box-shadow: 0 4px 12px rgba(59, 130, 246, 0.15);
}

.dropdown-category:hover .dropdown-item::before {
  color: var(--primary-blue);
  transform: translateX(2px);
}

.dropdown-item.router-link-active {
  background: linear-gradient(135deg, rgba(59, 130, 246, 0.15) 0%, rgba(59, 130, 246, 0.1) 100%);
  color: var(--primary-blue);
  border-left-color: var(--primary-blue);
  font-weight: 600;
  box-shadow: 0 2px 8px rgba(59, 130, 246, 0.2);
}

.dropdown-item-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
}

.category-name {
  flex: 1;
  transition: all 0.2s ease;
}

.dropdown-category:hover .category-name {
  font-weight: 600;
}

.dropdown-arrow-right {
  font-size: 0.7rem;
  color: var(--gray-400);
  transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
  margin-left: 0.5rem;
  cursor: pointer;
  padding: 0.35rem 0.5rem;
  border-radius: 6px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-width: 24px;
  height: 24px;
  background: rgba(0, 0, 0, 0.03);
}

.dropdown-arrow-right.expanded {
  transform: rotate(90deg);
  background: rgba(59, 130, 246, 0.15);
  color: var(--primary-blue);
}

.dropdown-arrow-right:hover {
  background: rgba(59, 130, 246, 0.2);
  color: var(--primary-blue);
  transform: scale(1.1);
}

.dropdown-category:hover .dropdown-arrow-right {
  color: var(--primary-blue);
  background: rgba(59, 130, 246, 0.1);
}

/* Submenús por nivel */
.dropdown-submenu {
  position: absolute;
  top: 0;
  left: 100%;
  background: linear-gradient(135deg, rgba(255, 255, 255, 0.98) 0%, rgba(255, 255, 255, 0.95) 100%);
  backdrop-filter: blur(20px);
  -webkit-backdrop-filter: blur(20px);
  border: 1px solid rgba(59, 130, 246, 0.2);
  border-radius: 16px;
  box-shadow: 0 20px 60px rgba(0, 0, 0, 0.15), 
              0 8px 24px rgba(59, 130, 246, 0.1),
              inset 0 1px 0 rgba(255, 255, 255, 0.8);
  min-width: 240px;
  max-width: 320px;
  z-index: 1001;
  overflow: visible;
  padding: 0.5rem 0;
  margin-left: 8px;
  animation: submenuSlideIn 0.25s cubic-bezier(0.4, 0, 0.2, 1);
}


.dropdown-submenu-level-1 {
  z-index: 1001;
}

.dropdown-submenu-level-2 {
  z-index: 1002;
}

.dropdown-submenu-level-3 {
  z-index: 1003;
}

.dropdown-submenu-level-4 {
  z-index: 1004;
}

.dropdown-submenu-level-5 {
  z-index: 1005;
}


.dropdown-submenu-level-6 {
  z-index: 1006;
}

.dropdown-submenu-level-7 {
  z-index: 1007;
}

.dropdown-submenu-level-8 {
  z-index: 1008;
}

.dropdown-submenu-level-9 {
  z-index: 1009;
}

.dropdown-submenu-level-10 {
  z-index: 1010;
}

@keyframes submenuSlideIn {
  from {
    opacity: 0;
    transform: translateX(-10px) scale(0.95);
  }
  to {
    opacity: 1;
    transform: translateX(0) scale(1);
  }
}

/* Responsive */
@media (max-width: 768px) {
  .dropdown-submenu {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    min-width: 200px;
    max-width: 80vw;
  }
  
  .dropdown-item-level-1,
  .dropdown-item-level-2,
  .dropdown-item-level-3,
  .dropdown-item-level-4,
  .dropdown-item-level-5 {
    padding-left: 1rem;
  }
  
  .dropdown-item-level-1::before,
  .dropdown-item-level-2::before,
  .dropdown-item-level-3::before,
  .dropdown-item-level-4::before,
  .dropdown-item-level-5::before {
    display: none;
  }
  
  .dropdown-submenu {
    margin-left: 0;
  }
}

/* Regla específica para móvil - debe ir al final para mayor prioridad */
@media (max-width: 768px) {
  .dropdown-submenu {
    position: static !important;
    top: auto !important;
    left: auto !important;
    background: transparent !important;
    border: none !important;
    border-radius: 0 !important;
    box-shadow: none !important;
    min-width: auto !important;
    width: auto !important;
    z-index: auto !important;
    padding: 0 !important;
    margin-left: 1rem !important;
    margin-top: 0.25rem !important;
    transform: none !important;
  }
}
</style>
