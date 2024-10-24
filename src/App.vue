<template>
  <div id="app">
    <h1>Устройства</h1>
    <div class="input-container">
      <input v-model="newDevice" placeholder="Название устройства" />
      <button @click="addDevice">Добавить Устройство</button>
    </div>

    <div v-for="(device, deviceIndex) in devices" :key="deviceIndex" class="device">
      <div class="device-content">
        <div v-if="device.isEditing">
          <input v-model="device.name" placeholder="Название устройства" class="edit-input" />
          <button @click="saveDevice(deviceIndex)">Сохранить</button>
          <button @click="cancelEdit(deviceIndex)">Отменить</button>
        </div>
        <div v-else>
          <span class="device-name">{{ device.name }}</span>
          <button @click="editDevice(deviceIndex)">Редактировать</button>
          <button @click="deleteDevice(deviceIndex)">Удалить</button>
          <button @click="addNode(deviceIndex)">Добавить Узел</button>
        </div>
      </div>

      <div class="nodes">
        <div class="node" v-for="(node, nodeIndex) in device.nodes" :key="nodeIndex">
          <span class="node-name">{{ node.name }}</span>
          <button @click="editNode(deviceIndex, nodeIndex)">Редактировать</button>
          <button @click="deleteNode(deviceIndex, nodeIndex)">Удалить</button>
          
          <input v-if="node.isEditing" v-model="node.name" class="edit-input" />
          <button v-if="node.isEditing" @click="saveNode(deviceIndex, nodeIndex)">Сохранить</button>
          <button v-if="node.isEditing" @click="cancelNodeEdit(deviceIndex, nodeIndex)">Отменить</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const devices = ref([]);
const newDevice = ref('');

const addDevice = () => {
  if (newDevice.value.trim()) {
    const newDeviceObject = { 
      name: newDevice.value,
      nodes: [],
      isEditing: false 
    };
    devices.value.push(newDeviceObject);
    newDevice.value = '';
  }
};

const editDevice = (index) => {
  devices.value[index].isEditing = true;
};

const saveDevice = (index) => {
  devices.value[index].isEditing = false;
};

const cancelEdit = (index) => {
  devices.value[index].isEditing = false;
};

const deleteDevice = (index) => {
  devices.value.splice(index, 1);
};

const addNode = (deviceIndex) => {
  const nodeName = prompt("Введите название узла:");
  if (nodeName) {
    const newNode = { 
      name: nodeName, 
      isEditing: false 
    };
    devices.value[deviceIndex].nodes.push(newNode);
  }
};

const editNode = (deviceIndex, nodeIndex) => {
  devices.value[deviceIndex].nodes[nodeIndex].isEditing = true;
};

const saveNode = (deviceIndex, nodeIndex) => {
  devices.value[deviceIndex].nodes[nodeIndex].isEditing = false;
};

const cancelNodeEdit = (deviceIndex, nodeIndex) => {
  devices.value[deviceIndex].nodes[nodeIndex].isEditing = false;
};

const deleteNode = (deviceIndex, nodeIndex) => {
  devices.value[deviceIndex].nodes.splice(nodeIndex, 1);
};

</script>

<style>
body {
  font-family: Arial, sans-serif;
  background-color: #f9f9f9;
  color: #333;
  margin: 20px;
}

h1 {
  text-align: center;
  color: #007bff;
}

.input-container {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin-bottom: 20px;
}

input {
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px;
  width: 200px;
}

button {
  padding: 8px 12px;
  border: none;
  background-color: #007bff;
  color: white;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s;
  margin-left: 10px; /* Добавляем отступ */
}

button:hover {
  background-color: #0056b3;
}

.device {
  background-color: white;
  border: 1px solid #ddd;
  border-radius: 4px;
  padding: 10px;
  margin-bottom: 10px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.device-content {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.device-name {
  font-weight: bold;
}

.nodes {
  margin-top: 10px;
  padding-left: 20px;
  border-left: 2px solid #007bff;
}

.node {
  margin: 5px 0;
}

.node-name {
  margin-right: 10px;
}
</style>
