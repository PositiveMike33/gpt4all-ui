<template>
    <div class="container overflow-y-scroll flex flex-col no-scrollbar shadow-lg p-10 pt-2 bg-bg-light-tone dark:bg-bg-dark-tone">
      <form @submit.prevent="submitForm" class="">
        <Card title="Training configuration" :isHorizontal="true" :disableHoverAnimation="true" :disableFocus="true">
          <Card title="Model" class="" :isHorizontal="false">
            <!-- Model/Tokenizer -->
            <div class="mb-4">
              <label for="model_name" class="text-sm">Model Name:</label>
              <ClipBoardTextInput id="model_path" inputType="text"  :value="model_name" />
            </div>
            <div class="mb-4">
              <label for="tokenizer_name" class="text-sm">Tokenizer Name:</label>
              <ClipBoardTextInput id="model_path" inputType="text"  :value="tokenizer_name" />
            </div>
          </Card>
          <Card title="Data" :isHorizontal="false">
            <!-- Dataset -->
            <div class="mb-4">
              <label for="dataset_path" class="text-sm">Dataset:</label>
              <ClipBoardTextInput id="model_path" inputType="file"  :value="dataset_path" onchange="selectDataset()"/>
            </div>
          </Card>
          <Card title="Training" :isHorizontal="false">
            <!-- Train Dynamics -->
            <div class="mb-4">
              <label for="lr" class="text-sm">Learning Rate:</label>
              <ClipBoardTextInput id="model_path" inputType="integer"  :value="lr"/>
            </div>
            <div class="mb-4">
              <label for="num_epochs" class="text-sm">Number of Epochs:</label>
              <ClipBoardTextInput id="model_path" inputType="integer"  :value="num_epochs"/>
            </div>
            <div class="mb-4">
              <label for="max_length" class="text-sm">Max Length:</label>
              <ClipBoardTextInput id="model_path" inputType="integer"  :value="max_length"/>
            </div>
            <div class="mb-4">
              <label for="batch_size" class="text-sm">Batch Size:</label>
              <ClipBoardTextInput id="model_path" inputType="integer"  :value="batch_size"/>
            </div>
          </Card>
          <Card title="Output" :isHorizontal="false">
            <!-- Logging -->
            <div class="mb-4">
            <label for="output_dir" class="text-sm">Output Directory:</label>
            <ClipBoardTextInput id="model_path" inputType="text"  :value="output_dir" />
            </div>
          </Card>
        </Card>
        <Card  :disableHoverAnimation="true" :disableFocus="true">
          <button type="submit" class="bg-blue-500 text-white px-4 py-2 rounded">Train LLM</button>
          <!-- <ProgressBar v-if="loading" :progress="progressValue" /> -->
        </Card>
  
      </form>
    </div>
  </template>
  
  <script>
  import ClipBoardTextInput from "@/components/ClipBoardTextInput.vue";
  import Card from "@/components/Card.vue"
  //import ProgressBar from '@/components/ProgressBar.vue';
import axios from "axios";
  export default {
    components: {
      ClipBoardTextInput,
      Card,
      //ProgressBar
    },    
    data() {
        return {
            model_name: 'jondurbin/airoboros-7b-gpt4',
            tokenizer_name: 'jondurbin/airoboros-7b-gpt4',
            dataset_path: '',
            max_length: 1024,
            batch_size: 4,
            lr: 5.0e-5,
            num_epochs: 2,
            selectedFolder: '',
            selectedDataset: '',
        };
    },
    methods: {
      submitForm() {
        const formData = {
          model_name: this.model_name,
          tokenizer_name: this.tokenizer_name,
          dataset_file: this.selectedDataset,
          max_length: this.max_length,
          batch_size: this.batch_size,
          lr: this.lr,
          num_epochs: this.num_epochs,
          output_dir: this.selectedFolder,
        };
        axios.post("/start_training", formData).then((res)=>{

        });
      },
      openFolderSelector() {
      this.$refs.folder_selector.click();
      },
      selectOutputDirectory(event) {
        console.log("here")
        const folderPath = event.target.files[0]?.path;
        console.log(folderPath)
        if (folderPath) {
            this.selectedFolder = folderPath;
        }
      },
      selectDataset(event) {
        const files = event.target.files;
        if (files.length > 0) {
            this.selectedDataset = files[0];
        }
      },
    },
    watch: {
      model_name(newVal) {
        // Watch for changes to model_name and propagate them to the child component
        console.log("watching model_name", newVal)
        this.$refs.clipboardInput.inputValue = newVal;
      },
    },    
  };
  </script>
  