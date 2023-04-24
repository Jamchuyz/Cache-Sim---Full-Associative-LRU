<template>
  <div class="body">
    <head>
      <link
        href="https://fonts.googleapis.com/css?family=Inter:400,500,700"
        rel="stylesheet"
      />
    </head>
    <nav-bar />
    <div class="parent-container">
      <div class="container-fluid">
        <b-form>
          <div class="row mt-4 mb-2">
            <h2 class="pl-10">Cache Configuration</h2>
          </div>
          <div class="row">
            <div class="form-config-wrapper p-3">
              <div>
                <b-row>
                  <b-col cols="6">
                    <b-form-group
                      id="fieldset-1"
                      label="Memory Block Size"
                      label-for="block-size"
                      :invalid-feedback="blockSizeInvalidFeedback"
                      :state="blockSizeState"
                    >
                      <b-form-input
                        id="block-size"
                        v-model.number="blockSize"
                        :state="blockSizeState"
                        type="number"
                        min="1"
                      ></b-form-input>
                    </b-form-group>
                  </b-col>
                  <b-col cols="6">
                    <b-form-group
                      id="fieldset-2"
                      label="Main Memory Size"
                      label-for="main-memory-size"
                      :invalid-feedback="mainMemoryInvalidFeedback"
                      :state="mainMemoryState"
                    >
                      <b-form-input
                        id="main-memory-size"
                        v-model.number="mainMemorySize"
                        :state="mainMemoryState"
                        type="number"
                        min="1"
                      ></b-form-input>
                      <b-form-select
                        v-model="mainMemoryType"
                        :options="['In blocks', 'In words']"
                        value="In blocks"
                      ></b-form-select>
                    </b-form-group>
                  </b-col>
                </b-row>

                <b-row>
                  <b-col cols="6">
                    <b-form-group
                      id="fieldset-3"
                      label="Cache Memory Size"
                      label-for="cache-memory-size"
                      :invalid-feedback="cacheMemoryInvalidFeedback"
                      :state="cacheMemoryState"
                    >
                      <b-form-input
                        id="cache-memory-size"
                        v-model.number="cacheMemorySize"
                        :state="cacheMemoryState"
                        type="number"
                        min="1"
                      ></b-form-input>
                      <b-form-select
                        v-model="cacheMemoryType"
                        :options="['In blocks', 'In words']"
                        value="In blocks"
                      ></b-form-select>
                    </b-form-group>
                  </b-col>
                </b-row>

                <b-row>
                  <b-col cols="6">
                    <b-form-group
                      id="fieldset-4"
                      label="Cache Access Time (in ns)"
                      label-for="cache-access-time"
                      :invalid-feedback="cacheAccessInvalidFeedback"
                      :state="cacheAccessState"
                    >
                      <b-form-input
                        id="cache-access-time"
                        v-model.number="cacheAccessTime"
                        :state="cacheAccessState"
                        type="number"
                        min="1"
                      ></b-form-input>
                    </b-form-group>
                  </b-col>
                </b-row>

                <b-row>
                  <b-col cols="6">
                    <b-form-group
                      id="fieldset-5"
                      label="Memory Access Time (in ns)"
                      label-for="memory-access-time"
                      :invalid-feedback="memoryAccessInvalidFeedback"
                      :state="memoryAccessState"
                    >
                      <b-form-input
                        id="memory-access-time"
                        v-model.number="memoryAccessTime"
                        :state="memoryAccessState"
                        type="number"
                        min="1"
                      ></b-form-input>
                    </b-form-group>
                  </b-col>
                </b-row>
              </div>
            </div>
            <div class="col-md-8"></div>
          </div>

          <div class="row mt-3 mb-2">
            <h2 class="pl-3">Memory Access Sequence</h2>
          </div>
          <div class="row">
            <div class="form-config-wrapper p-3">
              <div>

                <b-form-group id="fieldset-1">
                  <b-form-group
                    id="num-inputs-group"
                    label="Number of entries in access sequence"
                    label-for="num-inputs"
                    :invalid-feedback="memAccessSeqInvalidFeedback"
                    :state="numInputsState"
                  >
                    <b-form-input
                      v-if="memAccessSeqType == 'In range'"
                      id="start-input"
                      v-model="startInput"
                      type="number"
                      min="0"
                      value="0">
                    </b-form-input>
                    <b-form-input
                      id="num-inputs"
                      v-model="numInputs"
                      type="number"
                      min="1"
                      :state="numInputsState"
                      @input="createInputs"
                    ></b-form-input>
                  </b-form-group>
                  <b-form-select
                        v-model="memAccessSeqType"
                        :options="['In blocks', 'In addresses', 'In range']"
                        value="In blocks"
                      ></b-form-select>
                  <b-form-group
                    v-if="memAccessSeqType !== 'In range'"
                    id="mem-access-sequence-group"
                    label="Enter the blocks or addresses to be accessed"
                    label-for="mem-access-sequence-inputs"
                    :invalid-feedback="memAccessSeqInvalidFeedback"
                    :state="memAccessSeqState"
                  >
                    <div class="row">
                      <div
                        v-for="(input, index) in inputs"
                        :key="index"
                        class="col-md-3 mb-3"
                      >
                        <b-form-input
                          v-model="memAccessSeq[index]"
                          :placeholder="'Entry ' + (index + 1)"
                          :state="memAccessSeqState"
                        ></b-form-input>
                      </div>
                    </div>
                  </b-form-group>
                  <b-form-group
                    v-else
                    id="num-runs-group"
                    label="Enter number of runs the sequence is accessed"
                    label-for="num-runs">
                    <b-form-input
                      id="num-runs"
                      v-model="numRuns"
                      type="number"
                      min="1"
                      value="1">
                    </b-form-input>
                  </b-form-group>
                </b-form-group>
              </div>
            </div>
            <div class="submit-button-wrapper">
              <b-button
                variant="primary"
                size="lg"
                block
                :disabled="!allInputsValid"
                :class="{ 'btn-secondary': !allInputsValid }"
                @click="simulateCacheAccess"
              >
                Simulate Cache Access
              </b-button>
            </div>
          </div>
        </b-form>
      </div>
      <div class="container-xl">
        <b-form>
          <div class="row mt-4 mb-2">
            <h1 class="pl-3">Cache Simulation Output</h1>
          </div>
          <div>
            <div class="row">
              <div class="output-wrapper p-3">
                <div>
                  <b-row>
                    <h2>Cache Snapshot</h2>
                  </b-row>
                  <b-row>
                    <div v-if="hasSubmitted">
                      <table class="table table-dark">
                        <thead>
                          <tr>
                            <th scope="col">Block/Word</th>
                            <th scope="col">Age</th>
                            <th scope="col">Content</th>
                          </tr>
                        </thead>
                        <tbody>
                          <tr v-for="(n, i) in cacheMemorySize" :key="i">
                            <th scope="row">{{ i }}</th>
                            <td>{{ (cacheSimBlocks[i] !== -1) ? cacheSimAge[i] : 'E' }}</td>
                            <td>
                              {{ (cacheSimBlocks[i] !== -1) ? cacheSimBlocks[i] : 'E' }}
                            </td>
                          </tr>
                        </tbody>
                      </table>
                    </div>
                    <div v-else>
                      <b-row>
                        <h4>
                          No output yet. Click 'Simulate Cache Access' to begin
                          simulation.
                        </h4>
                      </b-row>
                    </div>
                  </b-row>
                </div>

                <b-row>
                  <h2>Cache Simulation Statistics</h2>
                  <table class="table">
                    <thead>
                      <tr>
                        <th scope="col">Property</th>
                        <th scope="col">Value</th>
                      </tr>
                    </thead>
                    <tbody>
                      <tr>
                        <th scope="row">Number of Cache Hits</th>
                        <td>{{ cacheHits }}</td>
                      </tr>
                      <tr>
                        <th scope="row">Number of Cache Miss</th>
                        <td>{{ cacheMiss }}</td>
                      </tr>
                      <tr>
                        <th scope="row">Miss Penalty</th>
                        <td>{{ missPenalty }}</td>
                      </tr>
                      <tr>
                        <th scope="row">Avg. Memory Access Time</th>
                        <td>{{ avgMemoryAccessTime }}</td>
                      </tr>
                      <tr>
                        <th scope="row">Total Memory Access Time</th>
                        <td>{{ totalMemoryAccessTime }}</td>
                      </tr>
                    </tbody>
                  </table>
                </b-row>
              </div>
            </div>
            <div class="txt-button-wrapper">
              <b-button
                variant="primary"
                size="lg"
                block
                :disabled="!outputsGenerated"
                :class="{ 'btn-secondary': !outputsGenerated }"
                @click="downloadTxtFile"
              >
                Download Text File
              </b-button>
            </div>
          </div>
        </b-form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      blockSize: null,
      mainMemorySize: null,
      cacheMemorySize: null,
      mainMemoryUnit: 'blocks',
      cacheMemoryUnit: 'blocks',
      cacheAccessTime: null,
      memoryAccessTime: null,
      startInput: null,
      numInputs: null,
      numRuns: null,
      memAccessSeq: [],
      hasSubmitted: false,

      cacheHits: 0,
      cacheMiss: 0,
      missPenalty: -1,
      avgMemoryAccessTime: -1,
      totalMemoryAccessTime: -1,
      cacheSimAge: [],
      cacheSimBlocks: [],
      outputsGenerated: false,
    }
  },
  computed: {
    blockSizeState() {
      return this.checkBlockSizeIsNumber()
    },
    blockSizeInvalidFeedback() {
      if (this.blockSize === null) {
        return ''
      } else if (isNaN(this.blockSize) || this.blockSize <= 0) {
        return 'Please enter a valid positive integer.'
      }
      return ''
    },
    mainMemoryState() {
      return this.checkMainMemoryInputIsNumber()
    },
    mainMemoryInvalidFeedback() {
      if (this.mainMemorySize === null) {
        return ''
      } else if (isNaN(this.mainMemorySize) || this.mainMemorySize <= 0) {
        return 'Please enter a valid positive integer.'
      }
      return ''
    },
    cacheMemoryState() {
      return this.checkCacheMemoryInputIsNumber()
    },
    cacheMemoryInvalidFeedback() {
      if (this.cacheMemorySize === null) {
        return ''
      } else if (isNaN(this.cacheMemorySize) || this.cacheMemorySize <= 0) {
        return 'Please enter a valid positive integer.'
      }
      return ''
    },
    numInputsState() {
      return this.numInputs !== null && this.numInputs > 0
    },
    numInputsInvalidFeedback() {
      if (this.numInputs === null) {
        return ''
      } else if (this.numInputs <= 0 || this.numInputs > 30) {
        return 'Please enter a number between 1 and 30.'
      }
      return ''
    },
    memAccessSeqRange(){
      return this.checkMemAccessSeqRange()
    },
    memAccessSeqState() {
      return this.checkMemAccessSeqIsArray() || this.checkMemAccessSeqRange()
    },
    memAccessSeqInvalidFeedback() {
      if (this.memAccessSeq.length === 0) {
        return ''
      } else if (
        !Array.isArray(this.memAccessSeq) ||
        this.memAccessSeq.length !== this.numInputs
      ) {
        return ''
      }
      return ''
    },
    inputs() {
      if (this.memAccessSeqRange)
        return null
      else
        return Array.from({ length: ((this.startInput === null) ? this.numInputs :
          (this.numInputs - this.startInput)) })

    },
    allInputsValid() {
      return (
        this.blockSizeState &&
        this.mainMemoryState &&
        this.cacheMemoryState &&
        this.numInputsState &&
        (this.memAccessSeqState || this.memAccessSeqRange)
      )
    },
  },
  methods: {
    checkBlockSizeIsNumber() {
      if (
        this.blockSize === null ||
        isNaN(this.blockSize) ||
        this.blockSize <= 0
      ) {
        return false
      }
      return true
    },
    checkMainMemoryInputIsNumber() {
      if (
        this.mainMemorySize === null ||
        isNaN(this.mainMemorySize) ||
        this.mainMemorySize <= 0
      ) {
        return false
      }
      return true
    },
    checkCacheMemoryInputIsNumber() {
      if (
        this.cacheMemorySize === null ||
        isNaN(this.cacheMemorySize) ||
        this.cacheMemorySize <= 0
      ) {
        return false
      }
      return true
    },
    checkMemAccessSeqIsArray() {
      if (this.memAccessSeq.length === 0) {
        return false
      }
      for (let i = 0; i < this.memAccessSeq.length; i++) {
        if (typeof this.memAccessSeq[i] !== 'string') {
          return false
        }
      }
      return true
    },
    checkMemAccessSeqRange(){
      return this.memAccessSeqType === 'In range'
    },
    createInputs() {
      this.memAccessSeq = Array.from({ length: ((this.startInput === null) ? this.numInputs :
          (this.numInputs - this.startInput)) })
    },
    simulateCacheAccess() {
      this.hasSubmitted = true
      const blockSize = this.blockSize
      let mainMemorySize = this.mainMemorySize
      const mainMemoryType = this.mainMemoryType
      let cacheMemorySize = this.cacheMemorySize
      const cacheMemoryType = this.cacheMemoryType
      const cacheAccessTime = this.cacheAccessTime
      const memoryAccessTime = this.memoryAccessTime
      const startInput = this.startInput
      let numInputs = this.numInputs
      const memAccessSeq = this.memAccessSeq
      const numRuns = this.numRuns
      const cacheSimAge = this.cacheSimAge
      const cacheSimBlocks = this.cacheSimBlocks

      let hitFlag = false
      let tempMaxIndex = 0

      if (mainMemoryType === 'In words') {
        mainMemorySize = mainMemorySize / blockSize
      }

      if (cacheMemoryType === 'In words') {
        cacheMemorySize = cacheMemorySize / blockSize
      }

      for (let i = 0; i < cacheMemorySize; i++) {
        cacheSimAge.push(0)
        cacheSimBlocks.push(-1)
      }

      if (this.memAccessSeqType === 'In range') {
        for (let i = startInput; i < numInputs; i++){
          memAccessSeq[i - startInput] = i
        }
        numInputs -= startInput
      }
      else if(this.memAccessSeqType !== 'In addresses'){
          for (let i = 0; i < memAccessSeq.length; i++) {
          memAccessSeq[i] = parseInt(memAccessSeq[i])
        }
      }

      // this.cacheHits = 0
      // this.cacheMiss = 0

      for (let g = 0; g < numRuns; g++) {
        for (let h = 0; h < numInputs; h++) {
          const data = memAccessSeq[h]

          hitFlag = false

          for (let i = 0; i < cacheMemorySize; i++) {
            if (cacheSimBlocks[i] === data) {
              hitFlag = true
              this.cacheHits++

              for (let j = 0; j < cacheMemorySize; j++) {
                if (
                  i !== j &&
                  cacheSimAge[j] < cacheSimAge[i] &&
                  cacheSimBlocks[j] !== -1
                ) {
                  cacheSimAge[j]++
                }
              }

              cacheSimAge[i] = 0
              break
            }
          }

          if (hitFlag === true) {
            return
          }

          let emptyBlockFlag = false

          for (let i = 0; i < cacheMemorySize; i++) {
            if (cacheSimBlocks[i] !== -1) {
              cacheSimAge[i]++
            } else {
              emptyBlockFlag = true
            }
          }

          this.cacheMiss++
          if (emptyBlockFlag === true)
            tempMaxIndex = cacheSimBlocks.indexOf(-1)
          else
            tempMaxIndex = cacheSimAge.indexOf(Math.max(...cacheSimAge))
          cacheSimAge[tempMaxIndex] = 0
          cacheSimBlocks[tempMaxIndex] = data
        }

      }

      // blockSize: null,
      // mainMemorySize: null,
      // cacheMemorySize: null,
      // mainMemoryUnit: 'blocks',
      // cacheMemoryUnit: 'blocks',
      // cacheAccessTime: null,
      // memoryAccessTime: null,
      // numInputs: null,
      // memAccessSeq: [],

      // cacheHits: -1,
      // cacheMiss: -1,
      // missPenalty: -1,
      // avgMemoryAccessTime: -1,
      // totalMemoryAccessTime: -1,
      // cacheSimAge: [],
      // cacheSimBlocks: [],
      // outputsGenerated: false

      console.log('Block Size: ', blockSize)
      console.log('Main Memory Size: ', mainMemorySize)
      console.log('Main Memory Type: ', mainMemoryType)
      console.log('Cache Memory Size: ', cacheMemorySize)
      console.log('Cache Memory Type: ', cacheMemoryType)
      console.log('Cache Access Time: ', cacheAccessTime)
      console.log('Memory Access Time: ', memoryAccessTime)
      console.log('Number of sequence inputs: ', numInputs)
      console.log('Memory Access Sequence: ', memAccessSeq)
      console.log(cacheSimAge)
      console.log(cacheSimBlocks)

      // console.log('Hit rate: ' + this.cacheHits / (this.cacheHits + this.cacheMiss))
      // console.log('Miss rate: ' + this.cacheMiss / (this.cacheHits + this.cacheMiss))

      this.cacheSimAge = cacheSimAge;
      this.cacheSimBlocks = cacheSimBlocks;

      this.cacheMemorySize = cacheMemorySize

      this.missPenalty =
        cacheAccessTime + blockSize * memoryAccessTime + cacheAccessTime

      this.avgMemoryAccessTime =
        (this.cacheHits / (this.cacheHits + this.cacheMiss)) * cacheAccessTime +
        (this.cacheMiss / (this.cacheHits + this.cacheMiss)) * this.missPenalty

      this.totalMemoryAccessTime =
        this.cacheHits * blockSize * cacheAccessTime +
        this.cacheMiss * blockSize * (memoryAccessTime + cacheAccessTime) +
        this.cacheMiss * cacheAccessTime

      // this.cacheHits = cacheAccessTime + memoryAccessTime
      this.outputsGenerated = true
    },
    downloadTxtFile() {
      const cacheHits = this.cacheHits
      const cacheMiss = this.cacheMiss
      const missPenalty = this.missPenalty
      const rows = []

      for (let i = 0; i < this.cacheMemorySize; i++) {
        let bwo = i.toString()
        let bw = bwo.length
        bw = 15 - bw
        for (let j = 0; j < bw; j++)
          if (j % 2 === 0)
            bwo = bwo + ' '
          else
            bwo = ' ' + bwo

        let ao = (this.cacheSimBlocks[i] !== -1) ? this.cacheSimAge[i].toString() : 'E'
        let a = ao.length
        a = 21 - a
        for (let j = 0; j < a; j++)
          if (j % 2 === 0)
            ao = ao + ' '
          else
            ao = ' ' + ao

        let dou = (this.cacheSimBlocks[i] !== -1) ? this.cacheSimBlocks[i].toString() : 'E'
        let d = dou.length
        d = 15 - d
        for (let j = 0; j < d; j++)
          if (j % 2 === 0)
            dou = dou + ' '
          else
            dou = ' ' + dou


        const row = [bwo, ao, dou]
        rows.push(row.join('|'))
      }

      const text = [
        `Number of Cache Hits: ${cacheHits}`,
        `Number of Cache Miss: ${cacheMiss}`,
        `Miss Penalty (nanoseconds): ${missPenalty}`,
        `Average Memory Access Time (nanoseconds): ${this.avgMemoryAccessTime}`,
        `Total Memory Access Time (nanoseconds): ${this.totalMemoryAccessTime}`,
        'Final Snapshot of Cache Memory:\n',
        '   Block/Word  |         Age         |      Data     ',
        '-----------------------------------------------------',
        ...rows,
      ].join('\n')

      const element = document.createElement('a')
      const file = new Blob([text], { type: 'text/plain' })
      element.href = URL.createObjectURL(file)
      element.download = 'cache-fa-lru-output.txt'
      document.body.appendChild(element)
      element.click()
    },
  },
}
</script>

<style>
body {
  font-family: 'Inter', sans-serif;
  background-color: #f0eded;
}

h2 {
  margin-left: 2%;
}

h4 {
  color: #0080e9;
  margin-left: 5%;
}

table {
  margin-left: 5%;
  margin-right: 5%;
}
.parent-container {
  display: flex;
  width: 100%;
}
.pt-2 {
  padding-top: 10px;
}
.form-config-wrapper {
  margin-left: 5%;
  border-radius: 30px;
  width: 70%;
  background-color: white;
  box-shadow: 0.1px 0.1px 12px 0.1px gray;
}

.output-wrapper {
  margin-left: 2%;
  border-radius: 30px;
  width: 90%;
  background-color: white;
  box-shadow: 0.1px 0.1px 12px 0.1px gray;
}
.submit-button-wrapper {
  display: flex;
  justify-content: center;
  width: 70%;
  margin-top: 30px;
  margin-bottom: 50px;
  margin-left: 5%;
}
.txt-button-wrapper {
  display: flex;
  justify-content: center;
  width: 90%;
  margin-top: 30px;
  margin-bottom: 50px;
  margin-left: 2%;
}

.submit-button {
  background-color: gray;
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.submit-button:hover {
  background-color: #2196f3;
  transform: translateY(5px);
}
</style>
