<template>
    <div>
        <div class="card">

            <router-link to="/schedule">
                <button class="btn btn-outline-primary mt-3 ml-3"><i class="fas fa-chevron-left mx-2"></i> Back</button>
            </router-link>

            <form class="card-body" v-on:submit.prevent="submit">
                <h4>Create a Time Schedule</h4>

                <div class="mt-4">
                    <div class="mb-3 col-12 d-flex flex-row gap-5">
                        <div class="form-check">
                            <input class="form-check-input" v-model="dayType" type="radio" value="normal" name="flexRadioDefault" id="flexRadioDefault1">
                            <label class="form-check-label" for="flexRadioDefault1">
                                Normal Schedule
                            </label>
                        </div>
                        <div class="form-check">
                            <input class="form-check-input" v-model="dayType" type="radio" name="flexRadioDefault" value="special" id="flexRadioDefault2" checked>
                            <label class="form-check-label" for="flexRadioDefault2">
                                Special Schedule
                            </label>
                        </div>
                    </div>
                </div>

                <div class="d-flex flex-row justify-content-between my-4">
                    <div class="col-4 gap-3">
                        <label for="inputPassword" class="col-form-label lbl-common" >Special Date</label>
                        <div class="">
                            <input type="date"  v-model.trim="$v.date.$model" :class="{'is-invalid': validationStatus($v.date) && dayType=='special'}" class="form-control text-uppercase" id="inputPassword" :readonly="dayType=='normal'">
                        </div>
                        <div v-if="!$v.date.required && dayType=='special'" class="text-danger"><small>If a special schedule!, date is required.</small></div>
                    </div>

                    <div class="col-4 gap-3">
                        <label for="inputPassword" class="col-form-label">Time Period</label>
                        <div class="d-flex flex-row gap-2">
                            <input v-model.trim="$v.timeFrom.$model" :class="{'is-invalid': validationStatus($v.timeFrom)}" type="time" class="form-control">
                            <input v-model.trim="$v.timeTo.$model" :class="{'is-invalid': validationStatus($v.timeTo)}" type="time" class="form-control">
                        </div>
                        <div v-if="!$v.timeFrom.required || !$v.timeTo.required" class="text-danger"><small>Time period is required.</small></div>
                    </div>

                    <div class="col-4 gap-3">
                        <label for="inputPassword" class="col-form-label">Day</label>
                        <div class="d-flex flex-row gap-2 w-100">
                            <select v-model.trim="$v.day.$model" :class="{'is-invalid': validationStatus($v.day)}" class="form-select" aria-label="Default select example">
                            <option selected>Select</option>
                            <option value="1">One</option>
                            <option value="2">Two</option>
                            <option value="3">Three</option>
                            </select>
                        </div>
                        <div v-if="!$v.day.required" class="text-danger"><small>Day is required.</small></div>
                    </div>
                </div>


                <div class="d-flex flex-row justify-content-between">
                    <div class="col-4 gap-3">
                        <label for="inputPassword" class="col-form-label lbl-common">Teacher</label>
                        <div class="d-flex flex-row gap-2">
                            <select v-model.trim="$v.teacher.$model" :class="{'is-invalid': validationStatus($v.teacher)}" class="form-select" aria-label="Default select example">
                                <option selected>Select</option>
                                <option value="1">One</option>
                                <option value="2">Two</option>
                                <option value="3">Three</option>
                            </select>
                        </div>
                        <div v-if="!$v.teacher.required" class="text-danger"><small>Teacher is required.</small></div>
                    </div>

                    <div class="col-4 gap-3">
                        <label for="inputPassword" class="col-form-label">Subject</label>
                        <div class="d-flex flex-row gap-2">
                            <select v-model.trim="$v.subject.$model" :class="{'is-invalid': validationStatus($v.subject)}" class="form-select" aria-label="Default select example">
                            <option selected>Select</option>
                            <option value="1">One</option>
                            <option value="2">Two</option>
                            <option value="3">Three</option>
                            </select>
                        </div>
                        <div v-if="!$v.subject.required" class="text-danger"><small>Subject is required.</small></div>
                    </div>
                    <div class="col-4 gap-3">
                        <label for="inputPassword" class="col-form-label align-middle">Classroom</label>
                        <div class="align-middle">
                            <select v-model.trim="$v.classroom.$model" :class="{'is-invalid': validationStatus($v.classroom)}" class="form-select" aria-label="Default select example">
                                <option v-for="(c, index) in dropdowns.cids" :key="index" value="25">{{c.cid}}</option>
                            </select>
                        </div>
                        <div v-if="!$v.classroom.required" class="text-danger"><small>Classroom is required.</small></div>
                    </div>
                </div>


                <div class="d-flex flex-row justify-content-between my-4">
                    <div class="mb-3 col-12">
                        <label for="exampleFormControlTextarea1" class="form-label">Review</label>
                        <textarea v-model.trim="$v.review.$model" :class="{'is-invalid': validationStatus($v.review)}" class="form-control" id="exampleFormControlTextarea1" rows="3"></textarea>
                        <small class="text-secondary">{{this.review.length}} /250 Characters</small>
                    </div>
                </div>
                 

                <div class="d-flex justify-content-center gap-3 row">
                    <input type="submit" class="btn btn-primary w-auto" value="Add Time Schedule">
                    <input v-on:click="clearForm" type="button" class="btn btn-outline-primary w-auto" value="clear">
                </div>

            </form>
        </div>
    </div>
</template>

<script>
import { required, maxLength, alphaNum } from 'vuelidate/lib/validators'
const dayTypeValidate = () => !((this.dayType == 'special') && (this.date == ''));

export default {
    data: function() {
        return {
            dayType:'normal',
            date: '',
            timeFrom: '',
            timeTo: '',
            day: '',
            teacher: '',
            subject: '',
            classroom: '',
            review: '',
            dropdowns: {
                tids:[],
                sids:[],
                cids:[]
            }
        }
    },
    validations: {
        date: {required},
        timeFrom: {required},
        timeTo: {required},
        day: {required},
        teacher: {required},
        subject: {required},
        classroom: {required},
        review: {maxLength: maxLength(250)}
    },
    methods: {
        validationStatus: function(validation) {
            return typeof validation != "undefined" ? validation.$error: false;
        },
        submit: function() {
            this.$v.$touch();
            if(this.$v.$pendding || this.$v.$error) {
                swal("Rejected","Fill all the required fields correctly !", "error", {
                    button: "Got It!"
                }); 
                return;
            } else {
                swal("Sussessfull", " A new time schedule successfully added !", "success");
            }
        },
        clearForm: function() {
            Object.assign(this.$data, this.$options.data.call(this));
        },
     },
     created() {
        //Fill form dropdowns when form created
        this.$http.get('http://localhost:8000/api/classrooms/getall')
        .then(function (response) {
            console.log(response);
            this.dropdowns.cids = response.body.allClassrooms;
        });
     },
    computed: {
        dateValidate(val) {
            switch(val) {
                case 'normal':
                    return;
                case 'special':
                    return 'required'
            }
        },
        // GET Methods
        
    }
}
</script>



<style scoped>

.lbl-common{
    width: 90px;
}
</style>