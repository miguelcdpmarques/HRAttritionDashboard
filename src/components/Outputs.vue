<template>
  <div class="output-section">
    <h2 class="output-title">Attrition Report</h2>
    <hr class="underline-output">
    <div class="columns parent-width">
      <div class="column is-half box box-prediction">
        <h3>Prediction</h3>
        <div class="columns large-margin">
          <div class="column">
            <small>Value:
              <span v-if="predictedClass == 1" class="tag is-danger prediction-tag"><b>Leave</b></span>
              <span v-else class="tag is-success prediction-tag"><b>Stay</b></span>
            </small>
          </div>
          <div class="column">
            <small>Probability: 
              <span 
                v-if="predictedProbability" 
                class="tag is-info prediction-tag">
                <b>{{ predictedProbability*100 }} %</b>
              </span>
            </small>
          </div>
        </div>
        <p class="model-description">
          This prediction is based on a predefined set of transformations on the most relevant features of the dataset. Then, a logistic regression model was applied to predict attrition probabilities.
        </p>
      </div>
      <div class="column is-half box box-prediction">
        <h3>Model Metrics</h3>
        <div class="columns">
          <div class="column">
            <small>AUC:</small> 
              <span 
                class="tag is-light prediction-tag">
                <b>{{ auc }}</b>
              </span>
          </div>
          <div class="column">
            <small>F1-Score: </small> 
             <span 
                class="tag is-light prediction-tag">
                <b>{{ f1score }}</b>
              </span>
          </div>
        </div>
        <b-table
          :data="data"
          :columns="columns"
          :striped="true">
       </b-table>
      </div>
    </div>
    <div class="columns">
      <div class="column box box-histogram">
        <h3>Variable Distribution</h3>
        <b-field>
          <b-select v-model="chartvariable" placeholder="Select a variable" size="is-small">
            <option value="Education">Education</option>
            <option value="MaritalStatus">MaritalStatus</option>
            <option value="NumCompaniesWorked">Nr Companies Worked</option>
            <option value="TotalWorkingYears">Total Working Years</option>
            <option value="BusinessTravel">Business Travel</option>
            <option value="DistanceFromHome">Distance From Home</option>
            <option value="YearsSinceLastPromotion">Years Since Last Promotion</option>
            <option value="YearsWithCurrManager">Years With Current Manager</option>
            <option value="YearsAtCompany">Years At Company</option>
            <option value="JobSatisfaction">Job Satisfaction</option>
            <option value="EnvironmentSatisfaction">Environment Satisfaction</option>
            <option value="OverTime">Overtime</option>
          </b-select>
        </b-field>
        <div class="box-chart">
          <barchart :chartData="chart.chartData" :chartOptions="chart.chartOptions"></barchart>
        </div>
      </div>
    </div>
  </div>
</template>
        
<script>
import hr_dataset from '../assets/hr.json'
import BarChart from './BarChart.vue'

export default {
  props: ['predictedClass', 'predictedProbability', 'inputData'],
  components: {
    'barchart': BarChart
  },
  data() {
    return {
      chartvariable: 'Education',
      data: [
        {'empty': 'Actual: NO', "Predicted: NO": "10%", "Predicted: YES": "20%"},
        {'empty': 'Actual: YES', "Predicted: NO": "30%", "Predicted: YES": "40%"}
      ],
      columns: [
        {field: 'empty', label: '', width: 130},
        {field: 'Predicted: NO', label: 'Predicted: NO', centered: true},
        {field: 'Predicted: YES', label: 'Predicted: YES', centered: true}
      ],
      auc: 0.69,
      f1score: 1.24,
    }
  },
  computed: {
    chart() {
      return {
        chartData: {
          labels: this.getLabels(hr_dataset[this.chartvariable]),
          datasets: [
            {
              label: this.chartvariable,
              backgroundColor: 'brown',
              data: this.aggregateCategoricalData(this.getLabels(hr_dataset[this.chartvariable]), Object.values(hr_dataset[this.chartvariable]))
            }
          ]
        },
        chartOptions: {
          maintainAspectRatio: false,
          responsive: true
        }
      }
    }
  },
  methods: {
    aggregateCategoricalData(labels, data) {
      let output = []
      labels.forEach(label => {
        let subset = Object.fromEntries(Object.entries(data).filter(([k, v]) => v === label))
        output.push(Object.keys(subset).length)
      })
      return output
    },
    getLabels(data) {
      let labels = Array.from(new Set(Object.values(data)))
      if(typeof labels[0] == 'number') {
        labels.sort(function sortNumber(a, b) {
          return a - b;
        })
      }
      return labels
    }
  }
}
</script>
            
<style lang='scss'>
.output-section {
  width: 95%;
  height: 95vh;
}
h2.output-title {
  color: brown;
}
hr.underline-output {
  margin: 0.25rem 0 0.75rem 0;
  border: 1px solid brown;
}
.parent-width {
  width: inherit; 
}
div.box {
  background-color: white;
  border: 1px solid lightgrey;
  &-prediction {
    margin: 1rem 1rem 1.5rem 1rem;
  }
  &-histogram {
    margin: .5rem 1rem;
  }
  &-chart {
    height: 350px;
    width: 100%;
  }
}
h3 {
  font-weight: 600;
  margin-bottom: .75rem;
}
p.model-description {
  margin-top: 1rem;
  font-size: .75rem;
  padding: .75rem;
}
.prediction-tag {
  margin-left: .5rem;
}
canvas#bar-chart {
  height: 350px !important;
}
.large-margin {
  margin: 2rem 0;
}
</style>