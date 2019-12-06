<template>
  <div class="input-section">
    <div class="input-fields">
      <h2>Person</h2>
      <hr class="underline">
      <div class="columns">
        <div class="column is-half">
          <p class="label">Education:</p>
          <b-field>
            <b-select v-model="education" placeholder="Select a level" size="is-small" expanded>
              <option value="1">Below College</option>
              <option value="2">College</option>
              <option value="3">Bachelor</option>
              <option value="4">Master</option>
              <option value="5">Doctor</option>
            </b-select>
          </b-field>
          <p class="label">Nr Companies Worked: <span class="label-value">{{ nrCompaniesWorked }}</span></p>
          <b-field>
            <b-slider size="is-small" v-model="nrCompaniesWorked" :min="0" :max="10">
            </b-slider>
          </b-field>
        </div>
        <div class="column is-half">
          <p class="label">Marital Status:</p>
          <b-field>
            <b-select v-model="maritalStatus" placeholder="Choose an option" size="is-small" expanded>
              <option value="Single">Single</option>
              <option value="Married">Married</option>
              <option value="Divorced">Divorced</option>
            </b-select>
          </b-field>
          <p class="label">Total Working Years: <span class="label-value">{{ totalWorkingYears }}</span></p>
          <b-field>
            <b-slider size="is-small" v-model="totalWorkingYears" :min="0" :max="40">
            </b-slider>
          </b-field>
        </div>
      </div>
      <h2>Job</h2>
      <hr class="underline">
      <div class="columns">
        <div class="column is-half">
          <p class="label">Business Travel:</p>
          <b-field>
            <b-select v-model="businessTravel" placeholder="Choose an option" size="is-small" expanded>
              <option value="Non-Travel">Non-Travel</option>
              <option value="Travel_Frequently">Travel Frequently</option>
              <option value="Travel_Rarely">Travel Rarely</option>
            </b-select>
          </b-field>
          <p class="label">Years Since Last Promotion: <span class="label-value">{{ yearsSincePromotion }}</span></p>
          <b-field>
            <b-slider size="is-small" v-model="yearsSincePromotion" :min="0" :max="15">
            </b-slider>
          </b-field>
          <p class="label">Years at Company: <span class="label-value">{{ yearsCompany }}</span></p>
          <b-field>
            <b-slider size="is-small" v-model="yearsCompany" :min="0" :max="40">
            </b-slider>
          </b-field>
          <p class="label">Job Satisfaction:</p>
          <b-field>
            <b-select v-model="jobSatisfaction" placeholder="Choose an option" size="is-small" expanded>
              <option value="1">Low</option>
              <option value="2">Medium</option>
              <option value="3">High</option>
              <option value="4">Very High</option>
            </b-select>
          </b-field>
          <div class="field">
            <b-checkbox size="is-small" v-model="overtime">Overtime</b-checkbox>
          </div>
        </div>
        <div class="column is-half">
          <p class="label">Distance From Home: <span class="label-value">{{ distanceHome }}</span></p>
          <b-field>
            <b-slider size="is-small" v-model="distanceHome" :min="1" :max="29">
            </b-slider>
          </b-field>
          <p class="label">Years with Current Manager: <span class="label-value">{{ yearsCurrentManager }}</span></p>
          <b-field>
            <b-slider size="is-small" v-model="yearsCurrentManager" :min="0" :max="17">
            </b-slider>
          </b-field>
          <p class="label">Monthly Income: <span class="label-value">{{ monthlyIncome }}</span></p>
          <b-field>
            <b-slider size="is-small" v-model="monthlyIncome" :min="1000" :max="20000">
            </b-slider>
          </b-field>
          <p class="label">Environment Satisfaction: </p>
          <b-field>
            <b-select v-model="environmentSatisfaction" placeholder="Choose an option" size="is-small" expanded>
              <option value="1">Low</option>
              <option value="2">Medium</option>
              <option value="3">High</option>
              <option value="4">Very High</option>
            </b-select>
          </b-field>
        </div>
      </div>
      <div class="section-button">
        <b-button class="btn-predict" @click="onClick" type="is-primary">Predict</b-button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      education: 1,
      nrCompaniesWorked: 0,
      maritalStatus: 'Single',
      totalWorkingYears: 0,
      businessTravel: 'Non-Travel',
      yearsSincePromotion: 0,
      yearsCompany: 0,
      jobSatisfaction: 1,
      distanceHome: 1,
      yearsCurrentManager: 0,
      monthlyIncome: 1000,
      environmentSatisfaction: 1,
      overtime: false
    }
  },
  methods: {
    onClick() {
      let mymap = new Map() 
      mymap.set(true, 'Yes')
      mymap.set(false, 'No')
      let postData = {
        Education: Number.parseInt(this.education),
        NumCompaniesWorked: this.nrCompaniesWorked,
        MaritalStatus: this.maritalStatus,
        TotalWorkingYears: this.totalWorkingYears,
        BusinessTravel: this.businessTravel,
        YearsSinceLastPromotion: this.yearsSincePromotion,
        YearsAtCompany: this.yearsCompany,
        JobSatisfaction: Number.parseInt(this.jobSatisfaction),
        DistanceFromHome: this.distanceHome,
        YearsWithCurrManager: this.yearsCurrentManager,
        MonthlyIncome: this.monthlyIncome,
        EnvironmentSatisfaction: Number.parseInt(this.environmentSatisfaction),
        OverTime: mymap.get(this.overtime)
      }
      axios.post('http://127.0.0.1:5000/model', postData)
      .then((response) => {
        console.log(response.data)
        this.predictedClass = response.data.prediction
        this.predictedProbability = response.data.probability
        this.$emit('prediction', {
          'prediction': this.predictedClass, 
          'probability': this.predictedProbability
        })
      })
    }
  }
}
</script>

<style lang='scss'>
h2 {
  font-weight: 800;
}
.input-section {
  height: 95vh;
  padding-top: 1rem;
}
.input-fields {
  width: 90%;
  margin: auto;
}
hr.underline {
  margin: 0.25rem 0 0.75rem 0;
  border: 1px solid #0B0D3E;
}
.btn-predict {
  background-color: #0B0D3E;
}
div.field {
  margin-bottom: 1.25rem;
  &:not(:last-child) {
    margin-bottom: 1.25rem;
  }
}

p.label {
  font-size: .75rem;
  font-weight: 500;
  &:not(:last-child) {
    margin-bottom: 0;
  }
}

span.label-value {
  font-weight: 800;
}
</style>
