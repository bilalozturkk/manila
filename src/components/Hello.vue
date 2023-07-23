<template>
    <div v-if="this.code !== undefined">
        <div class="container">
            <div class="jumbotron text-center bg-light">
                <div class="row">
                    <div class="col-sm-2 text-left">
                        <img src="../assets/m_1_logo.png" width="100">
                    </div>
                    <div class="col-sm-10 align-middle">
                        <br>
                        <br>
                        <h2 class="align-middle">Welcome {{ username }}!</h2>
                    </div>
                </div>
            </div>
        </div>
    </div>
    <div v-else>
        <div class="container">
            <div class="jumbotron text-center bg-light">
                <div class="row">
                    <div class="col-sm-4"></div>
                    <div class="col-sm-4">
                        <img src="../assets/m_2_logo.png" width="100%">
                    </div>
                    <div class="col-sm-4"></div>
                </div>
            </div>
        </div>
    </div>
    
    <div class="container">
        <div v-if="this.code !== undefined">
            <div class="row" style="margin-top: 50px;">
                <div class="col-sm-4">
                    <div id="MainMenu">
                        <div class="list-group panel">
                            <a href="#" @click="getPatientDetails()" class="list-group-item" v-bind:class="getColorClassProfile"><i class="fas fa-user"></i> Profile</a>
                            <a href="#" @click="searchObservations()" class="list-group-item" v-bind:class="getColorClassLab"><i class="fas fa-flask"></i> Lab Records</a>
                            <a href="#" @click="searchImmunizationRecords()" class="list-group-item" v-bind:class="getColorClassImmunization"><i class="fas fa-syringe"></i> Immunization Records</a>
                            <hr style="margin: 0;"/>
                            <a href="#" @click="logout()" class="list-group-item list-group-item-light"><i class="fas fa-sign-out-alt"></i> Logout <small>({{ username }})</small></a>
                        </div>
                    </div>
                </div>
                <div class="col-sm-1"></div>
                <div class="col-sm-7">
                    <div v-if="showPatientData">
                        <h2 style="text-align: center;">Profile</h2>
                        <hr/>
                        <table class="table" style="margin-top: 20px;">
                            <tbody>
                                <!-- <tr>
                                    <td>Id</td>
                                    <td>{{ patientId }}</td>
                                </tr> -->
                                <tr>
                                    <td>Name</td>
                                    <td>{{ patientData.name[0].text }}</td>
                                </tr>
                                <tr>
                                    <td>IsActive</td>
                                    <td>{{ patientData.active }}</td>
                                </tr>
                                <tr>
                                    <td>City</td>
                                    <td>{{ patientData.address[0].city }}</td>
                                </tr>
                                <tr>
                                    <td>BirthDate</td>
                                    <td>{{ patientData.birthDate }}</td>
                                </tr>
                                <tr>
                                    <td>Gender</td>
                                    <td>{{ patientData.gender }}</td>
                                </tr>
                                <tr>
                                    <td>Marial Status</td>
                                    <td>{{ patientData.maritalStatus.text }}</td>
                                </tr>
                                <tr>
                                    <td>Telecom</td>
                                    <td>{{ patientData.telecom[0].value }}</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                    <div v-if="showReportList">
                        <table>
                            <thead>
                                <tr>
                                    <th colspan="2">Diagnostic Report List</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr>
                                    <td>Total Count</td>
                                    <td>{{ reportList.total }}</td>
                                </tr>
                                <tr>
                                    <td>Code</td>
                                    <td>{{ reportList.entry[0].resource.code.text }}</td>
                                </tr>
                                <tr>
                                    <td>Url</td>
                                    <td>{{ reportList.entry[0].fullUrl }}</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                    <div v-if="showReport">
                        <select @change="getDiagnosticReport(this.value)" v-model="selectedReportId">
                            <option key="0" value="0">Choose a Diagnostic Report</option>
                            <option v-for="option in reportOptionList" :key="option.value" :value="option.value">
                                {{ option.text }}
                            </option>
                        </select>
                    </div>
                    <div v-if="showObservations">
                        <h2 style="text-align: center;">Lab Records</h2>
                        <hr/>
                        <table class="table" style="margin-top: 20px;">
                            <thead>
                                <tr>
                                    <th>Date&Time</th>
                                    <th>Code</th>
                                    <th>Value</th>
                                    <th>Unit</th>
                                    <th>Reference Value Range</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="entry in filteredObservationList">
                                    <td>{{entry.resource.effectiveDateTime}}</td>
                                    <td>{{entry.resource.code?.text}}</td>
                                    <td>{{entry.resource.valueQuantity?.value}}</td>
                                    <td>{{ entry.resource.valueQuantity?.unit }}</td>
                                    <td>{{ entry.resource.referenceRange?.text}}</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                    <div v-if="showImmunizations">
                        <h2 style="text-align: center;">Immunization Records</h2>
                        <hr/>
                        <table class="table" style="margin-top: 20px;">
                            <thead>
                                <tr>
                                    <th>Date</th>
                                    <th>Vaccine</th>
                                    <th>Manufacturer</th>
                                    <th>Dose</th>
                                    <th>Unit</th>
                                    <th>Status</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="entry in filteredImmunizationList.slice().reverse()">
                                    <td>{{entry.resource.occurrenceDateTime}}</td>
                                    <td>{{ entry.resource.vaccineCode?.text}}</td>
                                    <td>{{entry.resource.manufacturer?.display}}</td>
                                    <td>{{entry.resource.doseQuantity?.value}}</td>
                                    <td>{{ entry.resource.doseQuantity?.unit }}</td>
                                    <td>{{ entry.resource.status}}</td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
            
        </div>
        <div v-else>
            <div class="row">
                <div class="col-sm-4"></div>
                <div class="col-sm-4">
                    <img src="../assets/login.png" width="100%">
                </div>
                <div class="col-sm-4"></div>
            </div>
            <div class="row">
                <div class="col-sm-4"></div>
                <div class="col-sm-4" style="text-align: center;">
                    <button type="button" style="width: 75%;" class="btn btn-primary" @click="signIn()">Login</button>
                </div>
                <div class="col-sm-4"></div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from "axios";
export default {
    data() {
        return {
            username: "",
            showDetails: false,
            code: undefined,
            accesstoken: "",
            patientId: "",
            patientData: undefined,
            showPatientData: false,
            reportList: [],
            showReportList: false,
            reportOptionList: [],
            showReport: false,
            selectedReportId: undefined,
            showObservations: false,
            immunizationList: [],
            showImmunizations: false,
            profileClicked: false,
            labClicked: false,
            immunizationClicked: false,
            clientId: "d817a94d-17f6-41b7-99ac-75e7f2306a80", // Replace with your client id 42f3b173-16a8-4c50-a3ea-0269294cb869
            redirect: import.meta.env.PROD
                ? "https://lucid-wozniak-940eae.netlify.app"
                : "http://localhost:3000",
        };
    },
    computed: {
        authorizeLink() {
            return 
        },
        filteredObservationList(){
            return this.observationList.entry.filter(x => "link" in x)
        },
        filteredImmunizationList(){
            return this.immunizationList.entry.filter(x => "link" in x)
        },
        getColorClassProfile(){
            return this.profileClicked === false ? "list-group-item-light" : "list-group-item-warning";
        },
        getColorClassLab(){
            return this.labClicked === false ? "list-group-item-light" : "list-group-item-warning";
        },
        getColorClassImmunization(){
            return this.immunizationClicked === false ? "list-group-item-light" : "list-group-item-warning";
        }
    },
    async mounted() {
        this.code = this.$route.query.code;
        if (this.code != undefined) {
            const params = new URLSearchParams();

            params.append("grant_type", "authorization_code");
            params.append("redirect_uri", this.redirect);
            params.append("code", this.code);
            params.append("client_id", this.clientId);
            params.append("state", "1234");
            const config = {
                headers: {
                    "Content-Type": "application/x-www-form-urlencoded",
                },
            };
            await axios
                .post(
                    "https://fhir.epic.com/interconnect-fhir-oauth/oauth2/token",
                    params,
                    config
                )
                .then((response) => {
                    this.accesstoken = response.data.access_token;
                    this.patientId = response.data.patient;
                    this.showPatientData = false;

                    this.getPatientDetails();
                })
                .catch(function (error) {
                    console.log(error);
                });
        }
    },
    methods: {
        signIn(){
            if(this.code == undefined || this.accesstoken != "")
                window.location.href = `https://fhir.epic.com/interconnect-fhir-oauth/oauth2/authorize?response_type=code&redirect_uri=${this.redirect}&client_id=${this.clientId}&state=1234`;
        },
        async getPatientDetails(){
            if (this.accesstoken != "") {
                this.profileClicked = true;
                this.labClicked = false;
                this.immunizationClicked = false;

                await axios
                    .get(
                        `https://fhir.epic.com/interconnect-fhir-oauth/api/FHIR/R4/Patient/${this.patientId}`,
                        { headers: { Authorization: `Bearer ${this.accesstoken}` } }
                    )
                    .then((response) => {
                        this.patientData = response.data;
                        this.username = this.patientData.name[0].text;

                        this.showImmunizations = false;
                        this.showPatientData = true;
                        this.showObservations = false;
                    })
                    .catch((error) => {
                        console.log(error);
                    });
            }
            else{
                alert("You're currently logged out!");
                location.reload();
            }
        },
        async searchDiagnosticReports(){
            if (this.accesstoken != "") {
                await axios
                    .get(
                        `https://fhir.epic.com/interconnect-fhir-oauth/api/FHIR/R4/DiagnosticReport/?patient=${this.patientId}`,
                        { headers: { Authorization: `Bearer ${this.accesstoken}` } }
                    )
                    .then((response) => {
                        this.reportList = response.data;
                        console.log(response.data);

                        this.showReportList = true;
                        this.showPatientData = false;
                        this.showReport = false;
                        this.showObservations = false;

                        this.reportOptionList.push({
                            text: this.reportList.entry[0].resource.code.text + " - " + this.reportList.entry[0].resource.effectiveDateTime,
                            value: this.reportList.entry[0].fullUrl.split("/").pop()
                        })
                    })
                    .catch((error) => {
                        console.log(error);
                    });

            }
            else{
                alert("You're currently logged out!");
            }
        },
        listDiagnosticReports(){
            this.showReport = true;
            this.showReportList = false;
            this.showPatientData = false;
            this.showObservations = false;
        },
        async getDiagnosticReport(){
            alert(this.selectedReportId);
            await axios
                .get(
                    `https://fhir.epic.com/interconnect-fhir-oauth/api/FHIR/R4/DiagnosticReport/` + this.selectedReportId,
                    { headers: { Authorization: `Bearer ${this.accesstoken}` } }
                )
                .then((response) => {
                    this.report = response.data;
                    console.log(response);

                    this.showReportList = false;
                    this.showPatientData = false;
                    this.showObservations = false;
                })
                .catch((error) => {
                    console.log(error);
                });
        },
        async searchObservations(){
            if (this.accesstoken != "") {

                this.profileClicked = false;
                this.labClicked = true;
                this.immunizationClicked = false;

                await axios
                    .get(
                        `https://fhir.epic.com/interconnect-fhir-oauth/api/FHIR/R4/Observation?patient=${this.patientId}&category=laboratory`,
                        { headers: { Authorization: `Bearer ${this.accesstoken}` } }
                    )
                    .then((response) => {
                        this.observationList = response.data;

                        console.log(response.data);
                        
                        this.showImmunizations = false;
                        this.showPatientData = false;
                        this.showObservations = true;
                    })
                    .catch((error) => {
                        console.log(error);
                    });
            }
            else{
                alert("You're currently logged out!");
            }
        }
        ,
        async searchImmunizationRecords(){
            if(this.accesstoken != ""){
                this.profileClicked = false;
                this.labClicked = false;
                this.immunizationClicked = true;

                await axios
                    .get(
                        `https://fhir.epic.com/interconnect-fhir-oauth/api/FHIR/R4/Immunization?patient=${this.patientId}`,
                        { headers: { Authorization: `Bearer ${this.accesstoken}` } }
                    )
                    .then((response) => {
                        this.immunizationList = response.data;
                        console.log(response.data);

                        this.showImmunizations = true;
                        this.showPatientData = false;
                        this.showObservations = false;
                    })
                    .catch((error) => {
                        console.log(error);
                    });
            }
            else{
                alert("You're currently logged out!");
            }
        },
        logout(){
            location.href = "http://localhost:3000";
        }
    }
}

</script>

<style>
    .list-group.panel > .list-group-item {
        border-bottom-right-radius: 4px;
        border-bottom-left-radius: 4px
    }
    a:hover {
        background-color: #FFF9E2;
        color: black;
    }
</style>